# The Moving Parts

A WordPress site looks like one thing
from outside — pages appear when you
ask — but inside it is five parts with
clear borders. Almost every confusion,
every scary error message, and every
piece of forum advice gets simpler once
you can name which part is involved.

## The core

The **core** is WordPress itself: a few
thousand PHP files you download as one
archive. It contains the machinery —
the installer, the login system, the
editor, the publishing rules, the REST
API — but almost none of *your* site.
Delete the core files and reinstall them
and, if the rest survived, your site
comes back. The lab verified its core
file by file against wordpress.org's
official checksums (chapter 11), which
is only possible because the core is
identical on every site running that
version.

## The database

Everything that is *yours* — every post,
page, comment, user, setting and menu —
lives in a **database**, in the lab's
case MariaDB. The core asks it questions
in SQL: *give me the ten newest
published posts*. Twelve tables hold the
lot; chapter 5 lists them as the
installer creates them. This split is
the single most useful fact in the
guide: **files are replaceable, the
database is the site.** Backups
(chapter 13) are built on it.

## Themes

A **theme** is a bundle of templates and
styles that decides how the database's
contents are presented: layout, fonts,
colours, which pieces appear where. It
changes how the site *looks*, never what
it *says* — chapter 8 switches themes
and the same post stands unchanged in
the new clothes, proven from the served
HTML. Themes live in
`wp-content/themes/`, one folder each.

## Plugins

A **plugin** is extra PHP code that
hooks into the core and adds behaviour:
a contact form, a shop, spam filtering,
backups. Where themes change looks,
plugins change *abilities*. They are
also the site's biggest risk surface —
a plugin runs with the whole site's
privileges — so chapter 9 treats the
marketplace with respect and switches
the two bundled plugins on and off for
real.

## wp-content and wp-config.php

Two residents of the site's folder tie
the parts together:

- **`wp-content/`** is the one folder
  that is truly yours: themes, plugins,
  and everything you upload. The core
  never overwrites it during updates.
- **`wp-config.php`** is a single file
  sitting above the machinery, written
  by you (chapter 4): the database's
  name, user, password and address, plus
  eight secret keys — the **salts** —
  that sign login cookies. It is the
  most sensitive file on the site,
  because it holds the keys to the
  database.

## The stack underneath

Finally, WordPress cannot run alone. It
needs **PHP** to execute its code, a
**database server** to answer its
questions, and a **web server** to take
visitors' requests and hand them to PHP.
Those three legs are chapter 3 — and
the honest dividing line of this guide:
WordPress itself is installed and proven
here; the production versions of the
legs are the PHP, nginx and Caddy
guides' territory.
