% Libreboot Git repositories now on Codeberg (RIP Notabug)
% Leah Rowe
% 8 April 2023

RIP Notabug
===========

Git repositories provided by Libreboot are still available via Notabug, but
the Notabug site has been quite unreliable for some time now. I notice it
mostly in the evenings, when more people are likely using it. Essentially, the
service is overloaded all the time and this results in regular HTTP 500 errors,
preventing pull requests, git clone, issue reports and other things to go
offline at random times.

I kept Libreboot on Notabug for as long as possible, because I hoped that the
admin (single) would fix issues, but I can't keep waiting.

Libreboot was originally a member of the Peers Community, which hosts Notabug.
So I had an affinity for Notabug.

Libreboot repos now hosted by Codeberg
--------------------------------------

I've decided to set up an account on Codeberg. You can find it here:

* <https://codeberg.org/libreboot>

On this day, 8 April 2023, the following repositories are available via
Codeberg:

* Build system: <https://codeberg.org/libreboot/lbmk>
* Website (markdown files): <https://codeberg.org/libreboot/lbwww>
* Images (av.libreboot.org): <https://codeberg.org/libreboot/lbwww-img>

Codeberg has nicer features
---------------------------

Codeberg runs on forgejo, itself a fork of Gitea, which *itself* is a fork
of Gogs. *Notabug* runs on an older, modified version of Gogs, which lacks a
lot of nicer features like issue search.

Codeberg's forgejo instance has issue search, and it has a nice CI built in,
namely *woodpecker*. All of this and more could be useful to Libreboot, and
is being looked into.

The interface is virtually identical to that of Notabug, since it's based
upon the same original codebase. Links on libreboot.org have been updated.
You can send issue reports and pull requests in much the same way as before,
but you will need to make a new account on codeberg.org if you don't already
have one.

Notabug still available
-----------------------

The notabug repositories are *still* available, and I'll still push new code
to them. I push to several repositories, not just codeberg/notabug, but those
are the ones that I openly advertise.

Notabug is *usually* available, but it is often overloaded in the evenings,
so it's no longer viable for production use, but it's still viable as a backup.
If codeberg is ever down, at least you'd be able to download from Notabug.

Why not self-host?
==================

Forgejo, based on Gitea, is what runs on Codeberg. They host the project, on
behalf of the developers. Forgejo is working on federating the git forge, so
that mastodon-like features are available (pull requests, issues and such).

Until federated collaboration is possible in Git (via the web), it's simply
not viable for a small project like Libreboot to provide a self-hosted
instance, because it would mean that people have to make an account on the
site *just* for Libreboot. This seems unreasonable as a request. Lots of
people are on codeberg, and already have accounts.

Codeberg is run by a non-profit organisation that seems pretty solid, with
donations infrastructure too, so they'll probably always have the resources
to manage the service effectively.

When federation becomes available, I assume Codeberg's forgejo instance will
become part of that, so it just makes practical sense for Libreboot to
use Codeberg.

Why not sourcehut?
------------------

I considered sourcehut. I like the concept of it (mailing lists made easier,
email-based collaboration) but I don't think most people will want to use that.

Forgejo provides a web-based interface in a style that most people are used to.

Libreboot is *not* hosted on sourcehut, officially, in any capacity.

That is all.
