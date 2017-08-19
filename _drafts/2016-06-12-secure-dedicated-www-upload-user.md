---
layout: post
title: 'Securely setting up a dedicated SFTP upload user for content management systems'
lang: en
keywords: "chroot, sftp, bind, bindfs, cms, content management, content management system, upload, user"
---

# Motivation

## Optimized usability

Content management systems (CMS) often provide a web based file manager to,
well, manage files that ought to be incorporated into the CMS. While this
browser based approach is often sufficient, uploading large files or lots of
small files quickly becomes a hassle. Interrupted file uploads often can not
be resumed and the workflow of selecting multiple files spanning different
directories is just tiresome. Both situations call for the use of a native
file manager capable of remote connections.

{% details CMS handling of user uploads %}
While managing user uploads could be done various ways, a straightforward and
popular one is to have one directory in the filesystem
holding all uploads, serving as both index and storage. This directory is then
represented in the CMS's file manager browser application as a tree view, e.g.
It is necessary for that directory to be the reference index, so that an added
file is immediately made aware to the CMS and usable right away.
{% enddetails %}

You can apply the following to your CMS only if it can also "see" files that
are copied to its uploads directory using other means than the CMS's
integrated file manager. There are many CMS that comply with that condition,
e.g. [Joomla!](https://www.joomla.org/), [Typo3](https://typo3.org/) and
[Contao](https://contao.org/). Contao will be used exemplary in the remainder of
this article.

## Security

{% comment %}
{% details Copying files securely %}
Copying files to remote servers is a problem solved at least a hundred times.
The two major players in this area are the ephemeral
[FTP](https://en.wikipedia.org/wiki/File_Transfer_Protocol) (along with its
less popular brother [FTPS](https://en.wikipedia.org/wiki/FTPS)) and
[SFTP](https://en.wikipedia.org/wiki/SSH_File_Transfer_Protocol).
SFTP inherits most of its security properties from
[SSH](https://en.wikipedia.org/wiki/Secure_Shell). SSH's implementation
[OpenSSH](http://www.openssh.com/) gives you the best
end-to-end hybrid (asymmetric to symmetric) key cipher implementations. 
OpenSSH's algorithms and key sizes are regularly updated and adapted to match
current and future attack vectors. 
{% enddetails %}
{% endcomment %}

In many CMS setups security aspects are second-tier and the focus seems to
be on ease of use for content maintainers. Since for many small
companies the CMS is the most complex piece of software in operation that
is exposed to the internet, this is a risky prioritization. Next to malware
installed by unaware employees inside the network, the CMS is one of a
companies' biggest attack vectors.
