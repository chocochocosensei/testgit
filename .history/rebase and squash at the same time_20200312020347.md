From [this link](https://stackoverflow.com/questions/3251011/can-i-rebase-and-squash-commits-at-the-same-time):

> When I have a fix for a change that was a few commits earlier, I always wind up running rebase twice in a row. Is it possible to do this workflow all in one step? Let's say I have 4 new commits.
> 
> ```
> * (master) D
> * C
> * B
> * A
> * Base
> 
> ```
> 
> I find a bug in B so I create a branch and fix it.
> 
> ```
> * (master) D
> * C
> | * (fix) Fix.
> |/  
> * B
> * A
> * Base
> 
> 
> ```
> Next I run `git rebase --onto fix B D` to move C and D onto B.
> 
> ```
> * (master) D'
> * C'
> * (fix) Fix.
> * B
> * A
> * Base
> 
> ```
> 
> Finally I run `git rebase --i fix^^` to see the last few commits and I squash B and Fix into a single commit.
> 
> ```
> * (master) D'
> * C'
> * B'
> * A
> * Base
> 
> ```
> 
> Is there a faster way to accomplish the same workflow? I imagine that merging would be easier, but merging is out for me because I'm using git svn which requires a linear history.

answer:




From [this link](https://stackoverflow.com/questions/5308816/how-to-use-git-merge-squash)

> Say your bug fix branch is called bugfix and you want to merge it into master:
> 
> ```
> git checkout master
> git merge --squash bugfix
> git commit
> ```
> This will take all the commits from the bugfix branch, squash them into 1 commit, and merge it with your master branch.
> 
> Explanation:
> 
> ```
> git checkout master
> ```
> Switches to your master branch.
> 
> ```
> git merge --squash bugfix
> ```
> Takes all the commits from the bugfix branch and merges it with your current branch.
> 
> ```
> git commit
> ```
> Creates a single commit from the merged changes.
> 
> Omitting the -m parameter lets you modify a draft commit message containing every message from your squashed commits before finalizing your commit.

> What finally cleared this up for me was a comment showing that:
> 
> ```
> 
> git checkout main
> git merge --squash feature
> 
> ```
> is the equivalent of doing:
> 
> ```
> git checkout feature
> git diff main > feature.patch
> git checkout main
> patch -p1 < feature.patch
> git add .
> 
> ```
> When I want to merge a feature branch with 105(!!) commits and have them all squashed into one, I don't want to git rebase -i origin/master because I need to separately resolve merge conflicts for each of the intermediate commits (or at least the ones which git can't figure out itself). Using git merge --squash gets me the result I want, of a single commit for merging an entire feature branch. And, I only need to do at most one manual conflict resolution.

