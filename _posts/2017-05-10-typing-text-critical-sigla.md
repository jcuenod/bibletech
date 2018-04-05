---
layout: post
title:  "Typing Text-Critical Sigla"
subtitle:  "Unicode fonts and symbols to use in textual criticism"
date:   2017-05-10 14:00:00 +0300
author: James Cuénod
header-img: "img/post-bg-hebrew.jpg"
---

This is not a complex problem to understand. There are a bunch of symbols that appear in our critical apparatuses. Most of them are weird gothic looking sigla that we don't know how to write. I found that they actually do exist in unicode but then I discovered that they don't look to great in the fonts I regularly use. Now I've discovered what seems to be the gold standard of font for critical apparatus sigla: SIL's [ApparatusSIL](http://scripts.sil.org/cms/scripts/page.php?item_id=ApparatusSIL).

In short, the solution seems to be to use this ApparatusSIL font for every text-critical sign in the document.

To type these on Linux, I use `ctrl`+`shift`+`u` and then just type the unicode hex number. There must be a similar procedure for other Operating Systems but you can also just copy and paste the characters from the table below. Yes, because it's pointless to try to memorise the unicode values for these things and I can never find them when I want them, here's a nice list (they look a lot better in ApparatusSIL than your browser's current font take a look at the end of the [ApparatusSIL glyph pdf](http://scripts.sil.org/cms/scripts/render_download.php?format=file&media_id=ApparatusSIL_ViewGlyph&filename=ApparatusSIL_ViewGlyph.pdf)):

Symbol|Unicode Hex|Meaning
---|---|---
⅏|`U+214F`|Samaritan Pentateuch
𝔄|`U+1D504`|Arabic Versions
ℭ|`U+212D`|Cairo Geniza
𝔊|`U+1d50a`|Septuagint (LXX)
𝔏|`U+1D50F`|Latin Versions
𝔐|`U+1D510`|Masoretic Text
𝔔|`U+1D514`|Qumran (Dead Sea Scrolls)
𝔖|`U+1D516`|Syriac
𝔗|`U+1D517`|Targum
𝔙|`U+1D519`|Vulgate

Other suggestion around the Web:

Symbol|Unicode Hex|Meaning
---|---|---
𝔓|`U+1D513`|Papyrus
𝔭|`U+1D52D`|Papyrus alternative
𝔖|`U+1D516`|Septuagint (LXX)*
𝑙|`U+1D459`|Lectionary Reading

**in ApparatusSIL this is clearly Syriac not LXX*

Take a look at <http://ntresources.com/blog/?page_id=3285> though. It says there that U+1D516 is correct for the Septuagint (with awareness of the confusion). It's probably worth checking back for an update.


Also note, according to SIL's site:
>You may be interested to know that all of the characters in Apparatus SIL are now supported in Charis SIL, Doulos SIL, Gentium Plus and Andika.

Some useful links I'm noting here for future reference:
 - <http://ntresources.com/blog/?p=774>
 - <https://evangelicaltextualcriticism.blogspot.co.za/2010/02/2135.html>
 - For the actual text-critical markings, see this pdf on additional punctuation in unicode <http://www.unicode.org/charts/PDF/U2E00.pdf>
 - <{{ site.url }}{{ site.baseurl }}/img/text-critical-marks-unicode.jpg>

Unforunately, both reference <http://en.foursenses.net/textcriticismsigns> which seems to be dead.