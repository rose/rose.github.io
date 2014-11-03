.. title: Ugly Kittens and Booze
.. slug: ugly-kittens-and-booze
.. date: 2014/11/03 07:41:26
.. tags: 
.. link: 
.. description: 
.. type: text

They say life is what happens while you're busy making other plans, and so it is in programming.  

I've spent my time as a programmer merrily bouncing from language to language ("Forth!  Oooh, Haskell... no, assembly!").  But in the meantime, Python was casually easing its way more and more into my brain and fingers.  So many moocs require it, so many blog posts use it for examples, so many of my friends know it.  Over the last few months I've resigned myself to the fact that it has become my strongest language.

I don't mean to sound unkind.  Python's a fine language.  It's the vodka of programming languages - powerful, convenient, mixes well with a lot of things.  If Point B is processed data and a lampshade on your head, Python and vodka will get you there.  But I can't shake the feeling that they're both a little *bland*.

.. TEASER_END

The Flavour of a Language
==========================

I've spent a little time [1]_ thinking about what gives a programming language its flavour.

Building a compiler is a *lot* of work.  Anyone who says they built one just for fun is talking about a toy - nothing wrong with that, I look forward to doing it someday myself, but it has little to do with how popular programming languages were created.  A serious language project starts because someone feels that they can make something significantly superior to all existing languages along some dimension. [2]_

The dimension of a language - the itch that it's meant to scratch - will determine both its initial design and how it evolves over the years.


So What's Python's Dimension?
=============================

There's one way to do it... eh, kinda.  In Masterminds of Programming, [3]_ Guido points out this phrase was coined some years after Python was created, and describes it as "in most cases a white lie." (p.31)

He says he created it for professional \*nix programmers (26) in situations "where C was overkill and shell scripts became too cumbersome." (29)  

"I don't believe there should be a clear distinction between prototyping and 'production' languages.  ... A common process is that a simple application gradually acquires more functionality ... and there is never a precise cutover point from prototype to final application." (29)

In other words, he wanted a language that was simple enough to smash out a quick script in (like bash or perl), but that would provide the abstractions necessary to make large applications manageable (like C++ [4]_).  That was to be Python's killer feature.

This was (and is) achieved by an aggressive process of exclusion.  When asked about adding new features, he talks at length of the "lines of defence" that he uses to keep those features away from the core language. (20)  

Some other relevant quotes:

* "Python is about having the simplest, dumbest compiler imaginable." (26)  
* "If the syntax is so ambiguous that it takes really advanced ... technology to figure it out, then human readers are probably confused half the time as well." (33)
* "You don't [debug a language design].  You won't find the bugs in the language definition until you have so many users that it's too late to change things." (20)
* "There's no feature that you can hide so well that most people don't need to know about [it].  Sooner or later ... they'll encounter some obscure corner case where they have to learn about it because things don't work the way they expected." (21)

The common theme here is simplicity.  Instead of optimizing for performance, or a particular field, or a particular scale, Python was designed to maintain reasonable simplicity over a broad range of fields and scales.  And finally, we've gotten to the heart of my hesitation over it.


Ok, Whatever.  Where are the Kittens?
=====================================

Glad you asked!

When I was a kid we had cats, and the cats frequently had baby cats. [5]_  

Kids have a real fascination with ranking anything they can get their hands on.  Remember making lists of your 'best' and 'worst' friends/songs/foods/numbers? [6]_  Well, we spent a great deal of energy arguing about which kitten was the best.  The winner's reward was intensive mauling by a dozen clumsy snot-covered 6-year-old hands, which may explain the kittens' general lack of enthusiasm for these competitions.  But we took them very seriously indeed.

Popular opinion was generally divided between two forms: sleek-black and fluffy-white.  If pure versions of these were unavailable, gray kittens could easily take the lead regardless of their hair length.  Tortoiseshells would often get a vote or two, as would black and white spotted with interesting patterns.  

After that it was all downhill.  

Spotted cats were tolerable if they were healthy and active and it wasn't your turn with the cute ones.  The existence of tabbies was acknowledged, barely, provided they demonstrated the minimal consideration of not being orange.  The orange ones and the runts were humourous props, over which the more worthy kittens could drape themselves adorably before falling asleep.

... And Then There Was Me
=========================

I always voted for, and subsequently fell in love with, the orangest, runtiest kitten in any given litter.  Bonus points for mewling incessantly, limping, and having one eye sealed shut with pus.  I don't really know why.  Certainly it didn't make me any more popular.  Some combination of perversity, maternal instinct, and budding hipsterism, I suppose.  

In any case, this taste seems to have carried itself over to programming languages.

When you read that Perl has dynamic scoping, or C will casually overwrite your function's return value if you look at a buffer funny, or about `php <http://programmers.stackexchange.com/questions/227756/why-is-phps-method-of-comparing-different-types-bad>`_ and `javascript <http://bonsaiden.github.io/JavaScript-Garden/#types>`_'s *truly* whimsical comparison methods, or pretty much anything about C++, your first thought is probably something like "Dear God why??"  Mine is, "Awww..." 

Now, liking orange cats isn't so bad.  As an adult, at least, it does not seem to have reduced my life satisfaction.  But loving the sickly runts, that's a heartbreak waiting to happen.  And I walked into that heartbreak, over and over and over again, despite how much easier it would have been to just agree with the other kids.  

Let's give Guido the mic again: (on the fundamental concept necessary to be a good Python programmer) "I would say pragmatism... Python is good if you're an instant gratification junkie like myself." (30)

That's not me.  I'm not sure it ever will be.


Can I Have a More Cheerful Ending?
===================================

Sure!  How's this?  

Part of growing up is losing your obsession over 'best' and 'worst', and developing the ability to appreciate things on their own merits.  I'm still a whisky girl, but if my professional life requires me to drink a flask of vodka and play with fluffy kittens, well, I'll suck it up and do my best.


.. raw:: html

  <br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;

.. [1] probably still too much
.. [2] How it becomes popular is another story entirely.  Not all serious projects become popular, but it's a safe bet that most popular languages started as serious projects (with the notable exception of javascript).
.. [3] O'Reilly, 2009.  And apparently the second hand copy I bought is illegal for sale outside of India and Africa, which makes me a dashing outlaw, I guess?
.. [4] Python is *4 years older* than Java; they came out in 1991 and 1995 respectively.  I know, right?
.. [5] My brother and I, and other kids in the neighbourhood, were considerably more enthusiastic about this than my mother was 
.. [6] Maybe that was just me.  But seriously, 7 can go fuck itself.


