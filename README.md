# WordPress for Mere Mortals

A large share of the world's websites —
news sites, shops, club pages, parish
newsletters — are **WordPress**: a
publishing system written in **PHP** that
stores everything it knows in a database
and assembles each page when a visitor
asks for it. You install it on a server
you control, log in through a web form,
and write. This guide installs the real
thing by hand — no one-click installer,
no hosting panel — so that when it works,
you know *why* it works, and when it
doesn't, you know which part to blame.

The lab behind this guide ran genuine
**WordPress 7.1.4**, the current release
tarball from wordpress.org, its checksum
checked against the sum wordpress.org
publishes. Underneath it, the lab
assembled the stack itself from Ubuntu
packages unpacked into a folder:
**PHP 8.3.6** to run the code and
**MariaDB 10.11** to hold the data,
reached over a socket file rather than a
network port, with PHP's own built-in
web server answering on the loopback
address. On that stack the lab built a
fictional village newsletter, *The
Millbrook Weekly*, with an invented
editor, and ran the real web installer
by sending its form the way a browser
sends it.

Then it did the everyday jobs, and
proved each one from the outside: the
front page carrying the site's title, a
login through the real login form, a
post about the village fête published
through the **REST API** and read back
in three places, a draft that readers
provably cannot see, a theme switched
with the change visible in the served
HTML, a reader's comment held for
moderation and then approved into
public view, an update check that really
talked to wordpress.org, and a backup —
database and files — that was read back
to prove it contains the site.

What the lab could not do, it did not
fake. It sent no real email, because
nothing here can. It did not put the
site on the public internet, give it a
real domain or a TLS certificate, or
run it under Apache or nginx — the
built-in PHP server is a lab bench, not
a production web server, and chapter 16
says so plainly. Two bundled plugins
were switched on and off; the plugin
marketplace, WooCommerce and multisite
are taught from the documentation and
labelled as such. `criteria.md` keeps
the full ledger of what was run and
what was only read.

## What this guide covers

- [**What WordPress is**](what-wordpress-is.md) —
  a publishing system, not a page
  builder: who it is for, what it costs,
  and the deal it makes with you
- [**The moving parts**](the-moving-parts.md) —
  core, the database, themes, plugins,
  `wp-content` and `wp-config.php`, and
  which part holds what
- [**What you need before installing**](before-you-install.md) —
  PHP, a database and a web server: the
  three legs, what each one does, and
  how this lab assembled them
- [**Installing WordPress**](installing-wordpress.md) —
  the download and its checksum, the
  database and its user, and the config
  file with its secret salts
- [**The five-minute install, honestly**](the-five-minute-install.md) —
  the one form that creates a site, what
  the installer writes to the database,
  and the Success page checked against
  the tables it made
- [**First checks**](first-checks.md) —
  the front page, the login form, the
  dashboard, and the error a wrong
  password earns you
- [**Posts and pages**](posts-and-pages.md) —
  the two kinds of writing, publishing
  and drafting, permalinks, and who can
  see a draft (nobody — proven)
- [**Themes**](themes.md) — what a theme
  controls and what it cannot touch,
  the three bundled themes, and a switch
  you can see in the page source
- [**Plugins**](plugins.md) — the two
  bundled plugins switched on for real,
  the API key Akismet wants, and the
  marketplace treated with respect
- [**Users and comments**](users-and-comments.md) —
  accounts and roles, a second user with
  less power, and a comment's journey
  from held to approved to public
- [**WP-CLI**](wp-cli.md) — the command
  line companion: checking the install
  against official checksums, changing
  settings, and doing in one line what
  the dashboard does in six clicks
- [**The REST API**](the-rest-api.md) —
  the site as JSON: reading posts with
  no password at all, and publishing
  with an application password — after
  the fix that makes WordPress honour it
- [**Keeping it updated and backed up**](updates-and-backups.md) —
  update checks that really phone home,
  and the two-part backup (database plus
  files) proven by reading it back
- [**Recipes**](recipes.md) — a page, a
  contributor, a new tagline, scheduling
  and moving house, each with its check
- [**The neighbours**](the-neighbours.md) —
  Ghost, BookStack, the static site
  generators, and wordpress.com: what
  each does that WordPress doesn't, and
  vice versa
- [**Limits**](limits.md) — a lab server,
  no email, the security burden that is
  now yours, and the edges of what one
  guide can prove
- [**Troubleshooting**](troubleshooting.md) —
  the checksum that isn't there, the
  config files that leak in from the
  host, the 401 that isn't a wrong
  password, and the traps this guide's
  lab fell into
- [**Glossary**](glossary.md) — the
  terms, collected in one place

## What it doesn't cover

Running WordPress as a public internet
service: choosing a host, wiring Apache
or nginx and PHP-FPM, TLS certificates,
DNS, and sending real email. Those are
production jobs with their own guides in
this series behind them (the PHP guide
covers the language; the nginx and Caddy
guides cover the servers in front).
WooCommerce and online selling, multisite
networks, and building your own theme or
plugin are each a guide of their own;
here they are named, placed, and left
honest. Nothing in this guide needs a
wordpress.com account, and the lab's
site, editor and readers are invented.

## How to use this guide

Read chapter 1 for the model and
chapter 2 before installing anything —
most WordPress confusion is two parts
being mistaken for each other, and the
moving-parts map prevents it. Chapters
3 to 5 get you a running site with an
editor who can log in; chapter 6 proves
it from the outside. After that the
chapters stand alone: themes when you
care how it looks, WP-CLI when clicking
gets slow, the REST API when a program
needs to read or publish. The lab's
transcripts live in `wordpress-lab/`,
numbered in the order the work happened.

---

## Part of the series

This guide is one of the [For Mere Mortals](https://paulsullivan33-dev.github.io/mere-mortals/) guides: plain-language guides to the tools programmers and makers use.

Related guides:

- [PHP for Mere Mortals](https://paulsullivan33-dev.github.io/php-for-mere-mortals/)
- [Ghost for Mere Mortals](https://paulsullivan33-dev.github.io/ghost-for-mere-mortals/)
- [BookStack for Mere Mortals](https://paulsullivan33-dev.github.io/bookstack-for-mere-mortals/)
- [SQL for Mere Mortals](https://paulsullivan33-dev.github.io/sql-for-mere-mortals/)
- [nginx for Mere Mortals](https://paulsullivan33-dev.github.io/nginx-for-mere-mortals/)
- [Caddy for Mere Mortals](https://paulsullivan33-dev.github.io/caddy-for-mere-mortals/)

## License

The writing in this guide is licensed under [CC BY 4.0](https://github.com/paulsullivan33-dev/wordpress-for-mere-mortals/blob/main/LICENSE). Code examples and lab scripts are licensed under the [MIT License](https://github.com/paulsullivan33-dev/wordpress-for-mere-mortals/blob/main/LICENSE-CODE).
