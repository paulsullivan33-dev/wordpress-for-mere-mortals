# The REST API

Every WordPress site carries a second,
machine-shaped face: the **REST API**,
which serves the site's contents as
**JSON** — structured text that programs
read — under addresses beginning
`/wp-json/`. Phone apps, headless front
ends, other websites and scripts all
drink from it. This chapter reads from
it with no credentials at all, then
publishes through it, and keeps the one
genuine fight the lab had — because the
fight is the lesson most worth learning.

## Reading: no password, by design

```
$ curl -s http://127.0.0.1:18080/wp-json/wp/v2/posts
```

answered a JSON list of the published
posts — the fête post and *Hello world!* —
with ids, slugs, titles, full text and
links (transcript `t07`). No login was
asked for, because none is needed:
**everything a visitor can read on the
site, the API will hand to a program.**
That openness is deliberate and usually
right — but new owners are sometimes
surprised that their *users list* is
similarly readable at `/wp-json/wp/v2/users`.
On a public site, assume anything
published is published to machines too,
and choose usernames you don't mind the
world enumerating.

Drafts are the proof of the boundary:
the harvest supper draft appears in no
public API answer (chapter 7), because
the API applies the same status rules as
the front page.

## Writing: application passwords

To *change* anything, the API needs to
know who is asking. The civilised way
for a program is an **application
password**: a separate, revocable
password for one user and one purpose,
created in the dashboard (Users →
Profile) or, as the lab did, in WP-CLI:

```
$ wp user application-password create millbrook_editor "lab curl" --porcelain
(the new password is printed once, here — the lab's is not
republished in this guide, because a working password is a
working password even in a fictional village)
```

The main password never travels; this
one can be revoked alone, and WordPress
records when it was last used. Then a
publish is one request, name and
password riding as HTTP basic auth:

```
$ curl -s -u "millbrook_editor:$APP" -X POST \
    --data-urlencode "title=Village Fête Returns to the Green This Saturday" \
    --data-urlencode "content=..." \
    --data-urlencode "status=publish" \
    http://127.0.0.1:18080/wp-json/wp/v2/posts
-> 201, with the new post's id, slug and link
```

## The 401 that wasn't a wrong password

The lab's first attempt failed, and the
failure is kept verbatim in transcript
`t07` because it will happen to you:

```
POST /wp-json/wp/v2/posts -> 401
{"code":"rest_cannot_create","message":"Sorry, you are not
allowed to create posts as this user.","data":{"status":401}}
```

The password existed, was correct, and
the user was an administrator. The real
cause: **application passwords only work
over HTTPS — or when WordPress knows
the site is a local/development one.**
On plain-HTTP loopback with no such
declaration, WordPress quietly declines
to honour them at all, and the API
reports the *consequence* ("not allowed")
rather than the cause. The fix is the
one line chapter 4 added to
`wp-config.php`:

```php
define( 'WP_ENVIRONMENT_TYPE', 'local' );
```

after which `/wp-json/wp/v2/users/me`
immediately named the editor, and the
publish went through. On a real site the
proper fix is TLS (the Caddy and nginx
guides' autopilot certificates exist
precisely so this class of problem
evaporates) — the `local` declaration is
for labs and development, which is what
it says on the tin.
