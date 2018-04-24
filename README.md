# Typical Git workflow

1. Pull master
2. `git checkout -b my-feature-branch`
3. Commit as you go
4. Push work up
5. Integrate latest changes from master with `git merge master` or `git pull origin master`

# Problems

This workflow leads to polluted master branch commit history that makes it more difficult to...

* Understand project history
* Cherry-pick changes into releases
* Deploy or rollback to past versions

# The solution

Keep your master branch commit history clean by:

1. Squashing commits before merge
2. Rebasing to integrate changes into feature branches

_Warning: only squash and rebase commits that exist exclusively in your feature branch and not in the mainline branch. Rewriting commits that are already master will lead to misery for your team._

# Rebasing

Rebasing accomplishes roughly the same thing as merging, but goes about it in a different way. Rebasing takes all the commits in your feature branch and replays them over the latest commits from master.

To rebase, use the command `git rebase master` instead of `git merge master`, and `git pull --rebase origin master` instead of `git pull origin master`. You may need to resolve conflicts just like when you merge.

For more information, see Atlassian's article on [Merging vs Rebasing](https://www.atlassian.com/git/tutorials/merging-vs-rebasing).

# How to Squash

Suppose you are in a feature branch that has 4 commits ahead of master. To squash the commits, you'd run the following command:

```
git rebase -i HEAD~4
```

The `-i` opens an editor to let you rewrite commits. The `HEAD~4` loads your last 4 commits for editing.

After your editor opens, follow the instructions presented to squash commits and then force push to upload your branch.

For more information, see Git's documentation on [Rewriting History](https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History).

As mentioned earlier, never rebase or squash commits that exist outside of the feature branch you are working on. Rewriting commits that are already master will lead to misery for your team.

# "Clean commit" Git workflow

The basic workflow looks like:

1. Check out a feature branch
2. Do work in your feature branch, committing early and often, and pushing up to a "WIP" pull request
3. Rebase frequently to incorporate upstream changes on master
4. When you're done with your feature, interactive rebase and squash your commits
5. Force push your cleaned up feature branch to have it reviewed and merged
