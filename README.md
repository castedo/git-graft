Keep your git history and squash it too
=======================================

Some `git` tasks work best with regular `git merge` and preserving commit
history.
But pull requests and main branches are much easier to understand with
simplified git histories resulting from commit squashing, re-basing and not
using regular `git merge`.

[git-prepr](bin/git-prepr) ("*prep* *r*equest" or "pre-PR") makes it possible
to keep commit history, use `git merge` while also pushing simplified histories
in pull requests.

In a "prepr" workflow a *topic branch* maintains all commit history while
it's corresponding *PR (pull request) branch* has a simplified history.
Multiple topic branches can be developed using regular `git merge`. This
includes merging the main branch into topic branches and merging multiple topic
branches into each other.

Every *PR branch* is created and updated using the script
[git-prepr](bin/git-prepr).

Here's the basic "prepr" workflow (main branch is `main`):

```
git checkout -b cool_feature main
...
git commit -m "cool new feature"
...
git commit -m "fix bug"
git prepr
... combine topic branch commit messages into single PR commit message ...
git push
```

Installation
------------

Copy [git-prepr](bin/git-prepr) into a directory that is in your `PATH`.

How It Works
------------

[git-prepr](bin/git-prepr) creates and updates a branch called `pr/mytopic`
when you are in a clean checkout of a topic branch named `mytopic`.

The `mytopic` branch is "grafted" into the `pr/mytopic` branch. Grafting
is done via `git replace --graft`. This means `pr/mytopic` will have a simple
linear history when published. At the same time local merging can be done
intelligently with regular `git merge` between `main` and *multiple* topic
branches.

Useful git commands to use with [git-prepr](bin/git-prepr)
----------------------------------------------------------

Use `--branches` instead of `--all` when using `git log` to not see
*replacement graft commits*.

Do `git --no-replace-objects log ...` to see the log without any grafting of
topic branches into pr branches.

Do `git replace --format=long` to see all *replaced commits* pointing to their
*replacement graft commits*.

Do `git replace -d pr/mytopic` to permanently remove the graft of `mytopic`
into `pr/mytopic`.

Use [git-isograft](bin/git-isograft) to re-graft a PR branch onto a topic
branch by doing `git isograft mytopic pr/mytopic`.


Similar scripts and acknowledgments
-----------------------------------

* https://github.com/sheerun/git-squash

