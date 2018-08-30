# A simple demo repo for gradle build tool

## Build Phases of Gradle

### Initialization Phase

Use to configure multi project builds

### Configuration Phase

Executes code in the task that's not the action

```groovy
Task myTask {
    description "myTask Description"
}
```

### Execution Phase

Execute the task actions

```groovy
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

```groovy
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

Using `mustRunAfter` and `shouldRunAfter` to assign a specific dependencies order.

## Setting Properties on Tasks

```groovy
def varName = "A variable Name"

task Task6 {
    doLast {
        println "Task 6 doLast - $varName"
    }
}
```

```shell
gradle Task6

> Task :Task6
Task 6 doLast - A variable Name
```

Variable can be defined in scope with `def` keyword.

Or using `ext.variableName = "a new variable"` and call with `$variableName` in project scope.

## Typed Task

[Typed Task DSL](https://docs.gradle.org/current/dsl/org.gradle.api.Task.html)

## Using Daemon to accelerate the build process

[Gradle Daemon](https://docs.gradle.org/current/userguide/gradle_daemon.html)

You can opt on the daemon by environment variables: add the flag `-Dorg.gradle.daemon=false` to the `GRADLE_OPTS` environment variable.

But would suggest to use properties files in the project or global configuration: add `org.gradle.daemon=false` to the `«GRADLE_USER_HOME»/gradle.properties` file

## Multi-project Builds

Add top level `build.gradle` file: define
