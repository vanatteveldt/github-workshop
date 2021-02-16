# Branches, tags, and releases

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
   for example because your change breaks everything so others will have trouble working with the code, or because you are rewriting a big section of a document and the text doesn't make sense until your change is done. In this case, you can create a branch 

## Merging branches


