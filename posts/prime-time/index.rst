.. title: Prime Time
.. slug: prime-time
.. date: 2014-11-17 02:43:54 UTC
.. tags: 
.. link: 
.. description: 
.. type: text

I just finished pairing with Gargravarr. [1]_  My sinister metacarpals have been in revolt for the last couple of days, [2]_ so he drove and I mumbled incoherently <esc>d2bi navigated.  We're learning Rust and finding it a little rough.  

.. TEASER_END

The language itself seems quite nice - MLesque syntax along with some really interesting ideas about how to ensure memory safety without incurring the costs of garbage collection - but it has changed so often in the past year that our combined google-fu availed us naught (or at least not much).  

The documentation is in the middle of a major overhaul; what's up so far looks good but seems to be aimed at people without much C or C++ experience.  We often found ourselves asking, "ok, but what's the compiler *doing*?"  This sort of information is not (yet) easily available.

In any case, it was a fun session.  We don't have anything postable just yet, but today's a code day, so I wrote the Sieve of Whatsisname with Python generators:

.. code:: python

  def numbers():
      i = 1
      while True:
          i += 1
          yield i

The generator defined here is about as simple as they get - it lazily generates an infinite sequence of integers one at a time.  Numbers is not a generator itself; it's a function that returns a generator.  You can play with it like this:

.. code:: python

  number_gen = numbers()
  print (next(number_gen)) # 2
  print (next(number_gen)) # 3
  print (next(number_gen)) # 4, and so on
  other_gen = numbers()
  print (next(other_gen)) # 2, we made a whole new generator
  print (next(number_gen)) # 5, picks up from where we left it

Starting at 2 is not a bug, it's a feature.  When you're working with prime numbers 1 is a nuisance.

The next function's slightly more complicated:

.. code:: python

  def make_filter(prime, prev_filter):
      while True:
          candidate = next(prev_filter)
          if candidate % prime != 0:
              yield candidate

This one takes arguments.  Fancy!  

The generators it creates will act as filters, to ensure our prime generator never sees a composite number.  For example, after we have made 7, we want to make sure that no other multiples of 7 are ever generated.  So we'll call make_filter with 7 and our previous chain of filters.  This will create a new generator which just pulls results out of the old chain, and passes them through - provided they are not divisible by 7.

Using numbers and make_filter, we can finally define a prime generator:

.. code:: python

  def primes():
      pipeline = numbers()
      while True:
          next_prime = next(pipeline)
          pipeline = make_filter(next_prime, pipeline)
          yield next_prime

Our pipeline starts as a generator that would like nothing better than to spew an endless stream of mixed prime and composite numbers into your lap.  But, every result it gives us lets us create the next filter we need and stick it on the end of the hose.

Try it out:

.. code:: python

  prime_generator = primes()
  for i in range(100): 
      print(next(prime_generator))

This will give you the first hundred prime numbers, up to 541.  This is not the most practical implementation - the chain of next's in our pipeline will max out your recursion depth pretty quickly - but it's kind of a fun way to play with generators.

I'm sure Whatsisname would be proud.

.. raw:: html

  <br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;

.. [1] The highly quotogenic Dan Luu.  You might remember him from such other posts as :doc:`the-poor-workman` and :doc:`the-write-stuff`.
.. [2] I'm typing this one-handed, and not for the usual reason.
