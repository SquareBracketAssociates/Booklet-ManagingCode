## Tips and Tricks

@cha:tips

This chapter contains some simple recipe-oriented frequently asked questions. If you have some more please send them to us or even better, present a _Pull Request_.

### How to use SSH keys


In case SSH is not set up \(and you will notice as soon as you try to clone a project or commit a change to one\), there are some simple basic points to be able to use SSH with github.

You can add SSH keys by following these steps \(on Windows, if you want a nice command line environment, install [http://mingw.org/wiki/msys](http://mingw.org/wiki/msys)\):

#### Generate a key pair


To do this, execute the command:

```
ssh-keygen -t rsa
```


It will generate a private and a public key \(on a unix-based installation in the directory `.ssh`\).
You should copy your `id_rsa.pub` key to your github account.
Keep the keys in a safe place.

On Windows, follow instructions on how to generate your keys for your given version of Windows.

#### Add the key to your ssh agent


In linux, execute in your shell:
```
ssh-add ~/.ssh/id_rsa
```


In OSX, execute in your shell:
```
ssh-add -K ~/.ssh/id_rsa
```


For both OSX and linux you can add such lines to your `.bash_profile` \(or the one corresponding to your shell installation such as `.zshrc`\) so they are automatically executed on each new shell session.


### Configuring automatically Pharo to commit in HTTPS/SSH

With recent version of github, you should use a token to connect using HTTPS to be able to commit to your repository.
Now you can configure Pharo to store your token as well your github account authentification using startup preferences.

In addition you can configure Pharo to point to your private and public SSH key as follows: 

```
StartupPreferencesLoader default executeAtomicItems: {
    StartupAction 
        name: 'Logo' 
        code: [ PolymorphSystemSettings showDesktopLogo: false] .
    StartupAction 
        name: 'Git Settings' 
        code: [ 
            Iceberg enableMetacelloIntegration: true.
            IceCredentialStore current
                    storeCredential: (IcePlaintextCredentials new
                    username: 'xxJohnDoe';
                    password: 'xxJohnDoePassword';
                    host: 'github.com';
                    yourself).
            IceCredentialsProvider sshCredentials
                username: 'git';
                publicKey: 'Path to your public rsa.pub file (public key)';
                privateKey: 'Path to your private rsa file (private key)'.
            IceCredentialStore current
                storeCredential: (IceTokenCredentials new
                    username: 'xxJohnDoe';
                    token: 'magictoken here ';
                    yourself) 
                forHostname: 'github.com'.
            ]. 
}
```



You should place this file in the preference folder that depends on your OS. 
You can find this place by using the `Startup` menu. Note that you can configure this 
for all or one specific version of Pharo.




### How to contribute back to a project


You cloned a project from a Github repository and you don't have any write permissions. You made some changes, and you'd like to send a pull-request to it. What is the "Iceberg way" of doing it?

Manually you can fork the project on Github, add a remote to the local repo, push the changes to this new remote and then create the pull request on Github.

With Iceberg you can do the same since Iceberg follows the git way:

1. Create fork on Github
1. Add a repository for this project  to Iceberg \(Repositories context menu then "Repository" and then `"+ Repository"`
1. Create an issue branch with Iceberg \(Repositories context menu then GitHub then Create new branch for issue\).
1. Commit your changes with Iceberg.
1. Push your change to your fork from Iceberg.
1. Create pull request from Iceberg \(Repositories context menu then GitHub then Create Pull Request\)



### How to distribute your changes in different branches

With Iceberg we can chose the packages, classes or methods when we save. 
Imagine that you are in branch `odd` and you code
`m1`, `m2`, `m3`, `m4`. 
Then later you realize too late that it would be great to extract `m2` and `m4` in another branch because you would like to merge it in another branch called `even`.

To commit changes into different branches, you must change your branch. Because a _commit_ always modifies the current branch.

When you change  a branch, there is first a preview. In the preview, you  have a diff and  you have a
combo-box to select a checkout strategy:

![Checkout choices.](figures/CheckoutChoices.png width=75&label=CheckoutChoices)

- The default checkout strategy is to reload the packages, this will override any local changes you had.
- The last one is to not load any packages: in other words, change the branch but do not touch my image.


So one way to do save your changes in separate branch is to:
1. Commit in branch odd by selecting a subset of changes
1. Change to branch even using the "Do not checkout" strategy
1. Commit in branch even a subset of changes


