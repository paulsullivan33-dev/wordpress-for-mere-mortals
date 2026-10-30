# Posts and Pages

WordPress holds two everyday kinds of
writing, and mixing them up is the
beginner's classic wobble:

- A **post** is dated, listed newest
  first, filed under categories and
  tags, and appears in feeds. News,
  articles, announcements: posts.
- A **page** is undated and stands
  alone at its own address, outside the
  stream. *About*, *Contact*, *Visit*:
  pages.

Both live in the same database table and
go through the same editor; the
difference is how the site presents
them. This chapter publishes one of
each, for real, and proves the draft —
the third state every writer uses.

## Publishing a post

The lab published through the **REST
API** — the machine-facing door that
chapter 12 opens properly — because the
whole exchange can be shown as text. The
post is the newsletter's real business:

> **Village Fête Returns to the Green
> This Saturday** — stalls, a raffle, the
> brass band, and a cake tent asking for
> contributions.

The API answered `201 Created` with the
facts of the new post (transcript
`t07`):

```
id:     5
slug:   village-fete-returns-to-the-green-this-saturday
status: publish
link:   http://127.0.0.1:18080/village-fete-returns-to-the-green-this-saturday/
```

The **slug** — the readable tail of the
address — was made from the title
automatically, accents folded away
(*fête* → *fete*). You can edit it
before publishing; afterwards, changing
it changes the address the world has
bookmarked.

Then the proofs, from the reader's side:

- the **front page** lists it (a search
  of the HTML finds the title);
- its **own address** answers `200` and
  contains the story, brass band and
  all;
- the **public REST API**, asked with no
  password at all, lists it among the
  published posts.

Publishing from the dashboard editor is
the same act through a friendlier door —
type, press Publish, and a post with
the same anatomy appears. The API route
is shown here because a guide made of
text can *show* it, click by click
equivalents and all.

## Permalinks

That readable address is a choice, not
a default. A fresh install gives posts
addresses like `/?p=5` — the *plain*
**permalink** style. The lab changed the
structure to `/%postname%/` with WP-CLI
(`wp rewrite structure`, chapter 11),
which is Settings → Permalinks in the
dashboard. Pretty addresses need the web
server to hand unknown paths to
WordPress — under Apache a rewrite rule,
under nginx a `try_files`, in this lab
the little router script from chapter 3.
A 404 on every post after switching
styles is almost always that hand-off
missing, and chapter 17 keeps it.

## Publishing a page

The newsletter's *About the Weekly*
page went up the same week, by WP-CLI
(transcript `t12`): created as a page,
published immediately, and proven at its
own address — `/about-the-weekly/`
answers `200` and contains its text. It
does not appear in the post stream,
which is precisely the point of a page.

## The draft, proven invisible

A **draft** is a post the world must not
see yet. "Must not" deserves a test, and
the lab ran one (transcript `t08`). A
draft notice — *Harvest Supper Tickets
Go On Sale Monday* — was created, then
hunted in the three places a reader
could meet it:

- front page: **0 mentions**;
- public REST API post list: **0
  mentions**;
- the editor's own list
  (`wp post list --post_status=draft`):
  **there it is**, ID 6, status `draft`.

Drafts, scheduled posts and private
posts all ride on this one mechanism —
a **status** field the public queries
filter on. It held under test. (The
administrator's preview link is the
deliberate exception, signed so only
staff can use it.)
