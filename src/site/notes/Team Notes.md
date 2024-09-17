---
{"dg-publish":true,"permalink":"/team-notes/","hide":true}
---

MOC: [[Web Team MOC\|Web Team MOC]]

## 2024-09-15

### WWW Redirect Issue

Attempting to redirect www.ucsc.edu/vote to UA's [Election and Voter Information](https://advancement.ucsc.edu/about/the-team/government-and-community-relations/vote/) page via the **Safe Redirect Manager** fails; instead, it redirects to a nonexistent page, returning a [404](https://advancement.ucsc.edu/about/units/government/vote/). Attempting the same on the `Dev` site works: [https://dev-www-ucsc.pantheonsite.io/vote](https://dev-www-ucsc.pantheonsite.io/vote)

**Safe Redirect Manager --  www dev**
![dev-redirect](/img/user/attachments/dev-redirect.png)

**Safe Redirect Manager --  www live**
![live-redirect](/img/user/attachments/live-redirect.png)

Deleting the redirect from Safe Redirect Manager on the **Live** site does not delete the bad redirect. I have deleted it from **Live** yet the redirect persists.

**Steps taken:**
- Cleared the **Pantheon Page Cache** in the WordPress Dashboard (Settings>Pantheon Page Cache)
- Cleared the cache in the Pantheon **Live** Dashboard
- Cleared browser cache (Linux: Shift+F5)
- Tried in Chrome incognito window
- Tried in Firefox browser
- Tried in Firefox incognito window
- Waited a day to let the CDN cache clear and did it all over again. 

Pantheon **Live** sites, naturally, are connected to their [Global CDN](https://docs.pantheon.io/guides/global-cdn), but **Dev** sites are not.

I'm unsure what to do next, Pantheon has instructions for [redirects in PHP](https://docs.pantheon.io/guides/redirect) via the `wp-config.php` file, but I'm not sure we should be doing that.

---
for rob:




## 242024-08-27

Evals? 

Docker issues with Linux: Naming and permissions. See [[wp-dev.ucsc notes\|wp-dev.ucsc notes]].
## 242024-08-22
- [x] Add `@ucschumanities` Instagram account to Social Media Directory📅 2024-08-23 ✅ 2024-08-23
## 2024-08-20
[Mark Budz issue](https://github.com/ucsc/ucsc-2022/issues/348): Try playing around with DOM elements in code-pen. See if possibled to pull one element on top of another with no whitespace beneath it [[Issue 348 Stat Pattern Bounding Box\|Issue 348 Stat Pattern Bounding Box]]. 
## 2024-07-30
- [x] make issue for [mark buds page](https://academicpersonnel.wordpress.ucsc.edu/) ✅ 2024-08-06
- [ ] GTM [Button click tracking](https://usefathom.com/learn/track-button-clicks-google-analytics) (for Adam)
- [x] Campus VPN->Directory Block ✅ 2024-08-21

## 2024-07-09
- [x] export Localist users from Calendar ✅ 2024-08-08
- [x] import into [Google Groups](https://groups.google.com/u/3/a/ucsc.edu/g/events-calendar-group/?pli=1) ✅ 2024-08-08
- [x] draft note in Google Groups ✅ 2024-08-08
- [x] New Plugin: "UCSC News Functionality" ✅ 2024-07-29
- [x] add Plugin reqires: ACF ✅ 2024-07-29

## 2024-06-25
- [x] **Gutenberg Blocks** release SNAFU: Installed. Cache? ✅ 2024-06-25
- [x] **Gutenberg Blocks** discussion. Meeting on Wed with Bryn and Kumar ✅ 2024-06-25
- [x] **Events Calendar** [[Update communication\|Update communication]]: Should be communicated with Paolina Fisher(?) on Events Council. I cannot attend Events Council. ✅ 2024-06-25
- [x] **News Site:** Process for "importing/entering" new taxonomies and terms (Pantheon Multi-Dev?) ✅ 2024-07-29
- [x] **News Site:** Article update, pulls in new versions of old taxonomies. How to deal with it? ✅ 2024-07-29
## 2024-06-20
- [x] [Merge PR25](https://github.com/ucsc/ucsc-custom-functionality/pull/25) #custom-functionality-plugin ⏫ 📅 2024-06-21 ✅ 2024-06-24
- [x] Write up events calendar [[Update communication\|Update communication]]: moving to a new platform. Nothing will change about interactivity, all events will be there. Simply a design update. #events-calendar 📅 2024-06-21 ✅ 2024-06-21
## 2024-06-18
- [x] Obsidian! I'm using Obsidian, trying things out, learning the UI ✅ 2024-06-18
- [x] Pull Requests, [Theme](https://github.com/ucsc/ucsc-2022/pull/342) and [Plugin](https://github.com/ucsc/ucsc-custom-functionality/pull/25) ✅ 2024-06-18
- [x] [Quarry Website](https://quarry.ucsc.edu/) ✅ 2024-06-18
