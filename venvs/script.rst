Demystifying Python Virtual Environments on UNIX/Linux/MacOS

- Sorry, Windows folks, this is specific to UNIX (Linux, MacOS, *BSD) unless
  you use Windows Subsystem for Linux, aka WSL.  If you'd like to see a
  Windows-native variant of this video, maybe let me know in the comments.  I
  can't promise I'll get to it, because I don't use Windows much, but it's
  possible.

- I've noticed much gnashing of teeth -- even from computer science grads -- about
  some topics related to Python virtual environments.

- When I was a kid, I was both fascinated and befuddled by operating system
  shells.  They always felt like a text dungeon game, ala Zork. Over the years,
  I used many different ones.  Repeated failure to understand them eventually
  led to a pretty decent understanding of what is happening under the hood when
  I typed things into a terminal.  As a twelfth-order result thirty five years
  later, I believe I've gained enough experience such that how Python virtual
  environments actually work is not mystifying to me.

- As we'll see later, it is possible to have multiple different installations
  of Python on the same system, and even if you don't have multiple installs of
  Python on the system, it's useful to know where your Python is coming from.

- Enter the concept of the ``PATH`` environment variable.  ``PATH`` is a
  colon-delimited list of locations which are searched for executables when a
  command is entered into the terminal.  This is true no matter which UNIX
  you're using: MacOS, Linux, or any other Unix.

- How do I see the path?  ``echo $PATH``.

- How can I tell where my Python is coming from?

  - ``type python3``: searches ``PATH`` for the first ``python3`` it finds.

  - "But wait, shouldn't I use ``which`` for that?"  Welp, I thought so too
    until a few hours ago.  But I found that ``which`` gives the wrong answer
    under ``bash`` when shell aliases are involved, and it becomes super
    confusing.  These days, ``type`` is a better idea than ``which`` because it
    understands shell aliases.  ``type python3`` (includes aliases) or
    ``type -p python3`` (doesn't) on zsh, ``type -P python3`` in bash.

  - ``type python3`` just prints the first one found but if you have multiple
    ``python3``s on your ``PATH``, ``type -a python3`` can tell you where each
    of them are, one per line.

- What's up with ``python`` vs ``python3``?

  - This is due to incompatibilities between Python 2 and Python 3, now ancient
    history.  If you're just getting started with Python, you should use
    Python 3.

  - Usually ``python`` is a symbolic link to ``python3``.  Sometimes it's a
    symbolic link to ``python2``.  It depends on the operating system you're
    using.

  - But sometimes ``python3`` just doesn't exist (e.g. in Nix) despite a Python
    Enhancement Proposal having been drawn to suggest it on most operating
    systems (see https://peps.python.org/pep-0394/).

- We will talk specifically about virtual environments soon, but first we need to 
  talk about normal Python installations on your system.

- You may say, "but I don't care about any of this because I only have *one*
  Python.  It's my 'system' Python!".  By this, you often mean
  ``/usr/bin/python3`` (Linux) or
  ``/Library/Frameworks/Python.framework/Versions/3.9/bin/python3`` (MacOS).

  - This is technically true, but if you do any serious Python work, or you
    don't want to drive your crusty coworkers mad when things don't work as
    theirs do, you likely shouldn't use that one (esp. on MacOS).

  - Think from the perspective of the people who write the OS.  They don't put
    Python there so *you* can use it, they put it there because *they* want to
    use it.  They write Python applications too, and they don't really give a
    rip about you.  The Python they ship may lack features, or it could be
    compiled in a way that is nonstandard, or it could even be ripped out or
    the Python version changed arbitrarily during OS updates.

  - Avoid "highlander" mentality. Almost always better to install "your own"
    Python *independent* of the system Python.

    - Usually more up-to-date.

    - Avoid potentially messing up your system.

    - Prevent operating system updates from messing you up when they make
      changes to the system Python.

- How to install another copy of Python?

  - It's particularly important on MacOS to not use the system Python because
    Apple's version of Python currently lacks features (like ``venv``) that
    we'll want to use later, and Apple tends to be more aggressive about making
    changes to its Python install during updates.  On MacOS, I recommend using
    ``nix`` to install another Python.  See
    https://nixos.org/download.html#nix-install-macos

    - After you get Nix installed, to install Python: ``nix-env -iA
      nixpkgs.python311``.  It ends up in ``~/.nix-profile/bin/python3``.  If
      you want to get Python's ``pip`` installed, you will then need to run
      ``~/.nix-profile/bin/python -m ensurepip``.  Note that the tilde stands
      in for your ``$HOME`` directory, e.g. ``/Users/chrism``.

  - On Linux, you can often get away with using the system Python, because
    Linux distributors don't screw with Python as much as Apple does on MacOS.
    But if you find weirdness when you try to use it (commands don't work,
    etc. or your Python is changed arbirarily during an update) you can install
    ``nix`` into your system (see
    https://nixos.org/download.html#nix-install-linux) and then do
    ``nix-env -iA nixpkgs.python311``.  It ends up in
    ``~/.nix-profile/bin/python3``.  If you want to get Python's ``pip``
    installed, you will then need to run ``~/.nix-profile/bin/python3 -m
    ensurepip``.  Note that the tilde stands in for your ``$HOME`` directory,
    e.g. ``/home/chrism``.

- OK, now you should have a Python install that is *all yours*.  It won't be
  used by anything or anyone else on the system, just you.  You can install
  things into it using ``pip`` if you want, and the things you install won't
  have any effect on the system Python.  Likewise, if the OS distributor makes
  changes to the system Python during updates, those won't effect you.

- The Python you installed via ``nix`` will likely be on the ``PATH`` *before* your
  system Python.  Use ``type`` to find out.  If it isn't, you can use the full
  path on the command line to invoke the new Python,
  e.g. ``~/.nix-profile/bin/python3``.
  
- But we aren't out of the woods yet. If you work on more than one project at
  the same time, you may run into situations where these projects need
  conflicting versions of Python libraries (or even conflicting versions of
  Python itself, which, mercifully, this video will not cover).

- How do you deal with those conflicts?  Python virtual environments.  A
  virtual environment is just a directory that pretends to be its own
  standalone Python installation, but it is based on some other installation
  that is already on the system.

- How do I create a virtual environment?

  ``$ cd ~/projects
    $ python -m venv test123
    $ cd test123``

  ``test123`` is "the virtual environment."  It's just a normal directory.

- Invoking its Python.

  ``$ ./bin/python3`` while cd'ed into the test123 directory.

  or
  
  ``$ ~/projects/test123/bin/python3``

- We're done.  We now have a virtual environment based on our custom/global
  Python installation (it will have the same version as our custom Python
  install).  When we use its ``bin/pip``, we will install files into *only* the
  virtual environment.  The custom Python install that we've based it on won't
  be effected.  We can even *delete* the virtual environment (the ``test123``
  directory) without having any impact on the custom Python install on which it
  is based.  Libraries that are installed into our "global" (nix or homebrew)
  Python won't have any effect on the virtual environment.  Life is good.

- "But wait!  Everything else I read tells me to use ``source bin/activate`` to
  'activate' the virtual environment.  Seems pretty important, buddy!"
  Yeaaaaaah, no.  It's just not really necessary and we want to understand
  virtual environments without magic.

  - The only thing ``activate`` does is make it possible to invoke our
    virtualenv's ``python3`` by simply typing ``python3`` and our virtualenv's
    ``pip`` by typing ``pip``.  That's nice, but totally unnecessary.  But
    let's review how executables are found on Linux:

    - If the command typed does not contain a slash, the ``PATH`` is consulted.
      The first executable named by the command found on the ``PATH`` is invoked.

    - If the command typed does contain a slash, the ``PATH`` is not consulted.

  - All ``activate`` really does is change our ``PATH`` so that our virtual
    environment's ``bin`` directory will be the first directory searched.

  - So if we just invoke our virtualenv's Python in such a way that the
    ``PATH`` is not consulted (e.g. we always type the command such that it
    contains a slash e.g.  ``./bin/python3`` or
    ``/home/chrism/projects/test123/bin/python3``), we will always invoke "the
    right" Python.  To quote the Zen of Python by Tim Peters, "explicit is
    better than implicit".  ``import this``.

  - Of course, if you prefer, you *can* use ``activate``.  It just gets kind of
    tiresome if you have many virtual environments on your system.

  - There are various solutions which automatically execute the equivalent of
    ``activate`` when you ``cd`` into the virtual environment directory or any
    of its subdirectories, and deactivate the virtual environment when you
    exit.  If these fit your brain, go for it.

  - But it's just useful (and, eventually, necessary) to understand that no
    magic is happening.  All ``activate`` does is munge ``PATH``.  If you
    don't know this, it's easy to get blocked when ``activate`` doesn't work
    due to some other conflicting configuration in your system.

- "When I'm inside a Python interactive shell, how can I tell if I'm using a
  virtualenv?"  Do ``import sys; sys.path`` at the ``>>>`` prompt.
  ``sys.path`` is a Python list. The last entry in the list will tell you.  If
  that directory is inside your virtualenv, you're using a virtualenv.  If not,
  you're not.
