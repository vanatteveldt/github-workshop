# Commits, parents, and rebasing

To understand more of the underlying mechanism behind git(hub), it's important to realize what a commit really is:

  + A git repository can be seen as a sequence of *commits*
  + Each commit consists of a list of changes to files (*diffs*) and at least one *parent* commit. 
  + Each commit is identified by a unique 'hash'
  + This hash is calculated from both the changes and parent, so the same changes with a different parent give a different hash (and hence is treated as a different commit)
  + The state of the files in the repository and completely determined by the 'sum' of the last commit and all its (grand)parents. 
 
This information is displayed directly on github when you click on a commit in the history.
For example [this commit](https://github.com/vanatteveldt/testrepo/commit/27b8900a27b8a95fd390a5cbcf2c4835955a11c3) has the hash `27...c3`, parent `ba10cdb`, and changed a single file.
You can also browse the code at that commit by clicking the 'browse files' button.

## Conflicts when pushing

Let's assume that you have a repository, with the latest commit being `C1`. Now, you make some changes to a file, and commit them, creating commit C2. 
However, in the meantime someone else committed a change to te repository, creating commit C3.
If you try to push your change, it will be *rejected* even if the other commit changes completely different files than you did.

The reason for this has to do with the parents. The commit you want to push has C1 as parent, while the current remote head commit is C3. 
Simply put, you can only push commits that are based (parented) on the head commit that you are trying to push to. 

![merge conflict](https://i.imgur.com/2iGX0bw.png)

## Merging and rebasing

So, you need to fix this situation in order to push your work. There are two options here, both involving `pull`ing the latest changes from the remote repository before pushing. 

 1. *Rebasing* The instructions we gave you earlier is to use `git pull --rebase` in case of a conflict like this. Rebasing means that git temporarily stashes your commit (C2), and pulls in the remote changes, bringing your local state to C3. Then, it 'replays' your changes on top of C3, creating a commit (C2b) with the same changes as C2, but with C3 as parent. Note that since the parent is part of the hash, this is treated as a different commit. 
 2. *Merging* The second option is to just use `git pull`. This will automatically try to merge your changes by creating a third commit (M), that has both commits C2 and C3 as a parent. 
 
In both cases, after doing the pull your local head commit is not strictly ahead of the remote head, so you are allowed to push the changes. 

![rebase or merge](https://i.imgur.com/2qOxHV8.png)

As before, if there are conflicts (i.e. the same part of a file was edited in both commits), you will need to manually solve these conflicts as normal and use 
`git add <file>` to mark the conflict as resolved. In the case of rebase, you then use `git rebase --continue`, which will fold the new changes into the rebased commit (C2b),
and then apply more commits if needed. In the case of merge, you then use `git commit` to create the new 'merge commit' M. 

When to use rebase or merge? Both get the job done, so it doesn't matter a lot. Rebasing gives a cleaner history, as you don't clutter the history with empty merge commits.
So, for most cases we would advice using `git pull --rebase`. For bigger changes, when you expect real conflicts, or when you are merging two branches, it is better to use merging, as the explicit commit is more informative.

## Exercises

Rebasing:

1. Have a look at the commits in your history, and browse the repository at an earlier version
2. Create a new file locally, and at the same time edit the README file of your repository. Commit your local changes. 
3. Locally, run `git log`. Note down the hash of your commit.
3. Try to push. Read and understand the resulting error message.
3. Use `git commit --rebase` to solve the conflict, and push your changes. Read and understand the messages.  
3. Locally, run `git log` (or check the history on github). See that your old commit is there, but with a new hash (and parent).
4. Go to github and navigate to *insights*->*Network*. Observe that both commits are a straight line (your commit follows directly on the conflicting commit)


Merging:

2. Create a new file locally, and at the same time edit the README file of your repository. Commit your local changes. 
3. Locally, run `git log`. Note down the hash of your commit.
3. Try to push. Read and understand the resulting error message.
3. Use `git commit` to solve the conflict using merge, and push your changes. Read and understand the messages.  
3. Locally, run `git log` (or check the history on github). See that your old commit is there with a same hash, and a new merge commit was created. 
4. Go to github and navigate to *insights*->*Network*. Observe that now there was a fork that was joined at the merge commit. 

Conflict resolution:

1. Repeat the exercises above, but change the README file both locally and remotely. Resolve the conflict using both merge and rebase. 


