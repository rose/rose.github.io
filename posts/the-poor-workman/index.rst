.. title: The Poor Workman
.. slug: the-poor-workman
.. date: 2014-11-12 04:13:03 UTC
.. tags: 
.. link: 
.. description: 
.. type: text

Recently I found, not one, but *two* people who actually admitted to liking C++!  I had always thought myself alone in that particular perversion.  These people were in a much better position to judge it than I - they both work in C++ for Google, which I understand is an advertising company of some kind that has a largish codebase.

Programmers are people, [1]_ and programming languages, like anything else people talk about, come in and out of fashion.  This has been on my mind since applying for `OPW <http://gnome.org/opw/>`_.

.. TEASER_END

OPW
===

When the projects list first went up, I felt like a kid in an improbably-nerdy-and-likely-soon-to-be-out-of-business candy store.  Linux!  FFmpeg!  Mesos!  I followed the getting started documentation on half a dozen projects, poked around the bug trackers, signed up for mailing lists, and even contacted a few of the mentors.  It was great fun.

I didn't really consciously decide which projects to apply for - I just worked on whatever was most exciting to me each day, and at the end of the application process I had submitted patches for two projects.

Somewhat to my surprise, neither of these were in C or C++.  I think my somewhat geriatric laptop was the deciding factor there - interminable compilation really does knock you out of the zone.  Some of them were also lacking in tests and/or documentation, [2]_ though this varied a lot.

Rather more to my surprise, `one <https://github.com/CPAN-API/cpan-api>`_ of the projects was in Perl.

Perl, Haha!  Wait, I Don't Get It.
==================================

Just to be clear, I don't know Perl.  I know *of* Perl, but my introduction to programming culture was via the blogotetrahedron, [3]_ and I doubt I'll offend anyone by observing that Perl is not super trendy in those triangles.  

But, their project involved learning Elasticsearch, which looked pretty interesting.  The project set up used Vagrant - cool.  The documentation was detailed, and when I hit a problem the mentor and irc channel were both friendly.  Automated test suite with good coverage and continuous integration?  Yes, yes, and yes.  And the code itself was... actually, it was good.  Huh.

As other projects fell by the wayside, I kept looking forward to playing with this one, and eventually that resulted in a pull request and an application.  If I get it, I'll be quite happy to spend this winter hacking Perl.

You Said You Were Going to Talk About C++ And If You Don't I'll Cry
======================================================================

Perl, like C++, has a reputation for impenetrable code.  But it's a poor workman who blames her tools.  Let's hear about the twisted horrors of Google's C++ codebase: [4]_

Antoinetta: "Note, this is C++11 on a very large, well-tooled, coding-rules-strictly-followed code base, which is what this language is designed for. Also, I'm learning it from kind in-person experts who encourage asking questions, which is probably the best way to learn C++."

Gargravarr: "C++11, with strict style guidelines and code review, and liberal use of static analysis tools, is quite nice. Then again, most languages are probably quite nice under those conditions." [5]_

Yes, some languages encourage good practices more than others.  Python does its best to make clean code the path of least resistance, but it is still quite possible to write :doc:`ugly Python<taste-test>`.

C++ and Perl were designed to excel along different dimensions, [6]_ and they certainly do make it easy to write an unreadable mess if you're so inclined (or even if you just don't care).  But it's not a foregone conclusion.  Every tool has its limitations; a good team who cares about what they are doing will work around those limitations and make something great.

.. raw:: html

  <br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;

.. [1] citation needed
.. [2] one project, which shall remain nameless, pointed me to a wiki page that was last edited in 2009.  No, it did not contain a design overview or other gracefully aging data.  No, I did not apply for that project.
.. [3] oh, your blog is still on a sphere?  How quaint.
.. [4] I don't know if my friends are ready to come out of the closet on this particular issue, so I'll use pseudonyms.
.. [5] pretty slick pseudonyms, right?  If you want a slick pseudonym you should tell me interesting things that I can steal for my blog.
.. [6] for C++, it was providing high level abstractions without *any* compromise of program performance compared to C; for Perl, unifying the functionality of disparate command-line tools in a terse system that emulates natural language.

