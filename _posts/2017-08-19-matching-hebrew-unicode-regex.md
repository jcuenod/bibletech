---
layout: post
title:  "Hebrew Unicode Regex Matching"
subtitle:  "Stand back, I know regular expressions"
date:   2017-08-19 14:00:00 +0300
author: James Cuénod
header-img: "img/post-bg-hebrew.jpg"
---

# Disclaimer

If you don't know what unicode is, this is not the post for you. If you don't know what regular expressions are, this is not the post for you. If you're still reading and you want to identify Hebrew characters in a string, this is the post for you. This is really just to document this stuff for myself...

# TL;DR

Just want to match the whole unicode block relevant to biblical studies, this should get you going:

```javascript
// The standard Hebrew unicode block
[\u0590-\u05FF]/.test("some string with Hebrew in it")

// With precomposed characters
[\u0590-\u05FF\uFB2A-\uFB4E]/.test("some string with Hebrew in it")
```

# Relevant Unicode Blocks for Hebrew

To begin with, it's worth noting the relevant unicode blocks. The **full block** is `\u0590-\u05FF`. We have the cantillation (`\u0591-\u05AF`), vowels (`\u05B0-\u05BB`), some other pointing (`\u05BC-\u05C7`), the alphabet (`\u05D0-\u05EA`) and some extras that I don't care about (`\u05Fx`). I have shamelessly stolen this table from <https://en.wikipedia.org/wiki/Unicode_and_HTML_for_the_Hebrew_alphabet>

<table border="2" cellspacing="0" cellpadding="5" style="width:100%;border-collapse:collapse;font-size:large;text-align:center;vertical-align:middle;">
<tr style="background:#F8F8F8;font-size:small">
<td>&#160;</td><td>0</td><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td><td>6</td><td>7</td><td>8</td><td>9</td><td>A</td><td>B</td><td>C</td><td>D</td><td>E</td><td>F</td>
</tr>
<tr>
<td style="background:#F8F8F8;font-size:small">U+059x</td>
<td title="Reserved" style="background-color:#CCCCCC;"></td>
<td title="U+0591: HEBREW ACCENT ETNAHTA"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">֑&#160;</span>‎</td>
<td title="U+0592: HEBREW ACCENT SEGOL"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">֒&#160;</span>‎</td>
<td title="U+0593: HEBREW ACCENT SHALSHELET"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">֓&#160;</span>‎</td>
<td title="U+0594: HEBREW ACCENT ZAQEF QATAN"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">֔&#160;</span>‎</td>
<td title="U+0595: HEBREW ACCENT ZAQEF GADOL"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">֕&#160;</span>‎</td>
<td title="U+0596: HEBREW ACCENT TIPEHA"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">֖&#160;</span>‎</td>
<td title="U+0597: HEBREW ACCENT REVIA"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">֗&#160;</span>‎</td>
<td title="U+0598: HEBREW ACCENT ZARQA"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">֘&#160;</span>‎</td>
<td title="U+0599: HEBREW ACCENT PASHTA"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">֙&#160;</span>‎</td>
<td title="U+059A: HEBREW ACCENT YETIV"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">֚&#160;</span>‎</td>
<td title="U+059B: HEBREW ACCENT TEVIR"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">֛&#160;</span>‎</td>
<td title="U+059C: HEBREW ACCENT GERESH"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">֜&#160;</span>‎</td>
<td title="U+059D: HEBREW ACCENT GERESH MUQDAM"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">֝&#160;</span>‎</td>
<td title="U+059E: HEBREW ACCENT GERSHAYIM"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">֞&#160;</span>‎</td>
<td title="U+059F: HEBREW ACCENT QARNEY PARA"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">֟&#160;</span>‎</td>
</tr>
<tr>
<td style="background:#F8F8F8;font-size:small">U+05Ax</td>
<td title="U+05A0: HEBREW ACCENT TELISHA GEDOLA"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">֠&#160;</span>‎</td>
<td title="U+05A1: HEBREW ACCENT PAZER"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">֡&#160;</span>‎</td>
<td title="U+05A2: HEBREW ACCENT ATNAH HAFUKH"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">֢&#160;</span>‎</td>
<td title="U+05A3: HEBREW ACCENT MUNAH"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">֣&#160;</span>‎</td>
<td title="U+05A4: HEBREW ACCENT MAHAPAKH"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">֤&#160;</span>‎</td>
<td title="U+05A5: HEBREW ACCENT MERKHA"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">֥&#160;</span>‎</td>
<td title="U+05A6: HEBREW ACCENT MERKHA KEFULA"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">֦&#160;</span>‎</td>
<td title="U+05A7: HEBREW ACCENT DARGA"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">֧&#160;</span>‎</td>
<td title="U+05A8: HEBREW ACCENT QADMA"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">֨&#160;</span>‎</td>
<td title="U+05A9: HEBREW ACCENT TELISHA QETANA"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">֩&#160;</span>‎</td>
<td title="U+05AA: HEBREW ACCENT YERAH BEN YOMO"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">֪&#160;</span>‎</td>
<td title="U+05AB: HEBREW ACCENT OLE"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">֫&#160;</span>‎</td>
<td title="U+05AC: HEBREW ACCENT ILUY"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">֬&#160;</span>‎</td>
<td title="U+05AD: HEBREW ACCENT DEHI"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">֭&#160;</span>‎</td>
<td title="U+05AE: HEBREW ACCENT ZINOR"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">֮&#160;</span>‎</td>
<td title="U+05AF: HEBREW MARK MASORA CIRCLE"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">֯&#160;</span>‎</td>
</tr>
<tr>
<td style="background:#F8F8F8;font-size:small">U+05Bx</td>
<td title="U+05B0: HEBREW POINT SHEVA"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ְ&#160;</span>‎</td>
<td title="U+05B1: HEBREW POINT HATAF SEGOL"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ֱ&#160;</span>‎</td>
<td title="U+05B2: HEBREW POINT HATAF PATAH"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ֲ&#160;</span>‎</td>
<td title="U+05B3: HEBREW POINT HATAF QAMATS"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ֳ&#160;</span>‎</td>
<td title="U+05B4: HEBREW POINT HIRIQ"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ִ&#160;</span>‎</td>
<td title="U+05B5: HEBREW POINT TSERE"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ֵ&#160;</span>‎</td>
<td title="U+05B6: HEBREW POINT SEGOL"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ֶ&#160;</span>‎</td>
<td title="U+05B7: HEBREW POINT PATAH"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ַ&#160;</span>‎</td>
<td title="U+05B8: HEBREW POINT QAMATS"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ָ&#160;</span>‎</td>
<td title="U+05B9: HEBREW POINT HOLAM"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ֹ&#160;</span>‎</td>
<td title="U+05BA: HEBREW POINT HOLAM HASER FOR VAV"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ֺ&#160;</span>‎</td>
<td title="U+05BB: HEBREW POINT QUBUTS"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ֻ&#160;</span>‎</td>
<td title="U+05BC: HEBREW POINT DAGESH OR MAPIQ"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ּ&#160;</span>‎</td>
<td title="U+05BD: HEBREW POINT METEG"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ֽ&#160;</span>‎</td>
<td title="U+05BE: HEBREW PUNCTUATION MAQAF"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">־</span>‎</td>
<td title="U+05BF: HEBREW POINT RAFE"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ֿ&#160;</span>‎</td>
</tr>
<tr>
<td style="background:#F8F8F8;font-size:small">U+05Cx</td>
<td title="U+05C0: HEBREW PUNCTUATION PASEQ"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">׀</span>‎</td>
<td title="U+05C1: HEBREW POINT SHIN DOT"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ׁ&#160;</span>‎</td>
<td title="U+05C2: HEBREW POINT SIN DOT"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ׂ&#160;</span>‎</td>
<td title="U+05C3: HEBREW PUNCTUATION SOF PASUQ"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">׃</span>‎</td>
<td title="U+05C4: HEBREW MARK UPPER DOT"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ׄ&#160;</span>‎</td>
<td title="U+05C5: HEBREW MARK LOWER DOT"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ׅ&#160;</span>‎</td>
<td title="U+05C6: HEBREW PUNCTUATION NUN HAFUKHA"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">׆</span>‎</td>
<td title="U+05C7: HEBREW POINT QAMATS QATAN"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ׇ&#160;</span>‎</td>
<td title="Reserved" style="background-color:#CCCCCC;"></td>
<td title="Reserved" style="background-color:#CCCCCC;"></td>
<td title="Reserved" style="background-color:#CCCCCC;"></td>
<td title="Reserved" style="background-color:#CCCCCC;"></td>
<td title="Reserved" style="background-color:#CCCCCC;"></td>
<td title="Reserved" style="background-color:#CCCCCC;"></td>
<td title="Reserved" style="background-color:#CCCCCC;"></td>
<td title="Reserved" style="background-color:#CCCCCC;"></td>
</tr>
<tr>
<td style="background:#F8F8F8;font-size:small">U+05Dx</td>
<td title="U+05D0: HEBREW LETTER ALEF"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">א</span>‎</td>
<td title="U+05D1: HEBREW LETTER BET"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ב</span>‎</td>
<td title="U+05D2: HEBREW LETTER GIMEL"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ג</span>‎</td>
<td title="U+05D3: HEBREW LETTER DALET"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ד</span>‎</td>
<td title="U+05D4: HEBREW LETTER HE"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ה</span>‎</td>
<td title="U+05D5: HEBREW LETTER VAV"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ו</span>‎</td>
<td title="U+05D6: HEBREW LETTER ZAYIN"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ז</span>‎</td>
<td title="U+05D7: HEBREW LETTER HET"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ח</span>‎</td>
<td title="U+05D8: HEBREW LETTER TET"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ט</span>‎</td>
<td title="U+05D9: HEBREW LETTER YOD"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">י</span>‎</td>
<td title="U+05DA: HEBREW LETTER FINAL KAF"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ך</span>‎</td>
<td title="U+05DB: HEBREW LETTER KAF"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">כ</span>‎</td>
<td title="U+05DC: HEBREW LETTER LAMED"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ל</span>‎</td>
<td title="U+05DD: HEBREW LETTER FINAL MEM"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ם</span>‎</td>
<td title="U+05DE: HEBREW LETTER MEM"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">מ</span>‎</td>
<td title="U+05DF: HEBREW LETTER FINAL NUN"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ן</span>‎</td>
</tr>
<tr>
<td style="background:#F8F8F8;font-size:small">U+05Ex</td>
<td title="U+05E0: HEBREW LETTER NUN"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">נ</span>‎</td>
<td title="U+05E1: HEBREW LETTER SAMEKH"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ס</span>‎</td>
<td title="U+05E2: HEBREW LETTER AYIN"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ע</span>‎</td>
<td title="U+05E3: HEBREW LETTER FINAL PE"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ף</span>‎</td>
<td title="U+05E4: HEBREW LETTER PE"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">פ</span>‎</td>
<td title="U+05E5: HEBREW LETTER FINAL TSADI"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ץ</span>‎</td>
<td title="U+05E6: HEBREW LETTER TSADI"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">צ</span>‎</td>
<td title="U+05E7: HEBREW LETTER QOF"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ק</span>‎</td>
<td title="U+05E8: HEBREW LETTER RESH"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ר</span>‎</td>
<td title="U+05E9: HEBREW LETTER SHIN"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ש</span>‎</td>
<td title="U+05EA: HEBREW LETTER TAV"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ת</span>‎</td>
<td title="Reserved" style="background-color:#CCCCCC;"></td>
<td title="Reserved" style="background-color:#CCCCCC;"></td>
<td title="Reserved" style="background-color:#CCCCCC;"></td>
<td title="Reserved" style="background-color:#CCCCCC;"></td>
<td title="Reserved" style="background-color:#CCCCCC;"></td>
</tr>
<tr>
<td style="background:#F8F8F8;font-size:small">U+05Fx</td>
<td title="U+05F0: HEBREW LIGATURE YIDDISH DOUBLE VAV"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">װ</span>‎</td>
<td title="U+05F1: HEBREW LIGATURE YIDDISH VAV YOD"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ױ</span>‎</td>
<td title="U+05F2: HEBREW LIGATURE YIDDISH DOUBLE YOD"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ײ</span>‎</td>
<td title="U+05F3: HEBREW PUNCTUATION GERESH"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">׳</span>‎</td>
<td title="U+05F4: HEBREW PUNCTUATION GERSHAYIM"><span class="script-hebrew" style="font-size: 115%; font-family: 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">״</span>‎</td>
<td title="Reserved" style="background-color:#CCCCCC;"></td>
<td title="Reserved" style="background-color:#CCCCCC;"></td>
<td title="Reserved" style="background-color:#CCCCCC;"></td>
<td title="Reserved" style="background-color:#CCCCCC;"></td>
<td title="Reserved" style="background-color:#CCCCCC;"></td>
<td title="Reserved" style="background-color:#CCCCCC;"></td>
<td title="Reserved" style="background-color:#CCCCCC;"></td>
<td title="Reserved" style="background-color:#CCCCCC;"></td>
<td title="Reserved" style="background-color:#CCCCCC;"></td>
<td title="Reserved" style="background-color:#CCCCCC;"></td>
<td title="Reserved" style="background-color:#CCCCCC;"></td>
</tr>
<tr>
<td colspan="17" style="background:#F8F8F8;font-size:small;text-align:left;padding:10px 20px 0 20px;"><b>Notes</b>
<ol>
<li>1. As of Unicode version 10.0</li>
<li>2. Grey areas indicate non-assigned code points</li>
</ol>
</td>
</tr>
</table>

Some highlights above:
 - Dagesh/Mappiq at `\u05BC`.
 - Maqaf (or however you want to spell that) at `\u05BE`.
 - Inverted Nun - that's fun `\u05C6`.
 - Qamets *Qatan* (`\u05C7`), I have no idea what that is...
 - The `\u05Fx` range is for yiddish digraphs (I think), note the Geresh and Gereshayim in there though are punctuation (rather than accents).

The table above provides "combining characters" - in other words, בְּ is "composed" of three individual code points: `\u05D1` + `\u05BC` + `\u05B0`. But unicode specification also supports "precomposed" characters at a another point in the range...

So the other block we need to consider is `\uFB1D-\uFB1F`. A bunch of these characters are irrelevant for my purposes (what's the point of the wide characters or the alternative Ayin?) but sometimes you've gotta match "Shin with a shin dot"... So again, I have shamelessly stolen the table below from <https://en.wikipedia.org/wiki/Unicode_and_HTML_for_the_Hebrew_alphabet>. In this range, the characters I **actually want to match** are just `\uFB2A-\uFB4E`.

<table border="2" cellspacing="0" cellpadding="5" style="width:100%;border-collapse:collapse;font-size:large;text-align:center;vertical-align:middle;">
<tr style="background:#F8F8F8;font-size:small">
<td>&nbsp;</td><td>0</td><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td><td>6</td><td>7</td><td>8</td><td>9</td><td>A</td><td>B</td><td>C</td><td>D</td><td>E</td><td>F</td>
</tr>
<tr>
<td style="background:#F8F8F8;font-size:small">U+FB1x</td>
<td colspan="13" style="font-size:small">(U+FB00–U+FB1C omitted)</td>
<td title="U+FB1D: HEBREW LETTER YOD WITH HIRIQ"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">יִ</span>‎</td>
<td title="U+FB1E: HEBREW POINT JUDEO-SPANISH VARIKA"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ﬞ&nbsp;</span>‎</td>
<td title="U+FB1F: HEBREW LIGATURE YIDDISH YOD YOD PATAH"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ײַ</span>‎</td>
</tr>
<tr>
<td style="background:#F8F8F8;font-size:small">U+FB2x</td>
<td title="U+FB20: HEBREW LETTER ALTERNATIVE AYIN"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ﬠ</span>‎</td>
<td title="U+FB21: HEBREW LETTER WIDE ALEF"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ﬡ</span>‎</td>
<td title="U+FB22: HEBREW LETTER WIDE DALET"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ﬢ</span>‎</td>
<td title="U+FB23: HEBREW LETTER WIDE HE"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ﬣ</span>‎</td>
<td title="U+FB24: HEBREW LETTER WIDE KAF"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ﬤ</span>‎</td>
<td title="U+FB25: HEBREW LETTER WIDE LAMED"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ﬥ</span>‎</td>
<td title="U+FB26: HEBREW LETTER WIDE FINAL MEM"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ﬦ</span>‎</td>
<td title="U+FB27: HEBREW LETTER WIDE RESH"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ﬧ</span>‎</td>
<td title="U+FB28: HEBREW LETTER WIDE TAV"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ﬨ</span>‎</td>
<td title="U+FB29: HEBREW LETTER ALTERNATIVE PLUS SIGN"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">﬩</span>‎</td>
<td title="U+FB2A: HEBREW LETTER SHIN WITH SHIN DOT"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">שׁ</span>‎</td>
<td title="U+FB2B: HEBREW LETTER SHIN WITH SIN DOT"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">שׂ</span>‎</td>
<td title="U+FB2C: HEBREW LETTER SHIN WITH DAGESH AND SHIN DOT"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">שּׁ</span>‎</td>
<td title="U+FB2D: HEBREW LETTER SHIN WITH DAGESH AND SIN DOT"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">שּׂ</span>‎</td>
<td title="U+FB2E: HEBREW LETTER ALEF WITH PATAH"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">אַ</span>‎</td>
<td title="U+FB2F: HEBREW LETTER ALEF WITH QAMATS"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl"><span style="line-height:normal">אָ</span></span>‎</td>
</tr>
<tr>
<td style="background:#F8F8F8;font-size:small">U+FB3x</td>
<td title="U+FB30: HEBREW LETTER ALEF WITH MAPIQ"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">אּ</span>‎</td>
<td title="U+FB31: HEBREW LETTER BET WITH DAGESH"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">בּ</span>‎</td>
<td title="U+FB32: HEBREW LETTER GIMEL WITH DAGESH"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">גּ</span>‎</td>
<td title="U+FB33: HEBREW LETTER DALET WITH DAGESH"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">דּ</span>‎</td>
<td title="U+FB34: HEBREW LETTER HE WITH MAPIQ"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">הּ</span>‎</td>
<td title="U+FB35: HEBREW LETTER VAV WITH DAGESH"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">וּ</span>‎</td>
<td title="U+FB36: HEBREW LETTER ZAYIN WITH DAGESH"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">זּ</span>‎</td>
<td title="Reserved" style="background-color:#CCCCCC;"></td>
<td title="U+FB38: HEBREW LETTER TET WITH DAGESH"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">טּ</span>‎</td>
<td title="U+FB39: HEBREW LETTER YOD WITH DAGESH"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">יּ</span>‎</td>
<td title="U+FB3A: HEBREW LETTER FINAL KAF WITH DAGESH"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ךּ</span>‎</td>
<td title="U+FB3B: HEBREW LETTER KAF WITH DAGESH"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">כּ</span>‎</td>
<td title="U+FB3C: HEBREW LETTER LAMED WITH DAGESH"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">לּ</span>‎</td>
<td title="Reserved" style="background-color:#CCCCCC;"></td>
<td title="U+FB3E: HEBREW LETTER MEM WITH DAGESH"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">מּ</span>‎</td>
<td title="Reserved" style="background-color:#CCCCCC;"></td>
</tr>
<tr>
<td style="background:#F8F8F8;font-size:small">U+FB4x</td>
<td title="U+FB40: HEBREW LETTER NUN WITH DAGESH"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">נּ</span>‎</td>
<td title="U+FB41: HEBREW LETTER SAMEKH WITH DAGESH"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">סּ</span>‎</td>
<td title="Reserved" style="background-color:#CCCCCC;"></td>
<td title="U+FB43: HEBREW LETTER FINAL PE WITH DAGESH"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ףּ</span>‎</td>
<td title="U+FB44: HEBREW LETTER PE WITH DAGESH"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">פּ</span>‎</td>
<td title="Reserved" style="background-color:#CCCCCC;"></td>
<td title="U+FB46: HEBREW LETTER TSADI WITH DAGESH"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">צּ</span>‎</td>
<td title="U+FB47: HEBREW LETTER QOF WITH DAGESH"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">קּ</span>‎</td>
<td title="U+FB48: HEBREW LETTER RESH WITH DAGESH"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">רּ</span>‎</td>
<td title="U+FB49: HEBREW LETTER SHIN WITH DAGESH"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">שּ</span>‎</td>
<td title="U+FB4A: HEBREW LETTER TAV WITH DAGESH"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">תּ</span>‎</td>
<td title="U+FB4B: HEBREW LETTER VAV WITH HOLAM"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">וֹ</span>‎</td>
<td title="U+FB4C: HEBREW LETTER BET WITH RAFE"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">בֿ</span>‎</td>
<td title="U+FB4D: HEBREW LETTER KAF WITH RAFE"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">כֿ</span>‎</td>
<td title="U+FB4E: HEBREW LETTER PE WITH RAFE"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">פֿ</span>‎</td>
<td title="U+FB4F: HEBREW LIGATURE ALEF LAMED"><span class="script-hebrew" style="font-size: 115%; font-family: Alef, 'SBL BibLit', 'SBL Hebrew', 'David CLM', 'Frenk Ruehl CLM', 'Hadasim CLM', Cardo, Shofar, David, 'Ezra SIL', 'Ezra SIL SR', 'Noto Sans Hebrew', FreeSerif, 'Times New Roman', FreeSans, Arial;" dir="rtl">ﭏ</span>‎</td>
</tr>
<tr>
<td colspan="17" style="background:#F8F8F8;font-size:small;text-align:left;padding:10px 20px 0 20px;"><b>Notes</b>
<ol>
<li>1. As of Unicode version 10.0</li>
<li>2. Grey areas indicate non-assigned code points</li>
</ol>
</td>
</tr>
</table>

# Unicode Regular Expressions (using javascript)

## Building Regexes in Javascript

So now what do we do with all this unicode goodness?

Javascript actually has a number of ways of building a regular expression. For more details, the best source is (MDN)[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions]. In short, regular expressions can be built in two ways:

 - A string (often as a parameter to a function that expects a regular expression). This is useful if you want to generate an expression.
 - A regex style string: `/regex-stuff/`

And you can choose to compile them for reuse: e.g. `const r = new RegExp("match")` or `const r = new RegExp(/match/)` and then `r.test("string with match")`.

Or you can use them directly `/match/.test("string with match")` (this only works with regex style strings).

## Matching Unicode with Regex

To get the unicode goodness into a regular expression, we use the `\u` syntax...

```javascript
// remember that 05BC is the unicode point for a Dagesh
/\u05BC/.test("בְּרֵאשִׁית")
//true
```

But we can also match ranges:

```javascript
/[\u0590-\u05FF]/.test("בְּרֵאשִׁית")
// true
```

It's worth also noting that this `.test` is just checking whether the string has a matching character. If we want to check that the whole string is Hebrew we need to use `^` and `$` (and remember, this will still not match spaces).

```javascript
// The match must span the whole string
/^[\u0590-\u05FF]*$/.test("בְּרֵאשִׁית")

// We can also match multiple ranges:
/[\u0590-\u05FF]/.test("מָקוֹם") // Using a precomposed Holem-waw
// false
/[\u0590-\u05FF\uFB2A-\uFB4E]/.test("מָקוֹם")
// true
```


## Quick Reminder about Regex Functions

Hopefully that will get you going. Just a reminder to check the difference between `search`, `test` and `match`: (note the object these functions exist on and their return type)

### Test

```
regexpObj.test(str)
```

**Returns:** true if there is a match between the regular expression and the specified string; otherwise, false.

### Match

```
str.match(regexp)
```

**Returns:** An Array containing the entire match result and any parentheses-captured matched results; null if there were no matches.

### Search

```
str.search(regexp)
```

**Returns:** The index of the first match between the regular expression and the given string; if not found, -1.