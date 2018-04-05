---
layout: post
title:  "Removing Hebrew Vowels and/or Accents"
subtitle:  "Because who needs the Masoretes?"
date:   2018-04-04 14:00:00 +0300
author: James Cuénod
header-img: "img/post-bg-hebrew.jpg"
---

To be clear, I'll freely admit I read with the Masoretes most of the time. It's far more important for me to figure out their interpretation than to strip it from the text. When submitting papers, however, accents make a lot less sense as words are embedded in different contexts. Indeed, sometimes publications won't even accept vowels.

Up until now I've been manually removing accents and/or vowels by backspacing them out. Yes, you can actually backspace a single diacritic mark. It's just painful when there's one on every word...

A problem also arises when you have unnormalised unicode and the accent is not the last mark to be added to the word. Now you have to reinsert a vowel or a dagesh. What a pain!

No longer :)

I recently threw together [this useful tool](http://jcuenod.github.io/hebrewHelper) to help strip accents and/or vowels and dageshes from Hebrew text.

**Step One**: Paste in some text that you want to strip of masoretic markings.

![Create Search Term: parabible.com](/bibletech/img/post-images/remove-accents-1.jpg)

**Step Two**: Copy out the output...

![Create Search Term: parabible.com](/bibletech/img/post-images/remove-accents-2.jpg)

That's it, I hope it's useful to you :)