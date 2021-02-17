# README, licence and .gitignore

When you created the GitHub repository, we asked you to check all the boxes for adding
the README, license and .gitignore files. Here we'll briefly elaborate on why these 
are useful.


## README.md

You'll probably know the idea of a readme file already. But good to know is that 
on GitHub, the readme file is also displayed on the main page of your repository.
So here you can add anything you want to say to your audience. The format is a [markdown](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet) file.


## licence

It can be useful for possible users of your data/code to know something about the 
legal status. Perhaps most importantly, to what extent can they just copy and use it?
There are some standard templates for licences, and on GitHub you can choose which licence
you want to include. [This here](https://choosealicense.com/) is a good simple guide for open source.


## .gitignore

A .gitignore file is a file in your repository that specifies which files git should ignore (what's in a name).
For example, if your repository contains an R project, there are certain files that
you often don't want to share.

Conveniently, GitHub has templates for certain types of content.
When you created the GitHub repository, we asked you to select the [R template](https://github.com/github/gitignore/blob/master/R.gitignore).
If you now open this file (or follow the link), you'll see a list of files, and patterns for matching files,
that are ignored. 

You can also add your own patterns. For instance, you might want to share your code and results
on github, but not your raw data. It is then convenient to still keep the raw data in your 
local directory, but NOT commit it to the repository. This can be achieved by adding the name
of the raw data file (or a pattern for matching multiple files) to .gitignore.

