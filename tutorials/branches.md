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

## Pull requests

When you have a branch on github, you can automatically create a *pull request* for the branch. 
A pull request is essentially a request to have the branch merged. 
This is quite a nice feature by itself as it allows you to automatically merge the branch without having to use the commands above.
There is even a built-in editor to resolve and merge conflicts. Kids these days really do have  it easy...

However, this feature really shines when you want to discuss whether the changes are a good idea.
For example, maybe you are the second author or otherwise not in charge of a project,
and your branch represents a significant rewrite or change.
In that case, you can create a 'pull request', that can be seen as a polite question to the other collaborator or first author
whether they agree with your changes. 
They can easily review the changes, merge them if required, and otherwise reject the request. 

In a pull request, you will also see if it can be merged without conflicts. 
In the 'second author' scenario, you probably want to present changes that can be merged easily.
Thus, if there are any conflicting changes in the main branch, you want to resolve them in your branch before discussing the pull request.
To do this, `git checkout main` to activate the main branch and `git pull` any changes. Now, switch back to your branch (`git checkout mybranch`),
and do `git merge main` to merge all commits in main into this branch. 
This will prompt you to resolve any conflicts, and create a new merge commit in your branch that has both your last commits and the last commit from main as a parent.
If you then `git push`, the new commit  is automatically added to the pull request (since a PR references the branch, not the commit),
and the request should now merge without problems. 


## Exercise

Suppose you are making a big change, and you want to make this change in a separate branch:

1. Create a new branch 'mybranch' locally, make a change, commit it, make another change, and commit that as well.
2. Run `git log`. Where do `HEAD`, `mybranch`, `master`, and `origin/master` point to? Is there an `origin/mybranch`? 
3. Push your new branch with `--set-upstream`. Check `git log` again. What changed? Why?
4. Go to github and look around in your new branch. 
5. On github, in the main branch, edit a separate file. 
6. On your computer, do `git pull`. Nothing will change, because your branch has not changed on github.
7. Checkout the main branch and `git pull`. The change on github is now pulled to your local *main* branch. 
8. Merge your branch with `git merge mybranch`. Check the `git log`. Does it all make sense?
9. Push your change and check the github network (in insights). Does this make sense?

Now, repeat the above exercise, but in step 5 change the same file that you edit in the branch, and fix the resulting merge conflict.

Finally, repeat this exercise, but using a pull request to merge the change after step 5. 
