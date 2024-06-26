## Preamble

Git is the defacto standard distributed source versioning system. Even though Pharo got its own distributed versioning 
system during more than 12 years. It lacked the maturity of git and one key features: branches.
This is why since Pharo 6.0 it was clear that Pharo needed to have support for git. 
Pharo started to support git since Pharo 6.0. Pharo 7.0 saw a major version since all its code and development migrated to git.

However managing Pharo with git is not just a matter of saving code into files and versioning. 
Any fool can do that in one afternoon. Pharo has a powerful reflective layer and execution change the objects that represent code 
itself. Therefore Pharo as a living system can be in a different state than the file checkouted from a git repository. 
From this we can imagine many complex scenarios that even smart programmers would have headaches to understand.

Therefore the Pharo consortium (E. Lorenzano, N. Passerini, G. Polito and P. Tesone) developed an advanced tool to help managing the live programming aspect of Pharo and the static perspective that file-based versioning systems such git have of the reality. 
Iceberg not only supports the management of large projects (such as Pharo itself with more than 600 packages and a couple of thousand classes)
but it helps us (the developers) to understand the situation between our image, the files on our disc and the multiple branches
and remote repositories that the git model offers. It offers strategies to address problems we may face. 

Iceberg is a tool to manage git projects from Pharo. It makes the experience of managing code really smooth.
A major effort went into the version of Iceberg present in Pharo 7.0. 
We are using the version of Iceberg available in Pharo 8.0.

This document is under writing but we decided to release before its completion 
because managing code can be a large topic when we start to discuss workflow and process. 

The book is structured for now in two main parts
1. Understanding from the command-line
1. Managing Pharo code with Iceberg.



The authors want to thank Sean de Nigris, Quentin Ducasse and Stefan Eggermont for the reviews and copy-edit of the early version. We also thank Peter Uhnak for his first blog on publishing Pharo code on Github.
We thank Iona Thomas for the enhancements of Iceberg. 


<!inputFile|path=Chapters/Git/git_basics.md!>
<!inputFile|path=Chapters/Git/git_advanced.md!>
<!inputFile|path=Chapters/Git/git_practical.md!>


<!inputFile|path=Chapters/Started/started.md!>
<!inputFile|path=Chapters/Started/extensions.md!>
<!inputFile|path=Chapters/Started/services.md!>
<!inputFile|path=Chapters/Started/pharo.md!>
<!inputFile|path=Chapters/Tips/tips.md!>
<!inputFile|path=Chapters/Started/glossary.md!>


