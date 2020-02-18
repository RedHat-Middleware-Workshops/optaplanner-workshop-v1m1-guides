
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

- CodeReady Workspaces uses OpenShift OAuth Single-Sign-On as the login mechanism. In the OpenShift login screen, click on `htpasswd_provider`.
    ![CodeReady Workspaces Login OpenShift]({% image_path codeready-login-openshift.png %}){:width="600px"}
- Login with the workshop credentials that have been provided to you.
    ![CodeReady Workspaces Login OpenShift 2]({% image_path codeready-login-openshift-2.png %}){:width="600px"}
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
2. Select the _stack_. In this case we select `Java Maven`.
3. By default, CodeReady Workspaces wants to import a sample `console-java-simple` project. We can remove that project by clicking on the `Remove` button.
    ![CodeReady Workspaces Remove Sample project]({% image_path crw-remove-sample-project.png %}){:width="600px"}
4. Click on `Create and Open` button to create the workspace.
    ![CodeReady Workspaces New Workspace]({% image_path codeready-new-workspace.png %}){:width="600px"}
5. When the workspace has been created, click on `Git Clone...`, which can be found under the `New` section.
    ![CodeReady Workspaces Workspace Created]({% image_path codeready-workspace-created.png %}){:width="600px"}
6. In the _Repository URL_ command window, enter the URL `https://github.com/RedHat-Middleware-Workshops/optaplanner-workshop-v1m1-labs` and press _Enter_.
    ![CRW Git Clone Command Window]({% image_path crw-git-clone-command-window.png %}){:width="600px"}
7. We now have to select the location where we want to store the repository. We want to store the repository in `/projects`. To do this, first select `/` in the dropdown menu at the top of the window. Next, double click on the `projects` folder to select it. Finally, click on the _Select repository location_ button at the bottom of the window.
    ![CRW Select Location Dropdown Home]({% image_path crw-select-location-dropdown-home.png %}){:width="600px"}
    ![CRW Select Location Projects List]({% image_path crw-select-location-projects-list.png %}){:width="600px"}
    ![CRW Select Location Projects Folder]({% image_path crw-select-location-projects-folder.png %}){:width="600px"}
7. In the lower right corner, a pop-up will appear, asking you whether you want to open the repository or add it to your workspace. Click on _Add to Workspace_.
    ![CRW Add Repository Workspace]({% image_path crw-add-repository-workspace.png %}){:width="600px"}
8. In the upper-left corner of your screen, click on the _Document_ icon to open your workspace _Explorer_. You will see your cloned repository in the explorer view.
    ![CRW Lab1 In Explorer]({% image_path crw-lab1-in-explorer.png %}){:width="600px"}


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
