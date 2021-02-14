# What are Git and GitHub?

There seem to be two levels of understanding Git.
The first is to know a handful of commands that allow you to effectively use
Git in your own projects. The second level is to actually understand the elegant 
Git *branching* model and using it to it's full potential.

In practice, even many people that use Git effectively and on a regular basis 
hardly get beyond that first stage. And off course there is an [xkcd](https://www.xkcd.com) about that:

<div style="text-align:center"><img src="https://imgs.xkcd.com/comics/git.png" width="250"/></div>

In this workshop we first and foremost focus on the first level of understanding.
We do address some of the more advanced features, but the primary goal is to show you
why Git is useful and how you can use it. Let's start with a simple description of what Git is, and how it's related to GitHub.

## Git is a version control system
A Git repository is a directory that contains all the files in a project,
including the complete history of revisions. 
In that sense, you could just use Git as just a version control system.
You can use it to restore the content of the directory to a previous version.

## Git is a *distributed* version control system
Git is designed to facilitate collaboration.
Multiple users can have their own *clones* of the same Git repository, in which
they can freely make updates. When they eventually want to share these updates,
they need to somehow merge the different versions back together.

You can imagine that this can lead to very complex situations. The goal of distributed
version control systems such as Git is to make this as painless and efficient
*as possible*.

## GitHub is a hub for Git repositories
People sometimes mistake Git and GitHub for being the same thing.
GitHub is a hosting service for Git repositories. 
It is literally a hub, that connects different users working
on the same repository ([some more details](https://www.sqlshack.com/integrating-azure-data-studio-with-git-and-github/)). 

If a repository is hosted (on GitHub), we call this the `remote repository` (or central repository). 
The repositories of the different users on their own local systems are called `local respository`.
Each local repository is directly connected to the remote repository, so if everyone manages to stay in sync with the remote repository, everyone stays in sync.
One of the most important topics of this workshop is how to manage this.

# Creating a repository

There are two main ways of creating a Git repository.

* `git init`: Create (initialize) a new local repository
* `git clone`: Create a local repository by cloning a remote repository


## git init: creating a local repository

A Git repository is essentially just a directory with extra bells and whistles. 
If you have a directory on your system that contains your project files, you can 
use the `git init` command to *initialize* the repository.

This is as easy as it sounds. Open your terminal, and navigate to your directory 
(to try this out now, you can just create an empty directory).
If you have [Git](https://github.com/git-guides/install-git) installed, you can now enter the command.

```
$ git init
```

To verify that it worked, you could check the status of the repo.

```
$ git status
```

This will tell you that you are currently on the `master` branch, and haven't
yet made any `commits`. If there are files in your directory, it will mention that
these files are currently `untracked` and that you can `add` them. We will
cover these soon. For now just celebrate that you initialized (your first?) Git!


## git clone: creating a local copy of a remote repository

This workshop itself has it's own [GitHub repository](https://github.com/vanatteveldt/github-workshop). 
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
You can also at any time check and pull new updates. 

What you can't do, is make changes to the remote repository, because you're not
a registered collaborator. Basically, you have read access, but no write access.
Still, you are allowed to make changes, and you can ask the maintainers of the
repository whether they want to adopt your changes. So without being an official collaborator
you can still be a nice person who helps. But more on that later when we get to `pull requests`.


# Creating a new repository on GitHub

When we create a new Git repository with `git init`, it is not automatically hosted
on GitHub. 
To do this, we would first need to create an empty GitHub repository, set this repository
as the remote repository for our local repository, and push the repository to GitHub. 
This isn't particularly difficult (see steps [here](https://docs.github.com/en/github/importing-your-projects-to-github/adding-an-existing-project-to-github-using-the-command-line)), but it is a bit involved. 

There is a somewhat easier alternative that only requires the `git clone` command.
We can just make the remote repository on GitHub, clone it, and then add the project files.
As a nice convenience, GitHub also let's you add a `README` file, and offers templates for a `licence` and `.gitignore` settings (more on these later).
We'll use this approach now to create your own GitHub repository to use throughout this workshop.

## Creating your repository via Github

To create a new repository on GitHub, go to you profile page, and open the `Repositories` 
tab. Here click on the bright green button that says `New`. Pick a name for your pet
repo, and optionally add a description. You can leave the public/private setting on public (i.e. anyone can see it).
Check all the checkboxes for `README`, `.gitignore` and `licence`.
For `.gitignore` select the R template (we won't use R, this is just for illustration).
Set the license to MIT. 

Finish creating the repository. You should now see you own GitHub repo with only the files
.gitignore, LICENCE and README.md. The next step is to clone this repository.
Copy the URL of your fresh new repository, and execute the `git clone` command (don't 
forget to navigate to a directory in which you want to put it).

```
git clone https://github.com/[your github profile]/[your repository].git
```

Make sure that this worked (and let us know if it didn't), because you'll be using this
repository for the next part of the tutorial, where you will update your local repository
and push the changes to the remote repository.