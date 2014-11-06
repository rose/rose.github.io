.. title: In Which I Mind the Gap
.. slug: in-which-i-mind-the-gap
.. date: 2014/11/05 20:27:01
.. tags: 
.. link: 
.. description: 
.. type: text

Oooh, a filler post consisting almost entirely of a `link <http://zenpencils.com/comic/90-ira-glass-advice-for-beginners/>`_ to an inspirational quote about why it bothers me to do a filler post consisting almost entirely of a link to an inspirational quote.  Meta!  

A week ago I had my first patch rejected.  Not "oh we'll need a few changes" rejected; we're talking actual, outright "we do not want this code" rejected.  Ouch.

.. TEASER_END

The patch was for Zaqar, the queueing service for OpenStack.  It involved modifying __getattr__ on a base class to allow the elimination of similar methods in its child classes.  Not a big thing, but the methods were just different enough to make it non-trivial; plus it was my first time working with OpenStack, and also the first bug I've tried that wasn't explicitly marked as 'easy' or otherwise appropriate for beginners. [1]_

Boy, did I feel good when I got that sucker working.  Net reduction of nearly 100 lines, booyah!  When someone on irc described my work as 'impressive' I did not object.  Oh hi, yeah, I'm an OpenStack developer.  You know, the impressive one?

And yet... it was kinda hard to read.  

That wasn't my problem though - I mean, they wanted it, and I gave it to them, so I was doing them a favour, right?  They knew what they were doing.  During the review process I added enough comments to make it comprehensible (cutting the line savings in half).

And just when it looked like it was about to be merged, another developer came and said what I had been uneasily thinking.  This is code is complicated.  It's magic.  Anyone looking at these classes in the future will waste many brain cycles trying to understand something which ought to be simple.  And if it ever needs to be modified... he was tactful, but the message was clear.  After some back and forth on irc they decided that no amount of tweaking was going to make this patch usable.

This was not easy to hear.  And yet, part of me was relieved.  I *know* that my skills need work, but I have faith in my taste, and that faith was a little shaken by seeing experienced developers get excited about code that smelled [2]_ a little off to me.  While it was a rejection of my work, it was also a vindication of my taste.

This is a huge change in attitude for me.  There was a time, not so long ago, when I would have been crushed.  

I guess I'm telling this story for people who are still in that boat.  Blogs are not diaries - they're conscious, professional presentations, and the endless stream of successes in your RSS feed is a reflection of that.  For every success they print in red, there are 20 undocumented failures (and probably a catastrophe or two).  This can give you the impression that you're the only one struggling.

There will be a time, perhaps not long from now, when I write a chirpy post about my first contribution to OpenStack.  I won't apologize for that, but I will link back to this one.

If self-confidence is a struggle for you, please go read the `comic <http://zenpencils.com/comic/90-ira-glass-advice-for-beginners/>`_ I linked above, and start being proud of your taste.

.. raw:: html

  <br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;

.. [1] `bug <https://bugs.launchpad.net/zaqar/+bug/1351462>`_, `final patch <https://review.openstack.org/#/c/129109/2/zaqar/queues/storage/pooling.py>`_, in case you're curious.
.. [2] this is not a mixed analogy, because taste is mostly smell.  So there.
