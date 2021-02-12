---
layout: post
title:  "Another Zotero Backup Update"
subtitle:  "We DON'T need to use git lfs"
date:   2021-02-12 11:00:00 -0600
author: James Cuénod
header-img: "img/post-bg-hebrew.jpg"
---


# The Backstory

It was not even two weeks ago that I wrote about [solving my storage problem on github](https://jcuenod.github.io/bibletech/2020/06/14/zotero-github-backups/). I'm using github to backup my Zotero folder (which is awesome because now it's also version controlled). And yes, I know, Github is not for backups. They even say that in their section on [managing large files](https://docs.github.com/en/github/managing-large-files/what-is-my-disk-quota). That is not going to stop me at this point. But because of space limitations, I started getting my pushes rejected. The solution I found was to [use git-lfs](https://jcuenod.github.io/bibletech/2021/02/02/free-zotero-storage-update/).

But a new problem arose.

It turns out Github's Large File Storage solution is not free. I can't remember how to find their pricing but it's not easily googleable.

# The New Non-LFS Solution

The new trick is to use `split` to break up the `zotero.sqlite` file into parts that are small enough that github doesn't mind hosting them. I'm using 25mb chunks.

```
split -b 25M zotero.sqlite "zotero.sqlite.part"
```

This produces a bunch of 25mb "parts" (note the last few rows):

```
$ ls -l
Jun 23  2020 backup.sh -> /route-to/zotero-backup-scripts/backup.sh
Dec  2  2019 locate
Feb 12 14:29 pipes
Feb 12 14:24 storage
Feb  1 15:03 styles
Feb  5 12:35 translators
Feb 12 14:29 zotero.sqlite
Feb 10 13:32 zotero.sqlite.1.bak
Feb 11 14:54 zotero.sqlite.bak
Feb 12 08:38 zotero.sqlite.partaa
Feb 12 08:38 zotero.sqlite.partab
Feb 12 08:38 zotero.sqlite.partac
Feb 12 08:38 zotero.sqlite.partad
Feb 12 08:38 zotero.sqlite.partae
```

So now, you can just add `zotero.sqlite` to your `.gitignore` and your troubles are all gone.

```
echo zotero.sqlite >> .gitignore
```

# Recovering The Parts

So now, your `zotero.sqlite` is not actually in the github repo. How do you get it back? Elementarys my dear Watson:

```
cat zotero.sqlite.part* > zotero.sqlite
```

And voila, Zotero is back up and running.

# The Final Product

Admittedly, it took a while to be sure I'd ripped git-lfs out of my Zotero repo correctly and wouldn't lose anything if everything exploded. But I think I am satisfied that I've done this successfully now.

And I've updated my backup scripts to account for this new oddity. In essence, they just run the split daily and add all the parts to the repo committing new and changed parts as appropriate (but the parts will probably all change every day because it's a binary file).

Here you go: https://github.com/jcuenod/zotero-backup-scripts