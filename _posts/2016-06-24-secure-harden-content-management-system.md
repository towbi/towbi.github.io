---
layout: post
title: 'Securing/Hardening CMS (content management systems)'
lang: en
keywords: "securing, security, hardening, cms, content management, content management system"
---

This post lists some measures that can be taken to secure/harden a CMS
(content management system) installation.

* **Using and enforcing complex passwords for CMS and database users**
* **Limiting the use of plugins in the CMS**
* Sandboxing the installation by virtualization or containerization
* Making all files read-only for the system user running the web server, except for temporary files and uploads
* Putting a firewall before the machine running the CMS and disallow all access except HTTP/HTTPS
* Restricting permissions ot the CMS's database user as much as possible
* Moving the backend login page to a non-standard location
* Not leaking information on the used web server (plus addons), web framework and CMS

The emphasized points can be implemented by everyone, do not require special
skills and are basically best practice. The other points also provide great
improvements in security, but might not be as easy to implement. Why using
**strong passwords** is important does not need further explanation.
By **limiting the use of plugins** you decrease the attack surface of your CMS
installation. Studies have shown that the cores of many popular CMS are quite
secure and receive security patches in time. The same is not true for the
plugin ecosystem by far.

**Sandboxing software** can either be viewn as an acknowledgement to the fact
that a given software can not be operated in a secure manner or as one of many
measures to enhance the operational security of an already security
hardened installation.

I am personally a big fan of making all files **read-only** for the system user
running the CMS, because this reduces the possibility of injected malicious
code into the CMS's code base greatly. On the downside, this also eliminates the
possibility of the CMS to update itself, so updates have to be performed
manually, resulting in increased maintenance cost for each update.

**Firewalling the CMS** should be a standard procedure in every corporate
network, because it limits the effects of an attack. If the CMS is only that,
a *content* management system, putting the CMS into a DMZ (demilitarized
zone), e.g. as a bastion host, is an easy and effective firewall. If the CMS
must have access to corporate data (e.g. to access current rates or customer
data), the firewall must be tweaked (and thus weakened).

**Moving the backend login page** to a non-standard location is security
through obscurity at best, but it helps to lower the number of login attempts
performed by bots trying to exploit known security holes. This keeps
logs smaller and cleaner and allows for a quicker identification of real
threats.

If the system running the CMS, i.e. web server, web framework used by the CMS
and the CMS itself, **do not leak information** about them and their versions,
attackers have a higher cost of identifying possible security weaknesses. This
is, again, security through obscurity, but has the same benefits stated before.

--------------------------

There are a lot of tools and plugins for any given CMS that will promise to
enhance security. But as is also often the case with anti virus software, adding
more software to an already complex system is not going to simply increase
security, it will also increase the attack surface for the malware of crackers.
There might be notable exceptions, but many *security plugins* will deluge
the user with *security notifications*, giving her or him the wrong feeling that
the plugin is taking care of things.

While a security hardened CMS installation is in the companies' best interest,
increased maintenance cost is not. As with most things, this is a trade off.
However, this article being written in 2016, most companies simply can not
afford website/CMS downtime or bad press because of leaked user data or
corporate secrets due to insufficient security measures, hence making the case
for a professionally hardened CMS installation.
