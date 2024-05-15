## Contributing to Pharo


Pharo itself is hosted on GitHub under [http://www.github.com/pharo-project/](http://www.github.com/pharo-project/). All the development of the core system is done via 
this code repository. 
You can contribute to Pharo itself fixing some issues.
The official and unique way of submitting code is by doing a _Pull Request_ from your fork to the official repository.


You can see the issues on the Pharo Github repository: [http://www.github.com/pharo-project/pharo/issues](http://www.github.com/pharo-project/pharo/issues). 
	
### In a nutshell


The general process is summarised by the following steps:

1. Download the latest version (this is important since I will make sure that you do not have to resync your fork).
1. Use repair (it will fetch some commits and make sure that your local repository is in sync the remotes).
1. Use repair (and create a branch let us called Bottom-123 that will point to the commits of your image).
1. Create a new branch for the issue you want to work on.
1. Code, then commit, then push to YOUR fork
1. Issue a PullRequest from your fork to Pharo
1. Check out your Bottom-123 branch and you can restart step 5



### Step 0: Setting up the development environment


Download and launch latest Pharo version:

The recommended way to download Pharo is to use the Pharo launcher, which you can get from Pharo download's page. Otherwise, if you have a nice command line environment (wget, readline, bash), download latest development pharo using zeroconf.

!!note Pro tip: If you're on windows and you want a nice command line environment, install msys

```
wget -O- get.pharo.org/64/90+vm | bash
./pharo-ui Pharo.image
```


If you don't have a command line environment, you can download both image and vm manually from the following links for windows for example:

- Image: http://files.pharo.org/get-files/90/pharo.zip
- VM: http://files.pharo.org/get-files/90/pharo-win-stable.zip

Or you can browse the file server and find the files suiting your environment.

### Fork the Pharo repository


All changes you'll make will be versioned in _your own_ fork of the Pharo repository. Then, from your fork you'll be able to issue pull requests to Pharo, where they will be reviewed, and luckily, integrated.

Go to Pharo GitHub's repository and click on the fork button on the top right. Yes, this means that you'll need a github account to contribute to Pharo, yes.

### Setup Iceberg


To be able to contribute to Pharo, you need a Pharo git repository.


You will need to set a valid set of credentials in your system to be able to work with Pharo. In case you use SSH (the default way), you will need to make sure those keys are available but you can start by using HTTPS since this is easier.
 In case they are not (and you will notice as soon as you try to clone a project or commit a change into one), you can add them following these steps:

#### Linux setting credentials

Execute in your shell:

```
    ssh-add ~/.ssh/id_rsa
```


#### OSX setting credentials

Execute in your shell:

```
    ssh-add -K ~/.ssh/id_rsa
```


Both for OSX and Linux you can add such lines in your `.bash_profile` (or the one corresponding to your shell installation) so they are automatically executed on each new shell session.

#### Windows setting credentials
Windows is more complicated, you may need to generate a pair of keys (that needs to be uploaded to your account on Git Hub). Please check online resources for the current Windows version you are using.


#### Pharo side settings

Then you need to go to Setting browser, search for "Use custom SSH keys" and complete your data there.

Alternatively, you can execute in your image playground:
```
IceCredentialsProvider useCustomSsh: true.
IceCredentialsProvider sshCredentials
	publicKey: 'path\to\ssh\id_rsa.pub';
	privateKey: 'path\to\ssh\id_rsa'
```

	
!!note Pro Tip: this can be used too in case you have a non default key file, you just need to replace "id\_rsa" with your file name.


### Step 1. Setting up your repository

A fresh Pharo image comes with a pre-configured Pharo repository. 
Now this repository is in state "Local Repository Missing". 
You can develop with the image without problem now if you want to send some enhancements to Pharo 
you will not be able to do it.
Indeed the message **Local Repository Missing** means that the image does not find a Pharo clone in your disk. 

![A fresh Pharo image by default indicates that it does not find the clone of Pharo.](figures/Contribute1-repository-missing.png width=65&label=repositoryMissing)


#### Repairing local repository missing.

To solve this situation, you need to rebind your repository with a clone on your disk. 
Indeed even if you can browse the system class code, the image does not know where its repository is. 
The repair menu item/button will propose you some solutions.

![Repairing the Pharo project.](figures/Contribute2-repositoryRepair.png width=65&label=repositoryRepair)

And clicking on the **Repair repository** menu will show you the repair view, showing an explanation of the current situation and some proposed solutions:

- Locate this repository in your file system: if you have already cloned your pharo fork repository on your disk you can simply point to it. Pharo will synchronise it as explained in the next steps. 
- Clone again this repository: you can clone again your Pharo fork from github to your local disc. Again your fork may be desynchronised from the actual Pharo version but Iceberg will manage it too. 


So depending on the action you chose: you can then choose to search in your disk for an existing clone or to clone your forked Pharo repository.

#### Solving fetch required. 

Once the repository is associated with its a clone, it will check the repository status and most probably you'll find out that your repository is in **Fetch required state**. 
Indeed "Fetch required" means that your image was built from a commit that cannot be found in your repository. 
In other words, we need to update your repository, doing a fetch it will update your repository. 

To solve again, launch the repair action again. Usually, if you already have all your remotes correctly configured, doing a fetch will put you in a **Detached Working Copy** state.

!!note Pro Tip: Instead of doing a simple fetch (second option of the repair) it is quite powerful to create a branch (first option), give it a name like sync, bottom... This branch is pointing to the point where your repository and image are in sync and this is super useful to be able to checkout it later when you want to do multiple PRs on different and unrelated issues. This way you can go back to a sync state in one click and be super productive.


#### Solving the Detached Working Copy.


**Detached Working Copy** means that the image commit does not correspond with the repository commit (more details of it in the Glossary). At this point, we need to synchronize both to be able to work.

Most of the times, the easier thing to do in this case is to just create a new branch as suggested in the Pro Tip before. If you plan to only fix on issue and you already know which issue you'll be working on, you can create a branch using the "New branch from issue" option. Otherwise, a nice alternative is to create a temporary branch like temp/synch that you can later on remove.

![Solving the detached working copy situation.](figures/Contribute4-DetachedWorkingCopy.png width=65&label=detached)

### Step 2: Work on your image and push your change


Pick up or create an issue and fix it. Once you have modifications in your image, it's time to push those changes to _your_ fork and make a pull request from your fork to the main Pharo repository. To do that, we enforce the following process:

- you create a local branch for the issue, 
- you issue a pull request of that issue to the main pharo development branch.


Once you have done and commited all your work, you need to push it and create a pull request. Right click on the repository and go to the Github plugin menu item.

Select the option **Create pull request** and select as target branch pharo's development branch.

And issue the pull request!

!!note Pro Tip: If your PR fixes the issue, you can put a keyword "fixes #IssueNumber" in the description to automatically close the issue on merge. 

### Step 3: Follow your pull request


If you go to github, you'll see your pull request open and that some checks are running.
Note that if your PR fails because of test failing or you want to integrate feedback provided by reviewed, it is super simple:
just continue to commit in your branch. Your Pull Request will be updated with your new changes. 
This is always a cool feeling, so enjoy it.

![Checking your pull request.](figures/Contribution6-pr-checks.png width=65&label=PRCheck)

### Step 4: Once your pull request is integrated


Some cleanups are required:
- remove your branch from your fork
- close the issue (this is done automatically if your PR contained "fixes #numberOfIssue")


### Why you do not need to resync your fork with the pharo repo?


No, you do not need to explicitly resync your fork. Why? Because when you push your commits, you push also the Pharo commits. But let us look at it in details:

- When you download an image (imagine that this image contains the code of commit3 and that Pharo' repository is at commit5), the repair action will fetch all the commits (up to 5) and put them in your _local_ repository.
- When you create your bottom branch (the branch that points to the situation of your image when you downloaded) and later  a child branch containing your fixes (call it myfix), it implies that when you commit your myfix branch, all the commits from myfixes and then from commit3 down to the common ancestor between your fork and your local repository will be pushed back to your _fork_.


So if you downloaded the latest image (imagine for commit6 and that the pharo repository is at commit6 - yes this is the latest image!) when you push your branch (which here is a child of commit6), automatically all the commits (6 down to the common ancestors between your local repo to your fork) will be pushed to your fork. So your fork will be in sync with Pharo's one.

So each time you work and push to your fork from the latest image, your fork gets updated!

### Update your Pharo fork using the command line


You can also update your Pharo fork using the command line in 6 commands. You need to have a `git` command line interface installed:

```
$ git clone https://github.com/YOUR-USER-NAME/pharo.git
$ cd Pharo
$ git remote add pharo https://github.com/pharo-project/pharo.git
$ git fetch pharo
$ git pull pharo Pharo8.0
$ git push origin Pharo8.0
```


You can see the results in Figure *@CommandLine@*.

![Command Line.](figures/PharoFork_CommandLine.png width=75&label=CommandLine)

### The following script can be put in a ==.st== in your preferences folder (see below how to find it) and it will automatically configure Iceberg to connect to your accounts.

### Conclusion

Now you know how to update a branch from your repository and can happily continue to improve Pharo.
