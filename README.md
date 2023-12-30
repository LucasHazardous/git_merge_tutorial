# Git Merge Tutorial

When using multiple different branches at one point there will be a need to combine changes from those different branches. In order to do that merging is used in Git.

> Git encourages workflows that branch and merge often, even multiple times in a day. ~ Git Book

## Understanding the core

Git doesn't store data as a series of differences from the original state, but rather it stores it as a series of snapshots. Each snapshot is like a new *filesysytem*, where new versions of changed files are. For efficiency reasons, files that haven't been modified are granted a pointer to its version from previous snapshot or *filesystem*. Snapshots can be thought of as a tree, which refers to a set of changed files, or in case of not changed files to files from the previous commit. Each commit is a pointer to that snapshot, this pointer also stores additional metadata.

![](./img/commit-and-tree.png)

<hr>

![](./img/commits-and-parents.png)

## Branches

Branching is thought of as a "killer feature" in Git. Why? According to the Git Book, it is because "branches are incredibly lightweight, making branching operations nearly instantaneous, and switching back and forth between branches generally just as fast".

A branch in Git is simply a lightweight movable pointer to one of these commits.
To create a new branch for this local repository use:

```
git branch test
```

How does Git know what branch you’re currently on? It keeps a special pointer called HEAD.

To switch to the newly created branch use:

```
git checkout test
```

you can also switch with:

```
git switch test
```

To create a branch and instantly switch to it use:

```
git checkout -b test
```

Now add a new commit to each of the branches and compare their histories with:

```
git log --oneline --decorate --graph --all
```

You see that your branches diverged. So now how to combine those changes back into one branch?

## Merge

Firstly select the branch to which you want to merge the other one and switch to it. In this case we will switch to main. Then use the following to perform the merge:

```
git merge test
```

Depending on what you changed, there are two possible states that can occur:

1. Git combined the changes to the current branch, because changes did not intersect.
2. You modified the same lines on the same files and there is a merge conflict.

So how does Git determine that there is a merge conflict or no? Let's look at this example. Someone had created a branch called *testing*, which then received a new commit and the master (or main) branch received a commit as well.

![](/img/advance-master.png)

We see that both of the new commits point to one commit. This commit is called a base. When Git merges two branches, it compares each branch's latest commit with the base to determine what changed between those branches and then looks for interferences.

## Merge conflict

Let's create a merge conflict and resolve it.

Remove your previous changes by going to the commit, which appears as the latest in the remote repository. Find with *git log* which commit is annotated with *origin/main*.

```
git checkout main
git reset --hard found_commit_hash
```

Now create file.txt with the following contents:

```
This line is amazing.
This line is not cool.
Hello there.
```

Commit the changes, then create a new branch called *fix*, but don't switch to it yet:

```
git branch fix
```

Add another change on the *main* branch to the file.txt to make it look like:

```
This line is very amazing.
This line is not cool.
Hello there.
Git.
```

Commit changes and now switch to *fix* branch:

```
git checkout fix
```

And modify the file.txt, and then commit changes:

```
This line is cool.
Hello there.
```

Switch back to main and attempt a merge:

```
git checkout main
git merge fix
```

Git adds standard conflict-resolution markers to the files that have conflicts, so you can open them manually and resolve those conflicts. IDEs and Code Editors may have custom methods for dealing with merge conflicts as shown in [Visual Studio Example](https://www.youtube.com/watch?v=HosPml1qkrg). If you have other graphical tool to deal with merge conflicts use:

```
git mergetool
```

Open file.txt. Markers with HEAD indicate that this is what you had before you attempted the merge. We have the state from main branch, because we were on it before doing the merge.
Let's check our current state:

```
git status
```

After that resolve the merge with preffered method to modify the file to make it look like this:

```
This line is very cool.
Hello there.
Git.
```

Then stage the file with:

```
git add file.txt
```

And commit the merge. Congratulations! You have revolved a merge conflict!

## Fast-forward

But what would happen if there was no merge conflict? Git would do a fast-forward and then you would not even have to create a commit for the merge. With --no-ff option, you can create a merge commit in all cases, even when the merge could be resolved as a fast-forward.

## Cancel merge

You can cancel a merge with:

```
git merge --abort
```

## Merge strategies

When you know that conflicts will occur but you don't want to resolve them manually in some you can use backend merge strategies and their options. There is a default merge strategy backend called *ort* that is used when you perform a simple *git merge*, and it can take the following options: 

* *ours* - resolves conflicts by automatically choosing the version of the file from the branch being merged into, disregarding changes from the branch being merged in.
* *theirs* - resolves conflicts by automatically choosing the version of the file from the branch being merged in, disregarding changes from the current branch.
* and others...

Example:

```
git merge -X theirs other_branch_name
```

## Pull requests

A pull request is a proposal to merge a set of changes from one branch into another. When you want to merge branches which are in a Github repository, you can either do the merge locally and push changes into remote repository or create a pull request and merge it on Github. Pull requests contain interesting functionality like assigning a reviewer to approve your code before it is merged, or leaving comments on the pull request. There is also a good use case for pull requests: contributing to public projects.

How to contribute:

1. Create a fork of a public repository (fork is a new repository that shares code and visibility settings with the original “upstream” repository).
2. Create a new branch.
3. Make some commits to improve the project.
4. Push the branch to your repository (fork).
5. Open a pull request.
6. Dicuss, optionally continue commiting.
7. Project maintainer merges changes and now the project contains your changes.

## Rebase

There is also another way to integrate changes from one branch to another, it's called a rebase. However using rebase involves tempering with commit history and it's a complex matter when to use it.

## The end

Through conflicts resolved and histories entwined,\
A symphony of collaboration, refined.\
So let us embrace this merging art,\
In coding journeys, it plays a vital part.

Knowledge about merging is an essential step in the journey of learning Git. Armed with this new skill, make sure to put it into practice. Thank you for your attention and good luck.

*Lucas Hazardous*
