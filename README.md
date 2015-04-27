## ⟢ GIT GEMS ⟣

_This is yet another "learn git by example" document. Enjoy the ride!_

<img src='images/git-icon.png' alt='Git logo' height='100px' />

__Table of Contents__

- [Basics](#basics)
  - [Delete a remote branch](#delete-a-remote-branch)
- [Github](#github)
  - [Fetch a single pull request](#fetch-a-single-pull-request)
  - [Fetch all pull requests](#fetch-all-pull-requests)

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
