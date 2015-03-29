# Example maven project using both jetty:run and m2e-wtp 

While eclipse properly integrate a range of JEE servers, Jetty support is a bit lacking wrt Jetty 9. As the existing solutions are not real WTP servers adapters, using the jetty-maven-plugin might be desirable.

The main issue with such a setup is that m2e-wtp does not process resources the same way a standard maven build does. What's more, on the latest Eclipse version to date (Luna), web resources filtering deactivate the maven archiver JEE integration, making things a bit harder.

This projects solves this with a simple profile activation:

* when the m2e-wtp directory exists in target - i.e. eclipse is running and processing the project - the jetty-m2e-wtp profile is active. It overlay the m2e-wtp/web-resources directory to the target/${project.build.finalName} where maven usually stores the processed resources

* when the m2e-wtp directory is missing - i.e. we're doing a conventional maven build - the jetty-no-m2e-wtp is active, and only integrates the target/${project.build.finalName} directory, thus preventing the jetty:run goal to fail on a missing source directory

You can check which profile is active at any given time using:
```sh
$ mvn help:active-profiles
```
**IMPORTANT** profile activation does not support properties resolution so ${project.build.directory} is hardcoded to target. This should be changed when the default build directory is changed in the pom.xml.
