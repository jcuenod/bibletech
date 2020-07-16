---
layout: page
title: "Projects"
description: "Stuff I'm working on (or *was* working on)"
header-img: "img/about-bg.jpg"
order: 3
---

# Interacting with Biblical Text

## parabible <https://parabible.com>

This project has received the most attention and most of what I find myself doing now somehow relates to enriching data that this app makes use of. The more data available to parabible, the more it will be able to do. The search function is basic but powerful and the thing I really enjoy about it is how rapidly it can produce results (i.e. in fractions of a second).

## qBible <http://www.qbible.tk>

This was the project that paved the way for parabible. It's for the Old Testament, there are even some tutorial videos on youtube about it...

## theMarker <http://jcuenod.github.io/theMarker/>

This one is a little older and it's for the NT. Doesn't search across different books but it's pretty handy in any case...


# Helper Projects

I've made a bunch of smaller projects to do random little things

 - <https://jcuenod.github.io/hexicon/> <br />
 I wanted a way to look up dictionary entries in BDB. I found a poorly formatted db and dumped it into firebase. This thing does a pretty neat job of helping you look up entries in it but it's kind of rough around the edges...

 - <https://jcuenod.github.io/findlexemes/> <br />
 I wanted to be able to find lexemes that are listed in the ETCBC Hebrew data. This is a list of lexemes that you can search using hebrew or latin characters (latin is going to search the glosses). It's handy for finding cognates or lexemes that occur multiple times with different glosses or usage for whatever reason. It's also great for finding synonyms if you can think of English synonyms and search the glosses that way.

 - <https://jcuenod.github.io/hebrewHelper/> <br />
 Sometimes you've just gotta strip accents out of chunnks of Hebrew text. This thing helps you do that.

 - <https://jcuenod.github.io/hebrewTransliterate/> <br />
 Very similar to the accent removal helper, this little webapp produces transliterated Hebrew for you that, I believe, conforms to SBL guidelines.

 - <https://jcuenod.github.io/craftbrew/> <br />
 Before I discovered the [SIL keyboard](https://jcuenod.github.io/bibletech/2017/12/01/enabling-sil-keyboard-layout-gnome-3/), I did not enjoy the prospect of learning a Hebrew keyboard layout. So made this thing that guess what you want to type based on phonetics. If it's not the character you wanted, just hit the key again and it will cycle through the options (e.g. "s" will produce sibilants). It also produces vowels unless there's already a vowel in which case, for example, an "a" will give you an aleph/ayin... So it tries to be a bit clever. I managed to get relatively proficient typing with it and the advantage was I never had to look at a keyboard map. I still can't figure out where צ or  ט are on the SIL layout (I just know roughly and hit the whole row until the right letter appears). So this was kind of cool. But, honestly, learn SIL...

 - <https://jcuenod.github.io/nlp/> <br />
 This is the future of searching the Bible. It uses NLP to convert natural language searches into structured queries that it executes on parabible's search backend. It's awesome but wit.ai (owned by facebook) has decided to change their login system and so I believe I've lost access to the api for now and it's not working (and I refuse to create a new account with them). So for the time being, I think it's down. A problem I place squarely at the feet of facebook.


*last updated: 2020.06.14*
