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

{% details CMS handling user uploads %}
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
SFTP inherits basically all its security properties from
[SSH](https://en.wikipedia.org/wiki/Secure_Shell). SSH's implementation
[OpenSSH](http://www.openssh.com/) gives you the best
end-to-end hybrid (asymmetric to symmetric) key cipher implementations. 
OpenSSH's algorithms and key sizes are regularly updated and adapted to match
current and future attack vectors. 
{% enddetails %}
{% endcomment %}

In many CMS setups security aspects are second-tier and the focus seems to
be on ease of use for content maintainers. Since for many
companies the CMS is the most complex piece of software in operation that
is exposed to the internet, this is a risky prioritization. Next to malware
installed by unaware employees inside the network, the CMS is one of a
companies' biggest attack vectors.

{% details Hardening a CMS installation %}
There are multiple ways to harden a CMS installation:

* **Using and enforcing complex passwords for CMS and database users**
* **Limiting the use of plugins in the CMS**
* Sandboxing the installation by virtualization or containerization
* Making all files read-only for the system user running the web server, except for temporary files and uploads or course
* Putting a firewall before the machine running the CMS and disallow all access except HTTP/HTTPS
* Restricting permissions ot the CMS's database user as much as possible
* Moving the backend login page to a non-standard location
* Not leaking information on the used web server (plus addons), web framework and CMS

The emphasized points can be implemented by everyone, do not require special
skills and are basically best practice. The other points also provide great
improvements in security, but might not be as easy to implement.

**Sandboxing software** can either be viewn as an acknowledgement of the fact
that a given software can not be operated in a secure manner or as one
measure of many to enhance the operational security of an already security
hardened installation.

I am personally a big fan of making all files **read-only** for the system user
running the CMS, because this eliminates the possibility of injected malicious
code into the CMS's code base. On the downside, this also eliminates the
possibility of the CMS to update itself, so updates have to be performed
manually, resulting in increased maintenance cost.

**Firewalling the CMS** should be a standard procedure in every corporate
network, because it limits the scope of an attacks. If the CMS is only that,
a *content* management system, putting the CMS into a DMZ (demilitarized
zone) is an easy and effective firewall. If the CMS must have access to
corporate data (e.g. current rates or customer data)



Moving the backend login page to a non-standard location is security through
obscurity at best, but it helps to lower the number of login attempts
performed by bots trying to exploit known security holes. This measure keeps
logs smaller and cleaner and allows for a quicker identification of real
threats.
{% enddetails %}

While a security hardened CMS installation is in the companies' best interest,
increased maintenance cost is not. As with most things, this is a trade off.
However, this article being written in 2016, most companies simply can not
afford website/CMS downtime or bad press because of leaked user data or
corporate secrets due to insufficient security measures, hence making the case
for a hardened CMS installation.



