
---

# Working with git on a local repository

- You want to use git for version control, but you are using it just for yourself for your own project. You are not uploading anything to a remote repository like GitHub.
- If this is the first time you are setting up the repository, then create a new git repository in the current directory:
```sh
git init
touch .gitignore
```
- edit `.gitignore` to include glob-style paths of files and folders you want ignored (such as `*.pyc` or `.DS_Store`)
- If the repository already exists and you are going to be doing work on it today, checkout a new branch and switch to it if necessary:
```sh
git checkout {branchname} # the branch exists
git checkout -b {branchname} # the branch does not yet exist
```
- If you don't remember what branches exist or what they're called, use
```sh
git branch
```
to list them all
- Do some work (add/delete files, modify files, etc)
- When you want to see the difference between the **working directory** and the **staging area**, use
```sh
git diff
```
- When you've made enough changes and want git to track a 'snapshot' of the changes, add them to the **staging area**
```sh
git add file1 file2 file3 # add specific files
git add -A # add everything ("All")
```
- If the current staging area represents a 'version' of the code that you want to save to the git history, make a commit
```sh
git commit -m "{commit message}"
```
- If at any time you want to see what files are in the **working directory** but are untracked (not in the **staging area**), and see which files are in the **staging area**, use
```sh
git status
```
- If throughout the day you have made a few commits and want to see the history of those commits, use
```sh
git log
```
to see all commits on the commit history visible from the current branch.
- When you are done with your changes and are happy, merge the changes from the current branch into the master branch and delete the current branch.
```sh
git checkout master # switch to master branch
git merge {branchname} # merge changes into master
git branch -d {branchname} # delete the new branch
```
- Overall:
```sh
git checkout -b {branchname}
# do work
git add -A
git commit -m "{message}"
# repeat as many times as needed until branch is 'done'
git checkout master
git merge {branchname} # merge changes into master
git branch -d {branchname} # delete the new branch
```
- Getting in the habit of doing this even for local projects, presentations, etc. will help you get into the habit of using git. git is lightweight and fast so there's really no reason not to use it! And remember the git mantra:
> Branch early and branch often.

---

# Working with git on a remote repository
- *disclaimer: you may have to log into GitHub at some point during this process*.
- You want to use git for version control, and your code is also hosted somewhere on a remote repository. You are the sole contributor to the remote repository.
- **While you can use remote repositories this way for collaboration, the workflow becomes more complex to ensure branches are synchronized. For collaboration I suggest using the fork/pull workflow described below through GitHub.**
- If you are starting from scratch, create an empty repository on GitHub. Navigate to where you want the directory for that repository to live on your local machine and clone the URL:
```sh
cd {path/to/repo}
git clone https://github.com/{username}/{reponame}.git
cd {reponame}
```
- this process automatically makes `{path/to/repo/reponame}` a git repository, so you can use git commands here.
- it also automatically sets up an initial commit so the commit history has one node on it.
- it also adds an alias to the remote repository, as if you had done
```sh
git remote add origin https://github.com/{username}/{reponame}.git
```
where the syntax
```sh
git remote add {alias} {https://github.com/{username}/{reponame}.git}
```
just lets you use `alias` in place of a url when using `git remote` commands (below).

- if you are NOT starting from scratch and have an existing git repository on your local machine and have decided you want to start hosting it on a remote repository, then create an empty github repo. Do not initialize it with a README or license.
- then do
```sh
git remote add origin https://github.com/{username}/{reponame}.git
```
this sets the local git repository to track the remote repository called 'origin'.
- from here you should be able to do
```sh
git fetch -v
```
to see the remote repository you just added.
- once you have done this, you can just follow the guide to working on a local git repo. This work is just to make sure that your work starts from the same point as the remote repo.
- when you are done making changes in your local repo and have made commits, etc. (see guide on using git locally), you can push your changes to the remote repository:
```sh
git push origin {branchname}
```
- this pushes to the repo `origin`, which we made as an alias earlier for `https://github.com/{username}/{reponame}.git`. It pushes the current series of commits, if possible, to the branch `{branchname}`. If you are following the guide for local development, this could just be `git push origin master` if you are working from the master branch. It is good practice to push to a branch of the same name on the remote repository so that the remote commit histories and remote branches match the local ones. If you decide to push to a non-master branch, you can actually perform a `git merge` on GitHub on the remote repository.
- there are more steps you should follow if you are using a remote repository for collaboration. I haven't covered those here because that is not the collaboration use case we will be using in this lab. There are plenty of resources for further reading if you're interested (https://www.atlassian.com/git/tutorials/syncing).
- overall:
```sh
# do your local git stuff
git push origin master
```
To save yourself from having to retype your login and password, run the following line of code. After the next time you enter your credentials, it will remember them.
```sh
git config --global credential.helper store
```
---

# Fork/Pull and the GitHub Collaboration Model
- This is the model of git and version control we will use in scenarios where multiple people are collaborating on code.

## You are the owner of the remote repository
- You do not need for fork and clone a repo. We assume that you can pull from and push directly to the remote repository being collaborated on.
- If you haven't already, clone the repo and `cd` into it.
- Before beginning your work, make sure that your local master branch is up to date:
```sh
git fetch origin
git checkout master
git merge remotes/origin/master
```
- from here, checkout and/or create a new branch to do your work. When you are done, repeat:
```sh
git fetch origin
git checkout master
git merge remotes/origin/master
```
- then, if you want, you can commit your changes from your branch to the master branch:
```sh
git merge {branchname}
```
- if you have approved changes to the master branch since you began working but did not update your local repository to match, then your local master branch will not have the same history as the remote repository, which will contain additional commits. You need to merge these changes into your master branch before pushing to the remote repository.
- If this causes merge conflicts (you have made changes to the master branch in the meantime), resolve them before continuing. Once they are all resolved:
```sh
git push origin {branchname}
```
- if you are the owner and you are comfortable with it, you can push directly to `master`, or you can push to another branch and merge them later on GitHub.

## You are **not** the owner of the remote repository
- There is a public github repository and you want to propose changes to that repo.
- Begin by forking the repository (button near the top right). This makes a copy of their repository onto your github account. Now you own that copy of the repository and can make changes to it as you like, just like the section on remote repositories described above!
- Add a second remote repository, called `upstream`, to serve as the 'source' repository. This is kind of like `origin`, except that we can push/pull/fetch from `origin`, but cannot push to `upstream` (since we don't own it).
```sh
git remote add upstream https://github.com/{original-username}/{reponame}.git
```
this URL should point to the **original** remote repository, *not* your **forked** copy.
- The next steps are very similar to if you owned the original repository:
- Before beginning your work, make sure that your local master branch is up to date:
```sh
git fetch upstream
git checkout master
git merge remotes/upstream/master
```
- from here, checkout and/or create a new branch to do your work. When you are done, repeat:
```sh
git fetch upstream
git checkout master
git merge remotes/upstream/master
```
- then, if you want, you can commit your changes from your branch to the master branch:
```sh
git merge {branchname}
```
- all we have really done is made sure that our master branch is always in sync with the **original** repository's master branch, since changes may have been made since we have last updated it. By keeping it in sync, it will prevent merge conflicts, so the only changes proposed are the ones that we have added.
- once we are ready, we can push to **our** remote repository:
```sh
git push origin {branchname}
```
- from here, we can go to GitHub and navigate to the branch that we pushed to. We can then create a new pull request to the original owner of the repository, who can decide to merge our changes.
- If we need to make fixes to our proposed changes before they are accepted, all we have to do is push those changes to the same branch - GitHub's pull requets operate on **branches**, so any commits added to a remote branch's history will be reflected in the content of the pull request.

---

# Tips and tricks

- Try to become familiar with navigating your git history and switching branches using this online visualization tool (30-45 mins):
https://learngitbranching.js.org

- Read the URLs posted in this guide!

- If at any point during working you decide that you want to switch to another branch but do not want to lose your changes **and** don't want to save your current changes using a commit, use
```sh
git stash
```
- When you are ready to go back to that version of things, just call
```sh
git stash list
git stash pop {stashname}
```
where `git stash list` will list a bunch of stashes like `stash@{0}` which you can use to replace `{stashname}` above.
- If at any point during working you decide you want to revert back to a previous version of things, there are many ways to do this. The easiest is to use
```sh
git reset
```
- this comes with different options, but I **really** suggest you all read this short article which gives a great overview of most of how git works with the intention of explaining `git reset`. https://git-scm.com/book/en/v2/Git-Tools-Reset-Demystified
- The default option (`git reset --mixed`) basically undoes your last `git commit` and `git add` operations. It leaves the files in the working directory alone.
- You can 'unstage' a single file to the version from a previous commit by doing
```sh
git reset {filename}
```
- This is what `git status` suggests you do and you can kind of thing of it as being the opposite of `git add`.
- You can completely remove a file from the staging area using
```sh
git rm --cached {filename}
```

- Append the following to your `~/.bash_profile` to see the current branch in your terminal prompt:
```sh
# Git branch in prompt.
parse_git_branch() {
  git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/ (\1)/'
}
export PS1="\u@\h \W\[\033[32m\]\$(parse_git_branch)\[\033[00m\] $ "
```
