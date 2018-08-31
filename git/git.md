# Git General

## Resolve conflicts

After review each files. If solution is to accept local/our version, run:

> `git checkout --ours PATH/FILE`

If solution is to accept remote/other-branch version, run:

> `git checkout --theirs PATH/FILE`

If you have multiple file, use this

> `grep -lr '<<<<<<<' . | xargs git checkout --ours`

> `grep -lr '<<<<<<<' . | xargs git checkout --theirs`

## Commit

Quick

> `git commit -am 'message'; git push`

Normal:

```
git add xxxx
git commit xxx
git push
```
