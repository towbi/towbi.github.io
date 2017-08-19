---
layout: post
title: "Cost-effective, retroactive deployment of a backup solution with rsnapshot on Linux"
lang: en
keywords: "linux, backup, snapshots, rsnapshot, filter, modified after, mtime, maximum age"
---

This post explains how to use rsnapshot to implement a backup solution that
* uses snapshotting to be able to go back to a specific point in time,
* uses disk space effectively by sharing file content across snapshots,
* only regards files newer a certain age and
* ignores files in a certain subdirectory.

# Basics

Since this solution uses rsnaphots it has to be installed first. Many
distributions provide a system wide rsnapshot configuration file along
with configuration lines for crontab. I won't be using rsnapshot this
way but instead keep all scripts, configuration, log files, the lock
file and the backup itself at one place and use the user's (root)
crontab for the periodic execution.


