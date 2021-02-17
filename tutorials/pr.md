# Forks and Pull Requests

In the [tutorial on branches](braches.md), you learned how you can use branches to make changes in a different 'snapshot' of the repository, and merge the changes back manually or using a pull request. 
That is a great work flow for collaborating within a project. 

However, in the world of open source / open science you sometimes want to make changes to code from another team or project,
and generally you will not have the right to push changes to the repository the code is in.

For example, if another group created a tool that is really useful, but it needs some minor changes to work for your task or there is a minor bug that is, well, bugging you.
Of course, you can politely ask the other group to make those changes or fix the bug, but you can also do it yourself!

# Creating a fork

The first step in changing someone else's code is to **fork** it. 
You can fork a repository using the fork icon in the top right on github. This creates a complete copy of the repository (including the full commit history)
in your own space (i.e. github.com/yourname/therepository). 
From there, you can clone, edit, commit and push it as normal, and create branches, issues, and collaborate on it. 
For all purposes, this is now a complete and independent copy of the project, owned (in the technical sense) by you.

## Aside: Licenses and copyright

Before forking, you should always check the license to make sure that it allows you copy and edit it. 
For  open source licences like MIT or GPL and creative commons licenses except for CC-ND, this is always allowed as it is the core of open source that you can make changes to a program.
However, note that with GPL or CC-SA you are legally obliged to also license your code under the same license, and with CC-NC you are not allowed to profit commercially from your copy. 

If there is no license file, you should probably contact the original author and gently prod them to [include an explicit license](https://docs.github.com/en/github/building-a-strong-community/adding-a-license-to-a-repository). 

# Creating a pull request

If you want to add a specific feature to or fix a bug in someone else's project, you of course want to share the changes back with them.
This is not only good citizenship, it also means that you can switch back to using the 'official' version of the tool, 
and not continue using your fork that will diverge or become outdated as development continues on the original project.

Creating a pull request is easy: if your fork is ahead of the original github will prompt you to 'create a pull request' on the front page of your repository.
The original owner will automatically be notified of this request, and they can decide to merge it or ask you to make specific changes. 
Pull request can also include comments to facilitate discussion (e.g. asking for clarification or specific changes before merging the change).

Note that if you intend to make a specific change or bug fix, it is almost always a smart idea to create a new local branch for it,
just as if you would make a major change locally. This keeps commits in your branch distinguishable from possible new commits to the main branch,
and makes it easier to make two separate pull request for separate changes. 

## Exercise

1. Ask your (virtual) neighbour for a link to their test repository, and fork it on github.
2. Create and checkout a local branch
3. Change one of "their" files, commit, and push.
4. Create a pull request of your change, and ask your neighbour to merge the request. 

Alternatively, if you see any typo's or mistakes in this repository, fork it, fix it, and create a pull request. 

# Updating your fork

The key feature of a fork is of course that both groups can now work independently on separate copies of the code.
This, however, also means that the two forks can diverge.
It is quite likely that while you were making your new feature or bugfix, the original project also continued developing. 

So, if you want to continue working with your fork, you want to pull the changes from the original project into your fork,
The easiest way to do this is to go to github and view your main branch. It should tell you that you are N commits behind the original,
and allow you to create a pull request, that you can then directly merge yourself.

Note that you can also make these changes yourself

1. Switch to your main branch 
1. Add the original project as a second 'upstream' remote: `git remote add upstram git@github.com:originalowner/repository`
2. Fetch the changes from the remote repository: `git fetch upsteam`
3. Merge the remote changes into your local main branch: `git pull upsteam main`
4. If desired, push the changes to github: `git push`

After bringing your local main branch up to date, you can of course merge these changes into any feature or bugfix branch if desired (for example, if your pull request fails to merge),
by switching to your repository and using `git merge main` as before. 

## Exercise

1. On your forked repository, create and activate a new branch, change a file, commit, and push. 
2. Ask your neighbour to make a change to the same line in the same file.
3. Try to make a pull request on the github page of your fork. It should tell you that there is a merge conflict.
4. Update your 'main' repository (either using a pull request or by adding a second remote)
5. Merge your neighbour's changes into your branch, resolve the conflict, and push the changes.
6. Make a pull request on the github page of your fork (or check the PR you make in step 3 if you didn't cancel it). It should now merge without problem. 



