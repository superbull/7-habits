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

- We can create custom task types by extending existing one. For example:
  ```groovy
  class MySqlTask extends DefaultTask {
    def hostname = 'localhost'
    def port = 3306
    def sql
    def database
    def username = 'root'
    def password = 'password'
    
    @TaskAction
    def runQuery() {
      def cmd
      if(database) {
        cmd = "mysql -u ${username} -p ${password} -h ${hostname}
          -P ${port} ${database} -e "
      }
      else {
        cmd = "mysql -u ${username} -p ${password} -h ${hostname} -P ${port} -e "
      }
      project.exec {
        commandLine = cmd.split().toList() + sql
      }
    }
  }
  ```
  
  We have 4 options for where to put this custom task code:
  
  1. in the build script itself
  2. `buildSrc` directory, the same level as build.gradle
  3. a separate build script file imported into the main build script
  4. in a custom plug-in written in Java or Groovy
