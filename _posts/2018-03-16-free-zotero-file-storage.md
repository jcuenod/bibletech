---
layout: post
title:  "Free Zotero File Storage"
subtitle:  "Because I'm too cheap to pay for my bibliography"
date:   2018-03-16 14:00:00 +0300
author: James Cuénod
header-img: "img/post-bg-hebrew.jpg"
---

I use Zotero for all my work - class notes, reading notes... If Zotero vanishes, I'm in trouble. Ironically my concern for its sustainability is not matched by unwillingness to pay a cent to use it. Zotero provides 100mb of free storage. 100mb goes a long way when you're just storing bibliographic information and reading notes. It gets swallowed up pretty quickly though when you start attaching pdfs. And honestly, attaching pdfs is pretty handy. It's also the default when you save article metadata from websites that provide pdfs of journal articles.

So I've been trying to find a way to not have to pay for Zotero since I started using it. Up until now my solution has been to hit the save to zotero button and then save the pdf where I want it separately, then right click on the item in Zotero -> "Attach" -> "Attach link to file" -> find the file. Then I also have to delete the "stored attachment" because it's using my precious 100mb.

In an effort to try to solve this problem I tried Zotfile for the first time (I'll post in future about it if it's life changing) hoping that it would help me to automatically convert stored files to linked files. It doesn't seem to do that but playing around I did run in to a very obvious solution. Before I mention that I will say that I also considered (again) using WebDAV to sync to google drive. Is that an ideal solution? Not really - the real problem with it though: Google Drive doesn't support WebDAV! What?

Anyway, it turns out that there's a much simpler solution. This solution lies in the fact that Zotero stores all your pdfs (often nicely named) in the filesystem (I had assumed they were blobs in a db somewhere and never actually looked). Yes indeed, the trust ol' filesystem:

	~/Zotero/storage/some-weird-hash/your-nicely-named.pdf

So duh! If they're all sitting there as pdfs, just sync the folder to dropbox or box or google drive or whatever. Now I have a dropbox folder sitting somewhere (probably the default) and normally can't figure out how to make it put my folders where I want them. So "where I want them" normally just becomes a symbolic link to my dropbox folder. The reverse works just as well:

	~/Dropbox$ ln -s ~/Zotero .

Of course, you could do `~/Zotero/storage` but I figured, why not just sync the whole folder... Seems safer that way.

Now this only stores your pdfs - you could always have done that (and I think maybe that's why this solution didn't occur to me). The last trick is that Zotero allows you *not* to sync attachments. `Edit` -> `Preferences` -> `Sync` -> Uncheck the `Sync attachment files in My Library using ....` option