---
layout: post
title:  "Searching for Text in Documents"
subtitle:  "Grep your .odt files"
date:   2022-03-24 14:00:00 +0300
author: James Cuénod
header-img: "img/post-bg-06.jpg"
---

# The Problem

It seems like it should be really easy to search for text content in documents (i.e., not text files but files like `*.odt`). Unfortunately, it's not.

This means that, even though I'm quite sure "I wrote something about that somewhere," I can't find it. This is a great reason to use `.md` files and write in plaintext. Then the solution would just be: *use `grep`*. But that's not going to work when you're writing more complicated content that has lots of footnotes and custom layouts and things (although I have seriously considered `.tex`).

# The Solution

Fortunately, `.od*` files are basically just archived `.xml` files. This means that you can actually `unzip` them and get your content. So if you wanted to grep `.odt` files, you might write a function something like this:

```bash
#!/bin/bash

find . -type f -name "*.od*" | while read i ; do
   [ "$1" ] || { echo "You forgot search string!" ; exit 1 ; }
   unzip -ca "$i" 2>/dev/null | grep -iq "$*"
   if [ $? -eq 0 ] ; then
      echo "string found in $i" | nl
   fi
done
```

> source: <https://askubuntu.com/a/938914>

When I ran this, I modified the script to only search files whose names match `*.odt` (not `*.od*`), but that's not required.

And now:

```
~/ $ sh ./search-libre.sh "my search term"
string found in ./
```
