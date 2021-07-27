---
layout: post
title:  "How to Remove PDF Annotations"
subtitle:  "Automating your PDF Library"
date:   2021-07-30 11:00:00 -0600
author: James Cuénod
header-img: "img/post-bg-04.jpg"
---

I periodically want to remove annotations from PDFs for all kinds of reasons. For example, I want to give the PDF to a student without all my comments and highlights. Or, more recently, I want to re-OCR the PDF but I'd highlighted an older version.

There's a perl program that does this easily called [rewritepdf.pl](https://metacpan.org/dist/CAM-PDF/view/bin/rewritepdf.pl), which is a part of [CAM:PDF](https://metacpan.org/pod/CAM::PDF).

On arch, you can install this library using `yay` (or your preferred package manager):

```
yay -S perl-cam-pdf
```

The incantation for removing annotations is:

```
rewritepdf.pl -C in.pdf out.pdf
```

Voila!
