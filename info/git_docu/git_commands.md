This file collects (hopefully) useful git commands.

Don't hesitate to extend and/or correct it. 

# Info

- Overview: [https://git-scm.com/doc](https://git-scm.com/doc)
    - Official tutorial: [https://git-scm.com/docs/gittutorial](https://git-scm.com/docs/gittutorial)
    - Book: [https://git-scm.com/book/](https://git-scm.com/book/) -- as PDF: [https://github.com/progit/progit2/releases/download/2.1.93/progit.pdf](https://github.com/progit/progit2/releases/download/2.1.93/progit.pdf)
    - Reference manual: [https://git-scm.com/docs](https://git-scm.com/docs)
- Heidelberg GitLab-Help: [https://gitlab.cl.uni-heidelberg.de/help/gitlab-basics/command-line-commands.md](https://gitlab.cl.uni-heidelberg.de/help/gitlab-basics/command-line-commands.md)
- Useful tutorials and overviews:
    - [https://de.atlassian.com/git/tutorials/setting-up-a-repository](https://de.atlassian.com/git/tutorials/setting-up-a-repository)
    - [https://rogerdudler.github.io/git-guide/index.de.html](https://rogerdudler.github.io/git-guide/index.de.html)
- Cheat sheats:
    - [http://ndpsoftware.com/git-cheatsheet.html](http://ndpsoftware.com/git-cheatsheet.html)
    - [https://services.github.com/on-demand/downloads/github-git-cheat-sheet.pdf](https://services.github.com/on-demand/downloads/github-git-cheat-sheet.pdf)

# Important commands

## get a working copy of an existing repository
```
git clone <url>
cd <projectfolder>

# optional
git checkout <branch>
```

E.g.
```
git clone https://mujdricz@gitlab.cl.uni-heidelberg.de/mujdricza/eml-ws18_sandbox.git
cd eml-ws18_sandbox

# optional
git checkout master
```

I prefer to use `https` for cloning and to give the *user name* with the url (`mujdricz` in my case).

Note that I renamed my home folder on gitlab to `mujdricza` (see in the path of the url), but this is just an alias name (i.e. "Full name" in GitLab's Settings).

With the `checkout` command, we switched to the main branch called `master`. This step is optional, since the `master` branch is the default one.

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
TODO

```
git status 
```

Checks whether there are not uploaded (not synchronized) changes in your local repository.

## get an update
```
git pull <remote> <branch>
```

E.g. get the master (default) branch of the `origin` ('central') repository:

```
git pull origin master

# origin master implicitly given if you just use:
git pull
```

Do this regularly before you change anything to make sure you have the last version of the repository.

See also [https://de.atlassian.com/git/tutorials/syncing/git-fetch](https://de.atlassian.com/git/tutorials/syncing/git-fetch) for alternatives.
- _When downloading content from a remote repo, git pull and git fetch commands are available to accomplish the task. You can consider git fetch the 'safe' version of the two commands. It will download the remote content but not update your local repo's working state, leaving your current work intact. git pull is the more aggressive alternative, it will download the remote content for the active local branch and immediately execute git merge to create a merge commit for the new remote content. If you have pending changes in progress this will cause conflicts and kickoff the merge conflict resolution flow._ 
- _synchronizing your local repository with a remote repository is actually a two-step process: fetch, then merge. The git pull command is a convenient shortcut for this process._


## commit a change


```
git add <what-to-add multiple file/path names possible> 
git commit -m "<message on changes>"
git push <remote> <branch>
```

E.g. 
``` 
git add .
git add README.md
git add src/ inputs/


git commit -m "readme added"
git commit -m "update src and input folders"
git commit -m "issue with reading irregular files solved"

git push origin master

# origin master implicitly given:
git push
``` 

Note that some regular-expression-like syntax is allowed. `git add .` adds and updates all changes.

See more on RE with git [here TODO](TODO).


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
Check for existing branches:
```
git branch
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


Read more:
- [https://de.atlassian.com/git/tutorials/using-branches/git-checkout](https://de.atlassian.com/git/tutorials/using-branches/git-checkout)
- [https://de.atlassian.com/git/tutorials/using-branches/git-merge](https://de.atlassian.com/git/tutorials/using-branches/git-merge)
- [https://git-scm.com/book/en/v2/Appendix-C%3A-Git-Commands-Branching-and-Merging](https://git-scm.com/book/en/v2/Appendix-C%3A-Git-Commands-Branching-and-Merging)

## zurück zu HEAD

See [https://rogerdudler.github.io/git-guide/index.de.html](https://rogerdudler.github.io/git-guide/index.de.html)

Local changes can be reverted to the last global state with:

```
git checkout -- 
```

Read more 
- on `reset` and `revert` [https://de.atlassian.com/git/tutorials/undoing-changes](https://de.atlassian.com/git/tutorials/undoing-changes)


## conflicts
TODO

See
- [https://de.atlassian.com/git/tutorials/using-branches/merge-conflicts](https://de.atlassian.com/git/tutorials/using-branches/merge-conflicts)


## gitignore

The repository-specific option to ignore specific files or folders is to create a file named `.gitignore` in the root folder of the repository.

TODO
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


## Pay attention to

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

The project-specific `.git/config` file could look like this:
```
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
	ignorecase = true
	precomposeunicode = true
[remote "origin"]
	url = https://mujdricz@gitlab.cl.uni-heidelberg.de/mujdricza/eml-ws18_sandbox.git
	fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
	remote = origin
	merge = refs/heads/master
```

There are several other options for customizing the configuration, see e.g. [https://git-scm.com/docs/git-config](https://git-scm.com/docs/git-config).


## Auto DevOps and Pipelines

Recommendations on Auto DevOps and Pipelines
- Deactivate Auto DevOps if not already deactivated (direct at the main view of the project; or in Settings)
- Deactivate the Pipelines if not already deactivated (Settings / General / Permissions / Pipelines: deactivate it)



# Contact

If you have problems or questions to your project, please contact [me](mailto:mujdricza@cl.uni-heidelberg.de).

