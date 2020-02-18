
# OptaPlanner Introduction

In this section you will learn:

1. What a _planning problem_ is.

2. Why _planning problems_ are hard to solve.

3. How OptaPlanner's A.I. heuristic algorithms solve these problems


## What is a Planning Problem?

Simply said, a _planning problem_ is a problem that wants to achieve certain _Goals_, with _Limited Resources_, _Under Constraints_.

![Planning Problem]({% image_path planning-problem.png %}){:width="600px"}

We can take a _vehicle routing problem_ (VRP) as an example. In a VRP, your goals can be to:

- Minimize driving time
- Minimize fuel consumption
- Minimize the required amount of vehicles.

The resources involved in a VRP can be:

- Vehicles (capacity, fuel)
- Deliveries (location, packages)

And (some of) the constraints are:

- Maximum of 8 hours consecutive driving time.
- Arrive before due time.
- Maximum vehicle capacity.

In a _cloud balancing problem_, the goals, resources and constraints are.

Goals:

- Minimize costs
- Optimize resource utilization

Resources:

- Computers (CPU, Memory, Bandwidth)

Constraints:

- Process requires {x} CPU.
- Process requires {y} Memory.
- Process requires {z} Bandwidth.

Each planning problem has a set of these goals, resouces and constraints.

Planning problems are everywhere. Almost every organisation has one or more of these planning problems. From shift assignments to vehicle routing, from course scheduling to machine assignment. OptaPlanner, being a highly adaptable, flexible and customizable Constraint Solver, can provide solutions to all of these different kinds of problems.

![Planning Problem]({% image_path planning-problems.png %}){:width="600px"}

## Why are Planning Problems hard to solve?

Planning problems fall into the category of NP-hard problems. Simply said, these kind of problems can not be solved in _polynomial time_: when the size of the problem grows, the required time to solve the problem grows exponentially. The solution space, the number of different possible solutions to a given problem, of these kind of problems grows exponentially to the size of the problem.

For example, if we look at the _Cloud Balancing_ problem, the number of possible solutions in which we can assign 1 process to 100 computers is: 100

The number of possible solutions in which we can assign 2 processes to 100 computers is: 100 * 100 = 10000

So, the number of possible solutions in which we can assign 300 processes to 100 computers is: 100 ^ 300 = 10 ^ 600

When we compare this with the number of _atoms in the observable universe_, which is 10 ^ 80, we can see how large these solution spaces are.

The image below shows the search spaces for a number of different planning problems:

![Planning Problem]({% image_path search-spaces.png %}){:width="600px"}


## Solving the problem

OptaPlanner OptaPlanner is a lightweight, embeddable planning engine. It enables everyday Java™ programmers to solve optimization problems efficiently. An OptaPlanner solution is made out of a number of different components:

- A _Domain Model_ that defines the problem domain. This model is created in POJOs (Plain Old Java Objects). This allows Java developers to define and implement planner implementations for complex planning problems.
- _Constraint Rules_ written in [Drools](https://www.drools.org) or Java. This cleanly separates the constraint rules from the implementation code, allowing for easy collaboration between IT professionals and business experts when defining and implementing constraints.
- A _Solver Configuration_ which allows for advanced tuning of OptaPlanner's heuristic algorithms. This enables the user to tune the solution specifically to his/her domain, rules and datasets, creating a solution that is capable of delivering the best possible planning result.
- An easy to use Java API. An OptaPlanner solution can work in tandem with other, de-facto, Java standards, libraries and solutions to create an solver application that integrates with various datastores, (micro)services and messsaging system. This also allows the application to be deployed on bare-metal systems, virtual machines and cloud/container platforms.


![Planning Problem]({% image_path optaplanner-input-output.png %}){:width="600px"}

OptaPlanner provides a vast array of optimization algorithms, like _Late Acceptance_, _Tabu Search_ and _Simulated Annealing_. These algorithms allow OptaPlanner to find solutions to optimization problems that are far better than solutions found with traditional algorithms. These kind of solutions can result in:

- better utilization of existing resources (machines, vehicles, etc.).
- improved employee well-being.
- improved customer satisfaction.
- massive reduction in operational costs.
