# prog21.dadgum.com static site generator

This is what generates the [Programming in the 21st Century](http://prog21.dadgum.com) site. It's one sub-4K Perl 5 script and a similarly small template. You only need the standard Perl 5 distribution. I initially wrote it in October 2007, and it has evolved since then.

The source for the first three prog21 articles is included as an example.

## Try it!

The `gen` script builds the contents of `site`, then you can open "site/index.html" in a web browser. Generate the latest entry with:

```
./gen
```

or create a specific entry:
```
./gen 1
```

or entries:
```
./gen 2 3
```

or the whole thing, which is instaneous for the prog21 site, so I do it all the time:
```
./gen all
```

Each page is formatted, merged with the template, crunched down a bit, and the "Previously" links are added. The latest page is both at "index.html" and a numbered, permalink-able version. The archive page and atom feed are automatically created.

## Now with your own content

The `titles` file contains a list of article dates and titles, one pair per line. The `entries` directory contains a numbered sequence of HTML files (1.html, 2.html, ...), one for each article. "1.html" matches up with the date/title on line 1 of `titles`. The articles themselves use a mix of custom markup and HTML.

The flow is: Add a new, sequentially numbered file to `entries`. Add a line to the bottom of `titles` with the publication date and a title. Run `gen` when you want to see the results.

The HTML for the site itself is (mostly) in `p21.html`. See "File formats" below.

### Images

Images go into the top-level `images` directory, which gets symlinked from `site`, so you can reference images like "images/vertical_tab.png". 

## Formatting

I designed this in 2007 before I knew about Markdown, so keep that in mind, ok? My goal was not to fully replace HTML (links in HTML are easy enough, for example), but to make it *possible* to write basic articles without any HTML and to have few tags in the general case.

Paragraphs are separated by either a blank line or one of the following block types. I typically keep paragraphs as one long line.

For block quotes, use three triple quotes, before and after, on lines by themselves:
```
""" 
This is a quote.
"""
```
For code examples, use triple minus signs:
```
---
Code line 1
Code line 2
---
```
The only inline formatting is `<|` and `|>` around code, much like Markdown's backticks.

### An advanced special case

Normally a source paragraph is enclosed in HTML paragraph tags by the formatter. This doesn't happen if it already starts with an HTML paragraph tag or if it starts with a heading tag or unordered list tag. This is to allow blocks of HTML for some [special formatting](http://prog21.dadgum.com/88.html) I was already using.

## File formats

The `p21.html` file contains the overall structure of the page, the title bar, and the side bar. Each entry is inserted where the tilde is. I created this file to keep the HTML out of the script, but there are still prog21-specific strings in `gen`.

`titles` is set-up like this:
```
2010-09-23 Advice to Aimless, Excited Programmers
```
The date is always 10 characters (note the leading zero), followed by a space, followed by the title.

## Things that aren't supported

1. Deleting articles. Only once have I wished for this, but when the atom feed has left the station it's hard to stop.
2. Specific times on posts. They're all the same. Always have been.
3. Posting multiple articles in the same day.
4. Escapes for, say, putting code tags around a code tag. The inline code tags were chosen to make this super rare, but use the HTML ampersand notation if needed.
5. Windows line terminators (haven't tried this).

## To do

A long overdue "About" page.
