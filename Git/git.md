<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [Git Commands](#git-commands)
  - [Change repo to SSH](#change-repo-to-ssh)
  - [Create a Remote Repository](#create-a-remote-repository)
  - [Create GitHub repository](#create-github-repository)
  - [Clone](#clone)
  - [Connect to the Original Repository:](#connect-to-the-original-repository)
    - [cloning @Spiced](#cloning-spiced)
  - [Branches](#branches)
  - [Remotes](#remotes)
  - [Pull Requests](#pull-requests)
  - [Rebase](#rebase)
  - [Merge](#merge)
  - [Tagging](#tagging)
  - [Go back in Git-history](#go-back-in-git-history)
  - [Restore last commit](#restore-last-commit)
  - [See differences](#see-differences)
  - [Stashing](#stashing)
  - [Undo all committed changes](#undo-all-committed-changes)
  - [Get a nice view](#get-a-nice-view)
  - [Useful commands](#useful-commands)
    - [merging / `git pull main`](#merging--git-pull-main)
    - [remove previously committed file which is now in `.gitignore`](#remove-previously-committed-file-which-is-now-in-gitignore)
    - [update local file strucure (on computer) with repo on GitHub](#update-local-file-strucure-on-computer-with-repo-on-github)
  - [Forks](#forks)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Git Commands

Verify Git (version)

```
$ git --version
```

Set your name:

```
$ git config --global user.name "<Your Name>"
```

Set your email:

```
$ git config --global user.email "<youremail@example.com>"
```

<br>

## Change repo to SSH

1.  List your existing remotes in order to get the name of the remote you want to change.

    ```shell
    $ git remote -v
    > origin  https://github.com/USERNAME/REPOSITORY.git (fetch)
    > origin  https://github.com/USERNAME/REPOSITORY.git (push)
    ```

2.  Change your remote's URL from HTTPS to SSH with the `git remote set-url` command.

    ```shell
    $ git remote set-url origin git@github.com:USERNAME/REPOSITORY.git
    ```

3.  Verify that the remote URL has changed.

    ```shell
    $ git remote -v
    # Verify new remote URL
    > origin  git@github.com:USERNAME/REPOSITORY.git (fetch)
    > origin  git@github.com:USERNAME/REPOSITORY.git (push)
    ```

<br>

## Create a Remote Repository

You want to sync your **local** version with one stored on GitHub.com called the **remote** version. So first create an empty remote repository on GitHub.com.

- Go to [github.com](http://github.com/), log in, and click the '+' in the top right to create a new repository.
- Give it a name that matches your local repository's name, 'hello-world', and a short description.
- Make it public.
- Don't initialize with a README because we already have a file, locally, named `readme.txt`.
- Leave `.gitignore` and `license` on `none`.
- Click `create repository`!

Now you've got an empty repository started on GitHub.com. At the top you'll see `Quick Setup`, make sure the `HTTP` (or `SSH`) button is selected and copy the address — this is the location (address) of your repository on GitHub's servers.

Back in your terminal, and inside of the `hello-world` folder that you initialized as a Git repository in the earlier challenge, you want to tell Git the location of the remote version on GitHub's servers. You can have multiple remotes so each requires a name. For your main one, this is commonly named `origin`.

```
git remote add origin git@github.com:andy-9/hello-world.git
git branch -M main
git push -u origin main
```

Your **local** repository now knows where your **remote** one named 'origin' lives on GitHub's servers. Think of it as adding a name and address on speed dial — now when you need to send something there, you can.

If you have **GitHub for Windows** on your computer, a remote named `origin` is automatically created. In that case, you'll just need to tell it what URL to associate with origin. Use this command instead of the `add` one above:

```
$ git remote set-url origin <URLFROMGITHUB>
```

## Create GitHub repository

Initializing empty git repository:

```
git init
```

Connect folder to github repository:

```
git remote add origin [https://github.com/andy-9/portfolio.git]
```

Check status

```
$ git status
```

Add file to commit (aka save):

```
git add readme.txt
```

To add all files changes

```
$ git add .
```

Commit those changes to the repository's history with a short description of the updates:

```
git commit -m "<your commit message>"
git commit --amend -m "Updated readme and added new section"  //
edits the previous commit to include additional changes with a new message
git commit --no-edit --amend  // updates the last commit without changing the commit message
```

Push

```
git push [origin] [master]   (remote) (branch)
git push origin andy -f		the `-f` forces the pushing
```

Pull

```
git fetch origin main  // download data locally, no merging
git pull origin main  // pull and merge
```

## Clone

```
git clone	// establishes connection between the repository on github and my computer
```

Clone a remote branch to your local computer. This is what you use if you want to copy code from github onto your computer:

```
$ git clone <URLFROMGITHUB>
```

Go into the folder for the fork it created (in this case, named 'patchwork')

```
$ cd patchwork
```

<br>

## Connect to the Original Repository:

```
$ git remote add upstream https://github.com/jlord/patchwork.git
```

Add remote connections:

```
$ git remote add
```

```
$ git remote add <REMOTENAME> <URL>
```

### cloning @Spiced

cd into directory where you want to store your project

```
git clone https://github.com/spicedacademy/msg-socialnetwork.git // (or other link!)
git checkout -b andy origin/master // (andy = name-of-branch)
npm install
```

<br>

## Branches

Git repositories use branches to isolate work when needed. It's common practice when working on a project or with others on a project to create a **branch** to keep your changes in until they are ready. This way you can do your work while the main, commonly named 'main', branch stays stable. When your branch is ready, you merge it back into 'main'.

GitHub will automatically serve and host static website files in branches named 'gh-pages'. Since the project you forked creates a website, its main branch is 'gh-pages' instead of 'master'. All sites like this can be found using this pattern for the URL:

```
http://githubusername.github.io/repositoryname
```

HEAD is a pointer to the local branch you're currently on.

See branches:

```
$ git branch
```

Create and switch to this branch in one line:

```
$ git checkout [branch name]
```

Create new branch (if it doesn't exist yet):

```
git checkout -b [branch name]
```

Establish new branch and automatically have it set to pull from main

```
git checkout -b [name] origin/main
```

Rename a branch you're currently on:

```
$ git branch -m
```

Verify what branch you're working on

```
$ git status
```

Merge a branch into current branch

```
git merge <BRANCHNAME>
```

Delete a local branch (you have to be in another branch to do this!)

```
git branch -d [branch]
```

Delete a remote branch

```
git push <REMOTENAME> --delete <BRANCHNAME>
```

## Remotes

See remotes

```
git remote
```

See full URL

```
git remote -v
```

Pull in changes from a remote branch:

```
$ git pull <REMOTENAME> <REMOTEBRANCH>
```

See changes to the remote before you pull in:

```
$ git fetch --dry-run
```

Rename remote:

```
git remote rename [bisheriger Name] [neuer Name]
```

Set a URL to a remote:

```
$ git remote set-url <REMOTENAME> <URL>
```

Push changes:

```
$ git push <REMOTENAME> <BRANCH>
```

## Pull Requests

Visit the original repository you forked on GitHub, in this case [http://github.com/jlord/patchwork](https://github.com/jlord/patchwork).

Often GitHub will detect when you've pushed a branch to a fork, and display a 'Compare & pull request' button at the top of both the original and forked repositories. If you see this for your 'add-username' branch, you can click it to continue. If not:

- Go to your forked repository.
- Click 'Pull requests' on the right-side menu, then 'New pull request' at the top.
- Select the branch with the changes you want to submit. **It should be the one with 'add-yourusername'**.
- Click 'Create pull request' at the top.

- Add a title and description to the changes you're suggesting the original author pull in.
- Click 'Send pull request'!

## Rebase

- lokalen branch committen
- auf main wechseln
- git pull
- auf feature branch wechseln
- git rebase main
- Ggf. Konflikt lösen
- git rebase --continue
- (ggf. git add)
- git push --force

Geht manchmal auch:

- git pull origin main
- git rebase
- wenn kein Konflikt: git push --force

Oder:

- Auf feature-branch: git fetch
- git rebase origin/main
- Ggf. Konflikt lösen
- git rebase --continue
- git push --force

## Merge

On feature branch:

- "git merge main"
- Ggf. resolve conflicts
- git commit -am "<commit_message>"
- git push

## Tagging

- Create annotated tag: `git tag -a <tagname> -m '<commit-message>'`
  e.g. `git tag -a v2.5.0-pipelines_master-none-library -m "upgrading master none library to v2.5.0` or: `git tag -a v3.2.0 -m "added new attribute camera_mounting_position_in_cora"` (annotated tags store metadata such as tagger name, email, date)
- Create tag: `git tag <tagname>`
- List stored tags: `git tag`
- Checking out tags: `git checkout <tagname>`
  • Find the tags matching HEAD: `git describe HEAD --tags --exact-match`
- See tag data: `git show <tagname>`
- Tagging after merged PR:
  ```
  cd into repository locally
  switch to main branch
  git pull
  git tag -a v<new_version> -m "<message_on_what_was_done>"
  push origin v<new_version>
  ```
- Delete a local tag: `git tag -d <tagname>`
- Delete a remote tag: `git push origin --delete <tagname>`

## Go back in Git-history

```
git log
git checkout [id von commit] // localhost zeigt commit von damals

git log --oneline // zeigt alles in einer zeile an

git log –p  // shows the commit history along with the diff for each commit

git diff [name]  // compare branches

gid diff --stat [name]  // dateiname mit veränderung der zeilen vergleich
```

<br>

## Restore last commit

```
git reset .
git clean -df
git checkout -- .
```

<br>

## See differences

In terminal, you can view the difference between the file now and how it was at your last commit.

```
git diff
git log					see commits
git reflog
```

The git log command only shows us the history, but `reflog` shows everything that we have done in the repo. It is very useful for accessing the full history of Git commands. It can be a lifesaver when trying to recover lost work or troubleshoot complex issues.

<br>

## Stashing

```
git stash					nicht commitete lokale Änderungen für später merken
git stash pop				die letzte dieser änderungen wieder hervorholen
```

<br>

## Undo all committed changes

```
git reset --hard
```

<br>

## Get a nice view

of all commits and history of repo (this command is frequently used!):

```
git log –-all -–graph –-decorate –-oneline
```

(it is like the view of GitExtentions)

<br>

## Useful commands

### merging / `git pull main`

To do: Wenn ich lokal (auf dem master branch) `git pull main` ohne Probleme ausführen kann, kann ich mergen und direkt nochmal `git pull main` ausführen und alles sollte in Butter sein.

### remove previously committed file which is now in `.gitignore`

```
git rm --cached /path/to/file
```

### update local file strucure (on computer) with repo on GitHub

```
git pull origin main
```

## Forks

When you **fork** a repository, you're creating a copy of it on your GitHub account. Your fork begins its life as a **remote** repository. Forks are used for creating your own version of a project or contributing back fixes or features to the original project.

Once a project is forked, you then **clone** (aka copy) it from GitHub to your computer to work on locally.

<br>
