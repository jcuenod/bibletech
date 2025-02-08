---
layout: post
title:  "Imagemagick to Clean PDFs"
subtitle:  "Using Imagemagick to make OCR work better"
slug: "imagemagick-clean-pdfs"
date:   2021-09-15 11:00:00 -0600
author: James Cuénod
header-img: "img/post-bg-04.jpg"
---

I OCR all the PDFs that the library sends me or that I scan myself so that (1) I can index and search them but also and especially (2) so that I can highlight them in my PDF reader and then extract those highlights into Zotero.

Sometimes, though, I get a rubbish scan that has too much colour and not enough contrast or brightness. The solution is Imagemagick. Here's the incantation I found to help out my latest troublesome PDF:

```
convert -colorspace gray -density 300 -brightness-contrast 5x25 -sharpen 0x1 input.pdf output.pdf
```

So I'm:

* Making it grayscale: `-colorspace gray`
* Setting the pixel density: `-density 300`
* Altering brightness and contrast: `-brightness-contrast 5x25`
* Sharpening: `-sharpen 0x1`

I tried `-adaptive-sharpen 0x1` (which I thought would be better given its efficacy on edges and I'm mostly trying to remove shadows) but it seems to reduce the image resolution and I wasn't getting better results. I'll need to experiment with these settings but this worked well enough for my purposes.