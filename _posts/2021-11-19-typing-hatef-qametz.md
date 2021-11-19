---
layout: post
title:  "Typing Hatef Characters"
subtitle:  "You have achieved level 3"
date:   2021-11-19 14:00:00 +0300
author: James Cuénod
header-img: "img/post-bg-hebrew.jpg"
---

# The Goal

I have come a long way since making my own hebrew typing assistant tool called [craftbrew](https://jcuenod.github.io/craftbrew/), still one of the best names I've ever come up with. I now type using SIL's Hebrew keyboard layout regularly. I wrote about setting that up [here](/bibletech/2017/12/01/enabling-sil-keyboard-layout-gnome-3/). The problem I encountered is that I can't figure out how to type hatef vowels. While writing this post, I realized that I totally forget that I [already solved this problem](/bibletech/2018/01/01/an-even-better-hebrew-keyboard-layout/) by using a different keyboard layout. But here's another solution that will hopefully help understand how keyboard layouts work a bit more...

# What are Levels?

I take a look at the layout of my keyboard and I see something like this:

![US English Keyboard Layout](/bibletech/img/post-images/keyboard-layout-en.png)

I know how to type "a" and "A"—use shift! But what happens when, instead of having two letters on a key, you have four? That's what SIL's keyboard layout looks like (and tbh, Gnome sucks at drawing this layout, a much better set of images is available in SIL's "manual" available [here](https://www.sbl-site.org/Fonts/BiblicalHebrewSILManual.pdf)—I've also duplicated it [here](/bibletech/files/BiblicalHebrewSILManual.pdf)):

![SIL Phonetic Biblical Hebrew Keyboard Layout](/bibletech/img/post-images/keyboard-layout-sil.png)

So to get from "a" to "A", you are moving to the second "level". Shift does that. But how do we move to the third (and fourth!) level?

The answer is AltGr.

# AltGr vs Compose

I have mistakenly conflated AltGr and the Compose Key in the past. But they're two different things. Hitting the Compose Key allows you to create characters like **š** (`Compose` + `c` + `s`), **ç** (`Compose` + `,` + `c`), or even symbols like °, ©, and ™   (`Compose` + `o` + `o`, `Compose` + `o` + `c`, `Compose` + `t` + `m`, respectively). This is great because I normally don't have to check a table to figure out what to type, I just guess and it works.

But AltGr is a different thing. And it turns out that I had inadvertently disabled AltGr by setting Right Alt (the default AltGr key) to be my Compose Key. Oops... Use tweak tools to fix that (I set Ctrl to be my AltGr key because I use Compose so often and I don't want to have to retrain myself).

`AltGr` is the *third level* key. In other words in a keyboard layout that has four levels, you can use `AltGr` to go up above `Shift` and `Shift` + `AltGr` to go to the *fourth level*. This means you can access keyboards that have layouts like this:

| Level 1 | Level 2 | Level 3 | Level 4 |
|---|---|---|---|
| patach | qametz | qametz | hatef-patach |

Voila, `AltGr` + `Shift` + `a` gives me a hatef-patach.

# Solution

So to solve my original problem, the hatef-qametz is actually the fourth level on `o` (the third level is a qametz-qatan) so: `AltGr` + `Shift` + `o` gets me: ֳ