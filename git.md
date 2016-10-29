# Using git

Moving a repo branch instance from your local machine to a GitHub repo is called **pushing**.  The other way around is **pulling**

Basic command set:
```
$ git add .
$ git commit -m " ... "
$ git push
```

There are 3 stages to pushing:

1. Adding files to a staging area
2. Creating a commit message to denote what was changed
3. Pushing

## Adding

```
$ git add .
```

Launching this code from inside your cloned repo (highest level) will add all files to the staging area.

## Commiting

Commits are required.  They let your fellow hackers know what you did.
```
$ git commit -m "Commit message"
```

## Pushing

At this point, your staging area has both the files and a commit, and it's ready to be pushed.
```
$ git push 
```

## Branches
The code before assumes you only work on master branch and like to live dangerously.  For those of us that like to utilize different workspaces, we use branches.

