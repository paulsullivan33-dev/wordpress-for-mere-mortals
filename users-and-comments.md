# Users and Comments

A WordPress site is a small society: a
few people who may write, one or two who
may administer, and the whole world
allowed to read and — if you permit —
to comment. This chapter proves both
halves: accounts with graded power, and
a comment's walk through moderation.

## Accounts and roles

Every account has a **role**, and the
role is a bundle of **capabilities** —
named permissions like *publish posts*
or *install plugins*. The standard
ladder, weakest to strongest:

- **Subscriber** — can read and comment
  with an account; writes nothing.
- **Contributor** — can write and edit
  their *own* drafts, but cannot publish;
  an editor reviews their work.
- **Author** — can publish and edit
  their own posts.
- **Editor** — can publish and edit
  *anyone's* posts and pages, moderate
  comments; cannot touch settings,
  themes or plugins.
- **Administrator** — everything,
  including installing code. The
  installer's first account
  (`millbrook_editor`) is one.

The lab added a second person to make
the ladder real (transcript `t12`):

```
$ wp user create pat_contributor pat@example.com \
    --role=contributor --display_name="Pat Contributor"
Success: Created user 2.
$ wp user list --fields=ID,user_login,roles
1   millbrook_editor   administrator
2   pat_contributor    contributor
```

Pat can draft the harvest supper notice
but cannot make it public — the draft
state from chapter 7 is exactly where a
contributor's work waits. Two standing
rules follow from the ladder: give every
human their own account (the audit trail
is the point), and keep the number of
administrators at the number of people
who truly need to install code — usually
one or two. A site with nine
administrators has nine master keys
under nine doormats.

(Passwords for other people are *set*,
never *shared into existence* carelessly:
on a mail-capable site the proper route
is the account-creation email that lets
the person choose their own. The lab,
sending no mail, set its contributor's
password directly and says so.)

## Comments and the moderation queue

Comments are where a site meets its
readers — and its spammers. WordPress's
default posture on a new install is
cautious: a comment from someone who
has never had an approved comment is
**held for moderation** rather than
shown. The lab proved the whole journey
(transcript `t10`) with a reader named
Alex Rivers, who commented on the fête
post the way a browser does, with no
login:

```
POST wp-comments-post.php -> 302 ->
  .../village-fete-returns-to-the-green-this-saturday/?unapproved=2&moderation-hash=...
```

The redirect URL itself confesses the
state — `unapproved=2`. And on the
public page, the comment's text appeared
**0 times**. In the editor's view it
waited, held:

```
$ wp comment list --status=hold
2   Alex Rivers   0        (comment_approved = 0)
```

Approval is one act, in the dashboard or
here:

```
$ wp comment approve 2
Success: Approved comment 2.
```

after which the public page carried the
comment — the same search found it
**1 time**. Held, approved, public: the
three states are the whole story of
comment moderation, and every bulk tool,
Akismet included (chapter 9), exists to
sort comments into them faster.

Settings worth knowing exist, all under
the dashboard's Discussion page: whether
commenting is on at all, whether a name
and email are required, whether comments
close automatically on old posts, and
the list of words or addresses that send
a comment straight to hold or to trash.
On a site with real traffic those
settings, plus Akismet's key, are the
difference between a conversation and a
chore.
