
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


1. Either start from the project you've been building in this workshop so far, or import the following project into your CodeReady Workspaces environment: [https://github.com/RedHat-Middleware-Workshops/optaplanner-workshop-v1m1-labs-step-4](https://github.com/RedHat-Middleware-Workshops/optaplanner-workshop-v1m1-labs-step-4)

2. Bla





Now that we have the ability to load planning problem data, we can go the next step of our project: configuring the solver!
