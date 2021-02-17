# Resolving conflicts

In the previous step you learned about pushing and pulling.
What we didn't yet say, is that you would rather push and pull too much than too little. 
This way, you can as much as possible prevent *conflicts*.

The problem is simple: say you want to *push* some changes to the remote repository.
But suddenly git refuses, because someone else already pushed some changed the remote repository since you last *pulled*.
Rule of the streets is that it's now your responsibility to fix this.

This often isn't very hard to fix, but it's always annoying. 
Git actually has some powerful tools for properly resolving conflicts, 
but the problem with the proper approach is that (unless coding in teams is your day job),
you'll so rarely run into complicated conflicts that you'll actually remember the steps.
In this tutorial we'll therefore focus on common problems that you might
encounter working in smaller teams, and the easy to remember ways of fixing them.


# A simple conflict

The common case of conflict we'll focus on, is that you committed some changes
to your local repository and want to push them, but while you were working on 
these changes, someone else beat you to the push. In storyboard:

* You: `pull`
* Someone else: `add`, `commit`, `push`
* You: `add`, `commit`, `push`

At this point, Git will tell you: `failed to push some refs to [respository]`, and tips that
`Updates were rejected because the remote contains work that you do not have locally. This is usually caused by another repository pushing to the same ref.`

Sound's fun, so let's orchestrate it. Remember that in the previous tutorial we manually
committed a change on GitHub? This time we're doing the same thing, but instead of pulling,
we're going to try and push our own local changes. Specifically, take the following steps.

* 1. Go to your GitHub repository
* 2. Open the README.md file, and add some text to it
* 3. Commit the changes

This changed the `remote repository`. Now let's change the `local repository`,
and try to push it. Create a new, empty text file in your working directory called "conflict.txt".
Now add, commit and push.

```
$ git add .
$ git commit -m "boy, I sure hope nobody pushed before me"
$ git push
```

If we did good (bad?) you should now receive the error mentioned above. 
(You might receive a similar but different message, since we didn't test if this
is identical on different systems).
Note that Git also gives a hint to use `git pull`. 
This could actually work, but we'll do it slightly differently.


## A simple solution

If we follow the advice to use `git pull`, what will happen is that git will
automatically try to merge the updated remote repository with your updated local
repository. This is good, but there is another option called `rebase` which has the
same purpose (resolving the conflict) but does it in a different way. Without going
into details, we recommend just always using rebase (but [see here](https://www.atlassian.com/git/tutorials/merging-vs-rebasing) 
if you're interested in the pros and cons).

To rebase, we also use git pull, but with a flag.

```
$ git pull --rebase
```

What happened, is that git now first pulled the updated remote repository, and then
just committed your change **on top of it**. In the current problem this is easy,
because the changes that you made in the local repository were in a different file from the changes
that you made to the remote repository (we'll deal with the harder problem below). 

So basically the problem is already solved! Check `git status` to be sure.
This should not tell you that you're 1 commit ahead. So let's push:

```
$ git push
```

And now you can check the GitHub repository to see that it all worked out. 


# A slightly less simple problem

Conflict within same file

## A pretty simple solution
But before we do anything, MAKE A BACKUP! Nothing fancy, just copy the directory, 
or at least the files you changed. Never underestimate a good backup.


## A super simple solution

[remember this one](https://xkcd.com/1597/)


################## say something about passwords: https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token)