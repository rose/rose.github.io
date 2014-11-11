.. title: Taste Test
.. slug: taste-test
.. date: 2014-11-11 03:13:38 UTC
.. tags: 
.. link: 
.. description: 
.. type: text

*Note: If you got here while googling for answers to your homework, use this code and be welcome!  With a few modifications it will work for fizzbuzz and the traveling salesman problem as well.*

:doc:`in-which-i-mind-the-gap` has gotten a lot more response than I expected. [1]_  Apparently a lot of people, some of whom are inarguably much better programmers than me, are hypercritical of their own code.  One of them questioned whether he had any taste at all, and I wrote this in response:

.. TEASER_END

.. code:: python

  def isMax_elmntINthe_Array(my_the_array, find_max = 3):
      find_min = not find_max
      highestMaxormin_found = 0 # there isn't one so its 0
      for array in my_the_array:
          new_Array1 = my_the_array
          if array in new_Array1:
              doFindMax = not find_min
          else:
              doFindmax = (find_max == 3)
          this_elem = array # set this_elem to the value represented by the variable array
          # this will ensure that they are equal.
          if "doFindMax" in locals():
              try:
                  if doFindMax:
                      if highestMaxormin_found > this_elem:
                          raise NotImplemented # the else clause is slow
                          # so we move these calculations out of this branch to balance the evaluation
                      else:
                          for elem1 in my_the_array:
                              this_elem1 = elem1 # this elem1 is a variable
                              newmax = this_elem
                              highestMaxormin_found = newmax
                  else:
                      raise NotImplemented # see line 25
              except: # see line 24 ^
                  if not doFindMax and doFindMax in locals():
                      this_elem1 = [this_elem2 for this_elem2 in new_Array1 if this_elem2 == this_elem]
                      this_elem1 = this_elem1[int("0")] # an int is 30 - 60 bytes depending on you're
                      # os (windows or iphone or vista) so we can optimize this by using one byte and
                      # making it an int.  TODO find what string would make 3 and optimize findMax
                      if this_elem1 < highestMaxormin_found and doFindMax in locals():
                          highestMaxormin_found = this_elem1
          elif "doFindmax" in locals():
              if not doFindmax:
                  find_min = True
                  if this_elem < highestMaxormin_found:
                      highestMaxormin_found = this_elem
                      raise NotImplemented # this is an error because doFindmax shouldnt exist
      return highestMaxormin_found

It was actually pretty fun to write, and I think he got the point.  While this is obviously super exaggerated, the mistakes (useless comments, untested optimizations, inefficient algorithms, misleading variable names, etc) are all ones that actual programmers actually make.  

Sometime when you're doubting yourself, try sitting down and writing some crappy code.  It's a great way to consciously remember what you've forgotten you know.

.. raw:: html

  <br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;<br>&nbsp;

.. [1] which was none
