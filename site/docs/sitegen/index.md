---
title: Libreboot Static Site Generator (lbssg)
x-toc-enable: true
...

Home of your favourite firmware to boot GNU / Linux
---------------------------------------------------

This document describes the *static site generator*, which is used to build
the Libreboot website; it was previously called the *Untitled Static Site
Generator* and hosted separately, but it is now an official component of
the Libreboot project, since 25 January 2025.

This static site generator is simply called the *Libreboot Static Site
Generator*, or just *lbssg* for short. It can be used for any number of
websites, and is in fact used elsewhere, but it is primarily used *by* and
developed *for* the Libreboot project.

The Git repository is now `lbssg.git`, which you can find linked on
the [Git page](../../git.md).

Websites made easy
------------------

This software is written in POSIX shell (`sh`) scripts, and uses Pandoc to
generate HTML from Markdown files. It is designed to be stupidly simple,
allowing Free and Open Source Software projects to easily host any number of
websites.

It is a *multi-site* static site generator; a single instance of LBSSG can
handle an unlimited number of websites, all with different *templates* in
Pandoc (for different page layouts), CSS files, footer, header, navigation and
more.

With lbssg, it is possible to generate an unlimited number of *news*
pages and *blogs*, with auto-generated RSS feeds.

The variant of markdown is the Pandoc variant, which you can read about here:\
<https://pandoc.org/MANUAL.html>

The *templates* feature in Pandoc is really what makes lbssg very powerful.
You can make any number of websites, with many different designs, using Pandoc
templates. This is precisely why lbssg uses Pandoc. See:\
<https://pandoc.org/MANUAL.html#templates>

Pandoc's *smart* mode is used. In this mode, it is possible to use HTML
inside Markdown files. For example, if you want to embed videos, use iframes,
use JavaScript and more.

lbssg is *Free Software*, released under the terms of the GNU General Public
License, version 3 of the license or, at your option, any later version as
published by the Free Software Foundation. You can find a copy of this license
in the `COPYING` file, when you download lbssg, or you can read it here:\
<https://www.gnu.org/licenses/gpl-3.0.en.html>

(some parts of it are under the MIT license)

Learn more about Free Software:\
<https://writefreesoftware.org/>.

### Real-world examples

Here are some websites built using the Libreboot Static Site Generator:

* <https://canoeboot.org/> (Libreboot fork maintained by Leah Rowe)
* <https://fedfree.org/> (Leah Rowe's self-hosted servers tutorial website)
* <https://libreboot.org/> (Libreboot project, maintained by Leah Rowe)
* <https://trans.chat/> (Leah Rowe's transgender advocacy website)
* <https://vimuser.org/> (Leah Rowe's personal blog)

Requirements
------------

Libreboot Static Site Generator is designed to be stupidly simple. You need:

* Unix-based operating system e.g. Linux-based distro, OpenBSD, FreeBSD. Etc
* Pandoc
* Web server (nginx or OpenBSD httpd recommended)
* Any implementation of these Unix utilities: `sh`, `find`, `grep` and `sed`.

### POSIX compatibility

As of revision `9cc6f5dc7349b7f339f99c84789b6c62ea7bb1c7` on 4 December 2023,
lbssg works with any POSIX `sh` implementation. Various utilities like
Sed, Find (that are POSIX) are now used in a POSIX way. This means that
lbssg can be used on pretty much any Unix system such as GNU+Linux,
Alpine Linux, Chimera Linux, BSDs like OpenBSD or FreeBSD, and so on. Older
revisions only worked with GNU tools, but the newer revision also still works
with GNU tools. Older revisions of lbssg were written in `bash`, not `sh`.

Security notes
---------------

USE THIS SOFTWARE AT YOUR OWN RISK. A FULL SECURITY AUDIT IS NEEDED.

Symlinks are intentionally unsupported, and *ignored*, in site directories
handled by lbssg. This is for security reasons. If you find that symlinks
are permitted, in any way, it's a bug and you should report it!

At the very least, you are advised to take the following precautions

* It is HIGHLY recommended that you run lbssg in a heavily chrooted
  environment, if you're using this on a production web server.
* Disable symlinks in your web server

DO NOT use lbssg in a shared hosting environment, especially on a crontab or
other automated setup. You, the operator, must ensure full control of all
sites handled by your lbssg installation. For the time being, this is
recommended until lbssg is full audited for security, and modified
accordingly. Efforts have already been made, but this software is very new
and requires a lot of testing. In general, you should consider lbssg to
be *alpha* quality, at best. It is not yet ready for a stable release; it is
currently only *rolling release*, hosted in a Git repository.

lbssg only allows sites to be processed under `www/` in the
current working directory. You may choose to put `www/` under the git clone
of lbssg itself. Or you could specify the full path to the `build` script
and process files in a `www/` elsewhere.

Getting started
---------------

Simply make a directory named `www/` somewhere.

In `www/`, make a directory *per website*. For each website,
set `www/sitename/site/` as the document root in your web server.
lbssg expects this `site` directory to exist.

### Example website

Check the `www-example` directory, for an example website. Also look at the
logic in the file named `build`, in the main directory of the untitled static
site generator.

The Libreboot website is also hosted in a Git repository, if you want a
real-world example of a website that uses lbssg.

How to use
----------

You should use a normal Unix shell (`sh` implementations such as KSH, BASH,
ZSH, CSH etc will all work). This, along with Pandoc.

See: <https://pandoc.org/>

You must then install a web server. The document root for your domain should
point to the `www/sitename/site` directory, where `sitename` is the name of
your site, e.g. `libreboot`, and `www/` is created under your instance of
lbssg.

The way lbssg is designed, you use it directly, running directly on the
server that then hosts the HTML files. However, it is possible to use it in
other ways; for example, you could rsync the resulting files over ssh, to your
server, while running lbssg locally. It's up to you.

### Generate entire websites

For security reasons, lbssg will only operate on sites under `www/sitename/`
where `sitename` can be anything.

To interact with `untitled`, your current working directory should be the main
directory *of untitled*, *not your website*. In that directory, the file named
`build` and the file named `clean` will exist.

You can have untitled go through an entire website and, if it detects that such
an action is needed, it will build each page. It will check whether a page
needs to be built. For example, if it was already built and no changes were
made, it will skip that page. It does this, using `date -r file +%s` to check
file modification dates. Files ending in `.date` are created, for files that
are tracked by lbssg.

For a given directory name under `www/`, do this:

	./build sites www/directoryname

You can also do:

	./build sites directoryname

For example, if your website was hosted at www/foobar:

	./build sites foobar

This will only work if your website is set up correctly. Look at the example
website in `www-example/` and read the sections below.

To build *all* available websites under `www/`, just run it without an argument:

	./build sites

You can specify *multiple* websites. For example:

	./build sites blogsite catblogsite

In this example, the following sites would be built:

* `www/blogsite`
* `www/catblogsite`

NOTE: If you simply run `./build` without any arguments, the `./build sites`
command will be executed.

### Build specific pages

These commands will only operate on pages under `www/sitename/` where `sitename`
can be anything. This is for security reasons.

If you want to save time (e.g. if you're just editing a few pages), you can
skip checking the entire site (or sites) and update just that page. Use this
command:

	./build pages www/sitename/site/path/to/page.md

You can also do:

	./build pages sitename/path/to/page.md

lbssg will automatically correct the path, if `www/` and `site/` are missing
from the path. For example, to update the homepage:

	./build pages sitename/index.md

You can specify multiple pages, *on multiple sites*. For example:

	./build pages joebloggsandhisdog/index.md janedoesblog/news/guitar.md

NOTE: the `mksitemap` and `mknews` functions are not automatically executed
when running these commands.

NOTE: if you run `./build pages` without specifying any paths to pages, then
the `./build sites` command will be executed instead. However, if you specify
paths and none exist, nothing will happen.

NOTE: If you simply run `./build` without any arguments, the `./build sites`
command will be executed.

### Cleaning up

If you want to un-make your site, such that the HTML files are purged, do this:

	./clean

This will clean *all* sites. If you only want to clean a specific site, do
this:

	./clean sitename

(sitename would be located at `www/sitename/`, in this example)

NOTE: an HTML file is only deleted if a corresponding Markdown file exists.
This means that if you permanently delete a markdown file from your site, you
should ensure that the HTML file is also delete dif you don't want that file
to be available anymore.

### Rolling builds

If you're running lbssg directly, SSH'd into your httpd machine, you
might want to update an entire website from scratch. This ordinarily means
that you must run `./clean`, first - in other words, the website will be
offline (HTML files deleted) while it re-builds.

To prevent this, you can use `roll` instead of `build`. For example:

	./roll sites libreboot

This also works with pages:

	./roll pages libreboot/index.md

You can also do this without argument, as with `build`, and it would build
everything. e.g.:

	./roll
	./roll sites
	./roll pages

This is especially useful for very large websites, where an entire re-build
might take 30 seconds or more.

Markdown files
--------------

Write your markdown files, with file extensions ending in __*.*md__ and the
build script will automatically see them. It will convert them into HTML,
stripping off the __*.*md__ file extension and replacing it with `.html`

Place these markdown files anywhere under `www/sitename/site/` where `sitename`
is whatever you named your website, per directory hierarchy rules of lbssg.

Again, the `www-example` directory can be extremely useful if you want to play
around first, before you make a real website with lbssg.

Files in www/sitename/
----------------------

These files affect the site globally, for each website under `www/`

Each site has its own directory: www/sitename/

`sitename` can be anything you want. e.g. `kittenblog`

www/sitename/site.cfg
---------------------

**REQUIRED**

sitename can be anything.

site.cfg is the main configuration file for your website.

Example entries (for libreboot.org):

```
TITLE="Libreboot"
CSS="/global.css"
DOMAIN="https://libreboot.org/"
BLOGDIR="news/"
DEFAULTLANG="en"
```

NOTE: Older versions of untitled had TITLE like this:

```
TITLE="-T Libreboot"
```

You must now omit the -T option, because it's hardcoded in the build script.

### LAZY

If `LAZY="y"`, lazy image loading will be enabled. This is useful for
conserving bandwidth. The default behaviour is `LAZY="n"` if not set
to `"y"`. See:

<https://developer.mozilla.org/en-US/docs/Web/Performance/Lazy_loading>

### DEFAULTLANG

This is optional. If unspecified, the default language will be English. Also,
if this entry defines an unsupported language, lbssg will default to
English.

It specifies the default language, for your website. It may
be that you have multiple languages available on your website,
where __file*.*md__ is your chosen language (e.g. German, Japanese, Englist)
and __file.xx*.*md__ is another language for the same page. xx can be anything.

### BLOGDIR

This is the directory for your main news section, if you have a news section.
You do not need to declare this.

The news section is only valid if it has the right files, in the directory
specified here. You can actually have as many news/blog sections as you like,
but this setting specifies the "main" one. For example, if you're a software
project you might have a news section, and a release section, and they might
both be news pages. News pages have RSS feeds and indexes automatically
generated by `untitled`.

If your website is primarily a blog, you might leave this string empty. That
way, your *home page* is the news index. In that scenario, you would not place
an index file in the root of your website. Let `untitled` generate it for you!

### DOMAIN

This is the URL for the home page of your website.

### CSS

Specifies the .css stylesheet that is to be used. See above example. Just add
an entry saying `/path/to/css/file` for the CSS file. The `/` is the root
of your website, when viewed publicly. Basically, / would be the home page,
and everything after that is relative. In other words, it is the URI of your
CSS file.

The actual domain name is not prefixed, when the CSS stylesheet path is given
inside the generated HTML document.

### TITLE

Use the format above. Don't use spaces or special characters. Just keep it
simply. E.g. `-T Gardening`

This would usually be the name of your website, company, project or whatever
else.

### SITEMAP

OPTIONAL.

Write this in `site.cfg` for a given site:

```
SITEMAP="n"
```

If you do this, the sitemap will not be generated. lbssg will otherwise
generate a sitemap, by default.

site/template.include
---------------------

**REQUIRED**

Put a pandoc template here, in this file.

There is an HTML5 template in `www-example/` which you can adapt for your site.
TODO: make an xhtml template. Patches welcome!

It is recommended that you put your common site navigation section in this
file (formatted in HTML).

site/nav.include
----------------

**optional**

Put this in the root of `www/sitename/site/`. You can actually put whatever
you want in here, but it can be used for a navigation menu. You could also use
it to put a side-wide notice, on your website.

Write this file in Markdown. You could also put a common navigation section
in the template, and use `nav` files for site-wide announcements, or for
announcements on specific pages. How you use lbssg is entirely up to you!

site/global.css
---------------

**optional**

The filename can actually be anything, for your CSS. In fact, it's entirely
optional. You can leave the CSS string empty in `site.cfg` and just have a
site without CSS.

The name `global.css` is merely what the example website (in `www-example/`)
uses, but you don't have to use this file name yourself.

This file is for styling your pages (HTML ones, after building your Markdown
files).

w3schools has a nice set of tutorials for learning CSS:

<https://www.w3schools.com/css/>

site/sitemap.html
-----------------

The file named __sitemap*.*md__ will be automatically generated, which is then
assembled into `sitemap.html`. This file will sit on the root directory of
your website.

Do not manually create these files. They are automatically generated by
lbssg. They simply index your entire website.

NOTE: It only looks at pages where a Markdown file exists. It ignores pages
where an HTML page exists but a Markdown page doesn't.

site/sitemap.include
--------------------

**optional**

This file MUST be included if you want a sitemap with your own text.

If this file is missing, a sitemap will be generated but it will contain a
generic header on the page.

Simply put a title and some text in here. lbssg and Pandoc will do the rest!

site/footer.include
-------------------

**optional**

If present, this will add a common footer to every page on your website. Write
it in Markdown.

Rules for Markdown files
------------------------

This applies to any __*.*md__ file under `www/sitename/site/` where `sitename`
is whatever you named your website, per directory hiearchy rules.

The file named __file*.*md__ will be converted into HTML and copied (in HTML
format) to `file.html`.

When you link to other pages, use absolute links or relative links, but don't
link to the `.html` pages. Link directly to the __*.*md__ files. lbssg
static site generator will use Sed to replace these with .html when generating
the HTML pages (it only skips replacing *external* links,
e.g. `https://example.com/cats.md`

### TOC (Table of Contents)

See pages in `www-example/` or example websites linked above (such as the
Libreboot project website). Look in the markdown files for those sites, on
pages that specify `x-toc-enable: true`. This is a special directive for the
Pandoc variant of Markdown. Some other Markdown parsers/generators will also
generate a table of contents.

In the untitled static site generator, when a TOC is enabled, the depth is 4.
This means that, in HTML, the h1, h2, h3 and h4 tags will show up on the TOC.
The TOC is generated *based on* how you use headers in Markdown, and how they
ultimately end up when converted into HTML.

### file.nav

If present, the page from __file*.*md__ will have its own navigation menu,
which will override the default nav.include file.

This can be anything. Write it in Markdown. For example, you could use this to
put announcements on a page.

### file.css

Where you have __file*.*md__, you can also include `file.css`, and this CSS
file will be applied specifically to that page.

This does not replace any global CSS files that you specified in `site.cfg`. It
is applied *after* that one. One possible use-case for this is if you have an
otherwise decent CSS file for your site, but you need to override certain
rules on a specific page.

### file.template

Where you have your Markdown file, strip off the `md`, you create a new file
with the same name but with `.template` file extension. This will override the
default, global template, for that page only.

### file.footer

If present, this replaces the global footer for the specific page.

News sections
-------------

This section defines what files should be placed in directories that are to
be news sections. For instance, you might have a directory on your website
named `news/`, and you would place these files in that directory.

An index page will be automatically generated, so don't bother making one
yourself. Simply write your news pages. Read the notes below for how to set up
news sections on your website.

An RSS feed will also be generated, for every news section.

*You can have as many news sections as you would like!*

### Rules for news pages

A table of contents is always enabled, on news pages (but not the news index),
unless you want it to be so. Technically, news indexes have a TOC but the page
isn't formatted such that a TOC will appear when Pandoc does its thing.

Start your news page like so, in the Markdown
file e.g. __www/catblog/site/news/spots-birthday*.*md__

```
% Page title
% Author name
% Date

Page text goes here
```

### MANIFEST

Place this file in the root of the directory where you want there to be a
news section.

The order of the file names dictate what order the links will appear in, on
the news index that is automatically generated (see below).

### news-list*.*md.include

This will have a page title, and introductory text. When the index page is
generated (see below), the resulting index will include the contents of this
file, and then links to each page per the contents of `MANIFEST`.

### index*.*md and index.html

Do not place these files in directories containing a MANIFEST file. These will
be generated automatically, with an index linking to news articles.

### news.cfg

In this file, write the following lines:

```
BLOGTITLE="The daily grind"
BLOGDESCRIPTION="Hot takes, juicy gossip and everything inbetween!"
```

You can change these strings to whatever you want. Keep it simple! For
example:

```
BLOGTITLE="Bianca's worldly musings"
BLOGDESCRIPTION="Come see what I'm thinking about! I have many thoughts"
```

For news about your software project, you might say:

```
BLOGTITLE="Project announcements"
BLOGDESCRIPTION="News about ongoing development will be documented here"
```

### RSS files

Files ending in `.rss` are automatically generated, if you have news sections.

Translated pages
----------------

lbssg supports having translated pages.

Here is an example of a website with multiple translations available, built
using untitled:\
<https://trans.chat/>

### Set a default language

English is the default in lbssg, but you can *change* the default language!
For example, you might want a Chinese-language website but offer translated
pages in German. lbssg will happily let you do this!

Simply set the `DEFAULTLANG` entry in `site.cfg`.

### How to translate pages

The way it works is quite simple:

* __file*.*md__ is the original file, in your default language. The *default*
   default language is English (en), but it can be anything.
* __file.xx*.*md__ is the translated file. xx can be anything, e.g. de, nl, pt
* `.include` files (footers, nav files, templates etc) are for your default
   language
* `.xx.include` is the translated file. again, xx can be anything e.g. de, nl

For translated pages, if a translated `.include` file is unavailable, the
default-language one will be used.

You can *also* do this per-page. For example, you might override the default
footer on a page, by including `file.footer`. In this
situation, __file.de*.*md__ will also use `file.footer`. HOWEVER, you can
include `file.de.footer` aswell, if you so choose!

### Translations done automatically

Translations are handled *automatically*, *per page*. It does not happen
globally; this way, a website doesn't have to have translated versions of each
page. In Let's look at example scenarios:

#### file*.*md, file.pt*.*md, file.pl*.*md

In this example, a language menu will appear on each of these pages, allowing
selection of DEFAULT(e.g. English), Polish or Portuguese

#### file.pt*.*md, file.pl*.*md

In this example, no *default* page is included, but Polish and Portuguese are
available. In this example, Portuguese and Polish will be available in a
language select menu on each page.

### template.LANGUAGE.include

This is *optional*, so long as `template.include` is included. The translated
file will override. This will override the default template, on the specified
language.

For example: `site/template.de.include`

### file.LANGUAGE.template

You might have `file.template` overriding the default global `template.include`
or `template.LANGUAGE.include` for a given page.

lbssg *also* lets you override it on a specific *translation* of that page!

For example: `site/file.de.template`

### nav.LANGUAGE.include

Override the default global navigation file, for a given website and a given
language.

For example: `site/nav.de.include`

### file.LANGUAGE.nav

You might have `file.nav` overriding the default global `nav.include`
or `nav.LANGUAGE.include` for a given page.

lbssg *also* lets you override it on a specific *translation* of that page!

For example: `site/file.de.nav`

### footer.LANGUAGE.include

Override the default global footer file, for a given website, on a given
language.

For example: `site/footer.de.include`

### file.LANGUAGE.footer

You might have `file.footer` overriding the default global `footer.include`
or `footer.LANGUAGE.include` for a given page.

lbssg *also* lets you override it on a specific *translation* of that page!

For example: `site/file.de.footer`

### file.LANGUAGE.css

You might have `file.css` available for __file*.*md__.
If __page.LANGUAGE*.*md__ exists, `file.LANGUAGE.css` will still be added to
that page, but that page can also have its own `file.LANGUAGE.css`.

The `file.LANGUAGE.css` file does not override `file.css`. It is merely added.

If `file.LANGUAGE.css` exists but `file.css` does not, it will still be
applied, but only on __file.LANGUAGE*.*md__.

lang/xx/strings.cfg
-------------------

### Add new language support

On language menus, if a language is unsupported, the two-letter code will be
used. For example, if Russian is unsupported, `ru` will be the anchor text for
selecting Russian, but you can still select it.

In untitled, you can see the file `lang/en/strings.cfg`.

In there, you will also see translated ones, e.g. `lang/pl/strings.cfg`.

If one does not exist for your language, you can create one. Patches welcome!

Check `lang/en/strings.cfg` to see how it should be done! Entries
in `strings.cfg` are as follows:

Example `strings.cfg` file:

### LANGNAME

This is the anchor text for a given language, on a language select menu, if
a site is to have translations. For example, `de` would be `Deutsch` here, for
the German language.

Example strings:

* Deutsch
* English
* Nederlands
* Espanol

### LANGSELSTR

This is the text for the language selection menu. **Currently not used**

E.g. "Select language:"

### BACKLINK\_PREVDIR

This is the index link, for the previous directory. This link should be `../`

### BACKLINK\_CURRENTDIR

This is the index link, for the current directory. The link should be `./`

### PUBLISHED\_BY

For news pages, this string says e.g. "Article published by:"

### PUBLICATION\_DATE

For news pages, this string says e.g. "Article published on:"

### MARKDOWN\_LINK

This is the anchor text for the Markdown link, to each Markdown file. lbssg
displays the link to the original Markdown file, for each page, on each given
page.

### RSS\_LINK

This is the anchor text for the default feed, if a news feed is generated on
a given site.

The link should be `/feed.xml`

### SITEMAP\_LINK

This is a Markdown link to the site map, if one is included on a given site.

The link should be `/sitemap.md`

### SHAMELESS\_PLUG

At the bottom of each page, a string of text can be inserted. By default, this
is a string that says the page was generated by the lbssg Static Site
Generator, along with a link to the lbssg website.

### PDIR

The `PDIR` option should be `ltr` or `rtl`. This defines which order the text
should be in. Some languages, such as Arabic and Hebrew, are right-to-left
languages instead of left-to-right.

Other files
-----------

Libreboot static site generator only cares about the files specified on this
page. It ignores all other files.

For things like images, videos and other binary files, it is recommended that
you create a specific directory or subdomain for them, and have only markdown
files on your lbssg website.

However, it's up to you how you run your infrastructure.

It is recommended that you use a Git repository to host your website. Then you
can easily keep track of changes to your site. You would use a static version
of lbssg on your server, and just build new changes to your site whenever
you push them. This is actually *why* the `site/` directory is enforced under
the `www/sitename/` directory for each site, so as to encourage good practise
(because by doing it this way, the `.git` directory will not be visible on your
public web server. You do *not* want that directory to be visible!)

Regarding Untitled -> lbssg transition
---------------------------------------

Untitled Static Site Generator renamed to Libreboot Static Site Generator and
became part of the Libreboot project, on 25 January 2025.

The entire website previously hosted at **https://untitled.vimuser.org/** has
been fully assimilated into the Libreboot website.

The old Untitled Static Site Generator git repositories were:

* <https://codeberg.org/vimuser/untitled>
* <https://codeberg.org/vimuser/untitled-website>
* <https://notabug.org/untitled/untitled>
* <https://notabug.org/untitled/untitled-website>

The old repositories have been retained, but are now archived. You can find
the new *lbssg* repositories linked from the [Libreboot Git
page](../../git.md).
