# Info

- [https://git-scm.com/book/](https://git-scm.com/book/) -- as PDF: [https://github.com/progit/progit2/releases/download/2.1.93/progit.pdf](https://github.com/progit/progit2/releases/download/2.1.93/progit.pdf)
- [https://de.atlassian.com/git/tutorials/setting-up-a-repository](https://de.atlassian.com/git/tutorials/setting-up-a-repository)
- [https://gitlab.cl.uni-heidelberg.de/help/gitlab-basics/command-line-commands.md](https://gitlab.cl.uni-heidelberg.de/help/gitlab-basics/command-line-commands.md)
- [https://rogerdudler.github.io/git-guide/index.de.html](https://rogerdudler.github.io/git-guide/index.de.html)

# Important commands

## get a working copy of an existing repository
```
git clone <url>
cd <projectfolder>
git checkout <branch>
```

E.g.
```
git clone https://mujdricz@gitlab.cl.uni-heidelberg.de/mujdricza/myproject.git
cd myproject
git checkout master
```

I prefer to use `https` for cloning and to give the user name with the url (`mujdricz` in my case).

Note that I renamed my home folder on gitlab to `mujdricza` (see in the path of the url), but this is just an alias name.

With the `checkout` command, we switched to the main branch called `master`. This step is optional (EMM you are on origin/master already as default (?)).

## ... or, make a new repository
```
cd <to-the-not-yet-repository-folder>
git init
```

This command creates the necessary files for a repository in the subfolder `.git`.
An important one is `.git/config` for the configuration of the repository.

Note that it does not create any `.gitignore` file.

(This repository is not connected to a `remote` yet -- learn more on 
[https://de.atlassian.com/git/tutorials/setting-up-a-repository](https://de.atlassian.com/git/tutorials/setting-up-a-repository)
, [https://de.atlassian.com/git/tutorials/syncing#git-remote](https://de.atlassian.com/git/tutorials/syncing#git-remote))

## check the status

```
git status 
```

checks whether there are changes since your last activity on the repository.

## get an update
```
git pull <remote> <branch>
```

E.g. get the master (default) branch of the `origin` ('central') repository:

```
git pull origin master

# origin master implicitly given:
git pull
```

Do this regularly before you change anything to make sure you have the last version of the repository.

See also [https://de.atlassian.com/git/tutorials/syncing/git-fetch](https://de.atlassian.com/git/tutorials/syncing/git-fetch) for alternatives.
- _When downloading content from a remote repo, git pull and git fetch commands are available to accomplish the task. You can consider git fetch the 'safe' version of the two commands. It will download the remote content but not update your local repo's working state, leaving your current work intact. git pull is the more aggressive alternative, it will download the remote content for the active local branch and immediately execute git merge to create a merge commit for the new remote content. If you have pending changes in progress this will cause conflicts and kickoff the merge conflict resolution flow._ 
- _synchronizing your local repository with a remote repository is actually a two-step process: fetch, then merge. The git pull command is a convenient shortcut for this process._


## commit a change


```
git add <what-to-add multiple file/path names posible> 
git commit -m "<message on changes>"
git push <remote> <branch>
```

E.g. 
``` 
git add .
git add README.md

git commit -m "readme added"

git push origin master

# origin master implicitly given:
git push
``` 

Note that some regular-expression-like syntax is allowed. `git add .` adds and updates all changes.


## branches

Show branches
```
git branch  # local
git branch --remote  # remote
git branch -r
git branch --all  # local and remote
git branch -a 
```

Creating a new branch and switching to it:
```
git checkout -b <new mybranch>
git checkout -b <new mybranch> <from existing branch>

e.g. 

git checkout -b mybranch
git checkout -b mybranch master
```

Checkout an existing remote branch:
```
git checkout --track origin/theirbranch

e.g.

git checkout --track origin/newsletter
```
Branch newsletter set up to track remote branch newsletter from origin.
Switched to a new branch 'newsletter'

Based on the remote branch "origin/newsletter", we now have a new local branch named "newsletter".


Switch to an existing branch:
```
git checkout <mybranch>
```

Push your changes in `mybranch`, merge with `master`, and delete `mybranch`:

```
git add <changes in mybranch>
git commit -m "<message on changes in mybranch>"
git push

git checkout master
git merge mybranch
git branch -d mybranch  # deleting the local branch
```

Delete (also a merged) branch on remote (= origin): https://stackoverflow.com/questions/2003505/how-do-i-delete-a-git-branch-locally-and-remotely
```
git push origin --delete mybranch
git push origin -d mybranch
```

<https://devconnected.com/how-to-set-upstream-branch-on-git/>
```
git push -u origin mybranch
(the same as)
git push --set-upstream origin mybranch
```

To create the remote branch `mybranch` in one step with the first push of the local branch `mybranch`:
```
$ git push --set-upstream origin mybranch:refs/heads/mybranch
$ git push --set-upstream origin mybranch
```

How do I rename a local and remote branch?

```
git branch -m oldname newname
git push origin :oldname newname
git branch --set-upstream-to=origin/newname newname
```

How do I rename a local branch whose remote tracking branch has changed its name?

```
git branch -m oldname newname
git branch --set-upstream-to=origin/newname newname
```


See
- [https://de.atlassian.com/git/tutorials/using-branches/git-checkout](https://de.atlassian.com/git/tutorials/using-branches/git-checkout)
- [https://de.atlassian.com/git/tutorials/using-branches/git-merge](https://de.atlassian.com/git/tutorials/using-branches/git-merge)

## zurück zu HEAD

See [https://rogerdudler.github.io/git-guide/index.de.html](https://rogerdudler.github.io/git-guide/index.de.html)

Local changes can be reverted to the last global state with:

```
git checkout -- .
```

See more on `reset` and `revert` [https://de.atlassian.com/git/tutorials/undoing-changes](https://de.atlassian.com/git/tutorials/undoing-changes)


## conflicts
TODO

See
- [https://de.atlassian.com/git/tutorials/using-branches/merge-conflicts](https://de.atlassian.com/git/tutorials/using-branches/merge-conflicts)

e.g.: "Reset" local changes to the state before the changes to make a new pull possible:
<https://stackoverflow.com/questions/1125968/how-do-i-force-git-pull-to-overwrite-local-files>;
<https://stackoverflow.com/questions/3639342/whats-the-difference-between-git-reset-and-git-checkout>

```
git reset --hard HEAD
git pull
```

To reset only one file:

`git checkout HEAD -- my-file.txt`


- Another recommendation (this works): <https://www.freecodecamp.org/forum/t/git-pull-how-to-override-local-files-with-git-pull/13216>

```
git fetch --all
git reset --hard origin/master   # <remote>/<branch>

e.g.:
eva@laptop:/mnt/d/uni/GSRL/github/srl4srl-results$ git fetch --all
Fetching origin
Password for 'https://mujdricza@github.com':
eva@laptop:/mnt/d/uni/GSRL/github/srl4srl-results$ git reset --hard origin/master
Checking out files: 100% (903/903), done.
HEAD is now at a795dfd 20200128
eva@laptop:/mnt/d/uni/GSRL/github/srl4srl-results$
```

## gitignore

The repository-specific option to ignore specific files or folders is to create a file named `.gitignore` in the root folder of the repository.

The content could be like this [https://gitlab.cl.uni-heidelberg.de/mujdricza/git-template/blob/master/.gitignore](https://gitlab.cl.uni-heidelberg.de/mujdricza/git-template/blob/master/.gitignore)


### ignore already tracked content

The following description is adapted from [https://stackoverflow.com/questions/1470572/ignoring-any-bin-directory-on-a-git-project](https://stackoverflow.com/questions/1470572/ignoring-any-bin-directory-on-a-git-project):

1. Step 1: Add file and folder names to be ignored to the file `.gitignore`.
  This file is not part of the initial state, but should be created first in the root folder of the repository.
  ```
  # ignore file temp.txt
  temp.txt
  
  # ignore folder temp/
  temp/
  
  # ignore all files ending with "_old" 
  *_old
  ```
  
2. Step 2: Make sure take effect

If the issue still exists, that's because settings in .gitignore can only ignore files that were originally not tracked. 
If some files have already been included in the version control system, then modifying .gitignore is not enough.
Execute a folder remove (rm) from index only (--cached) recursivelly (-r). 
Command line example for root bin folder:

```
$ git rm -r --cached .
$ git add .
$ git commit -m 'Update .gitignore'
```

- Remove tracked: for only a file or folder:

To stop tracking a file you need to remove it from the index. This can be achieved with this command.

```
git rm --cached <file>
```

If you want to remove a whole folder, you need to remove all files in it recursively.

```
git rm -r --cached <folder>
```

The removal of the file from the head revision will happen on the next commit.

WARNING: While this will not remove the physical file from your local, it will remove the files from other developers machines on next git pull.

## stash

If you would like to keep your local changes, but in the same time you need a `pull`:

```
git stash 
--> put your local changes on the side
git stash list
--> check the current stash list: you will see your last changes under the name stash@{0} 
git pull
--> getting the newest remote changes
git stash apply stash@{0}
--> apply your local changes to the new state
```

## revert

See e.g. [https://www.hostingadvice.com/how-to/git-rollback-commit/](https://www.hostingadvice.com/how-to/git-rollback-commit/)

Use `log` to find out the id of the commits:

```
git log
git log --online
```

Reverting already pushed commits:
```
Case 3: Reverting a Git Commit That Was Pushed

After you check out the remote repository, you can first use git revert and then push as usual:
	
git revert 1a890e7980283e348cde0444cabe709f6342a851
git push origin

```

Reverting local commits:
```
Case 1: Reverting a Single, Local Git Commit

Now let’s say since you just added a contact-us.htm file to your project, you’ve realized you don’t really need the about-us.htm file anymore.

You can revert to the time when you made that commit and keep all changes after that by doing the following:

git revert 1a890e7 

```

## Update branch with master status


https://docs.microsoft.com/en-us/azure/devops/repos/git/pulling?view=azure-devops&tabs=command-line#update-your-branch-with-the-latest-changes-from-master
https://docs.microsoft.com/en-us/azure/devops/repos/git/pulling?view=azure-devops&tabs=command-line#update-your-branch-with-the-latest-changes-from-main

To merge the latest changes from main into your branch, in this example named users/jamal/readme-fix, you can use the following commands:

```
git checkout mybranch
git pull origin master
...
git push
```

If there are any merge conflicts, git shows you after the pull. Resolve the merge commits before you continue. When you're ready to push your local commits, including your new merge commit, to the remote server, run git push.


# Setup the project, and pay attention to

## config

TODO

There are several possible configuration methods. The project-specific file `.git/config` is a collection of the configuration setup.
I used to extend the configuration with user name and e-mail address.
I can do this e.g. either in the project-specific file `.git/config` file (if you are working alone or with a specific user name), 
or in a global `.gitconfig` file in my local home repository like this:

```
#e.g. make a file ~/.gitconfig with the content

[user]
    name = mujdricz
    email = mujdricza@cl.uni-heidelberg.de
```

There are several other options, see e.g. [https://git-scm.com/docs/git-config](https://git-scm.com/docs/git-config).


## Auto DevOps and Pipelines

Recommendations on Auto DevOps and Pipelines
- Deactivate Auto DevOps (direct at the main view of the project; or in Settings ...(?))
- Deactivate the Pipelines (Settings / General / Permissions / Pipelines: deactivate it)


## Tag

Source: <https://git-scm.com/book/en/v2/Git-Basics-Tagging>

Like most VCSs, Git has the ability to tag specific points in a repository’s history as being important. Typically, people use this functionality to mark release points (v1.0, v2.0 and so on).

**Important**: By default, the `git push` command doesn’t transfer tags to remote servers. You will have to explicitly push tags to a shared server after you have created them. This process is just like sharing remote branches — you can run `git push origin <tagname>`.
  * If you have a lot of tags that you want to push up at once, you can also use the `--tags` option to the git push command. This will transfer all of your tags to the remote server that are not already there.

Git supports two types of tags: lightweight and annotated.

A lightweight tag is very much like a branch that doesn’t change — it’s just a pointer to a specific commit.

Annotated tags, however, are stored as full objects in the Git database. They’re checksummed; contain the tagger name, email, and date; have a tagging message; and can be signed and verified with GNU Privacy Guard (GPG). It’s generally recommended that you create annotated tags so you can have all this information; but if you want a temporary tag or for some reason don’t want to keep the other information, lightweight tags are available too.


See tags: `git tag`, with possible RE-search: option `-l` or `--list`: `git tag -l "v1.8.5*"`

Annotated tags: option `-a`

```
git tag -a v1.4 -m "my version 1.4"

git show v1.4

git push origin v1.4

git push origin --tags
```

Tagging later:

To tag a previous commit, you specify the commit checksum (or the first part of it) at the end of the command:
```
git log --pretty=oneline

git tag -a v1.2 9fceb02

```



To delete a tag on your local repository, you can use `git tag -d <tagname>`. To delete the tag from remote server, use `git push origin --delete <tagname>`.

```
git tag -d v1.4-lw

git push origin --delete <tagname>
```


If you want to view the versions of files a tag is pointing to, you can do a `git checkout <tagname>` of that tag, although this puts your repository in “detached HEAD” state, which has some ill side effects.
To make a new branch with that tag: `git checkout -b <new-branch>`.


# Submodule

https://git-scm.com/docs/git-submodule

Install submodul: TODO check

`git submodule add <path-to-submodule.git>`

`git submodule update --init --recursive`



Install a specific commit for a submodule: e.g.
`pip install git+https://smarthub-wbench.wesp.telekom.net/gitlab/nlu-coli/miscaux.git@f1b56a46c8a1abe5e489a2f50870e4ec92523947#egg=dataset&subdirectory=dataset`

Update submodule

`git submodule foreach git pull origin master`

* If a submodule has submodules: `git submodule foreach --recursive git pull origin master`

Also: `git submodule update --remote --merge`

See: https://stackoverflow.com/questions/5828324/update-git-submodule-to-latest-commit-on-origin


Delete submodule: (https://www.atlassian.com/git/articles/core-concept-workflows-and-tips)

* Delete the relevant line from the `.gitmodules` file.
* Delete the relevant section from `.git/config`.
* Run `git rm --cached path_to_submodule` (no trailing slash).
* Commit and delete the now untracked submodule files.

`git rm -rf <submodulname>`



# Stash

https://www.freecodecamp.org/news/git-stash-explained/

If you have local changes which should not block a `pull`, and should be applied after a `pull` again.

Save local changes:
`git stash save "optional message"`

List all stash-status:
`git stash list`

Show a specific stash-status:
`git stash show -p stash@{0}`

Apply the stash-status: 
* applies the changes and leaves a copy in the stash:
`git stash apply STASH-NAME` 
  * apply a particular stash no from stash list: `git stash apply stash@{1}`

* applies the changes and removes the files from the stash:
  * a named status: `git stash pop STASH-NAME`

  * apply top of stash stack  `git stash pop` 


Delete Stashed Changes without applying them:

* one status: 
`git stash drop STASH-NAME`

* clear the entire stash:
`git stash clear`

* stash of submodules
`git submodule foreach 'git stash'`


# Worktree

* See 
  * https://smarthub-wbench.workbench.telekom.de/gitlab/alexander.blesius/git_worktree_tutorial
  * https://git-scm.com/docs/git-worktree
* Worktrees lets you check out more than one branch/commit at once (called a "working tree").
* Worktrees don't allow you to check out the same branch several times
   * You can checkout each branch including master, if there is no other worktree currently using it.
   * you'll get an error that the branch is already checked out at the other path (prevents you from accidentally overwriting work).

* list of worktrees
    ```
    git worktree list
    ```
* checkout a new branch or an existing one from remote
    ```
    git checkout -b <branch_name>
    or
    git checkout --track origin/<branch_name>
    ```
* create new worktree for the branch
    ```
    
    git checkout master  # to avoid conflicts with the branch <branch_name>
    git worktree add <path_to_repo_with_worktree> <branch_name>
    cd <path_to_repo_with_worktree>  # in this path, the branch <branch_name> is checked out
  ```
* remove worktree
   ```
   git worktree remove <path_to_repo_wit_worktree>
   ```

# Merge feature branch in another feature branch

* https://stackoverflow.com/questions/11582894/how-do-i-merge-another-developers-branch-into-mine


Let's say you are currently working on branch feature/feature_a and you want to merge the changes made in another branch called feature/feature_b to feature/feature_a. The following commands should do the trick:

```
git checkout feature/feature_b
git pull
git checkout feature/feature_a
git merge feature/feature_b
```

# Caching credentials

https://git-scm.com/docs/git-credential-cache

```
git config --global credential.helper cache
```
You can specify a one hour (=3600 seconds) timeout like this:
```
git config --global credential.helper 'cache --timeout=3600'
```
