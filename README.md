## ⟢ GIT GEMS ⟣

_This is yet another "learn git by example" document. Enjoy the ride!_

<img src='images/git-icon.png' alt='Git logo' height='100px' />

__Table of Contents__

- [Github](#github)
  - [Fetch all pull requests](#fetch-all-pull-requests)


## Github

__NOTE:__ Github puts all pull requests into the _pull_ namespace:
_refs/pull/*/head_.

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
