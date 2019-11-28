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

E.g. get the master (default) branch of the `origin` ('central') repository (EMM why it is called origin?):

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
TODO

Creating a new branch and switching to it:
```
git checkout -b <new mybranch>
git checkout -b <new mybranch> <from existing branch>

e.g. 

git checkout -b mybranch
git checkout -b mybranch master
```

Switch to an existing branch:
```
git checkout <mybranch>
```

Push your changes in `mybranch`, merge with `master`, and delete `mybranch`:

```
git add <changes in mybranch>
git commit -m "<message on changes in mybranch>"
git checkout master
git merge mybranch
git branch -d mybranch
```



See
- [https://de.atlassian.com/git/tutorials/using-branches/git-checkout](https://de.atlassian.com/git/tutorials/using-branches/git-checkout)
- [https://de.atlassian.com/git/tutorials/using-branches/git-merge](https://de.atlassian.com/git/tutorials/using-branches/git-merge)

## zurück zu HEAD

See [https://rogerdudler.github.io/git-guide/index.de.html](https://rogerdudler.github.io/git-guide/index.de.html)

Local changes can be reverted to the last global state with:

```
git checkout -- 
```

See more on `reset` and `revert` [https://de.atlassian.com/git/tutorials/undoing-changes](https://de.atlassian.com/git/tutorials/undoing-changes)


## conflicts
TODO

See
- [https://de.atlassian.com/git/tutorials/using-branches/merge-conflicts](https://de.atlassian.com/git/tutorials/using-branches/merge-conflicts)


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


## Setup the project, and pay attention to

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

