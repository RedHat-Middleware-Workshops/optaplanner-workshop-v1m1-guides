
# Domain Model

In this section you will learn

1. How to analyze an optimization problem domain.

2. How to create a domain model in CodeReady.

3. How to annotate your domain model with the correct OptaPlanner annotations.

The Domain Model of a planning problem is defined as simple POJOs (Plain Old Java Objects), annotated with OptaPlanner annotations to provide additional meta-data to the OptaPlanner engine. This meta-data describes, among other things, the:

- Planning Entities: A planning entity is a JavaBean (POJO) that changes during solving. For example, a `Process` that gets assigned to different computers, or a `ShiftAssignment` that gets assigned to employees.
- Planning Variable: a variable (or property, or attribute) of a _PlannningEntity_. This is the property that OptaPlanner can 'play with' during planning. For example the `computer` property of a `Process`, or the `employee` and `shift` of a `ShiftAssignment`.
- PlanningSolution: A dataset for a planning problem needs to be wrapped in a class OptaPlanner to solve. The solution class represents both the planning problem and (if solved) a solution. The `PlanningSolution` also holds the _score_ of the solution.



## The Planning Problem

As stated in the use-case, our planning problem is assigning _Processes_ to _Computers_. The _Computers_ have a certain capacity of:
- CPU
- Memory
- Network bandwidth

Also, a _Computer_ has a certain cost. I.e. when a computer is being used because a _Process_ has been assigned to it, we have to pay for the computer. As stated in the use-case, the optimization goal is to minimize the costs of our computers.

---
**NOTE**

In this planning problem we only have a single property on which we want to optimize, the costs of our computers. However, real planning problems usually have multiple properties on which you want to optimize. For example, in this use-case, apart from optimizing on the lowest costs, we could also define that we want to use 80% of the available resources (CPU, memory, network bandwidth) to allow for load spikes at runtime.

---

The _Processes_ that we need to assign to our _Computers_ require a certain amount of:
- CPU
- Memory
- Network bandwidth

Our goal is to define the domain model that describes our planning problem, and allows OptaPlanner to find the optimal solution of assigning processes to computers in a such a way that we minimize the costs of the required computers.

## The Model: Computer

When we analyse our use-case description, we can clearly identify 2 classes:
- `Computer`
- `Process`

Let's start with the `Computer`. Our computer has 4 properties:

|       Name       | Type | Description                    |
|:----------------:|:----:|--------------------------------|
| cpuPower         |  int | Amount of available CPU power. |
| memory           | int  | Amount of available memory.    |
| networkBandwidth | int  | Available network bandwidth.   |
| cost             | int  | Cost of the computer           |


We will call our class `CloudComputer`. Let's implement this class in our project. We will use the following package name to keep our implementation aligned with the examples provided with the OptaPlanner distirbution: `org.optaplanner.examples.cloudbalancing.domain`

1. Go back to your CodeReady Workspaces environment. In the project you've imported, expand the folder `src/main/java`. Right-click on the _java_ folder and click on _New -> Java Package_. Give the package the name: `org.optaplanner.examples.cloudbalancing.domain`

    ![CodeReady Workspaces New Package]({% image_path codeready-new-package.png %}){:width="600px"}

2. Right-click on the package you've just created and click on _New -> Java Class_. Name the class `CloudComputer`.

    ![CodeReady Workspaces New Java Class]({% image_path codeready-new-java-class.png %}){:width="600px"}

3. In your newly created class, create the 4 private variables listed in table above:

    ![CodeReady Workspaces Cloud Computer 1]({% image_path codeready-cloud-computer-1.png %}){:width="600px"}

4. With the 4 variables created, we need to create the accessors (getter and setter methods). Right click in the editor and in the pop-up menu select _Quick Fix_ and double click on _Generate Getters and Setters_:

    ![CodeReady Workspaces Cloud Computer Quick Fix]({% image_path codeready-cloud-computer-quick-fix.png %}){:width="600px"}

5.  With the getters and setters created, we only need to create the following 2 constructors to our class:

```
public CloudComputer() {
    }

public CloudComputer(int cpuPower, int memory, int networkBandwidth, int cost) {
  this.cpuPower = cpuPower;
  this.memory = memory;
  this.networkBandwidth = networkBandwidth;
  this.cost = cost;
}
```

    ![CodeReady Workspaces Cloud Computer Constructors]({% image_path codeready-cloud-computer-constructors.png %}){:width="600px"}

6. Test your implementation by running a Maven build. One way of doing this is by clicking on _Run -> Commands Palette_, and in the Commands Palette, click on _Maven Build_:

    ![CodeReady Workspaces Run Commands Palette]({% image_path codeready-commands-palette.png %}){:width="600px"}

    ![CodeReady Workspaces Run Commands Palette Maven Build]({% image_path codeready-commands-palette-maven-build.png %}){:width="600px"}


7. If everything has been implemented correctly, your terminal output should say something like this:

```
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 12.911 s
[INFO] Finished at: 2019-07-03T10:30:36Z
[INFO] ------------------------------------------------------------------------
```

## The Model: Process

The second class we can identify is the `Proces`. The `Process` class has the following attributes:

|       Name               | Type          | Description                                     |
|:------------------------:|:-------------:|-------------------------------------------------|
| requiredCpuPower         | int           | Amount of required CPU power.                   |
| requiredMemory           | int           | Amount of required memory.                      |
| requiredNetworkBandwidth | int           | Required network bandwidth.                     |
| computer                 | CloudComputer | The computer to which this process is assigned. |

We will call our class `CloudProcess`. Implement the class in the same package as the `CloudComputer` we created earlier.

1. Create the new class `CloudProcess` in the package `org.optaplanner.examples.cloudbalancing.domain`.
2. Create the variables and their getters and setters.
3. Create the following 2 constructors:
```
public CloudProcess() {
}

public CloudProcess(int requiredCpuPower, int requiredMemory, int requiredNetworkBandwidth) {
    this.requiredCpuPower = requiredCpuPower;
    this.requiredMemory = requiredMemory;
    this.requiredNetworkBandwidth = requiredNetworkBandwidth;
}
```
4. Run a Maven build to verify that your project compiles successfully.


## The Model: Planning Entity and Planning variable

So far, we've implemented the 2 classes of our domain, `CloudComputer` and `CloudProcess`. We now need to define which of these classes is our `PlanningEntity`, and which property (or properties) of the `PlanningEntity` is the `PlanningVariable`.

There is a rule of thumb to determine the `PlanningEntity` and `PlanningVariables` of your problem domain.
1. Draw a class diagram of your problem domain, including the relationships between your classes:

    ![Cloud Balance Class Diagram 1]({% image_path cloudBalanceClassDiagram_1.png %}){:width="600px"}

2. Determine which relationships (or fields) change during planning. In this use-case, we need to assign processes to computers. Hence, the relationship that changes during planning is the one-to-many relationship between `CloudProcess` and `CloudComputer`. One side of this relationship will become the `PlanningVariable`.

3. In a many to one relationship, usually the many side is the `PlanningEntity` class. In our case this is the `CloudProcess`. This means that the `computer` variable of the `CloudProcess` class will be the `PlanningVariable`:

    ![Cloud Balance Class Diagram 2]({% image_path cloudBalanceClassDiagram_2.png %}){:width="600px"}

4. In CodeReady Workspaces, open your project and open the `CloudProcess` class. Annotate the class with the OptaPlanner `@PlannningEntity` annotation:

    ![CodeReady Workspaces Planning Entity]({% image_path codeready-planning-entity.png %}){:width="600px"}

5. Annotate the `getComputer()` method of the `CloudProcess` class with the OptaPlanner `@PlanningVariable` annotation.

    ![CodeReady Workspaces Planning Variable]({% image_path codeready-planning-variable.png %}){:width="600px"}

6. Run a Maven Build to compile the project.

For a more elaborate guide on domain modelling in OptaPlanner, please consult [the following blogpost](https://www.optaplanner.org/blog/2016/10/26/DomainModelingGuide.html)


## Planning solution

So far, we've defined our `PlanningEntity` and `PlanningVariable`. As outlined in the introduction of this module, our domain model also requires a class that holds that dataset, the set of computers and processes, of the problem that needs to be solved. We call this the `PlanningSolution`. This class will hold both the unsolved dataset, and after OptaPlanner has found a solution, the solution and solution score of the problem.

We will call the `PlanningSolution` of our planning problem `CloudBalance` (as our problem is called the _Cloud Balancing Problem_).

1. In CodeReady, create a new class called `CloudBalance`, in the same package as our `CloudComputer` and `CloudProcess` classes.

2. Add the following annotation to the class:
```
@PlanningSolution
```

3. This planning solution class will be the container for the dataset of the problem that we need to solve. Hence, we create 2 `Lists`, on list of computers and one list of processes:

```
private List<CloudComputer> computerList;

private List<CloudProcess> processList;
```

4. It can be that the `List` type is not yet imported into your class and it will be marked with red crosses with the message "_List cannot be resolved to a type_". This can be solved by using the _Organize Imports_ feature of CodeReady. Click on _Assistant -> Organize Imports_, select `java.util.List` and click on _Finish_:

    ![CodeReady Workspaces Organize Imports]({% image_path codeready-organize-imports.png %}){:width="600px"}

5. Generate the getters and setters for these 2 `Lists`.

6. Add the following 2 constructors to the class:

```
public CloudBalance() {
}

public CloudBalance(List<CloudComputer> computerList, List<CloudProcess> processList) {
  this.computerList = computerList;
  this.processList = processList;
}
```

7. Run a Maven Build to test the project.


This completes the initial domain model. In the next step of this lab we will add the OptaPlanner `ScoreHolder` to our planning solution and we will configure the so called _value range_ of our planning variables.
