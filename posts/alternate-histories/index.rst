.. title: Alternate Histories
.. slug: alternate-histories
.. date: 2014-11-09 02:02:35 UTC
.. tags: 
.. link: 
.. description: 
.. type: text

A couple days ago I wrote :doc:`a post <starting-with-a-bang>` about bash's history completion.  It has since been brought to my attention that lesser programmers are in the habit of making "mistakes" when typing commands.  This post is dedicated to those unfortunate souls.

If you're unsure what a history expansion will do, you can have bash print it out:

.. code:: console

  $ touch !-3*:p  # recall that !-3* is the arguments of the third most recent command.
                  # the new command is echoed but *not* run
  touch king of the world

The expanded line will be added to your history even though it wasn't run, so if it's what you wanted you can run it with !!.  If it's close but not quite right, hit up or ^P to edit it.

This colon nonsense also lets you perform substitutions:

.. code:: console

  $ vin ordinaire
  bash: vin: command not found
  $ !!:s/n/m
  vim ordinaire

Note that only the first instance of the first string is substituted.  Using gs instead of s will replace every match.

That's handy, but a little verbose.  There's a short form that's equivalent to !!:s:

.. code:: console

  $ grep -lIr awesomeness ./* # whoops, I wanted that to be case insensitive
  $ ^l^il
  grep -ilIr awesomeness ./*
  ./todo:1005: (Note: this item should increase my AWESOMENESS by 2.7%.)

Finally, in the last post I forgot^H^H^H^H^H^Hwas too awesome to mention that !? will find a match anywhere in the command, not just the beginning:

.. code:: console

  $ !?dev
  vim /dev/null

This is not exhaustive, believe it or not, but we've covered all of the options that I use.  If you think you need to know more about bash history expansion, I recommend seeking immediate psychiatric help.  But while you're waiting you could read what `Arabesque <http://blog.sanctum.geek.nz/bash-history-expansion/>`_ or `Peteris Krumins <http://www.catonmat.net/blog/the-definitive-guide-to-bash-command-line-history/>`_ have to say about the subject (spoiler: a lot).


