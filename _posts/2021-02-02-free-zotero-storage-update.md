---
layout: post
title:  "Update Regarding Free Zotero File Storage"
subtitle:  "We need to use git lfs"
date:   2018-03-16 14:00:00 +0300
author: James Cuénod
header-img: "img/post-bg-hebrew.jpg"
---


# The Backstory

Some time ago I wrote about [free, versioned Zotero file storage](https://jcuenod.github.io/bibletech/2020/06/14/zotero-github-backups/). Basically, this is the best Zotero setup I can imagine. I now have automatic backups to multiple git hosting providers of all my pdfs and notes. I can lose at most one day of work in Zotero and that's pretty darn comforting.

But, recently I noticed that my `git push`es weren't reliably going through. I thought maybe it had something to do with the warning GitHub kept sending me. Something like:

> The size 97.4mb of zotero.sqlite exceeds the GitHub's recommended maximum of 50mb.

But I didn't heed that warning, even though it came with the recommendation to use [Git LFS](https://git-lfs.github.com/) (for Large File Storage).

It turns out that, while 50mb is a recommended limit, 100mb is the actual limit. And my Zotero backups basically stopped working. That's pretty powerful motivation to figure out how to get git-lfs working. Turns out, it was easy.

# The Boring, Getting-it-done Story

You could probably afford to backup before doing this, I definitely freaked myself out towards the end of this process given the fact I was rewriting my git history...

**1. Install git-lfs (I'm on Arch)**

```
yay -S git-lfs
```

**2. Track zotero.sqlite**

Obviously, you have to `cd` into the Zotero folder to do this (I'm using a relative path here)

```
git lfs install
git lfs track "zotero.sqlite"
```

**3. Rewrite History**

Unfortunately, my assumption that I could just `git push` was wrong. You still have those old gigantic files in your history. So you've gotta migrate:

```
git lfs migrate import --include="zotero.sqlite"
```

**4. Push**

```
git push
```

**5. Panic**

Just kidding. But that is what happened to me. The last (essential) step is to:

```
git lfs checkout
```

# What Have You Done?

Okay, I haven't spent much time figuring out how exactly this works but at step 3, my `zotero.sqlite` file became a file that pointed to an LFS stored file (so suddenly it was 134k plain text). Without running `git lfs checkout`, Zotero will stop working. Anyway, now it works great again. I think.
