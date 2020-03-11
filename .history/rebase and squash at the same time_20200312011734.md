When I have a fix for a change that was a few commits earlier, I always wind up running rebase twice in a row. Is it possible to do this workflow all in one step? Let's say I have 4 new commits.

```
* (master) D
* C
* B
* A
* Base

```

I find a bug in B so I create a branch and fix it.

```
* (master) D
* C
| * (fix) Fix.
|/  
* B
* A
* Base


```
Next I run `git rebase --onto fix B D` to move C and D onto B.

```
* (master) D'
* C'
* (fix) Fix.
* B
* A
* Base

```

Finally I run `git rebase --i fix^^` to see the last few commits and I squash B and Fix into a single commit.

```
* (master) D'
* C'
* B'
* A
* Base

```

Is there a faster way to accomplish the same workflow? I imagine that merging would be easier, but merging is out for me because I'm using git svn which requires a linear history.