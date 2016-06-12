---
layout: post
title: 'Securely setting up a dedicated SFTP upload user for content management systems'
lang: en
keywords: "chroot, sftp, bind, bindfs, cms, content management, content management system, upload, user"
---

# Motivation

Content management systems (CMS) often provide a web based file manager to,
well, manage files that ought to be incorporated into the CMS. While this
browser based approach is often sufficient, uploading large files or lots of
small files quickly becomes a hassle. Interrupted file uploads often can not
be resumed and the workflow of selecting multiple files spanning different
directories is just tiresome. Both situations call for the use of a native
file manager.

{% details More on the CMS handling user uploads %}
While the implementation of managing user uploads could be done various ways,
a straightforward and popular one is to have one directory in the filesystem
holding all uploads serving as both index and storage. This directory is then
represented in the CMS's file manager e.g. as a tree view. It is necessary for
that directory to be the reference index, so that an added file is immediately
made aware to the CMS and usable right away.
{% enddetails %}

