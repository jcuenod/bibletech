---
layout: post
title:  "Formatting Markdown Tables in Neovim"
subtitle:  "Pipe them to Pandoc"
date:   2022-03-14 14:00:00 +0300
author: James Cuénod
header-img: "img/post-bg-06.jpg"
---

# The Problem

You're writing a markdown file and you need a table. No problem, there's a standard for tables... Except that now you have this ugly mess:

```
| ID | Name | Category ID |
|-|-|-|
| 11092 | Gorilla Fist | 27 |
| 11212 | Plate 3 x 3 | 14 |
| 11209 | Tyre 21 x 9.9 | 29 |
| 11640pr0003 | ELECTRIC GUITAR SHAFT Ø3.2 NO. 3 | 27 |
```

Wouldn't it be nice if there were a magical way to turn that table into a nicely structured one? (like at the end of this post). Well there is.

# The Solution

Brian Hicks just [posted about doing this](https://bytes.zone/posts/aligning-markdown-tables-in-helix/) in `helix`, a modal editor comparable to `neovim` in various ways. I have been using `neovim` for a while, though, so I wondered how to accomplish the same thing.

I assumed that there might be some plugin or at least a way to pipe the table to `column`, a tool I've used to [format CSVs on the command line](/bibletech/2022/06/03/reading-csv-on-the-cli/). The answer was found in a [comment on lobste.rs](https://lobste.rs/s/fqgzrk/aligning_markdown_tables_helix). In short, you pipe the table to `pandoc`.

The solution he suggests is:

```
vip:!pandoc -t markdown-simple_tables
```

Let's break that down for a sec:

1. `vip`: This character sequence
	1. Takes you into `visual mode`
	2. Tells `neovim` to select "inner"
	3. `paragraph`
2. The `!` pipes out the paragraph to a command (everything else on that line). The other stuff happening on that line doesn't matter too much except to say that "pipe tables" is a style of tables but, for the more interested reader:
	1. The `-t` switch tells pandoc the output format (short for `--to`)
	2. `markdown` is the output target
	3. With just `markdown`, the table will get a horizontal separator for the `thead` but not vertical separators. The `-simple_tables` modifier tells pandoc to add the `|` between cells.

```
| ID          | Name                             | Category ID |
|-------------|----------------------------------|-------------|
| 11092       | Gorilla Fist                     | 27          |
| 11212       | Plate 3 x 3                      | 14          |
| 11209       | Tyre 21 x 9.9                    | 29          |
| 11640pr0003 | ELECTRIC GUITAR SHAFT Ø3.2 NO. 3 | 27          |
```

# Tying it Together

In `neovim` (and `vim`), you can map commands to keys. In my `init.lua`,  I added this line:

```
vim.keymap.set('v', '<leader>t', '!pandoc -t markdown-simple_tables+pipe_tables<CR>',
  { silent = true, desc = 'Align selected md table using pandoc' })
```

Now, when I'm in visual mode (specified by 'v' in the function call), I can select my table (with something like `vip`, hit `<leader>` (I've set mine to space) and then `t` et voila: beautifully formatted table. You're welcome. 
