---
layout: post
title:  "Typing in Hebrew"
subtitle:  "My custom approach and a new keyboard layout"
date:   2017-05-03 14:00:00 +0300
author: James Cuénod
header-img: "img/post-bg-hebrew.jpg"
---

A big problem for me when I first encountered the need to use Hebrew on the computer was that I didn't know how to type it. I knew the importance and value of unicode (see a rationale for unicode [here](https://tyndaletech.blogspot.com/2005/03/unicode-fonts-for-biblical-studies-made.html)). So I *wanted* to use unicode but actually typing Hebrew was going to be a problem.

Typically if you want to type using another character set, like Greek, you just add a keyboard layout and off you go. That's what I did to type nice, polytonic Greek on my nerdy Arch Linux install. When I looked at adding a Hebrew keyboard, however, I realised that things were not going to be so easy. The Greek keyboard maps pretty well to my Qwerty English one but when I looked for a Hebrew keyboard I realised that with the good ol' "Biblical Hebrew" keyboard layout (called "Israel - Biblical Hebrew (Tiro)" and it gets promoted by SBL!*) mapping is not so transparent. For example, hitting an "a" gives me a "α" in Greek but a "‫ש‬" in Hebrew. This seemed like a terrible solution because it means learning a new keyboard layout - and I'm not keen to do that.

Part of the problem with whatever solution arises is that some letters are never going to map well to English (in Greek that's a problem with "ξ", for example). In Hebrew what English letters are you going to assign "שׂ", "שׁ" and "ס"? The answer that I came to was to write my own solution to the problem. I'm just going to provide all the logical mappings to every english letter and then have an app cycle through the options if I hit a letter again within a certain threshold. So [here's my app](https://jcuenod.github.io/craftbrew/).

That solution served me for some time but it's limited; I don't have an actual text editor going there so it makes editing the text complicated. It's also less than ideal because you can't just jump back to a previous position in the text. Sure, there are ways that I could improve it but before I started doing those things, I found this: [https://www.sbl-site.org/educational/BiblicalFonts_SBLHebrew.aspx](https://www.sbl-site.org/educational/BiblicalFonts_SBLHebrew.aspx). SIL has another keyboard layout - one that is much better mapped to Qwerty. So yes¸ there are still issues (what character does an "s" give you?) but the issues are more like learning j = ξ for a handful of characters: f = שׂ and j = שׁ.

So my recommendation is that you install SIL's keyboard layout. That's the best way forward but, if you want a quick solution, just use [craftbrew](https://jcuenod.github.io/craftbrew/) :)


**I'm not linking to it because I don't want it to be more popular than it already is...*