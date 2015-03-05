---
layout: post
title: Converting PDF to ePub
---

There are many converters one the Internet, but I am not satisfied with their quality. And this is my way to convert the files.

I have a Nook Simple Touch (without backlight), I like it. Cheap, convenient side buttons, Wi-Fi (I dunno why I need that).

Reading PDF on each e-book is really hard, due to resolution, pdf structure itself and maybe a poor functionality of e-book software.

To write good PDF parser is really hard, and impossible. So after some time write my own, I dropped that idea (you can find many posts about the problem of parsing the PDF files).

I decided to use existing software.

First time, I used pdftohtml. It generates HTML exactly the same how book looks, page by page. The text is good but, unfortunately, everything in absolute position. So it is not possible to parse all this HTML and make good ePub book.

Next way was generating DOC file and DOC to HTML. I used Nitro Pro 9 to generate DOC. Again the same problem, there is no possibility for post-processing.

Finally, I used Foxit PhantomPDF, it can generate HTML. This generator was almost perfect. The file is good for post-processing. There is no absolute positioning. Tables, code, titles very good.

Of course, I think it impossible to make everything automatically. You can make some regexp, functions to fix some issues in a file. But most of the time you spend to format the HTML by a hand, all previous steps just can reduce your work.

So my way is:
<ol>
	<li>Foxit PhantomPDF converting PDF to HTML</li>
	<li>Post-processing HTML with regexp, functions and so on.</li>
	<li>Fixing by hand.</li>
</ol>
