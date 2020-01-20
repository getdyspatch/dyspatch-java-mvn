dyspatch-java-mvn
===================

Update instructions:
--------------------

1. Clone getdyspatch/dyspatch-java-mvn and getdyspatch/dyspatch-java to your local machine.
2. Make your changes in the dyspatch-java repo, and merge into master.
3. Build the project and update your local copy of dyspatch-java-mvn.  From the root of your dyspatch-java project, run:

    `DYSPATCH_API_KEY=<api key for qa env> mvn -DaltDeploymentRepository=snapshot-repo::default::file:FIXME/PATH/TO/dyspatch-java-mvn/releases clean deploy`
    
    Be sure to update the `file:FIXME/PATH/TO/dyspatch-java-mvn...` part of the command!
    
    As of Jan 20, 2020 there is currently a bug with the generated code and the javadoc mvn plugin, add the following to the above command `-Dmaven.javadoc.skip=true`

4. Push all of the changes in the dyspatch-java-mvn project to github.

Update with Docker
------------------

If you want to avoid installing all of the Java requirements on your own local machine, perform the steps above within a docker image.

```
docker run -it --rm --name dyspatch-java-maven -v "$PWD":/usr/src/mymaven -v
**PATH to maven repo source**:/usr/src/mvn-repo -w /usr/src/mymaven
maven:3.2-jdk-7 mvn
-DaltDeploymentRepository=snapshot-repo::default::file:/usr/src/mvn-repo/releases
clean deploy
```

Description:
- Run that command from within the `dyspatch-java` directory.
- Important: change the `**PATH to maven repo source**` to the absolute path to the maven directory.
- The first `-v` mounts the local (dyspatch-java) directory to the working directory ('-w') within the docker image.
- The second `-v` mounts the maven repo directory into the docker image also.
- The last part of the command runs maven and sets the output directory to the mounted maven dir where the package is generated.


Notes:
--------------------

- To build at any time, run `mvn clean install` from the root dyspatch-java directory.  This will also run unit tests.

- When any undeployed changes to the dyspatch-java client are made, '-SNAPSHOT' should be appended to the version number to indicate it is in a development state.  Remove '-SNAPSHOT' when work has completed.
