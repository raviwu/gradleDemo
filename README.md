A simple demo repo for gradle build tool.

## Build Phases of Gradle

### Initialization Phase

Use to configure multi project builds

### Configuration Phase

Executes code in the task that's not the action

```
Task myTask {
    description "myTask Description"
}
```

### Execution Phase

Execute the task actions

```
task Task6 {
    description "this is task 6"
    doLast {
        println "Task 6 doLast 1st time"
    }
    doFirst {
        println "Task 6 doFirst 1st time"
    }
    doLast {
        println "Task 6 doLast 2nd time"
    }
    doFirst {
        println "Task 6 doFirst 2nd time"
    }
}
```

The `doFirst` is actually executed in DESC order, and `doLast` executed in ASC order.

```shell
gradle Task6

> Task :Task6
Task 6 doFirst 2nd time
Task 6 doFirst 1st time
Task 6 doLast 1st time
Task 6 doLast 2nd time
```

## Task Dependencies

By using `TaskName.dependsOn DependedTaskName` can provide dependencies assignment for gradle during execution phase.

```
task Task5 << { println "this is task 5" }
Task5 << { println "another line" }

task Task6 {
    description "this is task 6"
    doLast {
        println "Task 6 doLast"
    }
}

Task6.dependsOn Task5
```

```shell
gradle Task6

> Task :Task5
this is task 5
another line

> Task :Task6
Task 6 doLast
```

Multiple task dependencies are supported. Gradle build an hirarchy of dependencies during configuration phases so that during execution phase can correctly execute the task in order.
