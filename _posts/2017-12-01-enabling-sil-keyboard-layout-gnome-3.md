---
layout: post
title:  "Enabling SIL Keyboard Layout on Gnome&nbsp;3"
subtitle:  "Because it's hidden by default"
date:   2017-12-01 14:00:00 +0300
author: James Cuénod
header-img: "img/post-bg-hebrew.jpg"
---

# The Missing Setting

Some time ago I posted on typing in Hebrew [here](https://jcuenod.github.io/bibletech/2017/05/03/typing-in-hebrew/). The conclusion to that post was:

> ... my recommendation is that you **install SIL's keyboard layout**

I just reinstalled Linux (I use Antergos with Gnome) and realised that enabling the SIL layout was not as straightforward as I remembered.

The process is supposed to be simple: `Settings` → `Region & Language` → `+` (add input source) → `⋮` (more sources) → `Other`. Then typing "Hebrew" in the search box should present you with "Biblical, SIL phonetic". Instead the only options are "Hebrew," "Hebrew (Biblical, Tiro)," "Hebrew (lyx)" and "Hebrew (phonetic)." Honestly, those keyboard maps all had serious weaknesses for an English user (like no vowel points or not being phonetic).

So how do we view SIL's layout? There are two methods:

# Showing the SIL Layout

## 1. Using Tweak Tools

In Gnome Tweak Tools (or "Tweaks"), go to `Keyboard & Mouse` and enable `Show Extended Input Sources`. You may need to restart the gnome session.

## 2. Using gsettings (or `dconf-editor`)

The setting you need to set to true is `/org/gnome/desktop/input-sources/show-all-sources`. You can find it in dconf-editor or you can use `gsettings` at the cli:

```
gsettings set org.gnome.desktop.input-sources show-all-sources true
```

Now you should be able to find `Hebrew (Biblical, SIL phonetic)` in the Region & Language settings.