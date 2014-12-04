# Just enought Git for a non-developer

In Sharetribe almost everyone has to deal with code. That's the spirit of a small tech startup. We use git and Github for source code management and when dealing with code, you have to have basic skills for using git.

This article was meant to be an internal document for our non-developers who have to deal with code, namely our designer and analytics experts. However, as this stuff migth interest others, the blog post was written.

The best way to learn is by doing. That's why this tutorial contains also an interactive part. The tutorial can be found here https://github.com/sharetribe/just-enough-git. Feel free to make your own fork.

## Prerequisite

The article concentrates on branching. I expect that you already have the basic knowledge of git, that is:

* You know how to access and use command-line
* You have git installed
* You have a Github account
* You know how to `git clone` a repository
* You know how to see current `git status`
* You know how to `git add` changes to next commit and how to `git commit` them
* You know how to `git push` the commits to Github
* You know how to `git pull` others’ commits from Github
* You know what is a code branch

That is, you already know quite a lot! However, to be efficient with Git, there’s still a couple tricks you need to learn.

## The branching model.

At Sharetribe, we you so called Github branching model. The model is pretty simple and easy to grasp. It goes like this:

1. **Branch:** When you start working with a new feature/bug fix/document improvement/anything, you create a new branch.
1. **Pull Request:** As soon as possible, even before the feature is ready, push the branch to Github and create a Pull Request, so that others can see what you are up to.
1. **Rebase:** Make sure, that your branch is up to date. If not, rebase.
1. **Review:** When the feature is ready, ask a fellow teammate for a review. Fix the issues the reviewer found, if any
1. **Merge:** When the reviewer gives green light, merge the Pull Request to master.

A rule of thumb is that you should **never brake the master branch**. Master branch goes always to production.

## Learning goals

In order to follow that branching model, you need to learn a couple new git skills:

* Your git is properly configured
* You know how to create a branch
* You know how to jump from branch to another
* You know how to push the branch to Github
* You know how to update (rebase) your branch to include newest changes by others
* You know how to resolve merge conflicts

## Git configurations

In order to work efficiently with the branching model, you need to have following git configurations set. Open command-line and type:

```
git config --global push.default current
git config --global branch.autosetuprebase always
```

Now you’re ready to start.

# Let the tutorial begin

Let's get in to action!

### 1. Fork

Go to the tutorial repository in Github, you can find it from https://github.com/sharetribe/just-enough-git. Click "Fork" on the upper right corner. This creates a copy of this tutorial in which you can try things out.

Now, clone the repository to your local machine. On command-line, type:

```
git clone git@github.com:sharetribe/just-enough-git.git
cd just-enough-git
ls
```

You should now see the content of the repository. There are not that many files, but the one we are interested in is the `index.html` file. Open it in your browser to view the content.

### 2. Let's add description!

Now let's add a description to our website! Do you still remember the branching model? The first thing to do, was **Branch**. So, let's create a new branch.

First, type:

```
git status
```

From the first line, you see that you are currently on `master` branch.

`git checkout` is a command to jumps from branch to another. If you give it an option -b, it will create a new branch. Let’s try this. Type:

```
git branch -b add-description
git status
```

Congrats! You created a branch. `git status` shows you that you are currently in `add-description` branch.

Let’s make a change to index.html. You can see that there’s a paragraph with class description, but the paragraph doesn’t have any content. Let’s add a description, so that the result is

```
<p class=“paragraph”>This is how you branch like a rockstar!</p>
```

Next, use add, commit and push to save and push your changes.

```
git add index.html
git commit
git push
```

Now, go to your repository at Github. You can see that Github is suggesting you to make a Pull Request.

Hit the Compare and create a Pull Request button and create you first Pull Request.

The repository currently looks like this:

Now, go to Github to your Pull Request and click Merge pull request. Now you branch got merged to master. The repository now looks like this:

Your changes has now been merged to master, so you don't need your branch anymore. On the Pull Request page, click Delete branch.

Now back to command-line. Jump back to master branch.

```
git checkout master
```

If you now take a look at `index.html` you can see that your latest changes are not there yet. That's because locally your `add-description` branch and `master` are still separated. We only did the merge on Github. Thus, you need to pull the newest changes from Github.

```
git pull
```

Now you can see your changes in place.

Let's recap what we've learned so far:

* `git checkout -b [branchname]` creates a new branch
* `git checkout [branchname]` jumps to another branch
* In Github, you know how to create a Pull Request from your branch and how to merge it

From the five step branching model, you already master 3 steps: **Branch**, **Pull request** and **Merge**. Next we learn how to **Rebase**.


### Updating your branch

While you are working in your own branch, others might be pushing their changes to master branch. It's important to update your branch to include these changes. That way you avoid having a huge merge hell after working one week in isolation with your own branch.

Now, let's create two new branches, `edit-description` and `edit-title` from the master branch:

```
git checkout -b edit-description
```

Go and edit the `index.html` file, so that the description block looks like following:

```html
<p class="paragraph">This is how you rebase like a rockstar!</p>
```

When your ready, push the changes to Github.

```
git add index.html
git commit
git push
```

Go to Github and make a new Pull request, but do not merge it this time.

Now back to the command-line. At the moment you are in `edit-description` branch. Jump back to master:

```bash
git checkout master
```

Now create the other branch:

```bash
git checkout -b edit-title
```

Edit the `index.html` so that the title is now:

```html
<h1>Welcome to interactive Git and Github tutorial</h1>
```

When your ready, push the changes to Github.

```
git add index.html
git commit
git push
```

Go to Github and make a new Pull request, but do not merge it this time.

Let's recap what we just did: We created two branches, `edit-description` and `edit-title`. We did Pull request for both of them. The repository currently looks like this:

Now go to Github and merge the `edit-title` Pull request.

On command-line, go to master and pull the changes

```
git chechkout master
git pull
```

Now the repository looks like this:

Now the `edit-description` branch is lagging behind. There are new changes in master, that are not included in `edit-description` branch. It's time to rebase.

Rebasing means that we change the "base" of the branch. Let's try it in practice.

First, go to the branch that you want to update:

```bash
git checkout edit-description
```

Then, rebase it on top of the master branch.

```bash
git rebase master
```

Now the repository looks like this:

The `edit-description` has now a new base. It has been placed on top of the latest changes on the master branch. Open the `index.html` file. You can see that the title includes now `and Github`, the change that was made in the `edit-title` branch.

Now, let's push the updated branch to Github. This time we have to force push the branch, because it's history has changed:

```
git push --force
```

**WARNING!** The main purpose of Git is to make sure you never lose any changed you've made. It's damn difficult to screw up things with Git so badly, that someone just lost his day's work. However, `--force` is the only command in Git which allows you to make catastrophical screw ups. So, be careful. Make sure you are in a correct branch when you force push. **NEVER force push on master branch.** **NEVER FORCE PUSH ON MASTER BRANCH**.

**WARNING #2!** You ONLY need to force push after rebase. **Never force push, unless you have just rebased**

Let's recap again:

* We have now learned how to `git rebase` your branch to include latest changes from master

Sometimes two people edit the exactly same file and same line at the same time. In this situation, git doesn't know which one of the changes it should include. This causes so called merge conflict. When you update your branch, a conflict may happen. Next you'll learn how to solve a conflict.

### Resolving conflicts

Git is pretty good at merging branches together. However, if two people edit the same part of the same file at the same time, Git doesn't know how to handle it. In this case, you have to manually resolve the conflict.

Let's try this out.

Remember that you have now an unmerged branch `edit-description`, which changed the `index.html` file description. Let's create yet another branch in which we edit the exact same file and line.

```bash
git checkout -b resolve-like-rockstar
```

Go and edit the `index.html` file, so that the description block looks like following:

```html
<p class="paragraph">This is how you resolve conflicts like a rockstar!</p>
```

When your ready, push the changes to Github.

```
git add index.html
git commit
git push
```

Go to Github and make a new Pull request and merge it immediately.

Now go to master and pull changes

```
git checkout master
git pull
```

Your repository looks now like this:

Yet again, we see that there's one branch, `edit-description` lagging behing. Let's rebase it.

```bash
git checkout edit-description
git rebase master
```

Whou! That did not go smoothly. Git is saying that there's a conflict in `index.html` file:

```
CONFLICT (content): Merge conflict in index.html
```

You can also see the situation with status command, type:

```bash
git status
```

Which prints:

```

...

Unmerged paths:
  (use "git reset HEAD <file>..." to unstage)
  (use "git add <file>..." to mark resolution)

    both modified:   index.html

no changes added to commit (use "git add" and/or "git commit -a")
```

Open the `index.html` file in your editor and you will see this:

```html
<<<<<<< HEAD
<p class="description">This is how you resolve conflicts like a rockstar!</p>
=======
<p class="description">This is how you rebase like a rockstar!</p>
>>>>>>> [your commit message]
```

Edit the file so that it include both changes and remove lines starting with `<<<<<<<`, `=======` and `>>>>>>>` so that the end result is:

```html
<p class="paragraph">This is how you rebase and resolve conflicts like a rockstar!</p>
```

Save the file. Now that you've resolved the conflict, you have to mark the file as resolved. Do this with `git add`:

```bash
git add index.html
```

Do this for every file that has conflicts.

After you have resolved all the conflicts, `git status` should not show you any `Unmerged paths`:

```bash
git status
```

Now that we are done, we can continue:

```bash
git rebase --continue
```

Now Git moves to the next commit. You may have to resolve some merge conflicts again.

If anything goes wrong during the rebase, you can always enter `git rebase --abort`. It will cancel the whole rebase process.

Now it's time to be extra careful again, and force push our changes.

```bash
git push --force
```

And that's it! Now you have resolved a merge conflict. `edit-description` is now ready to be merged to master. Your repository looks now like this:

### Final recap

You have now learned all the necessary steps: Branch, Pull request, Rebase, Review (just ask someone to take a look at your code) and Merge.

Here's a list of commands you need when working with branches:

* `git checkout [branchname]`: Jump from a branch to another
* `git checkout -b [branchname]`: Create a new branch
* `git rebase master`: Rebase your branch on top of master branch
* `git add [filename]`: Mark conflicted file as resolved
* `git rebase --continue`: Continue rebasing after resolving conflicts
* `git rebase --abort`: Abort rebasing

And that's it! With these commands, you should be able to work with the development team and git!
