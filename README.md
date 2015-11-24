## ⟢ GIT GEMS ⟣

_This is yet another "learn git by example" document. Enjoy the ride!_

<img src='images/git-icon.png' alt='Git logo' height='100px' />

__Table of Contents__

- [Basics](#basics)
  - [Delete a remote branch](#delete-a-remote-branch)
- [Advanced](#advanced)
  - [Changing the root commit](#changing-the-root-commit)
  - [Squash your branches](#squash-your-branches)
- [Github](#github)
  - [Fetch a single pull request](#fetch-a-single-pull-request)
  - [Fetch all pull requests](#fetch-all-pull-requests)
- [Aliases](#aliases)

## Basics

#### Delete a remote branch

Before git 1.7 you would simply use push with a refspec (_\<src>:\<dst>_) that has
an empty _\<src>_ field:

```
git push origin :mybranch
```

The new way is to use this instead:

```
git push origin --delete mybranch
```

Friendly reminder: This works for any ref, so not only branches, but also tags.

## Advanced

#### Changing the root commit

Consider this state:

```
$ git log --oneline
902ed06 third
5305bfc second
d41d64a init
```

If you want to change the commit from 3 commits ago, you would usually use `git
rebase -i HEAD~3`. But in this case it's the root commit and the command would
fail:

```
$ git rebase -i HEAD~3
fatal: Needed a single revision
invalid upstream HEAD~3
```

Use the following in these cases:

```
$ git rebase -i --root
```

`--root` usually takes a reference, but defaults to HEAD (like most git
commands) which means "rebase all commits from root up to HEAD".

#### Squash your branches

Often development happens in feature branches with multiple commits, but the
project's guidelines require them squashed into a single commit before merging
into master.

Now the usual approach would be to to `rebase -i` all new commits in the testing
branch and mark them with `s` to meld them into their parent commits.

Afterwards you have to rebase master onto the testing branch, because
development in master didn't stand still in the meantime.

Only then you can make a fast-forward merge into master (you don't want a merge
commit if you're merging only a single commit anyway).

But, usually it's way easier if you have commit rights for master. Then you can
simply use:

```
git merge --squash testing
git commit
```

This will create a single new commit on top of your master that contains all new
changes from the testing branch without creating a merge commit.

Another small advantage is that your testing branch is still split up into
multiple commits, since it was never rebased and squashed.

## Github

Github puts all pull requests into the _pull_ namespace:
_refs/pull/*/head_.

That namespace is read-only, so you can't push back to it.

#### Fetch a single pull request

Let's assume you cloned [vim-startify](https://github.com/mhinz/vim-startify)
and want to fetch this
[pull request #36](https://github.com/mhinz/vim-signify/pull/36) locally:

```
git fetch origin refs/pull/36/head:newbranch
git checkout newbranch
```

#### Fetch all pull requests

A standard remote section for origin can look like this:

```
[remote "origin"]
	url = git@github.com:mhinz/vim-startify.git
	fetch = +refs/heads/*:refs/remotes/origin/*
```

A `git fetch origin` would fetch all refs from the remote's _refs/heads/*_ and
put them into _refs/remotes/origin/*_ locally.

Now we just have to add a single line to also fetch all pull requests:

```
	fetch = +refs/pull/*/head:refs/remotes/origin/pr/*
```

Finally we need to issue just another `git fetch origin`, so that the refs from
the remote's _pull_ namespace are fetched into  our local _pr_ namespace.

If you want to work on a certain pull request, it's a good idea to create a
branch for it, e.g. `git checkout -b pr23 origin/pr/23` or `git checkout -b
fix-null-pointer-deref origin/pr/42`.

## Aliases

A list of useful aliases. Put these in the `[alias]` section of your .gitconfig
and adjust as needed.

Show all branches that aren't merged into master yet:
```
unmerged = branch --no-merged master
```
Show committers sorted by commit count:
```
rank = shortlog -sn --no-merges
```
Show root directory of the repo:
```
root = rev-parse --show-toplevel
```
