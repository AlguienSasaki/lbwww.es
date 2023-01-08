% How libreboot.org is hosted
% Leah Rowe
% 8 January 2023

I've recently started a new project, which I call the *Federation of Freedom*.
It is a website that teaches people how to self-host their own servers on the
internet, on all libre software. You could actually do it all on Libreboot
hardware.

When I say recently, I mean it; Fedfree launched on 25 December 2022. Today
is 8 January 2023. Thus, Fedfree is just about two weeks old, on this day.

**This is the website: [fedfree.org](https://fedfree.org/)**

I'm basically starting out with it, documenting each part of libreboot.org in
terms of hosting, but it will later expand. On that first part, it's still not
complete; it lacks a mail server guide (libreboot.org has mail), rsync
guide (ditto) and cgit guide (ditto) - I'm planning to host
a [forgejo instance](https://forgejo.org/) for git, and that'll be yet
another guide for Fedfree.

The guides it does currently have are:

* [L2TP tunnel router, with redundant
  routing](https://fedfree.org/docs/router/debian-l2tp-aaisp.html)
* [Nginx web server on Debian with LetsEncrypt HTTPS and
  Certbot](https://fedfree.org/docs/http/debian-nginx.html)
* [BIND9 authoritative name
  server](https://fedfree.org/docs/dns/debian-bind.html)

The setups described in those guides is exactly how libreboot.org is hosted,
for the types of services described.

Help is greatly appreciated, if people want to submit their own guides. The
basic premise behind it is this: hardware and software freedom are all well and
good, but most people with good ideas don't know how to do hosting, so they
default to using proprietary services like GitHub. I want to change that!

Basically, I want every website on the internet to be hosted in someone's
living room. I'm only half-joking when I say that. That is literally how
libreboot.org was hosted, for many years; the setup is still self-hosted, but
it's not currently hosted in a *living room*.

More information is available on the [Fedfree website](https://fedfree.org/)
