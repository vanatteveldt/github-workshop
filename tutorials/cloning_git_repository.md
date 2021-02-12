# Cloning a Git repository

The main purpose of Git repositories is to share them.
They allow us to work on the same project directories at the same time without bumping in to each other, 
and help us merge our progress afterwards.
To achieve this, we don't just send around copies of these Git repositories, 
but host them on platforms such as GitHub. 
From there we then `clone` the repository on our local system. 
This clone becomes our own `local repository`, and the repository on the platform
that all team members can use is called the `remote repository`.

The nice thing about Git is that the local and remote repositories like to stay in touch.
As you'll see later on, we can easily instruct this local repo to `pull` new changes from the 
remote repo, or to `push` our own updates to it.
Since collaborators are all connected to the same remote repository, and can quickly
synchronize it with their local repository, everyone stays in sync (as much as possible).

So to become a collaborator, the first thing you need to do is to clone the remote repository.
Here we'll first show you how to do this with an existing repository.
Then, you're going to create a new repository on GitHub and clone it.
You'll also need this second one for the other parts of this workshop.

## Cloning this very workshop 

This workshop itself has a repository on GitHub. 
You can clone this repository to your local system with the `git clone` command,
followed by the url.
Open your terminal and navigate to a directory in which you would like to place
the clone. Now run the following code.

```
git clone https://github.com/vanatteveldt/github-workshop.git
```

Now that you have created the directory, you can navigate to it.

```
cd github-workshop
```

And here you can again verify that this is actually a github repository by checking the status.

```
git status
```

So what can you do with this local repository? Well, in addition to having the files,
you also have the complete version history, and you can revert back to previous versions.
You can also at any time check and gather new updates. 

What you can't do, is make changes to the remote repository, because you're not
a registered collaborator. Basically, you have read access, but no write access.
Still, you are allowed to make changes, and you can ask the maintainers of the
repository whether they want to adopt your changes. So without being an official collaborator
you can still be a nice person who helps. But more on that later when we get to `pull requests`.


## Cloning your own repository from GitHub

Now you're going to make your own repository on GitHub, and clone it to your local system.
This puts you in charge, which you need to be for some parts of this workshop.

To create a new repository on GitHub, go to you profile page, and open the `Repositories` 
tab. Here click on the bright green button that says `New`. Pick a name for you pet
repo, and optionally add a description. You can leave the public/private setting on public.
Check all the checkboxes for README, .gitignore and licence (we'll get to these later).
For the .gitignore, select the R template (this is just for illustration).
Set the licence to MIT. 

Finish creating the repository. You should now see you own GitHub repo with only the files
.gitignore, LICENCE and README.md. The next step is to clone this repository.
Copy the URL of your fresh new repository, and execure the `git clone` command (don't 
forget to navigate to a directiory in which you want to put it).

```
git clone https://github.com/[your github profile]/[your repository].git
```

Make sure that this worked (and let us know if it didn't), because you'll be using this
repository for the next part of the tutorial, where you will update your local repository
and push the changes to the remote repository.
