## Publishing your first Pharo project with Iceberg


In this chapter, we explain how you can publish your project on GitHub using Iceberg.
We do not explain basic concepts like commit, push/pull, merging, or cloning.

A strong precondition before reading this chapter is that you must be able to publish from the command line
to the `git` hosting service that you want to use. If you cannot do not expect Iceberg to fix it magically for you. 
Let us get started.

### For the impatient

If you do not want to read everything, here is the executive summary.

- Create a project on GitHub or any git-based platform.
- \[Optional\] Configure Iceberg to use custom SSH keys.
- Add a project in Iceberg.
  - Optionally but strongly recommended, in the cloned repository, create a directory named `src` on your file system. This is a good convention.
- In Iceberg, open your project and add your packages.
- Commit to your project.
- \[Optional\] Add a baseline to ease loading your project. 
- Push your change to your remote repository.


You are done. Now we can explain calmly.

### Basic Architecture


As `git` is a distributed versioning system, you need a _local_ clone of the repository and a _working copy_. Your working copy and local repository are usually on your machine. This is to this local repository that your changes will be committed to before being pushed to remote repositories (Figure *@commit_in_workflow@*). We will see in the next Chapter that the situation is a bit more complex and that Iceberg is hiding the extra complexity for us. 

![A distributed versioning system.](figures/commit_in_workflow.pdf width=75&label=commit_in_workflow)

### Create a new project on GitHub

While you can save locally first and then later create a remote repository, in this chapter we first create a new project on GitHub. Figure *@onGitHub@* shows the creation of a project on GitHub.
The order does not matter. What is different is that you should use different options when addin a repository to Iceberg as we will show later.

![Create a new project on GitHub.](figures/onGitHub.png width=75&label=onGitHub)

### \[Optional\] SSH setup: Tell Iceberg to use your keys

To be able to commit to your `git` project, you should either use HTTPS or you will need to set up valid credentials in your system. In case you use SSH (the default way), you will need to make sure those keys are available to your GitHub account and also that the shell adds them for smoother communication with the server.
See the Chapter *@cha:tips@* Tips and Tricks for some help with setting up your ssh keys.


Go to settings browser, search for "Use custom SSH keys" and enter your data there as shown in Figure *@UseCustom@*\).

![Use Custom SSH keys settings.](figures/useCustom.png width=75&label=UseCustom)

Alternatively, you can execute the following expressions in your image playground or add them to your Pharo system preference file \(See Menu System item startup\):

```
IceCredentialsProvider useCustomSsh: true.
IceCredentialsProvider sshCredentials
  publicKey: 'path\to\ssh\id_rsa.pub';
  privateKey: 'path\to\ssh\id_rsa'
```


!!note **Pro Tip:** This can be used too in case you have a non-default key file. You just need to replace `id_rsa` with your file name.


### Iceberg _Repositories_ browser


Figure *@freshiceberg@* shows the top-level Iceberg pane.
It shows that for now you do not have defined nor loaded any project.
 It shows the Pharo project and indicates that it could not find its local repository by displaying 'Local repository missing'.

First, you do not have to worry about the Pharo project or repository if you do not want to contribute to Pharo.
So just go ahead. Now if you want to understand what is happening here is the explanation.
The Pharo system does not have any idea where it should look for the `git` repository corresponding to the source of the classes it contains. Indeed, the image you are executing may have been built somewhere, patched or not many times. Now Pharo is fully operational without having a local repository. You can browse system classes and methods because Pharo has its own internal source management. This warning indicates that if you want to version Pharo system code using `git` then you should indicate to the system where the clone and working copy are located on your local machine.
So if you do not plan to modify and version the Pharo system code, you do not have to worry.

![Iceberg _Repositories_ browser on a fresh image indicates that if you want to version modifications to Pharo itself you will have to tell Iceberg where the Pharo clone is located. But you do not care.](figures/S1-OpeningIceberg.png width=75&label=freshiceberg)


### Add a new project to Iceberg


The first step is then to add a project to Iceberg:
- Press the '+' button to the right of the Iceberg main window.
- Select the source of your project. In our example, since you did not clone your project yet, choose the GitHub option.


Notice that you can either use SSH \(Figure *@Cloning@*\) or HTTPS \(Figure *@CloningHTTPS@*\).

Figure *@Cloning@* and *@CloningHTTPS@*\) instruct Iceberg to clone the repository we just created on GitHub.
We specify the owner, project, and physical location where the local clone and `git` working copy will be on your disk.

![Cloning a project hosted on GitHub via SSH.](figures/S5-CloneFromGitHub.png width=75&label=Cloning)

![Cloning a project hosted on GitHub via HTTPS.](figures/S5-AddWithHTTPS.png width=75&label=CloningHTTPS)


Iceberg has now added your project to its list of managed projects and cloned an empty repository to your disk. You will see the status of your project, as in Figure *@FirstTimeCloned@*. Here is a breakdown of what you are seeing:
- MyCoolProjectWithPharo has a star and is green. This usually means that you have changes which haven't been committed yet, but may also happen in unrelated edge cases like this one. Don't worry about this for now.
- The Status of the project is 'No Project Found' and this is more important. This is normal since the project is empty. Iceberg cannot find its metadata. We will fix this soon.


![Just after cloning an empty project, Iceberg reports that the project is missing information.](figures/S6-ProjectNotFound.png width=75&label=FirstTimeCloned)


Later on, when you will have committed changes to your project and you want to load it in another image,
when you clone again, you will see that Iceberg reports that the project is not loaded as shown in Figure *@ProjectWithCommits@*.

![Adding a project with some contents shows  that the project is not loaded - not that it is not found.](figures/S7-ProjectNotLoaded.png width=75&label=ProjectWithCommits)





### Repair to the rescue


Iceberg is a smart tool that tries to help you fix the problems you may encounter while working with `git`.
As a general principle, each time you get a status with red text \(such as "No Project Found" or "Detached Working Copy"\), you should ask Iceberg to fix it using the **Repair** command.

Iceberg cannot solve all situations automatically, but it will propose and explain possible repair actions.
The actions are ranked from most to least likely to be right one.
Each action has a displayed explanation of the situation and the consequences of using it.
It is always a good idea to read them.
Setting your repository the right way makes it extremely hard to lose any piece of code with Iceberg and Pharo is general since Pharo contains its own copy of the code.

![Create project metadata action and explanation.](figures/S8-RepairFirst.png width=75&label=RepairFirst)

### Create project metadata

Iceberg reported that it could not find the project because some metadata were missing such as the format of the code encodings and the example location inside the repository. When we activate the repair command we get Figure *@RepairFirst@*.  It shows the "Create project metadata" action and its explanation.


When you choose to create the project metadata, Iceberg shows you the filesystem of your project as well as the repository format as shown in Figure *@Tonel@*. Tonel is the preferred format for Pharo projects.
It has been designed to be Windows and file system friendly. Change it only if you know what you are doing!

![Showing where the metadata will be saved and the format encodings.](figures/S9-MetaData1.png width=65&label=Tonel)

Before accepting the changes, it is a good idea to add a source \(`src`\) folder to your repository.
Do that by pressing the + icon. You will be prompted to specify the folder for code as shown in Figure *@metadatasrc@*. Iceberg will show you the exact structure of your project as shown in Figure *@metadatasrc2@*.

![Adding a src repository for code storage.](figures/S9-MetaData2.png width=50&label=metadatasrc)

![Resulting situation with a src folder.](figures/S9-MetaData3.png width=50&label=metadatasrc2)

After accepting the project details, Iceberg shows you the files that you will be committing as shown in Figure *@PublishingMetaData@*

![Details of metadata commit.](figures/S9-MetaData4.png width=75&label=PublishingMetaData)

Once you have committed the metadata, Iceberg shows you that your project has been repaired but is not loaded as shown in Figure *@ProjectWithCommits@*. This is normal since we haven't added any packages to our project yet. You can optionally push your changes to your remote repository.

Your local repository is ready, let's move on to the next part.

### Add and commit your package using the _Working copy_ browser

Once your project contains Iceberg metadata, Iceberg will be able to manage it easily.
Double click on your project to bring a _Working copy_ browser for your project. It lists all the packages that compose your project. Right now you have none.
Add a package by pressing the + \(Add Package\) iconic button as shown by Figure *@WithAPackage@*.

![Adding a package to your project using the _Working copy_ browser.](figures/S10-AddingAPackage.png width=75&label=WithAPackage)

Again, Iceberg shows that your package contains changes that are not committed using the green color and the star in front of the package name as shown in Figure *@Dirty@*.

![Iceberg indicates that your package has unsaved changes -- indeed you just added your package.](figures/S11-Dirty.png width=50&label=Dirty)


#### Commit the changes

Commit the changes to your local repository using the Commit button as shown in Figure *@DirtyCleaned@*.
Iceberg lets you choose the changed entities you want to commit. Here this is not needed but this is an important feature. Iceberg will show the result of the commit action by removing the star and changing the color. It now shows that the code in the image is in sync with your local repository as shown by Figure *@DirtyBecomeClean@*.
You can commit several times if needed.

![When you commit changes, Iceberg shows you the code about to be committed and you can chose the code entities that will effectively be saved.](figures/S11-DirtyCommit.png width=50&label=DirtyCleaned)

![Once changes committed, Iceberg reflects that your project is in sync with the code in your local repository.](figures/S11-DirtyBecomeClean.png width=50&label=DirtyBecomeClean)

#### Publish your changes to your remote

Now you are nearly done. Publish your changes from your local directory to your remote repository using the Push button. You may be prompted for credentials if you used HTTPS.

When you push your changes, Iceberg will show you all the commits awaiting publication and will push them to your remote repository as shown in Figure *@Push@*. The figure shows the commits we are about to make to add a baseline, which will allow you to easily load your project in other images.

![Publishing your committed changes.](figures/push.png width=75&label=Push)

Now you are basically done.


### Conclusion


You now know the essential aspects of managing your code with GitHub.
Iceberg has been designed to guide you so please listen to it unless you really know what you are doing.
You are now ready to use services offered around GitHub to improve your code control and quality!
