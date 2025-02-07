---
layout: post
title:  "Free Zotero Backups with Version Control"
subtitle:  "Just because it's not code doesn't mean we can't roll it back"
date:   2020-06-14 14:00:00 +0300
author: James Cuénod
header-img: "img/post-bg-hebrew.jpg"
---
# 1. Git-ify your Zotero Folder

That's right. The first step is that we're using git to make your Zotero last longer and be braver in this brave new world.

```
cd ~/Zotero
git init
```

Follow the prompts and you're done.

If you're a maverick (and you want to actually back up all your Zotero stuff), then go ahead and use your favourite source control setup (like Github, Gitlab, Bitbucket...) and create a repository. You'll want to add a remote to this one, though.

Maybe something like this:

```
git remote add origin git@github.com:username/zotero-backup.git
```

# 2. Make Initial Backup

Now you just have to use your regular ol' git tools to make the backup back up. From your `~/Zotero` folder, do something like this:

```
git add *
git commit -m "Initial Zotero snapshot"
git push -u origin master
```

Now you should see magic happening. It's pushing all your beautiful pdfs and notes and everything up into the cloud.

# 3. Make a bunch of changes and make an incremental backup

You'll probably normally do things like:

- take notes
- add documents
- create bibliographies (actually this doesn't matter)

So go ahead and do those things (especially adding the pdfs, I like that bit).

You could, of course, back this up using git commands (as above). Or you could use my handy-dandy backup script from <https://github.com/jcuenod/zotero-backup-scripts>. Clone that repo somewhere and then symbolic link `backup.sh` to your `~/Zotero` folder.

```
cd ~/Zotero
ln -s somewhere-i-cloned-that-repo/backup.sh .
```

Now try:

```
./backup.sh
```

If it doesn't work, check your permissions. You should have a new commit that has a message something like this:

```
Daily backup

(new) Author_1983_Title of work with 'quotes' and every_thing.pdf
(mod) Some other great document that you edited (e.g. with annotations).pdf
```

Yes, it even has those cool extra lines about what new/modified pdfs are in this commit.

# 4. Automate: Rinse and Repeat

I want these backups to happen daily so I made a systemd timer (kind of like a cron job). That means you've gotta symbolic link those files to your systemd as well and you have to start the timer.

```
cd ~/.config/systemd/user/
ln -s somewhere-i-cloned-that-repo/zoterobck.service
ln -s somewhere-i-cloned-that-repo/zoterobck.timer
```

To start the timer:

```
systemctl --user start zoterobck.timer
```

And voila, you have daily incremental backups of your Zotero work (and if you do it right [i.e. find a private free version control utility], it's free).

# 5. Bonus Points: Version Control your Dissertation...

Obviously this principle works with documents in general. Now if only there were a way to get version control to understand documents. Like if there were a way of writing them in plain text but still have them nicely formatted when necessary...
