.. title: Keeping Perl Classy
.. slug: keeping-perl-classy
.. date: 2015-01-08 22:21:24 UTC
.. tags: Perl
.. link: 
.. description: 
.. type: text

Perl 1 was released in 1987, before object oriented programming was a popular paradigm; while Perl 5 did add an object system, it is very minimal.  Moose is an extremely popular module which adds more extensive support for object oriented programming.  

I don't know anything about Moose or its history, but I figured I could write something anyway, making educated guesses about how things went down. [1]_  Fortunately haarg came along and sacrificed an hour of his time to save us all from that.  Here's some stuff we typed at each other, with minor rearrangements and editing to make me look literate.  Any resemblance to a real interview is entirely fictional, and you should contact your doctor if it lasts longer than 4 hours.

.. TEASER_END

**I'm talking today with haarg, which I can only hope is his real name.  Haarg is a contributor to Moose, which is Perl's de facto object system.**

Haarg or Graham Knop both work.

**So, how long have you been working on Moose?  Were you one of the first contributors or did you join later?**

I started playing with Moose in 2008, but began contributing a few years later.  My first change was included in the 1.0 release. 

**Was it the standard at that time or were there competing object systems?  Like, I know there's Moo but that obviously came afterwards.**

Around 2008, it was starting to become widely used, but I think there was still a bit of resistance to moving a lot of stuff to use it.

**Performance concerns or something else?**

Performance concerns, memory usage, and just inertia with older designs and systems.  Some of that was unfounded, but not all.

**So, Moose is basically a dynamic system, i.e. more like Python than C++, but Perl does let you** `mix <http://modernperlbooks.com/mt/2009/08/how-a-perl-5-program-works.html>`_ **compile time operations with runtime ones in interesting ways.  Is that used?**

One of the tricks Moose uses is generating accessors and constructors as strings of code and then evaling them.  This eliminates the need for them to do expensive sub calls during object construction.  This is what the ->meta->make_immutable call is for. [2]_  This has a significant up front cost though, so loading Moose and Moose classes is pretty slow.  At runtime, the operations are of equivalent speed to the hand written versions, so if you have long running processes, this isn't a problem.

**Is that where the Tiny** [3]_ **postfix comes from - was it specifically to avoid Moose or is it just to indicate not too many dependencies?**

::Tiny was born earlier on, as a reaction to other heavy modules.  YAML and DateTime are two early examples.A YAML parser was needed as part of the Perl installation toolchain, but the YAML module was very heavy and slow.  This led to YAML::Tiny, which is less capable, but can still do enough to be useful.  It was included in Perl core, although under a different name.  Moo is roughly the ::Tiny equivalent to Moose.  Early in its development, it was called Class::Tiny.  Moo also uses similar tricks to Moose with code generation, but makes different trade offs in terms of when to do that and what other capabilities it has.

**So what's an example of something that Moose does, which the Moo developers decided to omit?**

The big thing here is the entire meta layer.  Moose was in some ways a reaction to the numerous other class builders that existed before it.  It was meant to be a complete system for describing classes, objects, roles, attributes, etc.  With this, it was easy to construct a friendly surface syntax for people to use, but allow other modules powerful tools for manipulating the classes, etc.  Moo replicates most of the surface syntax of Moose, but implements only what is needed to make that part work.  Because it turns out, for 90% of code, the backend meta layer doesn't need to be used directly.

**Can you say a little more about what that meta layer is?  It'd be used to dynamically generate classes, or monkeypatching, or what?**

All of that.  The meta layer is basically having objects to represent all of the Moose classes, attributes, roles, and everything else.  This allows you to interrogate them for information, or attach new behavior.  The meta layer for a class can give you a list of all of the methods the class has, all of the attributes, and all of the parent classes.  And then you can modify these through a consistent interface.  This is all basically possible using raw symbol table manipulations and such, but it's hard to do and hard to keep consistent.

**With great power comes great obfuscation.**

That is also true.  One of the reasons I usually prefer Moo is that it doesn't let you get into the internals, forcing you to keep things simple and only do what is actually required.

**So Perl does actually have an object system, I think it was added in Perl 5?  What do Moose & Moo add to that?  You've already partly answered this.  I guess I'm curious what was left out by the language designers, that made it necessary to add systems like this.**

Yes.  Perl 5 introduced object orientation to Perl, but it is a very rudimentary system.  At the basic level, objects are a combination of data and behaviors.  Perl lets you attach a package to a reference.  The reference is the data, and the package contains the sub[routines]s determining the behavior.  This isn't very user friendly though, so various systems have been created to provide a better syntax and other tools.

**So if you're using the built in object system,the constructor would make a hashref** [4]_ **or something as the object and return that.**

Yes.  The classic Perl constructor is a sub called new (although it could be anything) that creates a hashref, blesses it into a package, and returns it.  It may also store some initial data in it.

**And blessing just means that looking things up on that object looks in the package namespace, and the object itself gets passed as the first arg to any sub calls?**

Yes.  When a method is called on a blessed reference, it looks it up in whatever package it was blessed into, and calls it.  It will also search parent classes to find the method if needed.

**Cool, I keep seeing references to blessed references but didn't get what they were.**

So as an example in code, a hashref is like

.. code:: perl

  my $data = { "key" => "value" };

If you output this as a string, it will be:

.. code:: perl

  HASH(0x7ff134004638)

If you bless it:

.. code:: perl

  bless $data, "My::Package";

it will be printed as:

.. code:: perl

  My::Package=HASH(0x7ff134004638)

This is actually a pretty accurate representation of how it's handled internally.  It's the same hash, but now it has a package attached so it knows where to look for method calls.

**Does it support multiple inheritance?  What's the mro if so?**

Perl does support multiple inheritance.  It uses a DFS algorithm by default, but with Perl 5.10, support for using C3 lookups was added.  Multiple inheritance can be very hard to reason about though, which is one of the things that led to roles being emphasized in Moose.

**Yeah, I did a** :doc:`post <meet-mr-o>` **about Python's move to C3 from DFS.**

Yes, the C3 change in Perl was inspired at least in part by Python.

**So, I'm a little confused about how the package is being 'attached' to the hashref internally.  Did that require a lot of changes to the internal data structures or was it something that was easy to do?**

Perl's various data types have a lot of indirection and ways to attach flags for different capabilities.  Being blessed is one of these.  The difficulty of this is a bit beyond my knowledge though.  An overview of the data types can be seen in the docs for the B module, [5]_ but there's a lot more to it.

**One more about Perl, then we'll get back to Moose :).  Why do you think they chose such a minimal implementation?  Was it a deliberate design decision, or a compromise that was easier to fit into the existing language?**

I wasn't around at the time so I can't give an accurate answer on the motivations, but in my mind there is a certain elegance to it.  If you already have data structures and namespaces, just linking those two together already gives you an object system.  And it ends up giving you a lot of power to construct things like Moose.

**Yeah, that's a good point.  I don't entirely understand the excitement around object oriented programming 10 or 20 years ago, it really is stuff that already existed, just organized in a different way.  I guess you could say that about a lot of big ideas though.**

Organization counts for a lot in large codebases though.  That's why we like Moose, roles, inheritance, etc instead of generating everything manually.

**So Moose adds roles, which are more like mixins or interfaces, right?  I know there's been an idea in the air recently that that's preferable to inheritance in most cases.**

Roles could be described as an interface with a partial implementation.  How much is implemented vs described is dependent on the role.

**So at one extreme, you could use a role as an interface, and at the other it becomes a class?**
A role could have a full implementation of a class, although roles never get constructors so it couldn't be used directly.

**Is that one of the features that has a lot of overhead?  How is it implemented?**

Not particularly.  Most of the overhead involved with Moose is in keeping track of the data structures describing everything, and generating the needed methods.  A non-inlined constructor for example has to look many things up and make many sub calls.  The way roles are implemented is primarily by copying subs between packages.  While subs can be named, they are references that can be stored and moved around like any other.  Named subs are stored in the symbol table, but you can copy the references between different parts of the symbol table to add them to other packages.

**You mentioned earlier that Moose has a substantial startup cost.  Could the method generation be done lazily?**

This is one of the tricks Moo uses.  It does very little processing upfront on the options you give it, but then does the full generation of constructors and accessors when they are first called.  This means if you load a class but never actually create any objects with it, you never have to pay for the constructor generation.

**But this isn't something that's on the docket for Moose.  What would you say is the difference in focus for the two projects?**

Moo is more opinionated than Moose is.  We can impose extra rules such as "don't modify classes after creating an object with it".  Part of what allows us to be more opinionated is that Moose already exists, so if you need the extra power or flexibility, you can use it instead.

**Are a lot of the Moo developers people who had been working on Moose?**

Yes.  Moo was created by mst. [6]_  He used Moose extensively, but because of how heavy it was, he wanted to create an lighter alternative.  In his words, he forced himself to do everything manually for about a year, to figure out what parts he couldn't do without. [7]_  Most of the contributors use Moose extensively as well.  Different projects call for different trade offs, which will suit one better than the other.

**For sure.  Do you feel like the feature sets in Moose and Moo are more or less complete, or are there changes that people are looking at making?**

For Moo, there's very little in terms of new features that we would want to add.  It's mostly extending it to cover new edge cases that are discovered, or allowing it to work better with other new tools that are developed.  For Moose they are looking at extending it a bit more.  While it mostly does what it needs to, there are a few MooseX extensions [8]_ that have proved themselves and are being considered for integration into Moose itself.  This is rather slow going though, as Moose is a large complex codebase.  I can keep the entire Moo codebase in my head at the same time, but changes to Moose always involve more research.

**One more question.  Any advice for people like me, who are working on a Perl project with Moose without much background?  What are the biggest differences for a user, compared to other object-oriented systems like Python or C++ (which are obviously very different from each other as well)?**

There are the obvious differences in terms of how attributes are handled, but I'd say the largest difference is the emphasis on roles as the primary tool to combine behaviors.  Unless you are using lots of MooseX modules, most of the way packages and methods interact are pretty straightforward, but roles are a change in organization that doesn't have a direct comparison in C++ as far as I'm aware.  It would be closer to mixins in Python, but I'm not as familiar with how those work.

**Awesome, thanks so much for your time!**

No problem.

.. raw:: html

  <br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;

.. [1] "The moose bellowed again, pawing the ground and snorting angrily.  I knew I better get the mro right this time..."

.. [2] after a package is defined, you'll often see a call to __PACKAGE__->meta->make_immutable.  As he's describing, it's basically announcing that you don't intend to make any more changes to the class, so as to allow certain optimizations.

.. [3] It's pretty common to see a Perl module called Foo::Tiny that implements similar functionality to Foo, but with more attention to avoiding dependencies and performance hits from uncommonly used features.  They generally don't use Moose.

.. [4] By default, all variables in Perl are passed around by value, but it does provide references (similar to those in Java).  So you'll often see Perl programmers talking about hashrefs and arrayrefs.

.. [5] A standard module that provides you with access to the compiler's parse tree.  I know, right?

.. [6] I've met mst on irc!  He told me (nicely) to rtfm.  I'm pretty sure that makes me a celebrity, but I won't let it go to my head.

.. [7] That's fucking hardcore.  I think this is why the two of us hit it off so well.

.. [8] The MooseX namespace is reserved for packages that extend Moose in some way.  For example, the code I'm working on uses MooseX::Types::ElasticSearch to provide something like an ORM on top of the basic Elasticsearch driver.  But because Moose is just implemented in Perl, you can make more fundamental changes to how the object system works.  I wonder if allowing this kind of evolution is why the Perl 5 team went with such a basic object system in the core language.

