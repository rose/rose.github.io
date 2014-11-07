.. title: Methodical Bondage
.. slug: methodical-bondage
.. date: 2014/11/06 16:34:45
.. tags: 
.. link: 
.. description: 
.. type: text

I mentioned a couple days ago that I look forward to writing a compiler 'someday'.  Well, in the spirit of Failure Month, let's lower our sights enough that we can s/some/to.  Let's just see how much of a class system we can put together.  In this post, we'll get method binding working properly, and in a future one we'll look at inheritance.

To avoid muddying the waters with the implementation language's classes, we'll use one that doesn't have any - javascript.  We'll avoid the dark corners & just work with functions and hash tables, so if you know any scripting language you should be able to follow this fine. [1]_

Let's start without methods, and just get attribute lookups happening.  We want to build the backend compilery bits that implement this sort of functionality:

.. code:: python

  class Foo: # make a class
    boop = 3 # instances of this class have an attribute boop, which by default is 3

  f = Foo()  # make an instance of the class
  f2 = Foo() # and another one

  f.boop = 7 # set f's attribute boop to 7

  print (f.boop) # get f's attribute boop, which is now 7
  print (f2.boop) # 3; f2's boop is undefined, so we get the class value, which is not changed

We're not bothering with the frontend compilery bits, so we won't have that nice syntax to work with.  Instead, we'll test by calling the backend functions directly in javascript.  So the equivalent of the code above will look something like:

.. code:: javascript

  var Foo = make_class({boop: 3}); 

  var f = instantiate_class(Foo); 
  var f2 = instantiate_class(Foo);

  set_attr(f, 'boop', 7);

  // console.log is the javascript equivalent of print
  console.log("This should be 7: ", get_attr(f, 'boop'));
  console.log("This should be 3: ", get_attr(f2, 'boop'));

After these functions are defined, then we can get to the good stuff.  Excited?

.. TEASER_END

We'll start with classes:

.. code:: javascript

  function make_class(attrs) {
    var new_class = {};
    new_class['attrs'] = attrs;
    return new_class;
  }

  function instantiate_class(cls) {
    var new_obj = {};
    new_obj['class'] = cls;
    new_obj['attrs'] = {};
    return new_obj;
  }

Nothing exciting here.  Classes and objects both store a hash of attributes, and objects also have a reference back to their class.

Now for get/set.  When we look for an attribute on an object, first we check if the object itself has it; if not, we check its class.  But setting attributes on an object should never touch the class attributes:

.. code:: javascript

  function get_attr(obj, attr_name) {
    var attrs = obj['attrs'];
    if (attr_name in attrs) {
      return attrs[attr_name];
    }
    return obj['class']['attrs'][attr_name]; // javascript will return undefined if this doesn't exist
  }

  function set_attr(obj, attr_name, value) {
    obj['attrs'][attr_name] = value;
  }

So far so good.  You can test this out on the sample code above if you want.  

Now we're ready for methods.  We'll define them as attributes that happen to be functions.  The only tricky bit is, a method needs access to the attributes of the object it was called on, but methods are defined before the object even exists!

No worries, we just need to ensure that methods get their calling object passed in as the first argument. [2]_  This is called binding the method, and it's less complicated than it sounds:

.. code:: javascript

  function bind_method(method, obj) {
    return function(args) {
      args.unshift(obj); // unshift is like pushing, but onto the front of the array
      return method(args);
    }
  }

See?  Instead of returning the original method, we're returning a function that adds the calling object to the front of its argument list, and calls the original method with that.  The description is more complicated than the code. [3]_

Now we just need to change get_attr to use bind_method when the requested attribute is a function:

.. code:: javascript

  function get_attr(obj, attr_name) {
    var ret;
    var attrs = obj['attrs'];

    if (attr_name in attrs) {
      ret = attrs[attr_name];
    }
    else {
      ret = obj['class']['attrs'][attr_name];
    }

    if (typeof(ret) == 'function') {
      ret = bind_method(ret, obj);
    }
    return ret;
  }

Again, if you're playing along at home, you can test it out.  The following code...

.. code:: javascript

  var Bar = make_class ({
    "bam": 8,
    "x_plus_bam": function(args) {
      var self = args[0];
      var x = args[1];
      return get_attr(self, 'bam') + x;
    }
  });

  var b = instantiate_class(Bar);
  var bound_method = get_attr(b, "x_plus_bam");

  console.log("This should be 13: ", bound_method([5]));
  set_attr(b, 'bam', 9);
  console.log("This should be 14: ", bound_method([5]));

... is equivalent to:

.. code:: python

  class Bar:
    bam = 8
    def x_plus_bam(self, x):
      return self.bam + x

  b = Bar()
  bound_method = b.x_plus_bam

  print(bound_method(5)) # 13
  b.bam = 9
  print(bound_method(5)) # 14

Finally, now that we have methods, let's alter our instantiate_class function to take a list of arguments, and pass them on to the class's init method if it exists:

.. code:: javascript

  function instantiate_class(cls, args) {
    var new_obj = {};
    new_obj['class'] = cls;
    new_obj['attrs'] = {};
    var init = get_attr(new_obj, 'init');
    if (typeof(init) == 'function') {
      init(args);
    }
    return new_obj;
  }

Note that we use getattr, so the new object is passed in as the first argument to init.  Thus defining an init function looks a lot like any other method:

.. code:: javascript

  var Baz = make_class ({
    "init": function(args) {
      var self = args[0];
      var num = args[1];
      set_attr(self, 'yay', num);
    }
  });

  var baz = instantiate_class(Baz, [42]);

  console.log("This should be 42: ", get_attr(baz, 'yay'));

All right, that was fun!  The code's `up on github <https://github.com/rose/classes>`_ if you want to play around with it.  Next code day we'll work on inheritance, and learn a little about MRO algorithms.

.. raw:: html

  <br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;

.. [1] I'm deliberately using a simplified version of javascript here.  {} is actually an object, it has properties other than the ones we define, and foo["blarg"] would often be written foo.blarg instead.  Javascript also has a very interesting inheritance system that is not based on classes at all.  I'm ignoring all that and using javascript objects as simple hash tables, so you shouldn't take this code as idiomatic.
.. [2] This is how Python does it.  A lot of other object oriented languages just have a keyword, like 'self' or 'this', which they put in scope by other means.  The basic idea, of referring to the object on which the method was called, is the same.
.. [3] Note that I'm assuming methods get exactly one array as their argument.  Remember, this is all internal compiler stuff - users of our language can pass however many arguments of whatever type they like, and we will put them into an array for our own use (or would, if we were building a whole compiler).
