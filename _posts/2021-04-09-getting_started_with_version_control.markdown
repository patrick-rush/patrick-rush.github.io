---
layout: post
title:      "Getting Started with Version Control"
date:       2021-04-09 21:52:13 +0000
permalink:  getting_started_with_version_control
---


One of the most perplexing things that I encountered at the very beginning of my Flatiron journey was version control. *Git who? Repo what now?* I was told to commit early and often, but what exactly was I committing to? It turns out that the concepts are easy to grasp, but the information was not centralized and I had to do a good bit of googling to find my way.

I wasn’t a total noob when I started, but GitHub and version control were new concepts to me. If you are just starting out at Flatiron, or you’ve only ever worked on projects locally (on your own machine) and solo, you may not yet have encountered the wonderful world of version control, so I’m going to give you a little crash course. But first, why should this matter to you now?

Well, I’m sure there are plenty of reasons, but here’s three: 

1. You’ll be working with a team someday, and that team will use version control. I like to get ahead of the game. If there is something I know I’m going to need down the road, I’d love to not look like a total amateur when it comes time to use it. While there are many different version control options out there, a basic understanding of one will get you a long way when learning another. The one you’ll be using at Flatiron and the one I’ll talk about in this post is GitHub.
2. You are going to make mistakes, and having a strong habit of committing changes to a version control system will ensure that you don’t lose valuable time trying to figure out where you went wrong.
3. Precious green squares! I had no idea how important those little green squares were. GitHub visualizes the work you do by placing various shades of green squares on a grid. Potential employers may look at that grid as a quick reference for how committed (pun intended) you are to this career path. Coding a little bit every day and committing that work to GitHub is a great way to not only keep a high level of proficiency, but also to show that you are serious about your work!

![squares](https://i.imgur.com/SCrHAUz.png)

Let’s talk about some lingo before we go any further:

**Repository (repo for short)**: In general, a repository is a place where something is stored. I like to think of a water reservoir, which is a repository of water being held until it is needed. A GitHub repository is no different. It is where all of the code and version information for a project is stored.

**Master/Main**: These two are interchangeable, but going forward you are more likely to see `main` and less likely to see `master` thanks to an October 2020 change at GitHub. However, as you read about version control and google questions you have, you’ll likely come across the word `master` a lot. Just know that `main` and `master` are the same thing. So what are they? You can think of them as the trunk of your working tree. All of the versions of your project are connected back to this main resource. Often, in the case of a program, app, or website that is live, this is where the polished and working code lives.

**Branch**: Whereas `main` is your tree trunk, branch is your… well, branch! When you want to make a change to a repository, say add a feature or refactor some code, it is always best to do so in a branch rather than in the trunk (`main`). This way, if you mess something up or introduce a bug, nobody in the real world has to see it! 

**Clone**: When a repository is in GitHub but doesn’t yet live on your computer, you have to clone the repository so you can make changes on your machine. This will connect the version of the repo on your machine with the version on GitHub. 

**Commit**: Whenever you make changes to the files on your computer, you want to make note of what you’ve changed and then `commit` those changes to your repository. The note you make about the changes is called a commit message and the change itself is called a commit. It is imperative that these messages be useful and professional as they will be visible to others including prospective employers. 

**Push**: When you’ve made a handful of changes on your computer and you are ready to add those changes to the repository, you have to `push` your commits to the repo. If you are in a branch, those changes will be pushed to the branch. 

**Pull**: As you might have guessed, pulling is the opposite of pushing! When you are working on a team, changes may have been pushed to the repo that you don’t have on your local machine. Pulling allows you to add those changes to your local directory. 

**Merge**: You will merge every which way when you are working on a team, but in the most basic sense, this is how you add changes made in a branch to your main branch. Of course, if you’ve made changes in `main` and want to add those changes to a branch, you can merge that way as well! This is normally a simple operation, but occasionally, you’ll end up with two different versions of the same code, in which case some version control magic will happen and you’ll have to decide which code to proceed with. 

So, let’s walk through what this might actually look like. I’m going to include the commands that you would use on the command line, but many of these operations can be performed via the version control features in your editing software (VSCode, etc.) or on GitHub itself. Additionally, I’m going to approach this first as if you are working alone. Version control is an essential tool for working with a team, and there are specific cases you will need to learn to address when code is changing and being added from multiple people and machines, but first getting comfortable using version control solo can be a low risk way of learning the basics. 

If we are starting with a repo that already exists, we’ll want to navigate to that repo’s GitHub page. There you will see a button in the top right corner that says `fork`. Clicking that button will create a whole new repo, one that belongs to you, that contains all of the code and version history of the other repo. This new repo will belong to you (be mindful of licensing), and anything you push to it will only live there and won’t go to the original repo. If you are collaborating on a project with others and want to work in the same repo as them, you’ll want to skip this step. However, if you are using a repo as a starting point and you never expect to push those changes to the original repo, this is the way to go. 

![fork](https://i.imgur.com/TeFFvVM.png)

Now, in order to get this repo onto your machine, you’ll need to `clone` it. There is a green button that says `code` on it. You will have a few options as to how you would like to copy the repo. Documentation for connecting to GitHub can be found [here](https://docs.github.com/en/github/getting-started-with-github/set-up-git#next-steps-authenticating-with-github-from-git). Depending on how you are connected, you’ll choose the appropriate link, copy it, then head over to your computer’s terminal.

![code](https://i.imgur.com/UlOI4js.png)

Making sure you are in the [correct directory](https://www.macworld.com/article/221277/master-the-command-line-navigating-files-and-folders.html), enter `git clone <repo-link>`. This will clone the repo onto your machine and connect it to the repo on GitHub. You aren’t yet in the directory you cloned though, so you’ll need to navigate to the newly added directory. 

Once there, if you are working in a repo that belongs to you and that only you are working on, perhaps a personal project or something similar, you could just start working, but it is a great idea to make changes on branches. That way, your main branch is always working. You can type `git branch` at any time in the command line to see a list of branches as well as the currently active branch. To create a new branch, in the command line, enter `git checkout -b <name-of-new-branch>`. This will create a new branch and move you over to it automatically! 

When you have made a few changes, it is time to commit! Getting in the habit of committing often is important. At the beginning, it can be helpful to set a timer or some other way to remind yourself to commit. You really only want to commit related changes in a limited number of files at a time. If you have made changes that are unrelated, you can use your editor’s version control system to help you stage only the changes you want to commit, [or you can use command line prompts to do the same thing](https://git-scm.com/book/en/v2/Git-Tools-Interactive-Staging). If you are good about only making related changes and are confident that everything that you’ve changed since your last commit can be committed together, enter `git add .` (that period at the end is important) on the command line. This will stage all changes that have been made since your last commit. You can also stage specific files by typing `git add <filename>`. Then, enter `git commit -m “<a brief but accurate message about what you are committing>”`. These commit messages are stored in version control and can help you find where you made changes. This is helpful if you come across a bug down the road and need to figure out where it was introduced. Be mindful that these commit messages can be seen by anyone who has access to the repo, so make them useful and professional.

You can keep going on like this (code, stage changes, commit, repeat) until you are ready to add your work to the repo. At this point, you will enter `git push`. This will push all of your commits to the repo! I often like to take this opportunity to remind myself where I am in the repo by typing `git branch` and making sure the branch I thought was active is indeed active. If you are working alone, and you would like the changes you made on your branch to go to the master, you can now switch branches by entering `git checkout main`. A good habit to form is to pull changes down from the main branch before merging branches. Type `git pull origin main` to pull down changes from the repo, and then type `git merge <branch-you-want-to-merge-into-main>`, and you’re all set! Remember when merging that the directory you want the changes to end up in is the one you need to be in when merging. Here, I wanted my changes to be in main, so I navigated to the main branch and then merged my other branch into main. 

This is only the very beginning of what you can do with version control! It is a powerful tool that can help you correct mistakes, find bugs, organize your code, work in a team, and collaborate on open source projects. Some useful things to learn about once you wrap your head around these basic concepts are pull requests, conflicts, and git log.

Version control and GitHub are intimidating when you first start learning to code, but don’t sweat it too much. Practice makes perfect, and by the time you are done with your first project, if you follow these tips, you’ll be feeling confident with the whole thing!

