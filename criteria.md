# Writing criteria used in this guide

This guide was drafted against the
following criteria. They are deliberately
strict: the test of a how-to is that the
reader who follows it is touching the
real thing — a genuine installation of
WordPress, maintained the way real sites
are maintained — not a mocked *WordPress-
like* demo layer or a drifted dialect.

1. **Lead with the outcome, not the
   command.** Every chapter opens with
   what will be true when the topic is
   understood, and every procedure ends
   with how the reader knows it worked.
2. **Every instruction must be
   reproducible by the reader.** Commands
   shown are commands actually run
   (transcripts in `wordpress-lab/`,
   numbered in the order the work
   happened); the site, editor and
   readers are invented and named as
   such.
3. **Technical accuracy over brevity.**
   Distinctions that matter later are
   made when they first appear: files vs
   database, theme vs plugin, post vs
   page, published vs draft, the software
   vs wordpress.com.
4. **No false generality.** Anything the
   guide claims works has been run: the
   web installer, the login, publishing
   via the REST API, draft invisibility,
   a theme switch, plugin activation, a
   held-then-approved comment, an update
   check and a backup read-back. What
   has not been run is declared
   documented-only, in place and in the
   ledger below.
5. **Where the real system disagrees
   with the pitch, print the real one.**
   The "five-minute install" is measured
   honestly (chapter 5); the missing
   published SHA-256 for the `latest`
   alias is stated, not smoothed over
   (chapter 4); the application-password
   401 is reproduced with its real cause
   (chapter 12); Akismet *active* is
   distinguished from Akismet *working*
   (chapter 9).
6. **Never teach a dangerous shortcut.**
   The database gets a dedicated user
   with least privilege; the salts are
   generated, never left as placeholders;
   no real credentials or personal data
   appear anywhere; the invented lab
   passwords are printed only because
   the lab is fictional and disposable,
   and the guide says so at the point of
   printing.
7. **Prefer plain words.** Jargon is
   admitted once, defined where it first
   appears and collected in the
   glossary.
8. **Mark speculation as speculation.**
   Marketplace judgement, upgrade
   mechanics and production practice are
   presented as advice or documentation,
   never as observed fact.
9. **Failure is instruction.** Every
   dead end the lab met is kept with its
   fix: the checksum that isn't
   published, the host's PHP and MySQL
   config bleeding into the lab's
   unpacked stack, the socket-path
   `DB_HOST`, and the WP-CLI Phar wart.

## Scope note

The lab assembled its platform from
Ubuntu packages downloaded without
installing and unpacked into a lab
folder — PHP 8.3.6 and MariaDB 10.11.14,
the standard Ubuntu builds of the exact
software WordPress runs on — because
the sandbox has no system package
manager access for this purpose. The
route is the one this series' PHP guide
established. MariaDB was the brief's
*first-choice* database route and it
worked: initialised in the lab, reached
over its unix socket with no TCP port,
and it served the site throughout. The
fallback (the official SQLite database
integration plugin) was never needed,
and the guide says so rather than
reviewing a road not taken. The sandbox
shell runs as root; where a tool cares
(WP-CLI's `--allow-root`), the guide
discloses it. The web server is PHP's
built-in development server, and the
guide claims nothing for it beyond the
lab.

## Verification ledger

Verified live in the lab (transcript in
`wordpress-lab/`):

- Download of `wordpress.org/latest.tar.gz`;
  version read from the archive as
  **7.1.4**; local MD5 compared equal to
  the MD5 wordpress.org publishes for
  7.1.4; every core file later verified
  against the official checksums API
  (`wp core verify-checksums`) — t00, t09
- PHP 8.3.6 from unpacked Ubuntu
  packages with the extensions
  WordPress needs; MariaDB 10.11.14
  initialised, started on a lab socket,
  and logged into as the dedicated
  WordPress user — t01, t02
- The real web installer, driven by
  form POST: site *The Millbrook Weekly*
  and administrator `millbrook_editor`
  created, `Success!` returned, twelve
  core tables present — t03, t04
- Front page `200` with the site title;
  login through `wp-login.php` with a
  cookie jar (redirect, `wordpress_logged_in_`
  cookie, dashboard, named greeting);
  wrong-password error verbatim — t05, t06
- Post published through the REST API
  with an application password (`201`,
  post 5), proven on the front page, at
  its permalink, and in the public REST
  list — t07
- Draft post absent from the front page
  and the public REST list, present in
  the editor's WP-CLI list — t08
- WP-CLI from the official phar, its
  SHA-256 equal to the published sum;
  core version, checksum verification,
  theme list and a theme switch proven
  in the served HTML, plugin list,
  permalink structure change — t09
- Plugin activation and deactivation
  (Hello Dolly, Akismet) — t09
- A reader comment submitted
  unauthenticated, held (`unapproved`,
  absent from the public page), then
  approved and publicly visible — t10
- Update checks against wordpress.org:
  core "at the latest version", plugins
  `update: none`; backup by `wp db
  export` plus a `wp-content` archive,
  read back and searched — t11
- A published page at its own address,
  a contributor account created, the
  tagline changed and read back — t12

Documented-only (described, not run —
each named in its chapter):

- Production hosting and wiring: Apache
  or nginx with PHP-FPM, TLS, DNS,
  caching — the sandbox serves loopback
  with PHP's development server only
- Real email delivery (password resets,
  notifications), which the sandbox
  cannot send
- Akismet's spam judgement (needs a
  registered API key) and the
  wordpress.org plugin/theme
  marketplaces beyond the two bundled
  plugins and three bundled themes
- WooCommerce, multisite, and
  wordpress.com
- An actual core version upgrade (the
  install was already current; the
  update *check* above is live, the
  upgrade mechanics are documented)
- Child themes and the restore half of
  the backup drill (the export and
  archive were verified as containing
  the site)

Kept verbatim failures, with fixes:

- `latest.tar.gz.sha256` serves a 404
  page: wordpress.org publishes no
  SHA-256 for the `latest` alias — t00,
  chapters 4 and 17
- Unpacked PHP double-loading host
  modules via the host `conf.d`; fixed
  by pointing the config scan at the
  lab — t01, chapter 17
- MariaDB aborting on the host
  `/etc/mysql` settings it had inherited;
  fixed with a lab defaults file and a
  fresh datadir — t02, chapter 17
- Application-password publish refused
  with `rest_cannot_create` (401) over
  plain HTTP until
  `WP_ENVIRONMENT_TYPE` was declared
  `local` — t07, chapters 4, 12 and 17
- A WP-CLI subprocess fatal
  (`Class "Phar" not found`) in this
  hand-assembled PHP, after which the
  command completed and worked — t09,
  chapters 11 and 17
