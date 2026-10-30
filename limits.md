# Limits

What this guide and its lab did not do,
collected in one place so nothing in the
earlier chapters quietly over-claims.

## The lab is not a deployment

- The web server is PHP's **built-in
  development server**: one request at a
  time, no TLS, and documented by PHP as
  a testing aid. Nothing here measures
  how WordPress performs in public, and
  no page of this guide should be read
  as "how to host". Production wants a
  real web server (the nginx and Caddy
  guides), PHP-FPM, TLS, and decisions
  about caching that a loopback lab
  cannot even stage.
- The database listens on a **socket
  file** with no network presence, and
  the whole stack runs as one lab user
  on one machine. Real deployments add
  users, permissions, firewalls and
  backups of the database server itself.
- There is **no email**. Password
  resets, comment notifications and
  Akismet registration all presume a
  mail path this sandbox does not have;
  where the guide met those doors, it
  named them rather than opening them.

## What popularity costs

WordPress's scale cuts both ways, and a
guide that skipped this would be
advertising. A large fraction of all
compromised websites are WordPress
sites — not because the core is uniquely
weak (it is professionally maintained
and promptly patched), but because it is
*everywhere*, and because sites accrete
plugins and then stop being updated.
Running WordPress means owning a small,
permanent maintenance duty: updates
(chapter 13), few and reputable plugins
(chapter 9), few administrators
(chapter 10), and backups that live
somewhere else. The duty is light; it is
not optional, and it never ends while
the site is public.

## Edges of the software itself

- **One site per install** is the core
  model. **Multisite** — one install,
  many sites — exists and is
  documented-only in this guide; it
  changes backups, updates and failure
  modes enough to deserve its own.
- The editor is powerful and
  occasionally bewildering; block
  themes and the Site Editor are still
  evolving between releases, so
  dashboard directions age faster than
  anything else in this guide.
- The API's openness (chapter 12) means
  usernames and published content are
  enumerable by design; hardening
  plugins that promise otherwise are
  fighting the platform's nature, and
  this guide installed none to test.
- WooCommerce turns WordPress into a
  shop of real consequence — payments,
  tax, customer data — and is a
  different risk class entirely. Named,
  not covered.

## Edges of this guide

Everything verified was verified on
WordPress 7.1.4, PHP 8.3.6 and MariaDB
10.11, on one Linux sandbox, in October
2026. WordPress releases several times a
year; the moving parts will outlast the
version numbers, but screens and a few
defaults will drift. Where the guide
describes rather than demonstrates —
marketplace plugins beyond the bundled
two, an actual version upgrade,
multisite, wordpress.com, production
hosting, real email — `criteria.md`
says which and why, and the chapters
say it again in place.
