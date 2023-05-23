Using Nix as a Homebrew Replacement on MacOS
============================================

- Companion to video at https://www.youtube.com/watch?v=c5xGk7oUvqw

Video Script
------------

- I don't use Macs much but I have a Hackintosh that I do music on.  It runs
  MacOS Catalina, quite old at this point.  I am too chicken to upgrade the OS.
  I used Homebrew on it in the past to get Unix-y software but these days
  Homebrew throws scary messages about Catalina being unsupported.  So Nix.

- I installed Nix on the Mac using the Determinate Systems installer at
  https://github.com/DeterminateSystems/nix-installer .  This is a nice way to
  get Nix because it has an easy uninstaller in case you decide Nix isn't for
  you::

    curl --proto '=https' --tlsv1.2 -sSf -L https://install.determinate.systems/nix | sh -s -- install

- Let's say you want to install ffmpeg, the full version with all the
  hardware-y codecs.

  Go to ``search.nixos.org``, and search for "ffmpeg". ``ffmpeg-full`` seems
  aboout right. Navigate to the nix-env tab for the ``ffmpeg-full`` package.
  It tells you what to type for a non-NixOS system::

    nix-env -iA nixpkgs.ffmpeg_5-full

  Now, this fails out of the box.  There are two workarounds. I can either use
  a slightly different command every time I install a Nix package::

    nix-env -f "<nixpkgs>" -iA ffmpeg_5-full

  Or, better, I can add a nixpkgs channel (I don't quite understand why the
  installer doesn't do this) and use the original command on the
  search.nixos.org website::

    nix-channel --add https://nixos.org/channels/nixpkgs-unstable 
    nix-channel --update

  If you somehow manage to add the wrong channel and ``--update`` fails to
  work, do::

    nix-channel --remove nixpkgs

  And try it again.  You can see the channels that are defined via::

    nix-channel --list

- In any case, once you get a package installed, you can do this to see your
  list of ``nix-env`` installed programs::

    nix-env --query --installed

- That's it.  Keep doing that for all the programs you want.

- It's a little underwhelming to be able to imperatively install programs if
  you're used to, say, NixOS (which is totally declarative) or if you'd like to
  keep a few systems' software in sync with each other by virtue of a shared
  text file describing which programs to install.

- We can keep a list of programs in a Nix expression file instead of
  managing this stuff imperatively, aka "declarative" installation of
  software.

- ``env.nix``::

   with import <nixpkgs> {}; [
     ripgrep
     ffmpeg_5-full
     emacs
     python311
   ]

- ``nix-env -irf env.nix``

- When you want a new program just add it to the stuff between the brackets and
  rerun ``nix-env -irf env.nix``.

  I just have a small batch script named ``upnix.sh`` that contains that single
  ``nix-env -irf /path/to/my/env.nix`` in it.  I run it every time I add a
  package to my ``env.nix``.

- If you've installed via the Determinate Systems installer, you can uninstall
  Nix and everything it installed via ``/nix/nix-installer uninstall``.

Extra credit
------------

  You may see stuff fail to install when you use ``nix-env``. It's a
  little fussy.  For example, you might have tried::

   nix-env -iA ffmpeg

- Aha!  You google around a bit and see that it must be prefixed with
  ``nixpkgs``::

    nix-env -iA nixpkgs.ffmpeg

- Sometimes package "attribute" names don't match their metadata
  names.  You need to install it using the attribute value it wants
  rather than its name.  For example, this would fail.::

    nix-env -iA nixpkgs.ffmpeg-full

  This is one of those annoying cases where the package's attribute
  name differs from its metadata name in ``nixpkgs``. See
  https://search.nixos.org/packages?channel=unstable&show=ffmpeg_5-full&from=0&size=50&sort=relevance&type=packages&query=ffmpeg
  You want the names in blue, not the names in red, so::

    nix-env -iA nixpkgs.ffmpeg_5-full
    
- Another case where common ``nix-env`` patterns and the difference betwen
  attribute names and metadata names cause confusion is *uninstalling* .

  *installing* ``ffmpeg`` is::

    nix-env -iA nixpkgs.ffmpeg_5-full

  Uninstalling the same package is::

    nix-env -e ffmpeg-full
  
  This is another good reason to use the ``env.nix`` file pattern, because
  deleting a package is really just removing it from the text file.
  
