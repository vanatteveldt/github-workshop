# What are Git and GitHub?

Roughly speaking, there are two levels of understanding Git.
The first is to know a handful of commands that allow you to effectively use
Git in your own projects. The second level is to actually understand the elegant 
Git *branching* model and using it to it's full potential.

In practice, even many people that use Git effectively and on a regular basis 
hardly get beyond that first stage. And off course there is an [xkcd](https://www.xkcd.com) comic to illustrate this point:

<div style="text-align:center"><img src="https://imgs.xkcd.com/comics/git.png" width="250" style="text-align:center"></div>

In this workshop we first and foremost focus on the first level of understanding.
We do address some of the more advanced features, but the primary goal is to show you
why Git is useful and how you can use it. Let's start with a simple description of what Git is, and how it's related to GitHub.

## Git is a version control system
Git is a system for managing repositories.
A Git repository is a directory that contains all the files in a project,
including the complete history of revisions (called *commits*). 
This allows you to restore the content of the directory to a previous version.

Now, version control is nice, but that's probably not the reason you joined
this workshop. Stuff like Dropbox also has some nice version control, and doesn't
require a workshop. However, if you ever used this type of software for real-time
collaboration, you get why it wouldn't really work with programming tasks. Perhaps
you've even lived the hell of conflicted copies, or lost a directory that someone
accidentally deleted. 

What you'd really want is something like a version control
system designed for this type of collaboration. 
The big difference between Git and something like Dropbox is that git version control is *explicit*:
as a user, you explicitly create and name a new version (a *commit*), and you control the process of merging with other people's changes.
That is a bit more work than the 'magic' versioning of Dropbox, but it means that you never accidentally overwrite changes or create conflicting versions. 

## Git is a *distributed* version control system
Git is designed to facilitate collaboration.
Imagine several programmers working together on a project.
To develop and test code they need to have local copies of the project,
but this makes it very difficult (and cumbersome) to ensure that the different
copies of the projects stay synchronized. 
A distributed version control system such as Git makes this as painless and efficient
as possible.

Rather than discuss how this works right now, it's much easier to show it, so
let's quickly move on.

## GitHub is a hub for Git repositories
People sometimes mistake Git and GitHub for being the same thing.
GitHub is a hosting service for Git repositories. 
It is literally a hub, that connects different users working
on the same repository ([as visualized here](https://phpenthusiast.com/blog/the-essentials-of-git-and-github-for-web-developers#local_vs_remote_repo)).

If a repository is hosted (on GitHub), we call this the `remote repository` (or central repository). 
The repositories of the different users on their own local systems are called `local respository`.
Each local repository is directly connected to the remote repository, so if everyone manages to stay in sync with the remote repository, everyone stays in sync.
One of the most important topics of this workshop is how to manage this.


# Setting up Git and GitHub

For this workshop you should have [git](https://github.com/git-guides/install-git) installed, and registered a [GitHub](https://github.com/) account. If you haven't done this already, please do it now.

## Username and email addres

Now, there are some basic settings that you need to look into. 
Most importantly, you need to specify your git email address and name (depending
on how you installed git, this might already have been asked).
What's important here is that your email address is **the same address you that you
use on GitHub**. 

Let's actually do this from the terminal. As a reminder, on Windows you can go to start (or press the windows key) and type cmd to find the command prompt. On Mac, you can press command + spacebar (or apple-key + spacebar) and type terminal. On Ubuntu and some other Linux distributions it's `ctrl+alt+t`.

In your terminal, first just type git and hit enter, to see if you installed it properly.

```
$ git
```

This should give you an overview of important git commands. Now let's see if git knows your
user name and email address. For this run the following two commands.

```
$ git config --global user.name
$ git config --global user.email
```

If these give empty results, you still need to specify them. You do so by running the same
command, but adding your name and email address (between quotes).

```
$ git config --global user.name "your name"
$ git config --global user.email "you@something.com"
```


## Setting up GitHub authentication

When at some point (in about 15 minutes) you're pushing your first commits to 
GitHub, you'll naturally be required to authenticate. It used to be the case
that you can just give your username and password, but [the times they are a changing](https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/#what-you-need-to-do-today).
For you now you can still do this (the dates mentioned in the link have been postponed), but you might get a warning that this is 'deprecated',
and will no longer be possible from August 13, 2021. For more details 

If you're completely new to Git, you might want to ignore this issue for now and continue
with the workshop. But at some point you will have to deal with it, and then there
are two options.

* The seemingly easiest option is to get a [personal access token](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token). You can then just use this token instead of your password (i.e. literally, give this token when they ask for you password). Chances are good that you only need to do this once (or every once in a while) if your systems uses a password manager.
* The better solution is to use SSH authentication. You set this up once for every computer that you use, and you're good to go. GitHub provides clear instructions for [Windows, Mac and Linux](https://docs.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#generating-a-new-ssh-key). One thing to remember though, is that with SSH authentication cloning a repository (as shown below) works slightly differently. Instead of `git clone https://github.com/user/repository`, it becomes `git clone git@github.com:user/repository`.

# Creating a repository

To work on an existing project, you *clone* it from GitHub. 
Similarly, the easiest way to create a new git repository is to create it on the GitHub webiste, and then clone it to your local  computer.

Note that you can also create a git repository locally (with `git init`) and later add it to GitHub (or even work without GitHub altogether),
but it's easier to just start working from GitHub rightaway.   

## git clone: creating a local clone of a remote repository

If a git repository already exists, and is hosted on a platform such as GitHub,
you can create a local `clone` of this repository using the `git clone` command.

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


## 