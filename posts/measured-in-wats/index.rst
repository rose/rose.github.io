.. title: Measured In Wats
.. slug: measured-in-wats
.. date: 2014-11-13 01:37:43 UTC
.. tags: 
.. link: 
.. description: 
.. type: text

Knowledge is power - it's measured in wats.  That's the best line in the post, but you can keep reading if you want.

Javascript has a parseInt function that does what you'd expect:

.. code:: javascript

  > parseInt("3")
  3

Its array prototype has a map function which also does what you'd expect:

.. code:: javascript

  > foo = [1,2,3]
  > function addOne(x) { return x + 1 }
  > foo.map(addOne)
  [ 2, 3, 4 ]

I lied.  Neither of these functions does what you probably think they do, at least not exactly.

.. code:: javascript

  > ['7','9','11','13'].map(parseInt)

I'm not going to tell you what the output of that is.  Type it in to your browser console or Node.  I dare you.

.. TEASER_END

Before I explain what's going on here, a little justification.  A friend recently objected to the common practice of reducing javascript (or any other language) to a series of wats.  I see her point, but I think wats serve a useful purpose.  

It's my experience, and there's `some research <http://www.scientificamerican.com/article/learning-by-surprise/>`_ backing up the idea, that I remember things that surprise me better.  So something I learn via a wat is likely to stick with me for longer than if I just read it in a textbook.  

For that matter, I'm more likely to learn it in the first place - I like thinking I understand the world, and when new information shakes that faith I'll go to great lengths to modify my mental model to accommodate that information.  Finally, that model gets reinforced every time I share a neat wat with my friends or blog readers. [1]_

I think this is what Peter Siebel is talking about in his `great post <http://www.gigamonkeys.com/code-reading/>`_ on code reading:

  Code is not literature and we are not readers... instead of trying to pick out a piece of code and ... then discussing it like a bunch of Comp Lit. grad students, I think [it's better] to play the role of a 19th century naturalist returning from a trip to some exotic island to present .. the crazy beetles they found.

In that spirit, let's examine our beetle a little more closely.  Like most wats, this one is not so difficult to understand once you have all the necessary information.

First, *all* arguments in javascript are optional.  If a caller sends more than the expected number, the extra ones will not be bound to any name (though they will be available via an args array [2]_ ).  If the caller sends too few, the omitted parameters will just be undefined.

.. code:: javascript

  > function describe_args(x, y) { return "x is " + x + "y is " + y }
  > describe_args(1,2,3) 
  'x is 1, y is 2'
  > describe_args(1) 
  'x is 1, y is undefined'

Second, parseInt takes an optional second argument, which is the base to use in parsing the string:

.. code:: javascript

  > parseInt("110", 2)
  6

It doesn't make sense for this argument to be less than 2.  Most of the time that will result in NaN, but 0 is interpreted as base 10. [3]_  

Third, as you may have guessed by now, map passes two arguments in to the mapping function for each element - the element itself, and the index at which it appears:

.. code:: javascript

  > foo = [5, 10, 20]
  > function describe_element(element, index) { return "element " + index + " is " + element }
  > foo.map(describe_element)
  [ 'element 0 is 5',
    'element 1 is 10',
    'element 2 is 20' ]

And that's all there is to it.  Map passes on the index to parseInt on the assumption that it will be ignored if it's not wanted; parseInt sees a second argument and assumes that the caller is specifying a base.  While I might never run across this exact beetle in the wild, it is a vivid and memorable demonstration of how optional arguments can cause unexpected - dare I call it buggy - behaviour.

.. raw:: html

  <br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;

.. [1] that's an inclusive or.  Please stop crying.
.. [2] it's not exactly an array, but that's a post for another day.
.. [3] I suspect that this is just an unintentional side effect of 0 being falsy (like undefined) - the implementation of parseInt might do something like `if (!base) base = 10`.  But that's just a guess.

