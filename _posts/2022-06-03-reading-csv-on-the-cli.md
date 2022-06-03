
---
layout: post
title:  "Reading CSV files with a CLI"
subtitle:  "An on the fly formatting alias for CSVs"
date:   2022-06-03 14:00:00 +0300
author: James Cuénod
header-img: "img/post-bg-06.jpg"
---

# The Problem

I open/print out a CSV file and something like this happens:

[Ugly print CSV](/bibletech/img/post-images/print-csv.png)

# The Solution

Turns out that there's a great little tool called `column` that helps us with columnar data. Usage is something like:

```
column -s, -t < file.csv
```

In short, `-s,` tells `column` that the separator is a comma. `-t` tells `column` to make the data into a table. I believe that this means `column` is processing the whole file before it produces output (this makes sense because you don't know how wide columns need to be without examining all the data).

This can then be piped back into `less` so that you have a nice tool to navigate your tabular data:

```
column -s, -t < file.csv | less -#2 -N -S
```

# Wrap it up

And now that you have a one liner, you can make a function and add it to your `.zshrc`:

```
# Pretty Print CSV
# $ ppcsv filename
function ppcsv {
	column -s, -t < $1 | less -#2 -N -S
}
```

And voila:

[Pretty print CSV](/bibletech/img/post-images/pp-csv.png)
