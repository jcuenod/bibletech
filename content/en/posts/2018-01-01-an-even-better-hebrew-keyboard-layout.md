---
layout: post
title:  "An Even Better Hebrew Keyboard Layout"
subtitle:  "Because hatef vowels matter"
date:   2018-01-01 14:00:00 +0300
author: James Cuénod
header-img: "img/post-bg-hebrew.jpg"
---

# A New Layout

A while ago I wrote on [typing in Hebrew](https://jcuenod.github.io/bibletech/2017/05/03/typing-in-hebrew/). I also decided (after actually discovering the content of this post) to write up how to [enable the advanced Hebrew keyboards](https://jcuenod.github.io/bibletech/2017/12/01/enabling-sil-keyboard-layout-gnome-3/).

The problem I encountered even with my fancy `Hebrew (Biblical, SIL phonetic)` keyboard layout is that typing hatef vowels (or "composite shewas") is seemingly impossible. So that got me looking for another solution which has replaced `Hebrew (Biblical, SIL phonetic)` for me.

In short, it is a keyboard layout that is based on SIL's phonetic layout but rearranges some letters and adds hatef vowels. The layout and accompanying write-up can be found [here](http://www.intoyourhandsllc.com/blog/152-7-quick-steps-for-configuring-ubuntu-16-04-linux-and-libreoffice-for-hebrew-text-entry-with-vowel-pointing-and-cantillation-marks.html). In case it disappears, I have also put it on this site so you can download it [here](https://jcuenod.github.io/bibletech/files/il.iyh-2017-10-05).

# Installation

```sh
# preserve a backup of the original Hebrew keyboard map
sudo mv /usr/share/X11/xkb/symbols/il /usr/share/X11/xkb/symbols/il.original

# install the Into Your Hands LLC version of the Hebrew keyboard map
sudo cp ./il.iyh-2017-10-05 /usr/share/X11/xkb/symbols/il
```

You will also need to enable it in the `Region & Language` settings. For a keyboard map (not a great one): [click here](http://www.intoyourhandsllc.com/download/blog/hebrew-phonetic-keyboard-map.pdf) or if that link dies, my backup is [here](https://jcuenod.github.io/bibletech/files/hebrew-phonetic-keyboard-map.pdf).

# Notes on the New Layout

I have been caught a few times by the moved `Holam` mark. It is no longer the standard `o` which is now the `Qamats Qatan`: instead you need to use `shift`+`o` to get `Holam`. In addition, ש (without a sin/shin dot) is inserted with `shift`+`s` instead of `shift`+`f`/`j` (`f` and `j` were and are still sin and shin).

Now however, we can get hatef vowels with `,` (אֳ), `.` (אֲ) and `/` (אֱ).