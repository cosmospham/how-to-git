# Revert server to past commit with bare repository hooks

>I have a server with a bare git repository that I push to for live deployment. It has a simple post-receive hook that updates my server code with the latest on master.
>
>I sometimes need to revert to a specific past commit when bugs are found on the production server (I test myself locally, but I don't catch everything). Is there a nice* way to revert to a past commit with this setup?
>
>*By nice I mean a one-liner like pushing to the bare repository: `git push prod master`

___

If you want to keep your mistake in the history, this will add commits which compensate the commits you specify. (master must be checked out locally) For example, if you want to revert the last commit, you can use:

```
git revert master
git push origin master
```

If you want to remove the last 2 commits from history (master must be checked out locally):

```
git reset master~2
git push -f origin master
```

Or without changing anything local, this should work as well:

```
git push -f origin master~2:master
```

The last two will force-update the master to the third from last master commit. Which is problematic if someone else already pulled the commits you are removing.

Ref:
[stackoverflow#17414275](https://stackoverflow.com/questions/17414275/revert-server-to-past-commit-with-bare-repository-hooks)
