# Recipes

Short, complete jobs. Each one was run
on the lab site unless it says
*assembled* — a recipe built from parts
the guide has already run and verified,
labelled so you know which kind of
confidence to give it.

## 1. Publish an About page

Pages stand outside the post stream, at
their own address (chapter 7).

```
$ wp post create --post_type=page \
    --post_title="About the Weekly" \
    --post_content="The Millbrook Weekly is a fictional village newsletter, ..." \
    --post_status=publish --porcelain
7
```

**Check:** the page answers at its own
address and contains its text:
`curl -s http://127.0.0.1:18080/about-the-weekly/`
returned `200` and the newsletter's
description (transcript `t12`).

## 2. Add a contributor who cannot publish

```
$ wp user create pat_contributor pat@example.com \
    --role=contributor \
    --display_name="Pat Contributor" --user_pass="..."
Success: Created user 2.
```

**Check:** `wp user list --fields=ID,user_login,roles`
shows the new account with the
`contributor` role beside the
administrator (chapter 10). On a
mail-capable site, prefer the dashboard's
Add User flow, which emails the person a
link to set their own password.

## 3. Change the tagline

Settings are data in `wp_options`, and
`wp option` edits them directly:

```
$ wp option update blogdescription "News from a fictional village"
Success: Updated 'blogdescription' option.
$ wp option get blogdescription
News from a fictional village
```

The same pair works for `blogname` (the
site title). **Check:** the `get` reads
back what the `update` wrote, and the
dashboard's Settings → General shows the
new wording.

## 4. Schedule a post for later *(assembled)*

A post whose status is `future` and
whose date is in the future publishes
itself when the time arrives — WordPress
checks due posts whenever the site is
visited (and on busy sites, via a real
cron). Through the API it is chapter
12's request with two changes:
`status=future` and a `date` field, e.g.
`date=2026-11-02T09:00:00` — the harvest
supper draft of chapter 7 is one date
away from being this recipe. **Check:**
the post lists as `future` in
`wp post list --post_status=future`, is
absent from the front page until the
date passes, and present afterwards. The
mechanism (statuses filtering public
queries) is the one proven in chapter 7;
the scheduled run itself is assembled
from those verified parts.

## 5. Move the site to a new address *(assembled)*

The classic move: new domain or new
folder. Copy the files, export the
database (chapter 13), and at the new
home run WP-CLI's search-and-replace,
which understands WordPress's serialized
settings where a text editor corrupts
them:

```
$ wp search-replace "http://old.example" "https://new.example" --all-tables --dry-run
# read the report, then again without --dry-run
$ wp option update home    "https://new.example"
$ wp option update siteurl "https://new.example"
```

**Check:** the dry run's table-by-table
report shows the expected counts before
anything changes; afterwards, the front
page loads at the new address and images
and links point there too. Every tool
used here was run in this guide
(`wp db export`, `wp option`) except
`search-replace` itself — listed,
therefore, as assembled, and always
behind a backup.
