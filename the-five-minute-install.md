# The Five-Minute Install, Honestly

WordPress is famous for a "five-minute
install". The fame is earned — the whole
act of creating a site is one web form —
and slightly padded, since the five
minutes assume chapter 4 already
happened. This chapter runs the form for
real: the lab sent it with `curl`, the
way a browser would, so every field and
every answer is on the record in
transcripts `t03` and `t04`.

## The form

Visit `/wp-admin/install.php` on an
uninstalled site and WordPress first
offers a language, then shows the entire
site-creation form. Its fields are the
whole decision set:

- **Site Title** — the name shown on
  every page. The lab: *The Millbrook
  Weekly*.
- **Username** — the first account, which
  will be an **administrator**: all-powerful.
  The lab invented `millbrook_editor`.
- **Password**, twice — with a checkbox
  that lets you keep a weak one if you
  insist. Do not insist; this account can
  install plugins, which means it can run
  any code on the server.
- **Your Email** — for password resets
  and notices. The lab used an address at
  `example.com`, the reserved fictional
  domain, because the lab sends no mail.
- **Search engine visibility** — a
  checkbox asking search engines to stay
  away. On a real site, unticked is
  normal; the lab ticked *public*, which
  only affects a suggestion WordPress
  makes to crawlers.

That is all. No licence key, no account
with wordpress.org, no phone-home. The
form's modesty is a real property of the
software.

## What happens when you submit

The lab's `curl` POST to
`install.php?step=2` was answered `200`
and a single word of heading:

```
<h1>Success!</h1>
```

Behind that word, the installer did the
one job only it can do: it wrote the
site into the database. Asking MariaDB
immediately afterwards (transcript
`t04`) shows the twelve core tables,
created in that moment:

```
wp_commentmeta   wp_posts          wp_users
wp_comments      wp_postmeta       wp_usermeta
wp_links         wp_options
wp_terms         wp_term_taxonomy
wp_termmeta      wp_term_relationships
```

Users in one pair of tables, everything
written in `wp_posts` (posts, pages and
more share it), comments in their own,
and `wp_options` holding the settings —
including the site title just typed.
The installer also seeds the site with a
first post (*Hello world!*), a sample
page, and a first comment, so a new site
is never blank. They are teaching aids;
deleting them is every real site's first
chore, and harmless either way.

## The honest footnote

"Five minutes" measures the form. The
lab's stopwatch truth is: the form took
seconds; standing up PHP and MariaDB by
hand took the afternoon — which is
exactly the part installers and hosting
panels exist to absorb. Neither number
is a lie; they measure different work.
What the long way bought is that nothing
in the next chapters is magic: every
later surprise happens in a part you
have already met.
