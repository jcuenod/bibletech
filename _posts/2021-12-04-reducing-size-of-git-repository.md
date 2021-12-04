---
layout: post
title:  "Reducing the Size of a Git Repository"
subtitle:  "For when you use Github to backup your Zotero Documents"
date:   2021-12-04 14:00:00 +0300
author: James Cuénod
header-img: "img/post-bg-06.jpg"
---

# TLDR

Git garbage collect: `git gc`

# Gitting Too Big

Some time ago, I switched to [using git to backup my Zotero folder](https://jcuenod.github.io/bibletech/2020/06/14/zotero-github-backups/). This means that I have a versioned backup of my Zotero content that [auto updates](2021-02-12-another-zotero-backup-update) every day. I noticed, however, that git is taking up kind of a lot of space:

```
$ cd ~/Zotero
$ du -h | grep git
...
13G    ./.git
```

Everything that's not hidden in my Zotero folder was taking up about 4GB. In other words, my git folder now exceeds the size of my Zotero folder by over 200%...

# Gitting a Smaller

Git comes to the rescue with "garbage collection". I don't know what it does but it makes things smaller:

```
$ git gc
$ du -h | grep git
...
3.4G    ./.git
```

It seems to do some compression and, I would think, deduplication.