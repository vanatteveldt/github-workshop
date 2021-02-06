# Removing sensitive files

We all know the problem: you accidentally commit a file you really shouldn't have added to github. 
For example, it can contain a password or private key, or can contain private or copyrighted data you are not allowed to share.
Of course, you can `git rm` the file, and it will not appear on the default github site anymore, 
but it will still be there in the commit history.  So, how to completely erase a file from a git(hub) repository?

# Scenario 1: You added and/or committed the file, but did not push yet

# Scenario 2: You just pushed the commit containing the sensitive file

# Scenario 3: The sensitive file has been in the repository for a while

This is in a way the biggest challenge, as you would have to change all commits in history that reference the sensitive file(s).
Fortunately, this is relatively easy with the [bfg](https://rtyley.github.io/bfg-repo-cleaner/) tool. 

First, download the tool and make sure you have java installed. If you call the tool like this, it should display information on how to use the tool:

```{sh}
$ java -jar ~/Downloads/bfg-1.13.2.jar
bfg 1.13.2
Usage: bfg [options] [<repo>]
[..]
```

Now that you know that the tool is correctly installed, the following steps will help you remove a file:

1. Notify any collaborators about your mistake, that you're about to change history, and tell them to commit any outstanding changes they have and remove local versions of the repository
2. Create a backup of the repository by cloning it somewhere safe
3. Make sure any local changes are committed, and remove your local clone and make a fresh clone (`rm -rf REPONAME; git clone git@github.com/USER/REPONAME`)
4. Run the bfg tool like this to change the existing commits: (make sure to check the output for errors. If you mess up, go back to step 3)

```{sh}
$ java -jar ~/Downloads/bfg-1.13.2.jar sensitivefilename.txt
```

5. Remove references to the old commits: `git reflog expire --expire=now --all && git gc --prune=now --aggressive`
6. Force push to github: `git push -f`
7. Check that on github all references to the file(s) are gone
8. Tell your collaborators to re-clone the repository. 

Note: It's important to inform your collaborators of this, since if they keep the local repository they will be on a commit that no longer exists, so they will not be able to commit or pull changes. If they then decide to force push, it would bring the sensitive file back to github. In general, whenever you need to change history on github like this, it's important to make sure that there are no collaborators working with the 'old' version of the repository. 
