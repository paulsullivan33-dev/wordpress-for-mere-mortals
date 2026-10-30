# Installing WordPress

Installation is four jobs: download the
core and check it, make the database and
its user, write the config file, then
let the web installer (next chapter)
build the site. This chapter does the
first three, in the open, with checks.

## Download, and check the download

WordPress lives at wordpress.org. The
address `https://wordpress.org/latest.tar.gz`
always serves the current release:

```
curl -sSL -o wordpress.tar.gz https://wordpress.org/latest.tar.gz
```

A download you cannot check is a
download you are trusting to luck and to
every network between you and the
server — the archive contains a program
that will hold your database password.
So check it. Here the lab met an honest
wrinkle, kept in transcript `t00`:
wordpress.org publishes a **checksum**
for each release, but as an MD5 sum for
the versioned file — and the convenient
`latest.tar.gz.sha256` address serves a
"page not found" page; there is no
published SHA-256 for the *latest*
alias. The lab's route, and a sound one:

1. Unpack the archive and read the
   version from `wp-includes/version.php`
   — the lab's said **7.1.4**.
2. Fetch the published MD5 for that
   exact version
   (`wordpress.org/wordpress-7.1.4.tar.gz.md5`)
   and compare it with the local file's
   MD5. They matched, digit for digit.
3. Later, with WP-CLI installed, re-verify
   **every core file** against the
   official checksums API — a stronger
   check than the archive sum, done in
   chapter 11 (transcript `t09`):
   `Success: WordPress installation verifies against checksums.`

MD5 is a weak checksum by modern
standards — fine for catching corruption
and casual substitution, not a proof
against a determined forger. The guide
says so rather than dressing it up; the
per-file verification in step 3 is the
stronger evidence.

Unpack the archive into the folder the
web server will serve — the **document
root**. The archive contains a single
`wordpress` folder; its *contents* are
the site.

## Make the database and its user

WordPress needs an empty database and a
user who owns exactly that database and
nothing else. On the lab's MariaDB,
over its socket (transcript `t02`):

```sql
CREATE DATABASE millbrook_wp
  CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'wp_millbrook'@'localhost'
  IDENTIFIED BY 'millbrook-db-press-7';
GRANT ALL PRIVILEGES ON millbrook_wp.*
  TO 'wp_millbrook'@'localhost';
```

Three ideas are doing work here.
`utf8mb4` is the character set that
holds every alphabet *and* emoji without
loss. The dedicated user means a
compromised WordPress can damage one
database, not the whole server — and
that you can change its password without
touching anything else. And the lab then
**logged in as that user** to prove the
grant works and sees only its own
database. (The password above is the
lab's invented one, printed because the
lab is fictional and disposable. Yours
should be long, unique, and not printed
anywhere.)

## Write wp-config.php

WordPress ships `wp-config-sample.php`;
your `wp-config.php` is that file filled
in. Four lines name the database:

```php
define( 'DB_NAME', 'millbrook_wp' );
define( 'DB_USER', 'wp_millbrook' );
define( 'DB_PASSWORD', '...' );
define( 'DB_HOST', 'localhost:/path/to/mariadb.sock' );
```

`DB_HOST` deserves a look. Usually it is
just `localhost`, meaning *a database on
this machine, at its usual socket*. The
lab's database listens on a socket in an
unusual place, so the path rides along
after the colon — the same setting also
accepts `host:port` for a database
reached over the network.

The sample's other famous block is the
eight **salts** — long random strings
that sign the cookies proving a visitor
is logged in. The sample ships them as
the placeholder *put your unique phrase
here*, and WordPress will run with the
placeholders — for a site whose login
security is then partly theatre.
wordpress.org runs a salt generator, and
generating your own locally is one line
of PHP per key; the lab generated all
eight from the machine's random source
and checked that **zero** placeholders
remained. Do the same, any way you like,
but do it: it is the cheapest real
security in the whole install.

One more line earns its place in a local
install, and chapter 12 depends on it:

```php
define( 'WP_ENVIRONMENT_TYPE', 'local' );
```

It tells WordPress *this site is a local
development site*, which among other
things lets it honour application
passwords over plain HTTP — a refusal
the lab met the hard way, in transcript
`t07`. On a public site with TLS you
would not need it.

With the three jobs done, the site
exists but is empty. Ask for its front
page and WordPress agrees — in the lab
it answered with a redirect:

```
GET / -> 302 -> http://127.0.0.1:18080/wp-admin/install.php
```

That redirect is the invitation to the
famous five minutes, next.
