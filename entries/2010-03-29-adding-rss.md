---
title: Adding RSS and Other Things
date: '2010-03-29 20:34:00'
tags: Meta
id: '2'
topic: Updates
description: Adding RSS to the old Perl-based engine
---

Someone at work today demanded that I add an RSS feed, so here you go: [atom][1]. It didn't take very much to [hack it in][6]. Basically, all I had to do was install a few more CPAN modules, specifically [DateTime::Format::Natural][2], [DateTime::Format::W3CDTF][3], and finally [XML::Atom::SimpleFeed][4]. The first two are so I can put natural-looking dates in my entries and still be able to get full-fledged DateTime objects out of them, and the second is to save me the pain of writing out the Atom format's preferred datetime format. Also, I get neat date formatting in blog entires almost for free with the CLDR syntax.

--fold--

Another thing to notice: [File::Slurp][5] instead of my own `read_file_contents` and `write_file_contents`. It works just as well as mine, except it's more sensitive to list vs scalar context.

[1]: /index.xml
[2]: http://search.cpan.org/dist/DateTime-Format-Natural
[3]: http://search.cpan.org/dist/DateTime-Format-W3CDTF
[4]: http://search.cpan.org/dist/XML-Atom-SimpleFeed
[5]: http://search.cpan.org/dist/File-Slurp
[6]: http://github.com/peterkeen/bugsplat.info/commit/82c41e3a5a27906421692120e2f93ce8869db02f
