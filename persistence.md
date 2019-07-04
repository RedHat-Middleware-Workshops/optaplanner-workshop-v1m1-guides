
# Loading Data

In this section you will learn

1. How to integrate OptaPlanner with storage solutions.
2. The OptaPlanner SolutionFileIO interface to load planning problems from persistent storage.

OptaPlanner is written in Java, based on open standards, and the domain models of planning problems are, as we've seen in the previous lab, based on Java POJOs. As such, OptaPlanner does not enforce any type of storage or persistence framework. OptaPlanner can integrate with any form of storage type and format, as long as there is a Java client or framework available. For examples

- Java Persistence API (JPA) can be used to interact with relational databases
- XStream, JABX2, etc. can be used to store to and read from XML files.
- Integration with Red Hat DataGrid can be accomplished via the DataGrid HotRod client.

Being able to import, and also export, planning problems and planning solutions in a proper way is extremely important when creating an OptaPlanner application/solution. Being able to show the business the output of a planning solution, and being able to properly visualize the solution, for example in an Excel file will give your business users insight in your solution, and enables them to give feedback about the planning solutions created by your OptaPlanner implementation. In other words, it enables your business users to be part of the development process and stay in control.


## SolutionFileIO

In this lab we will read our planning problem data from XML files. OptaPlanner provides an interface called `SolutionFileIO`, which provides methods to read and write planning solutions from and to persistent storage. This interface is, among other things, used by the OptaPlanner Benchmarker to load unsolved planning problems from storage and to optionally write the best solution back to disk. Although it is not mandatory to use this interface in your OptaPlanner application (i.e. your production application), it provides a convenient abstraction between your planning solution class and storage.

We will use XStream as our XML serialization and deserialization library. OptaPlanner provides and out-of-the-box `SolutionFileIO` implementation that uses the XStream library to serialize to and from XML, the `XStreamSolutionFileIO`. We will use this out-of-the-box implementation to read our planning problem  from XML files (these files will be provided).


1. Create a new class with the name `AbstractPersistable` in the same package of our domain model and give it the following implemenation:
```
package org.optaplanner.examples.cloudbalancing.domain;

import org.optaplanner.core.api.domain.lookup.PlanningId;

/**
 * AbstractPersistable
 */
public abstract class AbstractPersistable {

    private Long id;

    protected AbstractPersistable() {
    }

    protected AbstractPersistable(Long id) {
        this.id = id;
    }

    @PlanningId
    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

}
```

2. Have our 3 domain model classes, `CloudBalance`, `CloudProcess` and `CloudComputer` extend from our `AbstractPersistable` class, e.g.:
```
public class CloudBalance extends AbstractPersistable {
```

3. Change the constructors of these classes to accept an id, and have them cal their abstract super class. I.e.:

```
public CloudBalance(Long id, List<CloudComputer> computerList, List<CloudProcess> processList) {
  super(id);
  this.computerList = computerList;
  this.processList = processList;
}
```

```
public CloudComputer(long id, int cpuPower, int memory, int networkBandwidth, int cost) {
  super(id);
  this.cpuPower = cpuPower;
  this.memory = memory;
  this.networkBandwidth = networkBandwidth;
  this.cost = cost;
}
```

```
public CloudProcess(long id, int requiredCpuPower, int requiredMemory, int requiredNetworkBandwidth) {
  super(id);
  this.requiredCpuPower = requiredCpuPower;
  this.requiredMemory = requiredMemory;
  this.requiredNetworkBandwidth = requiredNetworkBandwidth;
}
```

4. Add an `@XStreamAlias` annotation to our 3 domain model classes:

```
@PlanningSolution
@XStreamAlias("CloudBalance")
public class CloudBalance extends AbstractPersistable {
```

```
@XStreamAlias("CloudComputer")
public class CloudComputer extends AbstractPersistable {
```

```
@PlanningEntity
@XStreamAlias("CloudProcess")
public class CloudProcess extends AbstractPersistable {
```

5. Create a _Repository_ class that is responsible for loading the planning problem from the XML files. Give the class the name `CloudBalanceRepository` and store it in the package `org.optaplanner.examples.cloudbalancing.persistence`. Give it the following implementation:

```
package org.optaplanner.examples.cloudbalancing.persistence;

import java.io.File;

import org.optaplanner.examples.cloudbalancing.domain.CloudBalance;
import org.optaplanner.examples.cloudbalancing.domain.CloudComputer;
import org.optaplanner.examples.cloudbalancing.domain.CloudProcess;
import org.optaplanner.persistence.xstream.impl.domain.solution.XStreamSolutionFileIO;

public class CloudBalanceRepository {

    public static CloudBalance load(File inputSolutionFile) {
        XStreamSolutionFileIO<CloudBalance> solutionFileIO = new XStreamSolutionFileIO<>(CloudBalance.class,
                CloudProcess.class, CloudComputer.class);
        return solutionFileIO.read(inputSolutionFile);
    }
}
```
