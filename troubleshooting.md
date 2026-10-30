# Troubleshooting

The failures this guide actually met,
in the order a new installer is likely
to meet their cousins. Each is kept
verbatim in the transcripts; the pattern
underneath all of them is the guide's
whole method: read the error, name the
part, check the claim from outside.

## "The checksum file is a 404"

**Symptom:** `wordpress.org/latest.tar.gz.sha256`
returns a page that says nothing was
found there.
**What it means:** there *is* no SHA-256
published for the `latest` alias
(transcript `t00`). Read the version
from the unpacked `wp-includes/version.php`,
fetch that version's published sum —
the lab's release offers MD5 — compare,
and later re-verify every core file with
`wp core verify-checksums`, which uses
the checksums API and is the stronger
check anyway (chapter 4).

## PHP modules load twice

**Symptom:** warnings like *Module
"mysqli" is already loaded*, and a
server that may or may not start.
**What it means:** PHP is reading *two*
config sets — here, the lab's and the
host machine's, because an unpacked PHP
still scans the system `conf.d` folder.
On packaged installs this is the same
bug as a stray file in `conf.d`; the
fix is to know which directories your
PHP actually scans (`php --ini` names
them) and make the set deliberate. The
lab pointed the scan at its own empty
folder (transcript `t01`).

## The database reads someone else's config

**Symptom:** MariaDB refuses to start,
aborting on variables you never set
(the lab's said the host's `provider_*`
settings were unknown).
**What it means:** a reused defaults
file from another MySQL-family install
on the same machine is being read.
Give the server an explicit defaults
file of its own, start it, and keep the
host's away from it (transcript `t02`).
Its *io_uring* warning in this sandbox
— the kernel feature is unavailable, it
falls back to ordinary file I/O — is the
other kind of message: alarming-looking,
harmless, and worth recognising as noise
only after reading it once.

## Every post and page 404s, but the front page works

**Symptom:** after switching to pretty
permalinks, post addresses return 404.
**What it means:** the web server is not
handing unknown paths to WordPress.
Apache needs the rewrite rules in
`.htaccess` (and permission to use
them); nginx needs `try_files $uri
$uri/ /index.php?$args;`; this lab's
built-in server needed its router
script. The setting is fine; the
hand-off is missing. The lab saw the
mirror image of this while wiring its
router — same diagnosis, same chapter.

## The REST API says 401, and the password is right

**Symptom:** application-password
publishing returns `rest_cannot_create`
— *you are not allowed to create posts
as this user* — for an administrator
holding a fresh, correct password.
**What it means:** almost never the
password. WordPress honours application
passwords only over HTTPS, or on sites
declared `local`/`development`. On plain
HTTP with neither, it silently treats
the credential as absent and reports the
consequence instead of the cause.
Chapter 12 has the one-line config fix
for local work; the production fix is
TLS. Transcript `t07` keeps both the
failure and the recovery.

## "Error establishing a database connection"

**Symptom:** every page shows that one
sentence.
**What it means:** PHP reached
WordPress, and WordPress could not log
in to the database. Read `wp-config.php`
as the suspect list, in order: is the
database server *running*, do the name,
user and password match a real account,
and does `DB_HOST` point where the
server actually listens — socket path
and all (chapter 4)? The lab's defence
was having logged in as the WordPress
user by hand before WordPress ever
tried (transcript `t02`); that splits
"database broken" from "config wrong"
in one step.

## The login page loops, or complains about cookies

**Symptom:** correct password, straight
back to the login form — or *ERROR:
Cookies are blocked*.
**What it means:** the login ceremony
(chapter 6) depends on cookies surviving
the round trip, including the
`wordpress_test_cookie` set on the login
page itself. Browsers with aggressive
cookie settings, or a site served from
two different hostnames (with and
without `www`), break the chain. The
lab's cookie-jar transcript (`t06`) is
the reference for what a healthy
exchange looks like: test cookie out,
redirect back, `wordpress_logged_in_`
cookie issued.

## WP-CLI prints a Phar fatal, then works

**Symptom (lab only):** a subprocess
complains `Class "Phar" not found`, yet
the command completes and its result is
correct.
**What it means:** in the lab's
hand-assembled PHP, one subprocess ran
under a PHP build lacking the Phar
extension (transcript `t09`). On a
normal install — packaged PHP with its
standard extensions — this does not
occur. The transferable lesson is to
judge a command by its verified result,
not by the cleanest output.
