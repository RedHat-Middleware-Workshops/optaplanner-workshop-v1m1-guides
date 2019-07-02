
# Import Labs

In this section you will learn:

1. How to access CodeReady Workspaces

2. Import an existing project into CodeReady Workspaces


## CodeReady Workspaces

Red Hat CodeReady Workspaces is a developer workspace server and cloud IDE. Workspaces are defined as project code files and all of their dependencies neccessary to edit, build, run, and debug them. Each workspace has its own private IDE hosted within it. The IDE is accessible through a browser. The browser downloads the IDE as a single-page web application.

Red Hat CodeReady Workspaces provides:

- Workspaces that include runtimes and IDEs
- RESTful workspace server
- A browser-based IDE
- Plugins for languages, framework, and tools
- An SDK for creating plugins and assemblies



## Accessing CodeReady Workspaces

A CodeReady Workspaces environment has been created for every workshop user. To access your environment access the [CodeReady Workspacess IDE]({{ CODEREADY_WORKSPACES_URL }}). You can login with the OpenShift username and password that have been provided to you.

- In the CodeReady Workspaces login screen, click on "Openshift v3" on the right part of the form. You will be redirected to the OpenShift login screen.
    ![CodeReady Workspaces Login OpenShift]({% image_path codeready-login-openshift.png %}){:width="600px"}
- Login with the workshop credentials that have been provided to you.
- An _Authorize Access_ screen will be presented. Leave `user_full` checkbox checked and click on `Allow selected permissions`.
    ![CodeReady Workspaces Authorize Access]({% image_path codeready-authorize-access.png %}){:width="600px"}
- In the next screen, provide additional user information. This can be dummy information for this workshop.
    ![CodeReady Workspaces User Information]({% image_path codeready-user-information.png %}){:width="600px"}

CodeReady Workspaces will open and show the initial screen.

![CodeReady Workspaces Initial Screen]({% image_path codeready-initial-screen.png %}){:width="600px"}


### Importing a Project

CodeReady allows us to directly import existing projects from GitHub.

In the initial screen, the `New Workspace` screen, that the platform provides us, we can import a new project.

1. Provide a name for your workspace, postfixed by your username. E.g, `optaplanner-lab1-user1` if you're username is `user1`.
2. Select the _stack_. In this case we select `Java 1.8`.
3. Set the RAM (memory) of the `dev-machine` to 2GB (default value).
4. Click on `Create and Open` button to create the workspace.
    ![CodeReady Workspaces New Workspace]({% image_path codeready-new-workspace.png %}){:width="600px"}
5. When the workspace has been created, click on `Import Project`.
    ![CodeReady Workspaces Workspace Created]({% image_path codeready-workspace-created.png %}){:width="600px"}
6. In the _Import Project_ window, select `GITHUB` as the _Version Control System_, set the URL to `https://github.com/RedHat-Middleware-Workshops/optaplanner-workshop-v1m1-labs` and click on the _Import_ button.
    ![CodeReady Workspaces Import Lab 1]({% image_path codeready-workspaces-import-lab1.png %}){:width="600px"}
7. In the _Project Configuration_ screen, select `Maven` and click the _Save_ button.

Our imported project is an empty Maven Java JAR project, with a number of predefined dependencies in the POM file, and a number of default folders, i.e. `src/main/java` and `src/main/resources`. Open the POM file by clicking on the `pom.xml` file of the project. Observe the dependencies that have been pre-defined, the the `optaplanner-core` dependency.

![CodeReady Workspaces Lab 1 POM]({% image_path codeready-lab1-pom.png %}){:width="600px"}


### Compiling the project

To make sure our skeleton project for Lab1 is properly imported and configured, we will first run a Maven build. To do this, we need to access the CodeReady commands as show in the image below:

![CodeReady Manage Commands]({% image_path codeready-manage-commands.png %}){:width="600px"}

In the commands section, expand the _Build_ section, click on `build`, and in the main window of the IDE, click on the green `RUN` button.

![CodeReady Maven Builds]({% image_path codeready-maven-builds.png %}){:width="600px"}

A build will run and the output will be displayed in the console at the bottom of the screen. If the build succeeded, the following message will be displayed:
```
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 9.697 s
[INFO] Finished at: 2019-07-02T14:19:06Z
[INFO] ------------------------------------------------------------------------
```
