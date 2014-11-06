.. title: Bad Code Should Read Like Poetry
.. slug: bad-code-should-read-like-poetry
.. date: 2014/11/04 11:37:56
.. tags: 
.. link: 
.. description: 
.. type: text

Yesterday's post went a lot longer than I expected, leaving me with a lot to get done today.  So, I wanted to do something short.  Unfortunately, I'm also aiming to alternate code/non-code articles, and code articles can take a while to do even passably well.  

So, the dilemma facing me this morning was, should I just post a cheesy poem, or try to write some (inevitably crappy) code?  Fortunately my friends pointed out that these two choices are not mutually exclusive.

.. TEASER_END

We were talking about Michael Spencer's `brilliant Python limerick <http://nedbatchelder.com/blog/200503/python_limericks.html>`_, and they challenged me to write a limerick, in C, which when compiled & run prints out a haiku.  Here's what I came up with:

.. code:: c

  #include <stdio.h>
  void
  main () { printf( // now the poem is deployed
  "Well it may not be \nKeats, \
  but it hits all the beats \n\
  Great, I surely am\n"); } // just overjoyed.

Now, in this kind of thing you could debate which items should be pronounced and which left out.  I think my choices are defensible but not the only ones possible.  If you need a hand, it's meant to read like this:

.. code::

  include STANdard eye OH dot aitch VOID
  main print EFF now the POEM is dePLOYED
  well it MAY not be KEATS
  but it HITS all the BEATS
  great i SUREly am JUST overJOYED

And that's it for today.
