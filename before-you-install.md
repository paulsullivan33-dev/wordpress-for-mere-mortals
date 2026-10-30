# What You Need Before Installing

WordPress has three requirements, and
installing it goes smoothly exactly when
all three are standing before you start.
This chapter names them, then describes
honestly how this guide's lab met them —
which is *not* how you would do it on a
real server, and the difference matters.

## The three legs

1. **PHP, version 7.2.24 or newer** —
   WordPress is written in PHP, so
   something must run PHP. The current
   release recommends PHP 8.3. PHP also
   needs a handful of **extensions** —
   add-on modules for talking to the
   database (`mysqli`), handling images
   (`gd`), fetching URLs (`curl`),
   unpacking zip files, and handling
   text in any alphabet (`mbstring`,
   `intl`). A missing extension rarely
   stops WordPress outright; it quietly
   removes an ability instead.
2. **A database: MySQL 5.5.5 or newer,
   or MariaDB 10.1 or newer.** MariaDB is
   the community fork of MySQL and the
   usual choice on Linux. WordPress will
   also state its database user and
   password in its config file, so the
   database needs an account made for it
   (chapter 4) — never the database's
   own root account.
3. **A web server** — something that
   accepts visitors' HTTP requests and
   passes page requests to PHP. In
   production that is Apache or nginx,
   usually with PHP-FPM behind them.

Most hosting packages and most one-click
installers bundle all three, which is
why many people meet WordPress without
ever seeing this list. Meeting it is the
point of this guide: when a real site
misbehaves, the fault is in one leg or
in WordPress, and you will know which
questions to ask.

## How the lab stood them up

The lab had no hosting package — just a
bare Linux sandbox. So it assembled the
legs from Ubuntu's own packages,
downloaded without installing and
unpacked into a lab folder, the route
this series' PHP guide established:

- **PHP 8.3.6**, unpacked from Ubuntu's
  `php8.3` packages, run with a lab
  `php.ini` listing exactly the
  extensions WordPress wants. The full
  module list is transcript `t01`; every
  extension named above is present.
- **MariaDB 10.11.14**, unpacked the
  same way, with its data directory
  inside the lab and — a deliberate
  choice — **no network port at all**.
  It listens only on a *socket file*, a
  doorway on the filesystem that only
  programs on the same machine can use.
  WordPress's config points at that
  socket (chapter 4), and transcript
  `t02` shows the server answering.
- **PHP's built-in web server**
  (`php -S`), answering on the loopback
  address, port 18080, with a small
  router script that hands any address
  that isn't a real file to WordPress —
  the same job a rewrite rule does under
  Apache or nginx, in nine lines.

Two traps from this assembly are worth
meeting here, because on a normal
install the package manager absorbs
them silently. First, the unpacked PHP
also read the *host* machine's PHP
config and loaded half its modules
twice; the lab's wrapper points PHP's
config-scan at an empty lab folder so
only the lab's own settings speak.
Second, the unpacked MariaDB read the
host's MySQL config and refused to start
on settings meant for a different
setup; a lab config file of its own
ended the argument. Both failures are
kept verbatim in the transcripts and
explained in chapter 17. The lesson
generalises: **a program's config comes
from somewhere, and knowing where is
half of troubleshooting.**

What this assembly cannot prove is
anything about production: PHP's
built-in server handles one request at
a time, speaks no TLS, and is documented
by PHP itself as a development aid. The
lab claims nothing for it beyond the
lab. Chapter 16 returns to this.
