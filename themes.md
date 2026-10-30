# Themes

A **theme** is the site's presentation
layer: a folder of templates and styles
under `wp-content/themes/` that decides
how the database's words become pages —
layout, type, colour, what appears in
the header and footer. This chapter
proves the border that makes themes
safe to experiment with: **a theme
changes how the site looks, and cannot
change what the site says.**

## The three you already own

A current WordPress download bundles
three themes, one per recent year, and
the lab's WP-CLI census (transcript
`t09`) is the typical picture:

```
twentytwentyfive   active    1.5
twentytwentyfour   inactive  1.6
twentytwentythree  inactive  1.7
```

Exactly one theme is active. The others
are not dead weight so much as a
fallback and a sandbox — and, on a real
site, a reason to delete what you don't
use, since inactive themes still need
security updates.

These bundled themes are **block
themes**: their templates are assembled
from blocks in the dashboard's Site
Editor rather than living only as PHP
files, and their personality is set
largely by a `theme.json` file of
colours and type. Older *classic*
themes build pages from PHP templates
directly. Both kinds are folders with
the same job; the guide's claims hold
for either.

## A switch you can see

The lab activated Twenty Twenty-Four
(transcript `t09`):

```
$ wp theme activate twentytwentyfour
Success: Switched to 'Twenty Twenty-Four' theme.
```

Success messages are claims, so the
check is in what the server serves.
Every page names its theme in the
addresses of its stylesheets:

```
before:  wp-content/themes/twentytwentyfive
after:   wp-content/themes/twentytwentyfour
```

and the fête post stands on the front
page in the new theme, word for word —
the content never moved, because it was
never in the theme. That is the
experiment to remember when a theme
update or a redesign looms: your words
are in the database (chapter 2); the
theme is replaceable clothing.

## Choosing and changing, honestly

The dashboard offers thousands of themes
from wordpress.org's directory, installable
in two clicks, and premium themes from
elsewhere besides. This lab installed
none of them: the sandbox could fetch
them, but grading a theme's looks is
exactly the kind of claim this guide
cannot verify from here, and a theme is
code — it deserves the same caution as a
plugin (chapter 9) about where it came
from. What can be said from the record:

- Preview before you activate; the
  dashboard will show your real content
  in the candidate theme.
- A **child theme** — a small theme that
  inherits and overrides pieces of a
  parent — is the supported way to tweak
  a theme you didn't write, so updates
  to the parent don't erase your changes.
  It is documented practice, described
  here, not run in this lab.
- If the site breaks the moment a theme
  is activated, switching back (dashboard,
  or `wp theme activate <name>`) is the
  whole remedy; nothing was migrated or
  converted by switching.
