# Working with Branches

(See chapter 3 of the [git pro book](https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell) for more details)

When looking at github, you have probably encountered the name *main* in multiple places.
This is the (default) name of the default (main) branch. 

Essentially, a branch is simply a named reference to a particular commit.
Since a commit (with all its (grand)parents) identifies a particular state (snapshot) of the repository,
a branch can also be seen as a particular state or snapshot of the repository.
So, if you have two branches, they are essentially two different versions of the repository,
usually sharing part of the commit history, but diverging at some point. 

[Note that main used to be called *master*, but as part of a push to remove references to master/slave relations from the computer science jargon, it was renamed to main on github. 
If you create a repository with `git init` rather than on github, the default is still *master*, at least on git 2.25.1]

## Creating a branch

You can use `git branch mybranch` to create a new branch called mybranch. 
Then, you can use `git checkout mybranch` to activate that branch (or more formally, set your *head* to that branch).
As a shorthand, you can use `git checkout -b mybranch` to create and activate a new branch. 

Now, if you create a new commit, your branch will point to that new commit, but the old *main* will still point to the previous commit. 
If you try to push this commit, you will get a somewhat cryptic message that *the current branch testbranch has no upstream branch*.
What this means is that git doesn't know where to push your changes to. 
Less cryptically, it tells you to run `git push --set-upstream origin mybranch` in order to fix this:
Push your changes, setting the upstream for your branch to a (new) branch mybranch on *origin* - the name for the place you cloned the repository from (i.e. Github; you can check this with `git remote -v`). 

![creating a branch](https://i.imgur.com/1saHbZD.png)

Exercise:

1. Create and checkout a new branch on your computer
2. Change a file, commit it, and push the change
3. Go to github. You can now select the branch you created in the top left corner. It will tell you that this branch is 1 commit 'ahead' of master.
4. Go to insights->Network. You will see that your branch diverges from the main branch, and both now exist side-by-side

## When to use branches?

Branches are very useful in at least three circumstances:

1. You are working on some changes that you are still unsure about or don't want to commit to the main line, 
   for example because your change breaks everything so others will have trouble working with the code, 
   or because you are rewriting a big section of a document and the text doesn't make sense until your change is done. 
   In this case, you can create a branch to still be able to commit your changes, without disturbing others working on the same repository.
2. Alone or together with others, you are working on a bigger feature or change request. If you create a branch for this, 
   you can collaborate on the feature (by both working in the same branch) without disturbing the main line, 
   and it is also clear that the commits used to create the feature or fix the bug all belong together. 
3. You have a stable version of your tool that you wish to maintain, while also continuing to work on a new version.
   You can create a separate branch for the released version (or for the development version - either works), 
   and have separate commits to the stable and development version.

## Merging branches

In each of the use cases, at some point you probably want to merge branches again. For example, if you finished your rewrite, feature, or bugfix, you want to apply the changes back to main. 
The way to do this is to activate the repository you want to merge the changes *into*, so `git checkout main`. 
Now, you can ask git to merge the changes from your branch by doing `git merge mybranch`.
If there are not changes to *main* in the meantime, this will "fast forward" main to include the commits from *mybranch*, and both branches will point to the same commit. More likely, the main branch will also be changed in the meantime, and git will create a new merge commit that has both the latest commit from main and from mybranch as parent. 

This is illustrated below. C1 was the last commit in the repository before the branch was created. 
Then, C2 was created on the branch. Merging directly after committing C2 allows `git merge mybranch` to *fast forward* the main branch, since mybranch is stricly ahead of main.
If C3 is committed on the main branch in the meantime, however, you cannot fast forward as the branches have diverges.
Thus, `git merge mybranch` will create a new merge commit (M), and point the main branch to this new commit. 
The mybranch branch will still point to C2, and now be strictly behind the  main branch.

![Merging branches](https://i.imgur.com/PaMfjb9.png)

As usual, if commits from both branches changed the same parts of files, there will be a merge conflict that you need to resolve manually, and then `git add` the resolved files and `git commit` the merge. 

In any case, after merging the branches you can delete the old branch if you no longer need to work on it. 
This can be done locally with `git branch -d mybranch`. 
To delete the branch on github, it's easiest to go to github, click on the *N branches* icon, and use the trashcan icon to delete it. 


