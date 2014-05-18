.. title: Baby's First Decorator - Caching Functions
.. slug: babys-first-decorator-caching-functions
.. date: 2014/05/18 12:17:11
.. tags: python, perfectionism
.. link: 
.. description: 
.. type: text

If and when I work up the nerve to interview, I know exactly how to answer one question:  My greatest weakness is perfectionism.

This isn't some bullshit "Ah'm so awesome, even mah weaknesses are strengths" answer.  Perfectionism sucks.  There's a line between paying attention to details and being utterly paralyzed by preemptive self-criticism.  My aspiration is to get closer to that line, so I can throw rocks at those attentive assholes.

Hah!  Just kidding, bro.  One thing an aspiring programmer is supposed to do is write a blog that appeals to the kind of person who makes hiring decisions based on blogs [1]_.  

You know the blogs I mean.  The ones full of bouncy little posts about writing a C++ compiler in SQL while completing an iron man triathlon, with just enough self-deprecating humour to show that the author hasn't let his obvious awesomeness go to his head.  Rather enjoy them myself.  

In fact, I've been looking forward to writing a blog like that for some time, but the impressive projects still don't seem to have materialized.  What to do?

Well, step number A is to get over my codestipation.  I've been doing that by forcing out a little pellet of code every morning, regardless of whether I feel the urge.  It's oddly satisfying to just lower my standards and *make* something, no matter how hard [2]_ it is.  

So, here's my first attempt at cheerily boasting about my latest emission.  Ugh.  I mean, enjoy!

ZOMG You Guys Decorators are So Cool
------------------------------------
.. raw:: html 

  <small>An awesome blog post by Rose Ames!  Follow me on Twitter or I'll eat your face!</small><br>&nbsp;


Today I was telling my husband about decorators!  They take a thing and run it through a bunch of publicly unacknowledged manoeuvers, leaving the thing outwardly the same even though inwardly it will never ever be the same again.  So they're basically like sex, except that I'm going to show you how we did them last night.  Ready?  Cool beans!

Here's a fibonacci function in python (I know, pretty vanilla.  We'll get to the good stuff in a minute).

.. code:: python
  
  def fib(n):
    if n <= 2: return 1
    return fib(n-1) + fib(n-2)
    
Import that sucker into your repl and give it a whirl.  If you've never encountered exponential runtime before it's pretty eye-opening.  On my computer, fib(25) is instant, fib(30) has a barely noticeable pause, fib(35) takes a couple seconds, and fib(40) maxed out one of my processors for about 20 seconds before I got bored and ^c'ed it.  

Even if your computer's twice as fast as mine, I bet you don't sit through fib(50).

Now, it's not too much trouble to alter this function, either to make it iterative or to store and look up previously calculated values.  But this is an example of a more general problem - what if this was a big hairy function we didn't want to mess around with?  Or if we had 100 small functions with the same problem?

Well, we're programmers.  Let's automate that shit.

.. code:: python

  def cache(func):
    cache = {}
    def cached_func(arg):
      if arg not in cache:
        result = func(arg)
        cache[arg] = result
      return cache[arg]
    return cached_func

That'll show 'em!  Thanks to the Magic of Closures, we can create a function that stores the output of func(n) the first time we calculate it, and then just returns the stored value the next time that result is requested.  Let's try it out in the Super Evaluating-Loop Box [3]_!

.. code:: python

  >>> cached_fib = cache(fib)
  >>> cached_fib(35) # takes a couple seconds!  HOW CAN THIS BE???
  >>> cached_fib(35) # second call is instant.  Hmmm...

Do you see what's going on here?  We're calling cached_fib(35), and it is caching its result just as we commanded lo these many seconds ago.  However.  All those nasty exponentially-breeding subcalls are to the original, uncached version of fib - so the initial calculation is still mucho crapslow.

But we can fix that, because we're *just that nifty*, aren't we? [4]_

.. code:: python

  >>> fib = cached_fib
  >>> fib(200) # instant

Isn't that beautiful?  If you're not somewhat sexually aroused right now get the hell off my blog.

It turns out this pattern...

.. code:: python
  
  def do_fancy_shit_to(func):
    # create some_other_func by doing fancy shit to func
    return some_other_func

  def pirate_say(arg):
    # do regular shit

  pirate_say = do_fancy_shit_to(pirate_say)

... is actually pretty common.  Common enough that Guido van-passing-an-object-explicitly-to-its-methods-was-good-enough-for-my-gramma-and-dammit-it's-good-enough-for-you-Rossum saw fit to include syntactic sugar for it; the above is exactly equivalent to

.. code:: python

  def do_fancy_shit_to(func):
    # create some_other_func by doing fancy shit to func
    return some_other_func

  @do_fancy_shit_to
  def pirate_say(arg):
    # do regular shit

Thanks Guido!  

Epilogue
--------

Well, I achieved half of the goals [5]_ I set for this post.  It's not perfect, but it's what I've got, and that'll have to do.  At least until I finish my interactive graphic novel/dissertation about the version-controlled distributed database I'm planning to write in brainfuck.

.. raw:: html

  <br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;

.. [1] Not to stereotype, but they're all upper middle class 21-year-old boys who live on pizza and red bull and play a lot of ping-pong.  God help me.
.. [2] and smelly
.. [3] That's what REPL actually stands for.  I think it's Finnish or something.
.. [4] I ASKED YOU A QUESTION SCUM!!!
.. [5] short and crappy.



