# Gradle memory aid

## gradle commands vs. maven commands

 Maven Command                  | Gradle Command                 | Explanation                                      |
|-------------------------------|--------------------------------|--------------------------------------------------|
| mvn clean                      | gradle clean                 | Cleans the build artifacts and temporary files.  |
| mvn compile                    | gradle compileJava           | Compiles the Java source code.                   |
| mvn test                       | gradle test                  | Runs unit tests.                                 |
| mvn package                    | gradle assemble              | Packages the compiled code into a distributable. |
| mvn install                    | gradle install               | Installs the artifact in the local repository.   |
| mvn deploy                     | gradle publish               | Publishes the artifact to a remote repository.   |
| mvn dependency:tree            | gradle dependencies          | Displays project dependencies as a tree.         |
| mvn spring-boot:run            | gradle bootRun               | Runs a Spring Boot application.                  |
| mvn clean install              | gradle clean build           | Cleans and then installs the artifact.           |
| mvn tomcat:run                 | gradle bootRun               | Runs a Tomcat server with the web application.   |
| mvn site                       | gradle site                  | Generates project site documentation.            |
| mvn eclipse:eclipse            | gradle eclipse               | Generates Eclipse IDE project files.             |
| mvn dependency:purge-local-repository | N/A                    | Removes project dependencies from local repository. |
| mvn validate                   | gradle check                 | Validates the project (syntax, configurations).  |

## troubleshoot

### IDLE/Lock problem

Error:

```shell
# command 
gradle assemble

# error message
Starting a Gradle Daemon, 1 incompatible and 3 stopped Daemons could not be reused, use --status for details

FAILURE: Build failed with an exception.

* What went wrong:
Gradle could not start your build.
> Cannot create service of type BuildSessionActionExecutor using method LauncherServices$ToolingBuildSessionScopeServices.createActionExecutor() as there is a problem with parameter #21 of type FileSystemWatchingInformation.
   > Cannot create service of type BuildLifecycleAwareVirtualFileSystem using method VirtualFileSystemServices$GradleUserHomeServices.createVirtualFileSystem() as there is a problem with parameter #7 of type GlobalCacheLocations.
      > Cannot create service of type GlobalCacheLocations using method GradleUserHomeScopeServices.createGlobalCacheLocations() as there is a problem with parameter #1 of type List<GlobalCache>.
         > Could not create service of type FileAccessTimeJournal using GradleUserHomeScopeServices.createFileAccessTimeJournal().
            > Timeout waiting to lock journal cache (/Users/XXX/.gradle/caches/journal-1). It is currently in use by another Gradle instance.
              Owner PID: 33272
              Our PID: 33401
              Owner Operation: 
              Our operation: 
              Lock file: /Users/XXX/.gradle/caches/journal-1/journal-1.lock

* Try:
> Run with --stacktrace option to get the stack trace.
> Run with --info or --debug option to get more log output.
> Run with --scan to get full insights.
> Get more help at https://help.gradle.org.
```

Solution:

```shell
gralde --stop
gradle assemble 

#or when this occurs "Timeout waiting to lock journal cache (/Users/wej/.gradle/caches/journal-1). It is currently in use by another Gradle instance."
find ~/.gradle -type f -name "*.lock" -delete
```
