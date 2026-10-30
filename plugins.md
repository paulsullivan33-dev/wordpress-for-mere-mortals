# Plugins

A **plugin** is code that hooks into
WordPress and adds an ability the core
doesn't have: a form, a shop, a calendar,
spam filtering. Where a theme is
clothing, a plugin is an organ transplant
— it runs inside the site, with the
site's full privileges, every time a
page is built. That power is the plugin
system's whole value and its whole risk,
and this chapter keeps both in view.

## The two you already own

WordPress ships with two plugins,
inactive, and the lab's census
(transcript `t09`) shows the standard
starting position:

```
akismet   inactive   5.7.2
hello     inactive   1.7.2
```

**Hello Dolly** is a toy — it puts a
line of song lyric in the dashboard
corner — kept in the bundle as the
smallest possible example of a plugin.
**Akismet** is a real service's client:
it sends comments to Automattic's
servers to be judged as spam or not.

The lab switched both on for real:

```
$ wp plugin activate hello akismet
Plugin 'hello' activated.
Plugin 'akismet' activated.
Success: Activated 2 of 2 plugins.
```

then listed them to confirm the state,
and switched Hello Dolly back off,
because a lab should end tidy.
Activation is a state change, not a
download — the code was already on disk —
and deactivation is equally clean:
a deactivated plugin runs nothing.

## The Akismet honesty test

Akismet *active* is not Akismet
*working*. It needs an **API key**,
obtained by registering with the service,
and until it has one it filters nothing.
The lab registered for nothing — no real
accounts is a rule of this series — so
the guide's claim is precise: activation
and deactivation are proven; spam
judgement by the service is
documented-only. That gap, between
*switched on* and *actually doing the
job*, is the most common plugin
misunderstanding on real sites too. A
plugin's settings page, not its green
*Active* label, is where its real state
lives.

## The marketplace, at arm's length

Beyond the bundle lies the wordpress.org
plugin directory — tens of thousands of
plugins, installable from the dashboard —
and a commercial market beyond it. The
lab installed none, deliberately, and
the chapter's advice is the standing
advice for any site you care about:

- **Provenance first.** A plugin is code
  running as your site. Prefer the
  official directory, recent updates,
  and a living maintainer; treat
  "premium plugin, free from a forum"
  as what it is — the classic smuggling
  route for malware.
- **Fewer is a feature.** Every plugin
  is a dependency and a door. The sites
  that get compromised are usually not
  running exotic code; they are running
  an abandoned plugin with a known hole.
- **Updates are the security model.**
  Chapter 13's update checks cover
  plugins too, and on a real site,
  turning on auto-updates for plugins is
  the single highest-value setting in
  this chapter (a dashboard setting,
  described here, exercised in the lab
  only as far as the update *check*).
- WooCommerce, the shop plugin that
  powers a large fraction of the web's
  small shops, is a world of its own —
  named here, not run, and honestly out
  of this guide's scope.
