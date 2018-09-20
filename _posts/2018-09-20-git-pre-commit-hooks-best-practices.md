---
layout: post
title: Git pre-commit hook to enforce best practices
lang: en
keywords: "Git, pre, commit, hook, best practices"
---

Git pre-commit hooks can be used to enforce version control best practices.

If your development process is like mine, it includes three stages of
maturity: develop, staging and production. The first two stages are
represented by the identically named Git branches 'develop' and 'staging',
whereas production is represented by the 'master' branch.

My standard workflow implies that all commits are done in the 'develop'
branch. When a certain level of maturity is reached, the whole branch is
merged up to 'staging'. When 'staging' has proven its production-readiness,
the code is merged up to 'master'.

The following script ensures that commits are only done in the 'develop'
branch, if such a branch exists, because that's where the development
takes place. It still allows to perform commits in other branches,
if that is deemed necessary, to enable hotfixes e.g. This can only be done
when using the CLI though. When using an IDE overriding is not possible and
and an appropriate message is issued (tested in IntelliJ IDEA only). 

{% highlight shell %}
#!/bin/sh
 
# This pre-commit hook verifies that commits are done in a develop branch
# only, if such a branch exists. Overriding is still possible though.
 
develop_branch_exists=$(git branch --list | grep " develop$")
if [ -n "$develop_branch_exists" ]; then
    current_branch=$(git symbolic-ref HEAD --short)
    if [ "$current_branch" != "develop" ]; then
        if readlink /proc/$$/fd/1 | grep -q "^pipe:"; then
            # Unable to query user for override since we can't get a stdin
            echo "Unable to perform commit in non-develop branch. Use the Git CLI to override."
            exit 1;
        else
            exec < /dev/tty # redirect stdin to this shell
            read -p "Perform commit in non-develop branch? (y/N) " perform_commit
            if [ "$perform_commit" != "y" ]; then
                exit 1;
            fi
        fi
    fi
fi
{% endhighlight %}

In order for this script to be called upon every use of `git commit` it must
be executable and be registered as a global Git pre-commit hook. To do that
it has to be named `pre-commit` and placed in a directory which has to be
registered as the global hook directory:

{% highlight shell %}
git config --global core.hooksPath /path/to/my/global/hooks
{% endhighlight %}

