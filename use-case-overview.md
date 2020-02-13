# Use Case Overview

The Cloud Optimization problem deals with the problem of assigning processes to machines more efficiently. An {x} number of computer processes, all with different CPU power, memory and network bandwidth requirements, have to be assigned to available computers. Depending on the optimization goals, for example, minimize the costs of the computers required to run the processes, these processes are assigned to the available computers.

The optimization goal in this use-case is to minimize the costs of the computers. Note that there could be other optimization goals for this planning problem, for example to only use 80% of the available resources of the computer.



## Background

Assigning processes to computers is a variant of the classical _bin packing problem_. In the _bin packing problem_, items of different volumes must be packed into a finite number of bins or containers, each of a fixed given volume in a way that minimizes the number of bins used.

In the _Cloud Balancing problem_, we have to place computer processes with different resource requirements (cpu, memory, network bandwidth) onto computers, each of them providing a certain amount of resources, in a way that minimizes the total cost of the computers.

![Cloud Optimization Problem]({% image_path cloudOptimizationValueProposition.png %}){:width="600px"}

As can be seen in the image, the value proposition of OptaPlanner/Business Optimizer is that the _hardware costs_ can be reduced by 18% and the _hardware congestion_ with 63%, compared to traditional algorithms with domain knowledge. This can greatly reduce the costs of a cloud computing infrastructure, while at the same time increase the quality of service.

--------------------------------------------------

### Requirements:

The requirements of the solution are:

- All the processes need to be assigned to a computer.
- None of the computers must be overloaded. I.e. the resource requirements of the processes assigned to a computer must not exceed the computer's available resources.
- The total costs of all required computers must be minimized.
