.. title: Incremental Complexity
.. slug: incremental-complexity
.. date: 2014/05/26 15:04:02
.. tags: c++
.. link: 
.. description: 
.. type: text

A friend suggested, perhaps facetiously, that I should do a post on incrementation.  It took me down some surprisingly interesting roads; of them all, here's my favourite.

There are a number of implementations of Peano numbers in C++ templates, [1]_ but most of them use ints in some form or other, for example:

.. code:: c++
  
  struct Zero { enum { value = 0 }; };

  template<typename N>
  struct Succ { enum { value = N::value + 1 }; };

This is fine as far as it goes, but it's still explicitly depending on the computer's ability to do arithmetic on ints.  If you're going to type the characters "+ 1" anyway, you might as well give up and use python.  I prefer the following:

.. code:: c++

  struct Zero { };

  template<class P>
  struct Succ { };

This shows what's happening much more clearly.  We're not using numbers to define incrementation; we're **using incrementation to define numbers**.  Let's see what we can do with these numbers...

.. TEASER_END

We can define mathematical functions on them, for example:

.. code:: c++
  
  struct Error { };

  template<class P>
  struct Double { typedef Error value; };

  template<>
  struct Double<Zero> { typedef Zero value; };

  template<class P>
  struct Double<Succ<P> > { typedef Succ<Succ<typename Double<P>::value> > value; };

Don't be dazzled by the otherworldly beauty of the syntax - if you've done any functional programming, you've seen this before.  The first branch is the default case, which would usually come last if we included it at all.  In templates, it's mandatory and it has to come first. [2]_

Then we've got the base and recursive cases:

* Double(Zero) = Zero 
* Double(NonZeroPeano) = 2 + Double(NonZeroPeano - 1) [3]_

The cool thing about doing this in C++ templates, as opposed to, say, Scheme or Haskell, is that all the computations happen at compile time.  The template language is Turing-complete; programs written in it execute when we run the compiler.  Normally, of course, this is only used to generate C++ code more efficiently.  But we can do whatever we like, including building our own language on top of it.

Specifically, we're writing a language that can be run by instantiating a variable of the corresponding type.  One possible program is:

.. code:: c++
  
  // calculate 2 * (1 + 1 + 1 + 2 * (1 + 0) )
  Double<Succ<Succ<Succ<Double<Succ<Zero> >::value > > > >::value program;

Notice that we don't take the successor of Double; we could but it wouldn't make a lot of sense.  Double is a function, and we 'evaluate' it by extracting its value, which will be a Peano number.

We're doing pretty well, but there are still two problems.  First, if we don't use the (C++) variable 'program' anywhere, the compiler will not bother figuring out what type it is.  Even with optimizations disabled, it knows that there's no point.  Another problem: if we did get our program to run somehow, we'd have no way of seeing the output!

We can kill two birds with one stone here, by viewing the final result of our program.  

We'll need to add a little runtime code, because C++ doesn't know how to print Succ<Succ<etc>>s.  The solution is barely horrifying at all if you're used to C++:

.. code:: c++
  
  // at the top of the file
  #include <iostream>
  using namespace std;

  // and down at the bottom, just before main()
  ostream& operator<<(ostream& out, Zero z) {
    out << ".";
    return out;
  }

  template<class Foo>
  ostream& operator<<(ostream& out, Succ<Foo> p) {
    Foo foo;
    out << "s" << foo;
    return out;
  }

This will print peano numbers snake-style, as a string of s's followed by a . which represents zero.  Let's put our program in main & print out the result:

.. code:: c++
  
  int main() {
    Double<Succ<Succ<Succ<Double<Succ<Zero> >::value > > > >::value program;
    cout << program << endl;
  }

and then

.. code:: bash

  $ g++ inc.cpp
  $ ./a.out
  ssssssssss.  # 10

Cool.

You might be wondering how I know that the compiler doesn't execute our program when we don't view the results.  Seems vaguely quantum, doesn't it?

Well, when we say a functional program has no side effects, we're using a very limited idea of what constitutes a side effect.  A program that runs a long time will heat up your processor if nothing else!  So let's make a program that runs for a long time. 

This will be a little less painful if we add some syntatic sugar to our new language:

.. code:: c++

  template<int n>
  struct MakePeano { typedef Succ<typename MakePeano<n-1>::value> value; };

  template<>
  struct MakePeano<0> { typedef Zero value; };

I'm ok with using ints here, because MakePeano is not a fundamental part of the language.  MakePeano<127>::value just saves us from typing Succ 127 times. 

Now we can add a new program to main:

.. code:: c++

  MakePeano<1000>::value program;
  cout << program << endl;

A thousand takes a couple seconds on my computer, and I have to increase the recursion depth with -ftemplate-depth=1000.  YMMV, obviously.  But if I comment out the cout line, it compiles instantly.  

That's it for now.  Once I figured out the material in this post, it wasn't difficult to add lists and conditional statements; now I'm thinking about how to bind and look up variables.  Feel free to clone the `repo <http://www.github.com/rose/schemepile>`_ and play with it yourself!

.. raw:: html
  
  <br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;

.. [1] I did my best to make this post interesting for people who aren't familiar with C++ templates, though obviously reading the code adds a lot.  You should definitely skim the list of `Peano axioms <http://en.wikipedia.org/wiki/Peano_numbers#The_axioms>`_ if you've never seen them.
.. [2] I made the class Error specifically to handle this.  As an actual error handling scheme, it's admittedly half-assed - a call to Double<Succ<ValidNonPeanoClass> > would return Succ<Succ<Error> >, which isn't really ideal.  
.. [3] Why yes, since you ask, I did once check whether C++ template processing is tail-call optimized.  It wasn't, at least not on the compiler I was using then.



