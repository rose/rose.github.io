.. title: Meet Mr O
.. slug: meet-mr-o
.. date: 2014-11-15 01:37:02 UTC
.. tags: python
.. link: 
.. description: 
.. type: text

A while back I :doc:`promised <methodical-bondage>` to add inheritance to the quick-n-dirty 'classes' I wrote in javascript.  But, I realized that this requires some background on the problems that can arise with multiple inheritance.  So, let's look at how this works in Python, and then we'll implement the algorithm in a later post.

Recall that inheritance is a way to find attributes on an object that the object and its class do not have themselves.  We can think of it as getting advice from a friend:

.. code:: python

  class Carol:
    def hide_body(self):
      return "Carol says:  I think there's still room in the root cellar."

  class Bob(Carol):
    pass

  bob = Bob()
  print (bob.hide_body()) # "Carol says:  I think there's still room in the root cellar."

*note: these code samples use Python 3.  If you're using Python 2, you should have all base classes inherit from object*

Even though Bob doesn't know how to solve his basic life problems, he's ok, because Carol has his back.  Things get a little more complicated when we add more friends:

.. code:: python

  class James:
    def hide_body(self):
      return "James says:  Just turn it over.  If it can't see the police, the police can't see it."

  class Betty(Bob, James):
    pass

  betty = Betty()
  print (betty.hide_body()) 

Does Betty's golf instructor end up in Carol's root cellar or not?  

.. TEASER_END

It's pretty clear that she'll ask Bob for advice first, but given that he has no ideas, will she wait for him to ask Carol (who she doesn't directly know), or will she prefer to go straight to James?  The answer, in every language with multiple inheritance that I know of, is that she waits for a response from Carol.  

This might seem counterintuitive.  After all, Betty has specifically indicated an interest in James' opinion.  But it makes things more predictable for her.  All other things being equal, she doesn't care where Bob and James get their ideas - it could change in the future, and she has no control over that - she just wants to do whatever Bob would do, and use James as a back up for when that fails.

The simplest way to get this behaviour is to do a depth-first search of the friend graph. [1]_  That's what Python did, up to and including version 2.2.  But this can run into some subtle problems too.  Let's look at agroup of people connected to James.  

This is called a diamond inheritance pattern, for reasons that should be obvious if you sketch it out:

.. code:: python

  class Susan(James):
    pass

  class Amelia(James):
    def hide_body(self):
      return "Amelia says:  You start a fire, I'll get the barbecue sauce."

  class Eric(Susan, Amelia):
    pass

  eric = Eric()
  print (eric.hide_body())

What happens now?  The answer depends on whether you're using Python 2.3+. [2]_  The simple depth-first search of 2.2- will end with Eric following James' advice, which is not too terrible for anyone other than Eric.  So why did they change it?

Well, there is a slight oddity here.  Amelia knows James, and has specifically decided that his corpse-hiding methods can be improved upon.  We kind of expect anyone who knows Amelia to respect that.  Eric respects Susan's opinion a little more, but if the best advice she can give him is to ask James, maybe he will prefer to go with Amelia's improved technique.

In other words, we want something that behaves mostly like depth-first search, but that respects any overriden methods that occur in our support network.

The algorithm Python uses to accomplish this is called C3 linearization.  It involves constructing a list of everyone whose advice you might conceivably be willing to follow, in order of preference.  You by getting all your direct friends' lists.  For example, suppose you know Alanis and Joshua:

.. code::

  Alanis would ask Peter, then Paul, then Mary
  Joshua would ask Joseph, then Mary
  You respect Alanis, then Joshua

You'll merge these lists (including your ordering of your friends) to create your own.  Since Alanis is your first choice, you'll start by running down her list in order (starting with her).  However, before adding anyone, you'll always check if there's someone else who should come first.  So the start of your list will look like this:

.. code::

  Alanis, Peter, Paul

But when you hit Mary, [3]_ Joshua will tell you that she is in his list.  So you'll want to add everyone who appears before her on his list (including him) before adding her:

.. code::

  Alanis, Peter, Paul, Joshua, Joseph

If all these people come up empty, then there's no reason not to ask Mary.  So, your final list is:

.. code::

  Alanis, Peter, Paul, Joshua, Joseph, Mary

You can give this list to anyone who wants your advice; they don't need to know the details of how you constructed it.  

This is an overview of how C3 works, and if you understand it you should be able to design inheritance hierarchies that compile and do what you expect them to do. [4]_  As you can see, the algorithm is not terribly complicated, and we'll implement it in a future post, thus solving our body-hiding problems once and for all.

.. raw:: html

  <br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;

.. [1] it's not necessarily a tree, as we'll see in a minute, though unlike a real friendship graph we have to assume there are no cycles.  Truly it is a dystopian world we are creating.
.. [2] spoiler: you are.  You can check by running python --version if you don't believe me.  Remember, if you're using Python 2, your base classes must all inherit from object to get the new behaviour.
.. [3] shame on you.
.. [4] just kidding.  It'll help you debug though.
