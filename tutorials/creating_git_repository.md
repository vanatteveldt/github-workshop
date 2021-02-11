# Creating a Git repository

A Git repository is a directory that contains all the files in a project,
including the complete history of revisions. 

# Creating a git repository

We will discuss two ways of creating a git repository: 

* Using `git init` to initialize a repository on you system
* Creating a repository on GitHub and cloning it

## Creating a repository from a local directory

A Git repository is essentially just a directory with bells and whistles
for version control. 
If you have a directory on your system that contains your project files, you can 
use the `git init` command to *initialize* the repository.

This is as easy as it sound. Open your terminal, and navigate to your directory 
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


## Creating a repository on GitHub 

If you plan on working with GitHub anyway, then an alternative approach is to create 
a new repository on GitHub, and then clone it to your system. The benefit is that it's slightly
easier than separately initializing a Git repository, creating a repository on GitHub, and syncing them.
Also, GitHub let's you conveniently add the `README` and `LICENCE` files, and offers templates for common `.gitignore` settings (more on these later). 

We'll use this approach in the next step, where you will create your own GitHub repository
to use throughout these tutorials.



