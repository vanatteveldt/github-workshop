# Resolving conflicts

In the previous step you learned about pushing and pulling.
You also learned that you should push and pull often to stay nicely in sync with the
remote repository. 
However, even if you do this diligently, you will at some point get into a 'conflict'
if you're working with others on the same repository.

The problem is simple: say you want to *push* some changes to the remote repository.
But suddenly git refuses, because someone else already pushed some changed the remote repository since you last *pulled*.
Rule of the streets is that it's now your responsibility to fix this.

One of the best things about Git is that it actually helps you resolve these conflicts.
However, before we talk about how this works, and what techniques you can use for properly resolving conflicts, we're going to focus on the most common types of conflicts, and the most simple solutions.


# A simple conflict

The common case of conflict we'll focus on, is that you committed some changes
to your local repository and want to push them, but while you were working on 
these changes, someone else beat you to the push. In storyboard:

* You: `pull`
* Someone else: `push`
* You: `push`
* GitHub: **no >:(**

Git will now tell you: `failed to push some refs to [respository]`, and tips that
`Updates were rejected because the remote contains work that you do not have locally. This is usually caused by another repository pushing to the same ref.`

Sound's fun, so let's orchestrate it. Remember that in the previous tutorial we manually
committed a change on GitHub? This time we're doing the same thing, but instead of pulling,
we're going to try and push our own local changes. Specifically, take the following steps.

* 1. Go to your GitHub repository
* 2. Open the README.md file, and add some text to it
* 3. Commit the changes

This changed the `remote repository`. Now let's change the `local repository`,
and try to push it. We're going to create a new file called `conflict.txt` containing
just the text "foreboding".

```
$ echo "foreboding" > conflict.txt
```

Now add, commit and push.

```
$ git add conflict.txt
$ git commit -m "boy, I sure hope nobody pushed before me"
$ git push
```

If we did good (bad?) you should now receive the error mentioned above. 
(You might receive a similar but different message, depending on system/version).

## A simple solution

In the error message git already gives the hint to use `git pull`.
If we do this, Git will automatically try to merge the repositories.
This is good, but we're going to use a little variation called *rebasing*.
This solves the same problem, but in a different way.
We'll talk more about the difference between merge and rebase in one of the more
advanced tutorials.
For now, just assume that rebasing is often a good approach.
It works as follows.

```
$ git pull --rebase
```

What happened, is that git now first pulled the updated remote repository, and then
just committed your change **on top of it**. 

And that actually solved the problem. Check `git status` to be sure.
This should now tell you that you're 1 commit ahead. So let's push:

```
$ git push
```

And now you can check the GitHub repository to see that it all worked out. 


# A slightly less simple problem

The previous problem was easy to solve, because the changes that you made in the `local repository` were in a different file from the changes that you made to the `remote repository`.
But another very real scenario is that you and a collaborator both made changes 
to the same file, and perhaps even the same lines of code.
In this case, Git can't just merge the changes or perform the rebase.

To orchestrate this, we'll first make some changes to the remote repo:

* 1. Go to your GitHub repository
* 2. Open the conflict.txt file, and change the text to "Marco"
* 3. Commit the changes

Now we'll also change the conflict.txt file locally, and then add, commit and (try to) push.

```
$ echo "Polo" > conflict.txt
$ git add conflict.txt
$ git commit -m "surely nobody touched this file"
$ git push
```

Now you get the same error as before. 
What you don't yet know, is that the problem is actually much worse.
This you'll only learn after trying to rebase

```
$ git pull --rebase
``` 

The message you get is pretty long and scary. It talks about a merge conflict, and
failing to deal with it. 

## A decently simple solution

If you look at the last few lines of the error message, you see that there's hope.
However, we will need to get our manual hands dirty.
Luckily, Git has marked the conflicts that it encountered, so you just need to
ask Git where it found them, and then manually fix the problems.
When you fixed the, you can run `git rebase --continue`.

Actually, let's just run this right now:

```
$ git rebase --continue
```

This should tell you that conflict.txt needs merge, and that you must edit all
merge conflicts and mark them as resolved (with git add). You can open
conflict.txt and look for the conflict. Git clearly marks them with `<<<<<<< HEAD`
You can also follow the hint to run `git am --show-current-patch` to see where the 
conflict is, but in practise it's often easier to just open the problematic files
and search for `<<<<<<`. 

Now open conflict.txt in your plain text editor. You'll see something weird:

```
<<<<<<< HEAD
polo
=======
marco
>>>>>>> surely nobody touched this file
```

So what do we see: 

* the start of the conflict is marked with `<<<<<<< HEAD`, and the end is marked with `>>>>>> [your commit message]`. Note that in this case we didn't have any text before 
* the change that was added on the `remote repository` is shown above the `=======` line
* the change you made in the `local repository` is shown below the `=======` line

Basically, you now just fix it in your text editor. You, as the intelligent human, decide how to resolve the conflict: is either marco or polo the better commit, or maybe some combination? Then just remove the whole chunk from start to end, and replace it with your solution. For this example, just replace it with "marco polo!!"

Now we mark them as resolved with add, and continue the rebase.

```
$ git add conflict.txt
$ git rebase --continue
```

And now the conflict should be solved, and we can push.

```
$ git push
```

Check on GitHub to verify if it worked. The conflict.txt file should now say "marco polo!!"


## A super simple solution

There is another... solution. Remember that [xkcd comic](https://xkcd.com/1597/) we began the workshop with?
It actually refers to these types of situations.
And the solution proposed in this comic, though far from elegant, can be a lifesaver
if you just really need to push something but forgot the whole --rebase dance.

That solution is:

* Make a back up of your repository
* Delete the repository, and make a fresh clone
* Now just redo/copy whatever changes you made in the fresh clone

Of course, this isn't really a good option if you made many changes. Maybe the conflict is 
just one line somewhere, in which case the rebase dance is a much better solution. But if you're really
stuck, this can be a last resort fix.
