# First Checks

An install that reports *Success!* has
made a claim. This chapter collects the
evidence — from outside, the way a
visitor and then the editor meet the
site. All of it is transcript `t05` and
`t06`.

## The front page

```
$ curl -s -o front.html -w "%{http_code}" http://127.0.0.1:18080/
200
$ grep -o "<title>[^<]*</title>" front.html
<title>The Millbrook Weekly</title>
$ grep -o "Hello world!" front.html | head -1
Hello world!
```

Three facts in ten seconds: the server
answers, the page is assembled with the
title the installer was given, and the
seed post is listed. That is the whole
publishing pipeline — database, PHP,
theme — exercised end to end. On a fresh
install of your own, this is the moment
to open the page in a browser and look
at it as a reader: the checks here prove
it *works*, not that it is beautiful.

## Logging in

The editor's door is `/wp-login.php`.
The lab walked through it with `curl`
and a **cookie jar** — a file where curl
keeps the cookies a browser would keep —
because the login is a small ceremony
worth seeing once:

1. Fetch the login page first. WordPress
   sets a *test cookie* to learn whether
   cookies survive the round trip.
2. POST the form: username, password,
   the test cookie's marker, and where
   to go next (`/wp-admin/`).
3. The answer is a **302 redirect** to
   the dashboard, carrying a new cookie
   whose name begins
   `wordpress_logged_in_` — the signed
   proof, signed with those salts from
   chapter 4, that this browser is the
   editor.
4. Ask for `/wp-admin/` with the jar.
   `200`, and the dashboard greets the
   account by name:
   *Howdy, millbrook_editor*.

Nothing about this is WordPress-specific
magic — it is how most web logins work —
but seeing the pieces means the common
failures have names: the redirect loop
(cookie not kept), the "cookies are
blocked" complaint (the test cookie
never came back).

## The wrong password

The lab also knocked with a wrong
password, because the failure is part of
the interface and it is kept verbatim in
`t06`:

> **Error:** The password you entered for
> the username **millbrook_editor** is
> incorrect.

Notice what it does and doesn't say. It
confirms the username exists — a small
information leak WordPress accepts in
exchange for not confusing legitimate
users — and offers a lost-password link,
which on a real site sends email, and in
this lab goes nowhere (chapter 16). If
you ever meet this page about your own
site, the fix paths are: the email
reset on a mail-capable site, another
administrator resetting it for you, or
WP-CLI — `wp user reset-password` —
which chapter 11 makes plausible.

The site is now proven from both sides
of the door. The rest of the guide is
about what the editor does all day.
