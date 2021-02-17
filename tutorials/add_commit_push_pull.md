
# Working in your Git repository 

In the previous step you've created a `remote repository` on GitHub, and created a 
`local repository` by cloning it. Now we'll discuss the main commands for
making changing in the `local repository`, and for interacting with the 
`remove repository`. We'll discuss 4 commands: 

* `git add`: add changes to the *staging area*
* `git commit`: save the *staged* changes in the `local repository`
* `git push`: push the committed changes to the `remote repository`
* `git pull`: if changes have been committed to the `remote repository`, pull them to the `local repository`, and update your local files.

![Pushing and Pulling](https://i.imgur.com/qeagQD6.png)

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
git status
```

If you haven't yet made any changes to your directory, this should tell you that there's
"nothing to commit, working tree clean". So now let's actually make some changes.
We're going to create a simple text file called `hello_world.txt`, that just contains the text
"hi". It doesn't matter how you create this file, but for this example we'll do this from the
command line (just because we can). 

```
echo "hi" > hello_world.txt
```

You can verify that this worked (`dir` on Windows, `ls` on Mac or Linux)
Now, if you run `git status` again, you should see something like:

```
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
git add .
```

Note that we put a dot after add. After `git add` you can specify which specific files you want to add. 
The dot here is convenient shorthand for saying `add everything`. 

So all the untracked changes have now been
*added* to the *staging area*. If you run `git status` again, it should now tell you so,
and color the changed files in a comforting green color. 

Now that the stage is set, we can *commit*. For this command we'll use an additional argument
to add a message to our commit.

```
git commit -m "I created a file called hello_world.txt"
```

If you run this, you get some minor feedback on what you changed (how many files, how many insertions).
The changes are now committed! 
The `-m "blablabla"` part of the command basically just adds a brief message (m) where you should
describe what changes you made. It's important to make clear descriptions (but in practice most people
don't, and yes, off course there is an [xkcd](https://xkcd.com/1296/) about that).

And that's it! Now we're ready to push our changes to the `remote repository`.

## Pushing changes from local to remote

You've now committed some changes to your `local repository` (this can be one commit, 
but also several). You figured it's about time to share your progress with your team,
so the next step is to `push` your changes to the `remote repository`.

Let's first take a look again at `git status`. If you've committed something, this should
now tell you that `Your branch is ahead of 'origin/main' by x commits`. We can now push
these local commits to publish them.

```
git push
```

This will probably ask you for your username and password. There are ways to prevent this,
and when you start using GitHub regularly you'll want this, but for now we'll keep it simple.
In any case, if all went well, you've now pushed the commits.
If so, you can open your GitHub repository in your web browser to see that your file has indeed
been added.
Also, if you again run `git status`, you should see that you're now back to a clean branch.


## The add, commit and push mantra

We've spent quite some time discussing `add`, `commit` and `push`.
But although these are three separate commands, you should get by just fine if you
think of them simply as a single mantra that you need to recite whenever you want to push your changes to GitHub.

```
git add .
git commit -m "short description of what changes you made"
git push
```

Sure, there are cases where it might be better to make several commits,
and then pushing everything in one go. But for smaller projects, and certainly
for individual projects, you can just think of `add-commit-push` as a single operation.


# Pulling changes from the remote repository

Now you know how to make changes to the remote repository.
If you are working just by yourself, this is enough to keep your project going.
But if you are working together with others **who also push changes**, it's important
that **you also pull changes**. 

Conveniently, you can just pull to see whether the remote repository has been updated.

```
git pull
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
git pull
```

This should now show you that it downloaded the changes, and should provide a brief
summary of which files specifically have changed, and how much.
Most importantly, your `local repository` is now again up to date, and if you
run `git pull` again it will tell you as much. 

## Good Workflow Habits

+ **Pull before you start working**. If you make changes, you want them to be based on the most recent version of the project. So, always `git pull` before you start working on something (and generally do a quick `git status` even before that so you can see if you accidentally forgot to commit something)
+ **Commit early, commit often**. Having lots of relatively small commits is better than one big commit after a day (or more) of work. Ideally, whenever you make a standalone change (fix some issue, rewrite or reorder some paragraph(s)), commit it with a message stating why you made those changes. This allows others (and your future you) to understand why you did certain things. If you wait a long time between commits you often don't really remember why you did something, and there is a bigger chance that someone else changed something in the meantime. So, commit and push your changes as often as possible (and we will later learn about [branches](branches.md) that allow you to commit changes without interfering with other users)

## Exercises

1. Make sure you have a GitHub repository for testing and you cloned it on your local computer.
2. Edit the README.md file on your computer
3. Check `git status`. Do you understand the output?
4. Stage your change using `git add README.md`, and check `git status`. Do you understand the output?
5. Commit your change using `git commit -m "Edited the readme"`,  and check `git status`. Do you understand the output?
6. Push your change using `git push`,  and check `git status`. Do you understand the output?
7. On Github, refresh and see that your change is now visible. Within GitHub, edit the file and make another change.
8. On your local computer, check `git status`. Can you see the change from GitHub? Why not?
9. Update your local repository with `git pull`. Do you understand the output?
