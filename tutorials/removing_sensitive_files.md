# Fixing mistakes 2: Removing sensitive files

We all know the problem: you accidentally commit a file you really shouldn't have added to github. 
Of course, you can `git rm` the file to remove and it will be removed from the active version of the repository.
More generally, you can use `git revert HEAD~1` to undo the last commit (`HEAD~1`), which automatically creates a new commit that undos the previous commit. 

However, removing the file through normal means does not suffice for a file containing e.g. a password or private key or private or copyrighted data you are not allowed to share, as the file(s) can still be found in the commit history. 

So, how to completely erase a file from a git(hub) repository?

# Scenario 1: You added and/or committed the file, but did not push yet

This is the easy case, and you can use the normal troubleshooting approaches here:

 + If you added but did not commit, use `git restore --staged secret.txt`
 + If you committed, but did not push, undo the add and commit using `git reset HEAD~` (where `HEAD~` means 1 commit before HEAD). 
   Alternatively, if you want to keep the rest of the commit, but only remove the offending file from the commit, use `git rm --cached secret.txt` (you can omit the `--cached` if you want to delete the file from disk as well, and then *amend* your last commit with `git commit --amend`.

# Scenario 2: You just pushed the commit containing the sensitive file

This is still easy from a technical perspective, as you can remove your last commit locally (`git reset HEAD~`) or edit it (with `git rm` followed by `git commit --amend`). However, this means that your last commit does not have the remote HEAD as parent, so you need to **force push** it with `git push --force`. This sets the remote HEAD to your last commit (either the amended one or the one before the offending one), and makes the commit containing the secret file an orphan commit which can no longer be found. 

Note that git also has a *reference log* to recent commits, in case you accidentally mess something up and lose track of a commit. 
To also clear that log, use `git reflog expire --expire=now --all` and `git gc --prune=now --aggressive`. 

# Aside: On rewriting history

The big difference between reverting a commit (which creates an 'undo commit') and actually removing a commit with a force push is that rather than having the history reflect the addition and removal, it has now *rewritten history* with no trace left of the removed file.  
Now, this is 'dangerous', in the sense that if another user pulled or cloned the repository in the meantime, they still have the secret file locally, and moreover their HEAD now points to a commit that is no longer in the remote repository, so they will get all kind of trouble if they continue working with it. 

This means that for rewriting history:

+ *Only rewrite history in emergency*, i.e. in case of accidentally publishing something secret, private, or confidential -- not for fixing a regular mistake or making the commit history look prettier.
+ *Always notify you co-authors* about your force push so they can make sure their code is still on the main line, and not on the abandoned line containing the offending commit. 

Finally, you should realize that even if the file can no longer be found on github normally, it can still be in someones local repository or a cache. 
So, if it's a password or certificate, assume that it is exposed and change the password/certificate. 
Also, if it is super sensitive, contact github to ask them to remove all traces from their cache. See [this github page](https://docs.github.com/en/github/authenticating-to-github/removing-sensitive-data-from-a-repository) for more information.

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
