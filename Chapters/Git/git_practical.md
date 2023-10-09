## Practical Git Scenarios In this chapter we will show some `git` features that can help you in your daily use of `git`.### Before commit little helpersYou may add too fast a file to your staging area or index. Or you may want to discard the changes you did to your working copy to get back in the situation where no changes were made. The two problems can be handled as follows. #### Remove Files from the Staging AreaIt may happen that you went too fast and added a file to your staging area \(remember the groceries' list\) and that you changed your mind and do not want to add it anymore. Your file is not committed so a simple `reset` will do the job.You can either remove single files or everything.```language=bash$ git reset HEAD file
# Or everything
$ git reset HEAD .```#### Getting back your file as in your local repositoryAnother common scenario is that you modified a file of your working copy and your realize that it would be better to drop the changes and get back the file that is versioned in your local repository. Simply checkouting again will fix your problem```language=bash$ git checkout file```### Exploring the History#### The `git log` CommandThe commit graphs we have shown so far are not evident at all while when we use the `git status` command.There is however a way to ask `git` about them using the `git log` command.```language=bash$ git log
commit 0c0e5ff55b56fe8eabc1661a1da64b41f9d74472
Author: Guille Polito <guillermopolito@gmail.com>
Date:   Wed Mar 21 15:37:32 2018 +0100

    Adding a title

commit 37adf4eaa945cbd7460991f88bff5aa902db06ce
Author: Guille Polito <guillermopolito@gmail.com>
Date:   Wed Mar 21 14:02:43 2018 +0100

    first version````git log` prints the list of commits in order of parenthood.The one on the top is the most recent commit, our last commit.The one below is its parent, and so on.As you can see, each commit has an id, the author name, the timestamp and its message.To display a more compact version \(commit ids + message\) of the log use```language=bashgit log --oneline```We can also ask `git` what are the changes introduced in a particular commit using the command `git show`.```language=bash$ git show 0c0e5ff55b56fe8eabc1661a1da64b41f9d74472
commit 0c0e5ff55b56fe8eabc1661a1da64b41f9d74472
Author: Guille Polito <guillermopolito@gmail.com>
Date:   Wed Mar 21 15:37:32 2018 +0100

    Adding a title

diff --==git== a/README.md b/README.md
index e69de29..cad05f1 100644
--- a/README.md
+++ b/README.md
@@ -0,0 +1 @@
+! a title
\ No newline at end of file```That will give us the commit description as in `git log` plus a \(not so readable\) diff of the modified files showing the inserted, modified and deleted lines.More advanced graphical tools are able to read this description and show a more user-friendly diff.#### Accessing the History GraphGit's log provides a more graphish view on the terminal using some cute ascii art.This view can be accesses through the `git log --graph --oneline --all` command.Here is an example of this view for a more complex project.In this view, stars represent the commits with their ids and commit messages, and lines represent the parenthood relationships.```language=bash$ git log --graph --oneline --all
* 4eb8446 Documenting
* e5a3e2e Add tests
* 680a79a Some other
| *   ed4854f Merge pull request #1137
| |\
| | * 9e30e37 Some feature
| * |   ba7f65c Merge pull request #1138
| |\ \
| | * | 31a40c4 Some Enhancement
| | |/
| * |   2d4698d Merge pull request #1139
| |\ \
| | * | 20c0ff4 Some fix
| | |/
| * |   ae3ec45 Merge pull request #1136```However, we are not always in the mood of using the terminal, or of wanting to decode what was done in ascii art.There are tools that are more suitable to explore the history of a project, usually providing some nice graphical capabilities.This is the case of tools such as SourceTree \(Figure *@commit_graph_sourcetree@*\) or Github's network view \(Figure *@commit_graph_github_network@*\).![Example of SourceTree's commit graph view.](figures/sourcetree_tree.png label=commit_graph_sourcetree)![Example of Github's commit graph view.](figures/github_network_tree.png label=commit_graph_github_network)### Discarding your Local Committed ChangesIt comes the time for every woman/man to make mistakes and want to discard them.Doing so may be dangerous, since once discarded you will not able to recover your changes.It is however possible to instruct `git` to do so.For it, there are two `git` comments that will perform the task for you and when combined they will completely discard every dirty file and directory in your repository:`git reset` and `git clean`. `commit_id` is the commit where you want to be. ```language=bash$ git reset --hard <commit_id>
$ git clean -df```The `-d` option removes untracked directories in addition to untracked files, while the  `-f` option is a shortcut `--force`, forcing the corresponding deletions.If you want to just revert the last commit you can also use```language=bash$ git reset --hard HEAD~1
$ git clean -df```The reason for needing two commands instead of one relies on the fact that `git` has several staging areas \(such as the ones used to keep the tracked files\), which we usually would like to clean when we discard the repository. Of course, experienced readers may search why they would need both in Git's documentation.% @@todo what about *this>https://stackoverflow.com/a/21718540* ?% stef### Ignoring Files@gitignoreMany times we will find that we do not want to commit some files that are in our repository's directory. This is mostly the case of generated or automatically downloaded files. For example, imagine you have a C project and some makefiles to compile it, generating a binary library. While it would be good to store the result of compilation from time to time, storing it in a `git` repository \(or SVN, or Bazar\) may be a cause of headaches. First, as you will see in *@expert_git@*, this may be a cause for conflicts.Second, since we should be able to generated such binary library from the sources, having the already compiled result in the repository does not add so much value.This same ideas can be used to ignore any kind of generated file. For example, pdfs generated by document generation tools, meta-data files generated by IDEs and tools \(e.g., Eclipse\), compiled libraries \(e.g., dll, so, or dylib files\).In such cases, we can tell `git` to ignore cetain files using the `.gitignore` file.The `.gitignore` file is an optional text file that we can write in the root of our repository with a list of file paths to ignore.```# Example of .gitignore file

# Lines starting with hashtags are comments

# A file name will ignore that file
someignoredfile.txt

# A file name will ignore that file
someignoredfile.txt

# A file pattern will ignore all pdf files
*.pdf```Once your file is ready, you have to add it and commit it to `git` to make it take effect.```language=bash$ git add .gitignore
$ git commit -m "Added gitignore"```From this moment on, all listed files will be ignored by `git add` and `git status`. And you will be able to perform further commands to add "all but ignored files":```language=bash$ git add .```!!note If a file or a file type is tracked but you want `git` to ignore its changes afterward, adding it to .gitignore file will not make the job i.e. `git` will continue to track it.To avoid keeping track of it in the future, but secure it locally in your working directory, it must be removed from the tracking list using ==git rm --cached <file> \(.<file\_type>\)==.Nevertheless, be aware that the file is still present in the past history!% @@todo Link to *@expert_git_remove_file* to remove a (sensitive) file from history% stef### Commiting a File Filtered out by the .gitignoreFor certain workflow, it is better to filter out certain files such as generated pdf but from time to time you may want to version a file that would not be because its extension or folder is listed in the `.gitignore` file. You can still commit such a such without having to touch the `.gitignore` file. You can use the `--force` option of the `add` command. ```language=bash$ git add -f that.pdf
$ git commit -m "that file is still important"```### Getting out of Detached HEADDetached head means no other than "HEAD is not pointing to a branch".Being in a detached HEAD state is not bad in itself, but it may provoke loss of changes.As a matter of fact, any commit that is not properly referenced by another commit or by another `git` reference \(tag, branch\) may be garbage collected.`git` will not forbid you to commit in this state, but any new commit you create will only reachable if you remember the commit hash.To get out of dettached HEAD, the easiest solution is to checkout a branch, as we will see in the next section.Checking out a branch will set HEAD to point to a branch instead of a commit, saving you some HEADaches.### Accessing your Repository through SSH@setupsshTo be able to access your repository from your local machine, you need to setup your credentials.Think it this way: you need to tell the server who you are on every interaction you have with it.Otherwise, Github will reject any operation against your repository.Such a setup requires the creation and uploading of SSH keys.An SSH key works as a lock: a key is actually a pair of a public and a private key. The private key is meant to reside in your machine and not be published at all. A public key is meant to be shared with others to prove your identity. Whenever you want to prove your identity, SSH will exchange messages encrypted with your public key, and see if you are able to decrypt it using your private key.To create an SSH key, in *nix systems you can simply type in your terminal```language=bash$ ssh-keygen -t rsa -b 4096 -C "your_email@some_domain.com"```Follow the instructions in your terminal such as setting the location for your key pair \(usually it is `$HOME/.ssh`\) and the passphrase \(a kind of password\). Finally, you'll end up with your public/private pair on the selected location. It is now time to upload it to Github.Connect yourself to your Github settings \(usually https://github.com/settings/profile\) and go to the "SSH and GPG keys" menu. Import there the contents of your public key file. You should be now able to use your repository.### Rewriting the History Many times it happens that we accidentally commit something wrong.Maybe we wanted to commit more or less things, maybe a completely different content, or we did a mistake in the commit's message. In these cases, we can rewrite Git's history, e.g, undo our current commit and go back to the previous commit, or rewrite the current commit with some new properties.Be careful! Rewriting the history can have severe consequences. Imagine that the commit you want to undo was already pushed. This means that somebody else could have pulled this commit into her/his repository. If we undo this already published commit, we are making everybody else's repositories obsolete! This can be indeed problematic depending on the number of users the project has, and their knowledge on `git` to be able to solve this issue.#### Undo a commit using `git reset --hard`To undo the last commit, it is as easy as:```language=bash$ git reset --hard HEAD~1````git reset --hard [commitish]` makes your current branch point to \[commitish\]. `HEAD` is your current head, and you can read `~1` as "minus one". In other words, `HEAD~1` is head minus one, which boils down to the parent of head, our previous commit.You can use this same trick to rewrite the history in any other way, since you can use any commitish expression to reset. For example, `HEAD~17` means 17 versions before head, or `someBranch~4` means four commits before the branch `someBranch`.#### Update a Commit's Message using `git commit --amend`To change our current commit's message you can use the following command:```language=bash$ git  commit --amend -m "New commit message"```Or, if you don't use the `-m` option, a text editor will be prompt so you can edit a commit message.```language=bash$ git  commit --amend```You can use the same trick not only to modify a commit's message but to modify your entire commit. Actually, just adding new things with `git add` before an `--amend` will replace the current commit with a new commit merging the previous commit changes with what you just added.### How to Overwrite/Modify CommitsWARNING: It is highly not recommended to rewrite the history of a repo especially when part of it has already been pushed to a remote.Modifying the history will most likely break the history shared by the different collaborators and you may deal with an inextricable merge conflict.#### Change the last CommitImagine you have just committed your changes and have not pushed them yet, but1. you are not satified with the commit message```language=bash$ git  log --oneline
$ git  commit --amend -m "Updated commit message"
$ git  log --oneline```2. you forgot to save some modification or to add some files before committing. Then make your changes and use```language=bash$ git  commit -a --amend --no-edit```#### Merge two commitsFirst, it is worth repeating that you must think twice before modifying the history of the repo.Now, assume that you have not pushed the corresponding commits.Merging two consecutive commits is a way to work with a cleaner tree.Consider you want to merge commits "Intermediate" and "Old"```language=bash$ git  log --oneline

eae7846 New
71c0c64 Intermediate
f039832 Old
cca92f1 Even older```Then, you can interactively `-i` focus on the last three `HEAD~3` commit. ```language=bash$ git  rebase -i HEAD~3

pick f039832 Old
pick 71c0c64 Intermidiate
pick eae7846 New```!!note Observe that the commits are displayed in the reversed order.Now you can squash the commit "Intermediate" into its parent commit "Old"```language=bashpick f039832 Old
squash 71c0c64 Intermidiate
pick eae7846 New```And set the message e.g. "Merge intermediate + old" attached to the single.```language=bash# This is a combination of 2 commits.
# This is the 1st commit message:

Old

# This is the commit message #2:

Intermediate

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.``````language=bash$ git  log --oneline

eae7846 New
651375a Merge intermediate + old
cca92f1 Even older```Note that  the commit id has changed#### Pushing Rewritten HistoryAs soon as the history we have rewritten was never pushed before, we can continue working normally and pushing our changes then without problems. However, if we have already pushed the commit we want to undo, this means that we are potentially impacting all users of our repository. Because of the problems it can pose to other people, pushing a rewritten history is not a completely favoured by `git`. Better said, it is not allowed by default and you'll be warned about it:```$ git  push
To git@github.com:REPOSITORY_OWNER/YOUR_REPOSITORY.git
 ! [rejected]        YOUR_BRANCH -> YOUR_BRANCH (non-fast-forward)
error: failed to push some refs to 'git@github.com:REPOSITORY_OWNER/YOUR_REPOSITORY.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: '==git== pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in '==git== push --help' for details.```With this message `git` means that you should not blindly overwrite the history.Also, it suggests to pull changes from the remote repository.However, doing that will bring back to our repository the history we wanted to undo!What we want to do is to impose our current \(undone\) state in the remote repository.To do that, we need to **force** the push using the `git push --force` or the `git push -f` option.```$==git== push -f
Total 0 (delta 0), reused 0 (delta 0)
To git@github.com:REPOSITORY_OWNER/YOUR_REPOSITORY.git
+ a1713f3...6e0c7bf YOUR_BRANCH -> YOUR_BRANCH (forced update)```### ConclusionThis chapter presented some operations that may help you. As a general principle avoid rewriting history because you may lose changes and get into unrecovable state.