---
layout: post
title:  "CSS Unicode Ranges"
subtitle:  "Custom Hebrew and Greek fonts on the web, without spans!"
date:   2018-04-10 14:00:00 +0300
author: James Cuénod
header-img: "img/post-bg-hebrew.jpg"
---

Often I find myself wanting to display Hebrew text in a webpage and so having to wrap everything in spans that portion off the bits I want to use SBL Hebrew or EZRA SIL (yup, [I have preferences when it comes to Hebrew fonts](https://jcuenod.github.io/bibletech/2017/07/27/unicode-hebrew-fonts/))

A while ago I discovered that you could use the `@font-face` rule in CSS to segment unicode ranges you wanted the font-face to apply to (so you could use one just for emojis, for example. Or, if you're using font-icons, you could make sure they don't interfere with other text). This got me thinking about using the rule for Hebrew and Greek text. In the process of starting to play with the idea I discovered that it had already been [done by John Dyer](http://johndyer.name/using-css-to-display-fonts-for-greek-and-hebrew-but-not-english/). Gah! All my fun...

Anyway, the interesting code snippet that you need is:

~~~ css
	@font-face {
	    font-family: EzraSILW;
	    src: url(SILEOT.woff);
	    unicode-range: U+0590-05FF;
	}
	@font-face {
	    font-family: GentiumPlusW;
	    font-style: italic;
	    src: url(GentiumPlus-I.woff);
	    unicode-range: U+0370-03FF, U+1F00-1FFF;
	}
	body {
	    font-family: EzraSILW, GentiumPlusW, Arial;
	}
~~~

Three things to remember:

1. The font that's listed first and has a matching glyph gets used (cf. the `body` rule).
1. You need to convert your font to a webfont - watch out for licensing issues!
1. You don't need the whole font, you only need the relevant range (apparently Font Squirrel has a generator that does this). So don't just convert the font and make users download the whole thing all the time.
1. If you use a popular font, like Gentium, you may want to change the name so that there's no ambiguity about which version the browser is using.
