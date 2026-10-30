# Keeping It Updated and Backed Up

Two habits decide whether a WordPress
site has a long life or a short dramatic
one: **updates**, because WordPress's
popularity makes old versions a target
with a published map of weaknesses; and
**backups**, because everything else can
fail anyway. Both are run here, for
real, in transcript `t11`.

## Updates

WordPress updates arrive in layers:
**minor core releases** (security and
bug fixes — these apply themselves
automatically by default), **major core
releases** (new features; offered in the
dashboard, applied with a click unless
you enable auto-update for them too),
**plugins** and **themes** (each
updatable in bulk, each with an
auto-update switch), and **translations**.

The lab could not install an update —
it was already current — but it ran the
check, and the check itself is the part
worth proving, because it means this
sandbox site really does converse with
wordpress.org:

```
$ wp core check-update
Success: WordPress is at the latest version.
$ wp plugin list --fields=name,status,update,version
akismet   active     none   5.7.2
hello     inactive   none   1.7.2
```

`update: none` across the board is the
healthy steady state, and it decays
daily: the correct ritual on a real site
is boring — glance at the dashboard's
Updates page (or let auto-updates carry
minors and plugins), and treat a site
months behind as an incident, not a
chore. What the lab did *not* do is
perform an actual version upgrade: there
was nothing to upgrade to, and
downgrading to manufacture one would
teach little. The mechanics — download,
checksum, replace core files, run the
database upgrade — mirror chapter 4's
install and are described here from the
project's documentation.

## Backups: two halves, both proven

Chapter 2's split decides what a backup
is. A WordPress backup has **two halves,
and either alone is close to useless**:

1. **The database** — posts, pages,
   comments, users, settings. Exported
   here with one command:

   ```
   $ wp db export backups/millbrook-2026-10-30.sql
   Success: Exported to 'backups/millbrook-2026-10-30.sql'.
   ```

2. **The files that are yours** —
   `wp-content/` (themes, plugins,
   uploads) plus `wp-config.php`. The
   core files need no backup; they are
   downloadable, checksum-verified
   (chapter 11). The lab tarred
   `wp-content` (13 MB, mostly the three
   bundled themes).

And then the step everyone skips,
performed: **read the backup back.** The
exported SQL was searched, and the fête
post is in it:

```
$ grep -c "Village Fête" backups/millbrook-2026-10-30.sql
1
```

A backup you have not inspected is a
hope with a filename. A backup you have
*restored* is a backup — a full restore
drill (import the SQL into a fresh
database, drop in the files, log in) is
the gold standard, and this lab, having
built the site from parts once, leaves
the drill described rather than
re-run: the honest statement is that the
export and the archive are verified as
*containing* the site, and the restore
path is the install chapter run in
reverse with your files instead of the
download's.

Two last properties of a real backup
routine, stated because the lab's single
manual run has neither: it is
**automatic** (a cron job or an update
plugin's schedule, not a memory), and it
lives **somewhere else** — a backup on
the same disk as the site is a souvenir,
not a recovery plan.
