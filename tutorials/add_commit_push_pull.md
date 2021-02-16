
# Working in your Git repository 

In the previous step you've created a `remote repository` on GitHub, and created a 
`local repository` by cloning it. Now we'll discuss the main commands for
making changing in the `local repository`, and for interacting with the 
`remove repository`. We'll discuss 4 commands: 

* `git add`: add changes to the *staging area*
* `git commit`: save the *staged* changes in the `local repository`
* `git push`: push the committed changes to the `remote repository`
* `git pull`: if changes have been committed to the `remote repository`, pull them to the `local repository`.


## Making changes in your local repository 

When we make changes in our directory, we don't automatically update the local repository.
That's a good thing: we don't want our version control system to store all the dumb 
stuff we tried in writing our code. We want to be able to tell Git when we're ready to *commit*.
For this, we need the `git add` and `git commit` commands.

The *commit* is the part where we actually save our changes to the repository.
This part should make sense: you've made changes to your directory, and now
you want to store them safely in your version control system. 
However, before you can do this, Git first requires you to specify which changes you
actually want to commit. You'll later see why this makes sense. For now, just consider that sometimes you
put stuff in your directory that you don't want to add to the public repository (e.g., private data).

We specify which changes to make by *adding* them to the *staging area*.
This is easiest to just show. Open your terminal and navigate to your local repository.
Let's first run `git status` to see what's up.

```
$ git status
```

If you haven't yet made any changes to your directory, this should tell you that there's
"nothing to commit, working tree clean". So now let's actually make some changes.
Create a simple text file called `hello_world.txt` (or any name you like), using whatever
text editor you like. Now, if you run `git status` again, you should see the following:

```
$ git status
On branch main
Your branch is up to date with 'origin/main'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	hello_world.txt

nothing added to commit but untracked files present (use "git add" to track)
```

It tells you that there's a file named hello_world.txt that's untracked (perhaps
it's even colored red in your terminal, to illustrate the drama). It also 
nicely tells you that you can use "git add" to track the file. 
In the next step, we're going to do this, and specifically we're going to run:

```
$ git add .
```

The dot here means: add everything. So all the untracked changes have now been
*added* to the *staging area*. If you run `git status` again, it should now tell you so,
and color the changed files in a comforting green color. 

Now that the stage is set, we can *commit*. For this command we'll use an additional argument
to add a message to our commit.

```
$ git commit -m "I created a file called hello_world.txt"
```

If you run this, you get some minor feedback on what you changed (how many files, how many insertions).
The changes are now committed! 
The `-m "blablabla"` part of the command basically just adds a brief message (m) where you should
describe what changes you made. It's important to make clear descriptions (but in practice most people
don't, and yes, off course there is an [xkcd](https://xkcd.com/1296/) about that).

And that's it! Now we're ready to push our changes to the `remote repository`.
As a closing note on `add` and `commit`, you should be able to get by just fine
by only remember the following two lines, and always running add and commit in tandem.

```
$ git add .
$ git commit -m "short description of what changes you made"
```

## Pushing changes from local to remote

You've now committed some changes to your `local repository` (this can be one commit, 
but also several). You figured it's about time to share your progress with your team,
so the next step is to `push` your changes to the `remote repository`.

Let's first take a look again at `git status`. If you've committed something, this should
now tell you that `Your branch is ahead of 'origin/main' by x commits`. We can now push
these local commits to publish them.

```
$ git push
```

This will probably ask you for your username and password. There are ways to prevent this,
and when you start using GitHub regularly you'll want this, but for now we'll keep it simple.
In any case, if all went well, you've now pushed the commits.
If so, you can open your GitHub repository in your web browser to see that your file has indeed
been added.
Also, if you again run `git status`, you should see that you're now back to a clean branch.


## The add, commit and push mantra

We've spent quite some time discussing `add`, `commit` and `push`, but here as
well you should be able to get by just fine if you only remember them as a single
mantra that you need to recite whenever you want to push your changes to GitHub.

```
$ git add .
$ git commit -m "short description of what changes you made"
$ git push
```

Sure, there are cases where it might be better to make several commits,
and then pushing everything in one go, but for smaller projects, and certainly
for individual projects, it's often 'just fine' to only run these commands in a single mantra.


# Pulling changes from the remote repository

Now you know how to make changes to the remote repository.
If you are working just by yourself, this is enough to keep your project going.
But if you are working together with others **who also push changes**, it's important
that **you also pull changes**. 

Conveniently, it never really hurts to pull. Try it:

```
$ git pull
```

This should tell you that you're `Already up to date`. Makes sense, since you're 
the only one working on this repository. But we can actually pretend that someone 
made some changes, because you are allowed to change files directly on GitHub. 

Visit your GitHub repository from your web browser, and click on the `.txt` file that you created before. 
Now to the top-right of the file, you should see the `Raw` and `Blame` buttons, and next to it a pencil.
Click on the pencil to enter edit mode. Now just type some stuff in the file.
At the bottom of the page, you can now *commit* this change.

Now the `remote repository` has changed, but your `local repository` hasn't. 
Specifically, your `local repository` is now *behind* by 1 commit. 
Now let's run pull again.

```
$ git pull
```

This should now show you that it downloaded the changes, and should provide a brief
summary of which files specifically have changed, and how much.
Most importantly, your `local repository` is now again up to date, and if you
run `git pull` again it will tell you as much. 



# Push and pull often

Pushing and pulling is important, and you would rather push and pull too much than too little. 
This way, you can as much as possible prevent *merge conflicts*.
The problem is simple: say you want to *push* some changes to the remote repository, but someone
else already pushed some changed the remote repository since you last *pulled*.
Rule of the streets is that it's now your responsibility to fix this.
This often isn't very hard, but it can be annoying. This is where the somewhat absurd
(but in practice quite common) solution comes from of just making a new clone from
the latest version and then adding your changes ([remember, this one](https://xkcd.com/1597/)).

We'll discuss some better solutions later on. But just remember that preventing beats
curing, and you can prevent this by pushing and pulling often.
