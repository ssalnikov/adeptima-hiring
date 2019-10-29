# Git Flow (with rebasing)


See http://nvie.com/posts/a-successful-git-branching-model/




## Features

A feature is based off the `develop` branch and merged back into the `develop` branch.
It will eventually get into `master` when we make a release.


### Working Locally

```
# checkout develop, fetch the latest changes and pull them from remote into local
git checkout develop
git fetch
git pull origin develop

# create a feature branch that is based off develop
git checkout -b feature/XX-123/some-description

# do your work
git add something
git commit -m "first commit"
git add another
git commit -m "second commit"

# rebase against develop to pull in any changes that have been made
# since you started your feature branch.
git fetch
git rebase origin/develop

# push your local changes up to the remote
git push

# if you've already pushed changes and have rebased, your history has changed
# so you will need to force the push
git fetch
git rebase origin/develop
git push --force-with-lease
````


### GitHub workflow

- Open a Pull Request against `develop`
- When the Pull Request has been approved, merge using `squash and merge`, adding the ticket number and a brief description:
ie, `MQ-330 enable users to order a pizza from the dashboard`.
- This squashes all your commits into a single clean commit.

If you are unable to squash merge because of conflicts, you need to rebase against `develop` again:

```
# in your feature branch
git fetch
git rebase origin/develop
git push --force-with-lease
```



## Releases

A release takes the changes in `develop` and applies them to `master`.


### Working locally


```
# create a release branch from develop
git checkout develop
git fetch
git pull origin develop
git checkout -b release/3.2.1

# finalise the change log, local build, etc
git add CHANGELOG.md
git commit -m "Changelog"

# rebase against master, which we're going to merge into
git fetch
git rebase origin/master
git push --force-with-lease
```


Usually at this point you will want to deploy the release branch to the staging server for final QA.
If there are any issues, fixes should be committed to the release branch.

### Github workflow

- Open a Pull Request against `master`
- When the PR is approved and the staging deploy has been verified by QA, merge using `rebase and merge`.
- **DO NOT SQUASH MERGE**. We don't want a single commit for the release, we want to maintain the feature commits in the history.
- Repeat the steps above against `develop` (may need to rebase first).
- Tag a release on master. Use the version number and put the changelog in the description.




## Hotfixes

A hotfix is a patch that needs to go directly into `master` without going through the regular release process.
The most common use case is to patch a bug that's on production when `develop` contains code that isn't yet ready for release.


### Working locally

```
# create a hotfix branch based on master, because master is what will be deployed to production
git checkout master
git fetch
git pull origin master
git checkout -b hotfix/describe-the-problem

git add patch.fix
git commit -m "fix the problem"
git push
```

### Github workflow

- Open a Pull Request against `master`
- When the PR's approved and the code is tested, `squash and merge` to squash your commits into a single commit.
- Open a Pull Request against `develop` (may need to rebase first).
- Tag a release on `master`. Describe the issue in the name, feel free to put details in the description.

### Advanced Git
- Pro Git: https://git-scm.com/book/en/v2
- How to teach Git, Rachel M. Carmena https://rachelcarmena.github.io/2018/12/12/how-to-teach-git.html
- Flight rules for Git https://github.com/k88hudson/git-flight-rules
- Become a git guru, Atlassian https://www.atlassian.com/git/tutorials
- Welcome to Learn Git Branching ;) https://learngitbranching.js.org/
- Git from the bottom up, John Wiegley http://ftp.newartisans.com/pub/git.from.bottom.up.pdf

## Submitting a patch in Go

  1. It's generally best to start by opening a new issue describing the bug or
     feature you're intending to fix.  Even if you think it's relatively minor,
     it's helpful to know what people are working on.  Mention in the initial
     issue that you are planning to work on that bug or feature so that it can
     be assigned to you.

  2. Follow the normal process of [forking][] the project, and setup a new
     branch to work in.  It's important that each group of changes be done in
     separate branches in order to ensure that a pull request only includes the
     commits related to that bug or feature.

  3. Go makes it very simple to ensure properly formatted code, so always run
     `go fmt` on your code before committing it.

  4. Any significant changes should almost always be accompanied by tests.  The
     project already has good test coverage, so look at some of the existing
     tests if you're unsure how to go about it.

  5. Do your best to have [well-formed commit messages][] for each change.
     This provides consistency throughout the project, and ensures that commit
     messages are able to be formatted properly by various git tools.

  6. Finally, push the commits to your fork and submit a [pull request][].

[forking]: https://help.github.com/articles/fork-a-repo
[well-formed commit messages]: http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html
[squash]: http://git-scm.com/book/en/Git-Tools-Rewriting-History#Squashing-Commits
[pull request]: https://help.github.com/articles/creating-a-pull-request


