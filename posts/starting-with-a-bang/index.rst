.. title: Starting with a Bang
.. slug: starting-with-a-bang
.. date: 2014/11/02 10:13:30
.. tags: 
.. link: 
.. description: 
.. type: text

You don't use a linux system for long without learning about !!.  It's handy in situations like this:

.. code:: bash

  $ echo "I'm king of the world!" >/etc/fstab
  lolwutno
  $ sudo !! # DO NOT ACTUALLY DO THIS
  sudo echo "I'm king of the world!" >/etc/fstab
  pswdplz:
  okfinewtvr

But history expansion can do much more than repeat the last command.

You can retrieve any command from your history:

.. code:: bash

  $ history
  ...
  502 echo "I'm king of the world"
  503 ls
  504 touch yesyouare
  505 echo "I'm king of the world"
  506 ls
  507 cd /etc
  $ !-3 # !! is the last command, !-2 is the command before that, and so on
  echo "I'm king of the world"
  I'm king of the world
  $ sudo !504
  sudo touch yesyouare

But it's often easier to search for the last command that started with a given string:

.. code:: bash

  $ !echo
  echo "I'm king of the world"
  I'm king of the world
  $ !v
  vim ~/reasons_I_need_a_psychiatrist

These are simple string substitutions - bash replaces !! with the characters of your last command, then executes the resulting command.  So with !! and the numerical operators you can do stuff like:

.. code:: bash

  $ vim dave
  dave's not here
  $ ls
  dave.py
  $ !-2.py # !vim.py would look for a command beginning with 'vim.py'
  vim dave.py

You can also extract parts of commands!  The one I use most is !$ for the last argument:

.. code:: bash

  $ ls some/godawful/long/path/probably/from/a/java/project/
  file.txt    otherstuff.java
  $ vim !$file.txt
  vim some/godawful/long/path/probably/from/a/java/project/file.txt

You can also get all the arguments, or just the first one:

.. code:: bash

  $ touch foo bar baz # whoops, I'm in the wrong directory
  $ mv !* projects/foobar
  mv foo bar baz projects/foobar

.. code:: bash

  $ cp /boot/initramfs-linux.img ~/backupdir
  $ sudo vim !^
  sudo vim /boot/initramfs-linux.img # I've got a GREAT idea!

There's more, but I'll stop here for now.

You can go pretty far with only a very basic understanding of bash, but shells are actually very interesting tools that will help you more if you give them a chance.  If you spend a lot of time on the command line, it's well worth the learning curve.
