# Git Basics

I've used Git a lot in my career and it has become a standard for me (and the rest of the industry) to use Git to manage
code files.

But what about the everyday folks & new developers who don't already live and breath this tech-y stuff? Reading the
official [Git](https://git-scm.com) website documentation and going to sites like [GitHub](https://github.com)
and [GitLab](https://gitlab.com) can make learning and using Git quite daunting and confusing.

Even worse, Cloud Service Providers like Amazon, Cloudflare, Google, and Microsoft often provide website-hosting
services that "source from Git" and lock off features if you don't use Git!

So, we have this daunting thing to learn that you kinda HAVE to use if you want to host a website.

Assuming you are someone who is feeling like this, this post will hopefully help you - or maybe you learnt the basics of
Git before and have just forgotten and need to remember the concepts and basic commands.

## What is Git

Git is a Version Control System (VCS) or a Source Control Management system (SCM). Those two terms are pretty much
interchangeable - feel free to use the one you prefer.

What a good VCS or SCM system (like Git) does for you is to provide a way to store code and perform changes to that code
in systematic ways. This systematic method of handling changes empowers us to:

- Handle the merging of multiple changes to the code provided by different people (allowing for easier collaboration on
  the code)
- Easily find who made certain changes and when they were made
- Undo certain changes if you need to

all while not needing worry about weird quirks & faults that come with using more automatic/guessing system of storing
things.

Git was authored by Linus Torvalds, the same guy who authored Linux.

## A note or two on commands

Before we move forward, this guide features a lot of commands. Whilst there are some tools with windows and buttons that
help people use Git, I personally find they make things more confusing that Git actually is.

Also, whenever you are running any of these commands, you will most likely want to have your terminal be in the root of
your Git repository (you'll find out about Git repositories shortly).

## Installing Git

If you're on Windows, got to the [Git website](https://git-scm.com) to download an installer and then follow the
instructions.

On macOS, I would recommend installing git through [Homebrew](https://brew.sh/) - by running `brew install git`

On Linux, use your preferred package-manager to install the `git` package.

## Repositories

To start working with Git with our code, we first need to know about the idea of a "Git repository".

A Git repository is a folder on a computer, that has a special folder inside it called `.git` that stores all the data
that Git needs to function. Unless you know what you're doing, NEVER touch, use or delete this `.git` folder.

A repository on your laptop or computer that you're working on is often referred to as your "local" repository.

A repository on a different computer - e.g. stored on GitHub, GitLab or a server - is often referred to as a "remote"
repository.

Semantically, a repository most often contains the code for a *single* coding project - like a single website,
application or logical bundle of configuration files.  
Many developers & companies like bundling ALL or MANY of their coding projects into a single Git repository, referred to
as a "monorepo" - the jury is still out on whether monorepos work effectively or not (personally, I don't think they
do).

### Local Repositories

To make a Git repository on your local computer, create a folder to store your code in and then open a Command-line /
Terminal window in that folder and run the command:

```bash
git init
```

(Incidentally, you can have code in this folder already, just make sure it's not in a sub-folder called `.git`)

### Remote Repositories

If you want to share your code to collaborate with other people or connect your code with a 3rd party service (like
Cloudflare or Google Cloud), or you just want to make your code publicly viewable (i.e. make it "open-source") then
you'll need to create a remote Git repository.

This can be done on websites like [GitHub](https://github.com) or [GitLab](https://gitlab.com). I recommend using
GitHub, since it's most popular and thus other websites almost ALWAYS provide integrations with it AND is one of the
sites that has support for unlimited private repositories (in case you don't want to make your code public).

When first setting this up, you'll need to configure your local repository to point to your remote repository. GitHub
provides [nice documentation](https://docs.github.com/en/get-started/getting-started-with-git/managing-remote-repositories)
for how to do this, with troubleshooting tips.

If you just need the command though, it should be:

```bash
git remote add <remote_name> <remote_URL>

# e.g. adding a remote repo that you will connect to via SSH:
git remote add origin git@github.com:Harmelodic/my-cool-site.git

# e.g. adding a remote repo that you will connect to via HTTPS:
git remote add origin https://github.com/Harmelodic/my-cool-site.git
```

If you want to learn about connecting to remote repos via SSH, I also
have [a guide on how to do this](https://harmelodic.com/blog/git-with-ssh).

Later, when you've "committed" your code & code changes into Git, you'll be able to "push" them from your local
repository to your remote repository. Then, if there are new changes in your remote repository but not locally, you
can "pull" those changes down to your local repository.

These actions are, respectively, done by performing the commands:

```bash
git push
git pull
```

You can think of this as kinda like uploading and downloading code changes.

## Status

Once we have a Git repository to work with, we can always check the status of our Git repository by performing the
command:

```bash
git status
```

Personally, I do `git status` A LOT - often before and after every other command. This might seem paranoid, but it
really helps me stay oriented with what is going on with my Git repository and the changes.

## Branches

Up until now, you've probably been working on a single copy of your code. Branches allow us to work on different copies,
ideally temporarily before "merging" that branch with all its changes back into our main copy of the code.

If you're going to be working by yourself on code, then you'll probably never need think about branches and instead only
ever work on the "main" branch, usually named "main" or "master".

This main branch is automatically created for you when you initialise a Git repository.

If you're working with other people and regularly pushing code, it can be easier to review and manage these changes by
putting them on a branch and then creating a "Pull Request" or "Merge Request" (depending on what the website calls
them) to merge the changes on your branch into the main branch. You can read more about basic branching with the
following documentation:

- Git-SCM's "Basic Branching and Merging": https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging
- Atlassian's "Using Branch" tutorial: https://www.atlassian.com/git/tutorials/using-branches

## Commits

When we work on our code, we make changes and save files to our computer. None of this changes with the introduction of
Git, it's just that Git tracks the files inside your repository and detects what has been changed.

However, at some point we need to tell Git: "I'm done with the editing/writing of my code" so that Git can efficiently
bundle all your changes into one bigger change. For Git, this is when you "commit" your changes.

Before we can commit, we need to prepare our changes. Sure we've changed lots of files, but that's not good enough for
Git. We need to "add" the specific changes we want to commit to be "ready to be committed" (because sometimes we make
changes but don't want to commit all of them).  
We add changes with the command:

```bash
git add <file>
```

If you want to add ALL changes to your commit, then just do:

```bash
git add .
```

Then to commit the files you've added, you can do:

```bash
git commit -m "Some commit message summarising your change"
```

and that's it!

If you have a remote repository configured, you can push this commit to your remote repository by doing:

```bash
git push <remote_name> <branch>

# e.g.
git push origin main
```

or just:

```bash
git push
```
