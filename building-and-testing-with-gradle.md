- The configuration block of a task is run during Gradle's **configuration lifecycle phase**, which runs *before* the **execution phase**, when task actions are executed.

- Every time Gradle executes a build, it runs through three lifecycle phases:
  - **initialization**
  - **configuration**, the place to set up variables and data structures that will be needed by the task action
  - **execution**
  
- All build configuration code runs every time you run a Gradle build file, regardless of whether any given task runs during execution.

- Tasks are objects. Each new task receives the type of **DefaultTask**, and have below methods:
  - dependsOn(task)
  - doFirst(closure)
  - doLast(closure)
  - onlyIf(closure)
  
  have below properties:
  - didWork
  - enabled
  - path
  - logger
  - logging
  - description
  - temporaryDir
  
  and can define arbitrary properties as long as the names don't collide with the built-in property names.
  
- Besides the DefaultTask, there are task types for copying, archiving, executing programs... Declaring a task type is a lot like extending a base class in an object-oriented programming language. Some common task types:
  - Copy
  - Jar
  - JavaExec
  
  example task definition extends Copy:
  ```groovy
  task copyFiles(type: Copy) {
    from 'resources'
    into 'target'
    include '**/*.xml', '**/*.txt', '**/*.properties'
  }
  ```
