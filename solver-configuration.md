
# Solver Configuration

In this section you will learn:

1. How to configure the Solver.
2. How to interact with the Solver API.


The main API that applications use to interact with OptaPlanner is the Solver API.

![Solver API]({% image_path solver-overview.png %}){:width="600px"}

The `Solver` API is pretty simple. As you can see in the diagram, the `SolverFactory` is created from the domain model (which we've created earlier), the constraints (that we will define in the next module) and the solver configuration (which we dill discuss now).


## Solver Configuration

The solver configuration of OptaPlanner is defined in an XML file. The basic file structure is very simple, as OptaPlanner promotes the concept of _convention over configuration_. This implies that the basic set-up does not require a lot of configuration, and the default configuration settings should work well in most use-cases. On top of that, the solver configuration provides extensive configuration capabilities to power-tweak the solver through in-depth and advanced configuration options.

A basic solver configuration looks like this:

```
<solver>
  <scanAnnotatedClasses/>
  <scoreDirectorFactory>
    <easyScoreCalculatorClass>org.optaplanner.examples.cloudbalancing.optional.score.CloudBalancingEasyScoreCalculator</easyScoreCalculatorClass>
  </scoreDirectorFactory>
</solver>
```

In this config, we define that we want to scan our classes for annotations. This allows OptaPlanner to find/discover our planning solution and planning entity classes.

Second, the solver needs to be instructed which `ScoreCalculator` needs to be configured. The `ScoreCalculator` is responsible for calculating the score (based on the constraints) of the solution. OptaPlanner supports multiple types of score calculators:

- Easy Java score calculation: Implement all constraints together in a single method in Java (or another JVM language). Does not scale.

- Incremental Java score calculation (**not recommended**): Implement multiple low-level methods in Java (or another JVM language). Fast and scalable. Very difficult to implement and maintain.

- Drools score calculation: Implement each constraint as a separate score rule in DRL. Scalable.

- Constraint streams score calculation (**not yet supported**): Implement each constraint as a separate ConstraintStream in Java (or another JVM language). Fast and scalable.

In our sample solver configuration, we're using the _Easy Java Score Calculation_. Let's configure our first solver, implement a simple score calculator and run our first OptaPlanner `Solver`.


1. Open CodeReady Workspaces and open your project from the previous lab. You can also import the following project from GitHub and use it as a starting point for this module: [https://github.com/RedHat-Middleware-Workshops/optaplanner-workshop-v1m1-labs-step-4](https://github.com/RedHat-Middleware-Workshops/optaplanner-workshop-v1m1-labs-step-4)

2. We will first implement a skeleton `ScoreCalculator`. In your project, create a new Java package called `org.optaplanner.examples.cloudbalancing.optional.score`. In this package, create a new Java class called `CloudBalancingEasyScoreCalculator`, and give it the following implementation:

```
package org.optaplanner.examples.cloudbalancing.optional.score;

import org.optaplanner.core.api.score.buildin.hardsoft.HardSoftScore;
import org.optaplanner.core.impl.score.director.easy.EasyScoreCalculator;
import org.optaplanner.examples.cloudbalancing.domain.CloudBalance;

/**
 * CloudBalancingEasyScoreCalculator
 */
public class CloudBalancingEasyScoreCalculator implements EasyScoreCalculator<CloudBalance> {

    @Override
    public HardSoftScore calculateScore(CloudBalance solution) {
        int hardScore = 0;
        int softScore = 0;

        return HardSoftScore.of(hardScore, softScore);
    }


}
```

    Note that this is a dummy implementation that always returns the same score. We only require this implementation at the moment to configure our solver and run our first OptaPlanner run. In a later module of this workshop we will properly implement our constraints and score.

3. We will now create our solver configuration. In the `src/main/resources` directory of your project, create a new folder called `org/optaplanner/examples/cloudbalancing/solver`. In this folder, create a new XML file called `cloudBalancingSolverConfig.xml`. Give it the following content:
```
<solver>
  <scanAnnotatedClasses/>
  <scoreDirectorFactory>
    <easyScoreCalculatorClass>org.optaplanner.examples.cloudbalancing.optional.score.CloudBalancingEasyScoreCalculator</easyScoreCalculatorClass>
  </scoreDirectorFactory>
  <termination>
    <millisecondsSpentLimit>5000</millisecondsSpentLimit>
  </termination>
</solver>
```

    Note that setting the `termination` is not strictly necessary. However, if we don't set the termination, and we start the `Solver`, OptaPlanner will keep solving indefinitely (as we're basically solving unsolvable problems). Setting the _termination strategy_ instructs OptaPlanner to stop the solver, in this case after 5000 milliseconds. OptaPlanner provides multiple termination strategies, like _Unimproved Time Spent Termination_, _Best Score Termination_ and _Step Count Termination_.

We now have an implementation of our domain model, a very basic score calculator, a solver config, and solution file loader. Let's create a unit test in which we can run and test our solve.

1. In our project's `src/test/java` folder, in the package `org.optaplanner.examples.cloudbalancing.solver` folder, create a new Java class called `CloudBalancingSolverTest.java`. Give it the following implementation:

```
package org.optaplanner.examples.cloudbalancing;

import java.io.File;

import org.junit.Test;
import org.optaplanner.core.api.solver.Solver;
import org.optaplanner.core.api.solver.SolverFactory;
import org.optaplanner.examples.cloudbalancing.domain.CloudBalance;
import org.optaplanner.examples.cloudbalancing.persistence.CloudBalanceRepository;

/**
 * CloudBalancingSolverTest
 */
public class CloudBalancingSolverTest {

    @Test
    public void testSolver() {
        //Create the solver from our solver configuration.
        SolverFactory<CloudBalance> solverFactory = SolverFactory.createFromXmlResource("org/optaplanner/examples/cloudbalancing/solver/cloudBalancingSolverConfig.xml");
        Solver<CloudBalance> solver = solverFactory.buildSolver();

        //Loud a problem into the PlanningSolution
        CloudBalanceRepository repository = new CloudBalanceRepository();
        File inputFile = new File("data/cloudbalancing/unsolved/4computers-12processes.xml");
        CloudBalance cloudBalance = repository.load(inputFile);

        //Run the solver.
        solver.solve(cloudBalance);

    }

}
```

    Because we've configured a _termination strategy_ of 5 seconds (5000 milliseconds), the solver will stop after 5 seconds and the test will pass.

2. Run the Unit test by running a Maven Build. The output should show the test being executed.


## Explaining the Logs

Let's quickly observe the output. When OptaPlanner solves a problem, it runs a number of different phases. The first real outpur line of the `Solver`, after the domain classes have been scanned for annotations, is the following:

```
13:20:39.825 [main] INFO org.optaplanner.core.impl.solver.DefaultSolver - Solving started: time spent (10), best score (-12init/0hard/0soft), environment mode (REPRODUCIBLE), random (JDK with seed 0).
```

Looking at this line, we can identify the _score_ of the solution when OptaPlanner starts: (-12init/0hard/0soft). The important part in this score is the _-12init_. This indicates that we have an _uninitialized_ solution. An _uninitialized solution_ is a solution in which the _PlanningEntities_ have not yet been assigned a _PlanningVariable_. In this case we have a problem with 12 _PlanningEntities_ (in our case 12 instances of `CloudProcess`), that have not yet been assigned to a `CloudComputer`.
When we scr
In the next line we see the following:

```
13:20:39.832 [main] DEBUG org.optaplanner.core.impl.constructionheuristic.DefaultConstructionHeuristicPhase -     CH step (0), time spent (17), score (-11init/0hard/0soft), selected move count (4), picked move (org.optaplanner.examples.cloudbalancing.domain.CloudProcess@1c742ed4 {null -> org.optaplanner.examples.cloudbalancing.domain.CloudComputer@333d4a8c}).
```

First we see _CH step (0)_. _CH_ stands for _Construction Heuristics_, the first phase in an OptaPlanner `Solver` run in which OptaPlanner will construct an initialized solution (assigning PlanningVariables to PlanningEntities) in a "smart" way (OptaPlanner provides various construction heuristics algorithms).

The second thing we observe is the score: (-11init/0hard/0soft). We see that the _init_ score is decreased by 1. This is due to the fact that OptaPlanner has assigned a planning variable to one of our planning entities. In fact, we can see the actual _move_ that was picked by OptaPlanner as well. The "_(org.optaplanner.examples.cloudbalancing.domain.CloudProcess@1c742ed4 {null -> org.optaplanner.examples.cloudbalancing.domain.CloudComputer@333d4a8c})_" part of the line indicates that the planning variable of `CloudProcess@1c742ed4` has been changed from `null` to `CloudComputer@333d4a8c`.

When we scroll down in the logs, we can see the following line:
```
13:20:39.836 [main] INFO org.optaplanner.core.impl.constructionheuristic.DefaultConstructionHeuristicPhase - Construction Heuristic phase (0) ended: time spent (21), best score (0hard/0soft), score calculation speed (5444/sec), step total (12).
```

This indicates that the _Construction Heuristics_ phase has ended. The next phase in an OptaPlanner run is the _Local Search_ or _LS_ phase. (note that you can have multiple _LS_ phases, but the default configuration is a single phase). We can see that in the next line in the logs:

```
13:20:39.841 [main] DEBUG org.optaplanner.core.impl.localsearch.DefaultLocalSearchPhase -     LS step (0), time spent (26), score (0hard/0soft),     best score (0hard/0soft), accepted/selected move count (1/1), picked move (org.optaplanner.examples.cloudbalancing.domain.CloudProcess@5c86a017
```

In this phase, OptaPlanner uses a _Local Search_ algorithm, like [_Late Acceptance_](https://docs.optaplanner.org/7.23.0.Final/optaplanner-docs/html_single/index.html#lateAcceptance) or [_Tabu Search_](https://docs.optaplanner.org/7.23.0.Final/optaplanner-docs/html_single/index.html#tabuSearch), to find better and better solutions. Because we're dealing with so called NP-Complete or NP-Hard problems, we can never know if we've found the optimal solution to a problem. Hence, OptaPlanner will continue searching for better solutions during the _Local Search_ phase until either:
- another thread stops the solver via the `Solver.terminateEarly()` method.
- the configured _Termination Strategy_ stops the `Solver`.

We can also see that every step in the _Local Search_ phase results in the same score: _(0hard/0soft)_. This is due to the fact that we've not implemented our constraints yet in a proper `ScoreCalculator`.

We now have an OptaPlanner application that is executable. In the the next module we will implement our constraints in two types of score calculators.



Now that we have the ability to load planning problem data, we can go the next step of our project: configuring the solver!
