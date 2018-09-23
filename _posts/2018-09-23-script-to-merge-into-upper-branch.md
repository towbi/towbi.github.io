---
layout: post
title: Script to merge a Git branch into the upper branch
lang: en
keywords: "Git, script, merge, upper, upperer, branch"
---

If using a development process that consists of multiple stages of maturity,
development mainly progresses in one branch only. I often use a process that
consists of the three branches 'develop', 'staging' and 'master', so in my
case the development mainline is 'develop'. You might want to check out my
post on how to
[enforce or safeguard such a development process]({% post_url 2018-09-20-git-pre-commit-hooks-best-practices %}).

If the development process also advocates the use of feature branches, the
'develop' branch often is in a state that is mature enough to be merged into
the upper branch.

To aid in this process I developed a little script to merge the current
branch into the upper branch. The script removes the need to issue a bunch
of Git commands and performs the following tasks:
* check for unclean working copies prior merging
* checkout target branch and pull from origin
* merge from lower branch
* push to origin
* repeat the last three steps for the branch higher than the target branch

The script asks for confirmation before every action and assumes relatively
save defaults (it's e.g. not the default to directly merge up into
master/production).

The script works for a process with three stages ('develop', 'staging',
'master') as well as for one with two stages ('develop', 'master') -- the
stages must be named as described, however.

{% highlight shell %}
#!/bin/bash
 
# This script performs a merge of the current branch in the upper branch
# (without fast forwarding).
#
# Author: Tobias König <tn@movb.de>
 
set -eu
 
#
# check branching
#
 
staging_exists=$(git rev-parse --verify staging 1>/dev/null 2>&1; echo $?) # 0 = true; 128 = false
 
#
# determine current and upper branch
#
 
current_branch=$(git symbolic-ref HEAD --short)
case $current_branch in
  develop) if [ "$staging_exists" = "0" ]; then
               upper_branch=staging
               upperer_branch=master
           else
               upper_branch=master
               upperer_branch=""
           fi ;;
  staging) upper_branch=master; upperer_branch="" ;;
  master)  echo You are on master, there is no upper branch to merge to.; exit 1 ;;
  *)       echo Current branch is unknown, I am dazed and confused.; exit 2 ;;
esac
 
#
# handle dirty working copies
#
 
dirty=$(git status --porcelain)
if [ -n "$dirty" ]; then
    echo Your working copy is dirty:
    echo "$dirty"
    echo
 
    # ask to continue if not really dirty, i.e. if only untracked files exist
    if ! echo "$dirty" | grep -qv '^?? '; then
        read -p "Still continue? (y/N) " continue_if_dirty
        if [ "$continue_if_dirty" != "y" ]; then
            exit 3
        fi
    # otherwise just exit because it's too dirty to continue
    else
        exit 4;
    fi
fi
 
#
# perform merge in upper branch
#
 
read -p "Merging from $current_branch to $upper_branch. Continue? (y/N) " perform_merge
if [ "$perform_merge" = "y" ]; then
    git checkout $upper_branch
    git pull
    git merge --no-ff $current_branch
    git checkout $current_branch
else
    exit
fi
 
#
# push upper branch
#
 
read -p "Push $upper_branch to origin? (y/N) " push_upper
if [ "$push_upper" = "y" ]; then
    git push origin $upper_branch
     
    if [ -z "$upperer_branch" ]; then
        echo
        echo All done.
    fi
else
    exit
fi
 
 
if [ -n "$upperer_branch" ]; then
 
    #
    # perform merge in upperer branch
    #
    read -p "Merging from $upper_branch to $upperer_branch. Continue? (y/N) " perform_merge
    if [ "$perform_merge" = "y" ]; then
        git checkout $upperer_branch
        git pull
        git merge --no-ff $upper_branch
        git checkout $current_branch
    else
        exit
    fi
 
    #
    # push upperer branch
    #
 
    read -p "Push $upperer_branch to origin? (y/N) " push_upperer
    if [ "$push_upperer" = "y" ]; then
        git push origin $upperer_branch
 
        echo
        echo All done.
    fi
fi
{% endhighlight %}

The script is executed by just calling `merge-up` when in a Git working copy
that has either the branch 'develop' or 'staging' checked out.

{% highlight shell %}
tobi@spieli:~/dev/myproject$ merge-up
{% endhighlight %}

