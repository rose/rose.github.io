.. title: Baby's First Decorator - Caching Functions
.. slug: babys-first-decorator-caching-functions
.. date: 2014/05/18 12:17:11
.. tags: python
.. link: 
.. description: 
.. type: text

Here's a fibonacci function in python.

.. code:: python
  
  def fib(n):
    if n <= 2: return 1
    return fib(n-1) + fib(n-2)
    
You probably know why this is terrible.  If you've never encountered exponential runtime before, try running this on increasing numbers - it's pretty eye-opening.  

.. code:: python

  >>> for i in range(0,51,5):
  ...   print ("fib %d: %d" % (i, fib(i)))

On my computer, the values up to 25 print instantly.  30 takes a noticeable fraction of a second, 35 a couple of seconds... and 40 maxes out one of my processors until I get bored and kill it.  Even if your computer and your attention span are both twice as good as mine, I bet you don't sit through fib(50).

Now, it's not too much trouble to write a better fib; we could make it iterative, or store and look up previously calculated values.  But this is an example of a more general problem - what if this was a big hairy function we didn't want to mess around with?  Or if we had 100 small functions with the same issue?

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

Thanks to the Magic of Closures, we can create a function that stores the output of func(n) the first time we calculate it, and then just returns the stored value the next time that result is requested.  Let's try it out in the Super Evaluating-Loop Box [1]_:

.. code:: python

  >>> cached_fib = cache(fib)
  >>> cached_fib(35) # takes a couple seconds!  HOW CAN THIS BE???
  >>> cached_fib(35) # second call is instant.  Hmmm...

Do you see what's going on here?  We're calling cached_fib(35), and it is caching its result just as we commanded lo these many seconds ago.  However.  All those exponentially-breeding subcalls are to the original, uncached version of fib - so the initial calculation is still mucho crapslow.

But we can fix that, because we're *just that nifty*.

.. code:: python

  >>> fib = cached_fib
  >>> fib(200) # instant

Awesome.

It turns out this pattern...

.. code:: python
  
  def do_fancy_stuff_to(func):
    # create some_other_func by doing fancy stuff to func
    return some_other_func

  def pirate_say(arg):
    # do regular stuff

  pirate_say = do_fancy_stuff_to(pirate_say)

... is actually pretty common.  Common enough that Guido van-passing-an-object-explicitly-to-its-methods-was-good-enough-for-my-gramma-and-dammit-it's-good-enough-for-you-Rossum saw fit to include syntactic sugar for it; the above is exactly equivalent to

.. code:: python

  def do_fancy_stuff_to(func):
    # create some_other_func by doing stuff to func
    return some_other_func

  @do_fancy_stuff_to
  def pirate_say(arg):
    # do regular stuff

Thanks Guido!  

.. raw:: html

  <br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;

.. [1] That's what REPL actually stands for.  I think it's Finnish or something.



