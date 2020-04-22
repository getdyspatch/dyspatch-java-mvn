dyspatch-java-mvn
===================


Update with Docker
------------------

If you want to avoid installing all of the Java requirements on your own local machine, perform the steps below within a docker image.

1. Make your changes in the dyspatch-java repo, and merge into master.
1. Clone getdyspatch/dyspatch-java-mvn and getdyspatch/dyspatch-java to your local machine.
3. Run the below command from within the `dyspatch-java` directory.

    ```
    docker run -it --rm --name dyspatch-java-maven -e DYSPATCH_API_KEY=<qa prod api key> -v "$PWD":/usr/src/mymaven -v $(PWD)/../dyspatch-java-mvn:/usr/src/mvn-repo -w /usr/src/mymaven maven:3-jdk-11 mvn -DaltDeploymentRepository=snapshot-repo::default::file:/usr/src/mvn-repo/releases clean deploy
    ```

4. Push all of the changes in the dyspatch-java-mvn project to github.

Description:
- The `-e` sets an environment variable (DYSPATCH_API_KEY) inside of the docker container. This is necessary for the integration tests to run. You can find it in 1password.
- The first `-v` mounts the local (dyspatch-java) directory to the working directory ('-w') within the docker image.
- The second `-v` mounts the maven repo (dyspatch-java-mvn) directory into the docker image also. _`$(PWD)/../dyspatch-java-mvn` assumes that dyspatch-java-mvn and dyspatch-java share the same parent directory. You may change the location if that's not the case._
- The last part of the command runs maven and sets the output directory to the mounted maven dir where the package is generated.

Note: As of April 21, 2020 there is currently a bug with the generated code and the javadoc mvn plugin, add the following to the above command `-Dmaven.javadoc.skip=true`.

Update with local:
--------------------

1. Make your changes in the dyspatch-java repo, and merge into master.
2. Clone getdyspatch/dyspatch-java-mvn and getdyspatch/dyspatch-java to your local machine.
3. Build the project and update your local copy of dyspatch-java-mvn.  From the root of your dyspatch-java project, run:

    `DYSPATCH_API_KEY=<api key for qa env> mvn -DaltDeploymentRepository=snapshot-repo::default::file:FIXME/PATH/TO/dyspatch-java-mvn/releases clean deploy`
    
    Be sure to update the `file:FIXME/PATH/TO/dyspatch-java-mvn...` part of the command!
    
    As of April 21, 2020 there is currently a bug with the generated code and the javadoc mvn plugin, add the following to the above command `-Dmaven.javadoc.skip=true`

4. Push all of the changes in the dyspatch-java-mvn project to github.

Notes:
--------------------

- To build at any time, run `mvn clean install` from the root dyspatch-java directory.  This will also run unit tests.

- When any undeployed changes to the dyspatch-java client are made, '-SNAPSHOT' should be appended to the version number to indicate it is in a development state.  Remove '-SNAPSHOT' when work has completed.
