# WP-CLI

**WP-CLI** is WordPress's command-line
companion: the same site, driven from a
terminal instead of a dashboard. Anything
an administrator can click, WP-CLI can
type — which means it can be scripted,
scheduled, and above all *shown*: this
guide uses it wherever a claim benefits
from a transcript. It is a separate
program from WordPress itself, written
by a community project, and trusted
enough that most WordPress hosts ship it.

## Installing it, with the same care

WP-CLI is distributed as a single
**phar** — a PHP archive, one file that
is a whole program. The lab downloaded
`wp-cli.phar` from the build project's
official home and did to it what chapter
4 did to WordPress: compared its
**SHA-256** against the sum the project
publishes. They matched:

```
ce34ddd838f7351d6759068d09793f26755463b4a4610a5a5c0a97b68220d85c
```

Then it is just PHP: `php wp-cli.phar
...`. Two honest wrinkles from the lab.
WP-CLI refuses to run as **root** without
`--allow-root` — a guard against
scripting a loaded gun — and the lab,
running as root in its sandbox, passes
the flag in its wrapper and says so.
And once, mid-lab, a WP-CLI subprocess
picked up a PHP *without* the Phar
extension in this hand-assembled
environment and printed a fatal error —
after which the command completed and
its result worked. Both are kept
verbatim in transcript `t09`; on a
normally installed system neither
appears.

## What it did here

The transcripts show WP-CLI earning its
keep across the whole guide; the
highlights deserve names:

- **`wp core version`** → `7.1.4`. The
  identity card.
- **`wp core verify-checksums`** →
  `Success: WordPress installation verifies against checksums.`
  Every core file re-hashed and compared
  with the official checksums at
  api.wordpress.org. Run this on any site
  you inherit: files that differ are
  either local edits or someone else's,
  and both deserve a hard look.
- **`wp theme list` / `wp plugin list`** —
  the census used in chapters 8 and 9,
  with versions and update state in one
  glance.
- **`wp theme activate`** and
  **`wp plugin activate/deactivate`** —
  state changes proven afterwards in the
  served HTML (chapter 8).
- **`wp rewrite structure`** — the
  permalink change from chapter 7,
  without six dashboard clicks.
- **`wp post create/list`** — the draft
  and the page (chapters 7 and 14).
- **`wp user create/list`**,
  **`wp comment list/approve`** — the
  society of chapter 10.
- **`wp option get/update`** — settings
  as data (chapter 14 changes the
  tagline with it).
- **`wp db export`** — the database half
  of the backup in chapter 13.
- **`wp core check-update`** — the
  update check, also chapter 13.

The pattern to carry away: WP-CLI is not
a different WordPress. It reads the same
`wp-config.php`, talks to the same
database, and obeys the same rules; a
change made in the terminal is visible
in the dashboard and vice versa — the
lab relied on that agreement in nearly
every chapter, and it never once didn't
hold.
