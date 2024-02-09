======================================
Visual Studio Code for Bad Emacs Users
======================================

Talky-script for this video available at
https://github.com/mcdonc/vids/blob/master/vscode4emacs (includes links to
config files and extensions).

This is a companion to the video at

Rationale
---------

- Customers / Codespaces

- Challenge

- GH copilot

Challenges
----------

- Been using Emacs for 25 years.  But TBH not a very good Emacs user.

- Big user of multiple Emacs frames, but code workspace features and maybe my
  lack of VSCode knowledge tend to conspire to make a single window with tabs
  more useful. Decided to try to limit my editing to a single window and retrain
  my brain to start rather than try to make multiple windows work optimally,
  which they probably never will.

Configuration
-------------

- `Awesome Emacs <https://github.com/whitphx/vscode-emacs-mcx>`_ extension and
  overrides in settings. Most of the bindings I'm used to are provided by this,
  even rectangle editing.  I made some custom overrides, like:

  .. code-block:: json

     // dont put the cursor at the first nonwhitespace character
     {
         "key": "ctrl+a",
         "command": "cursorLineStart",
         "when": "editorTextFocus"
     }

  See `keybindings.json <./keybindings.json>`_ for more.

- Multiroot workspace permits consistent file explorer / git views if you open
  up code every time with that workspace.  I created a directory in my home dir
  named ``vscoderoot`` and put a file named ``vscoderoot.code-workspace`` in it.

  .. code-block:: json

       {
      	"folders": [
     		{
      			"path": "../../../etc/nixos"
      		},
     		{
      			"path": ".."
      		},
     		{
      			"path": "../projects"
      		},
     		{
      			"path": "../projects/apex/climo"
     		}
     	],
      	"settings": {}
    }

  Any time I open vscode, I arrange to open it with this workspace.  It's just
  ``code /home/chrism/vscoderoot/vscoderoot.code-workspace``.  As long as I open
  code for the first time like this, all my buffers are restored.  After this,
  as long as I use the ``-r`` flag to code, all subsequent files are opened in
  the same window and workspace.

- Emulate emacsclient-like behavior poorly:  Shell script to make code open all
  files as a tab in the same workspace and window.  I alias this to the command
  ``edit``.

  .. code-block:: nix

      code-client = pkgs.writeShellScript "code-client" ''
        ${pkgs.procps}/bin/pgrep -x "code" > /dev/null
        if [ $? -eq 1 ];
        then
            ${pkgs.vscode-fhs}/bin/code \
                  /home/chrism/vscoderoot/vscoderoot.code-workspace
        fi
        exec ${pkgs.vscode-fhs}/bin/code -r $@
      '';

  In English, "open code with my multiroot workspace if it's not already
  running. Then open the file I want to edit in that workspace."" The ``-r``
  there means "reuse open window."  This doesn't work for opening directories,
  unfortunately.

- Keyboard shortcuts, see `keybindings.json <./keybindings.json>`_

  - ``Alt+e`` and ``Alt+d`` to hide the left-hand sidebar and activity bar.

- `vsnetrw extension <https://github.com/danprince/vsnetrw>`_ instead of dired.
  mapped to ``Ctrl+x d``.

- `vscode-emacs-tab extension <https://github.com/garaemon/vscode-emacs-tab>`_
  to get Emacs-like tab-to-indent behavior (but don't conflict with accepting
  suggestions).

  .. code-block:: json

     {
        "key":"tab",
        "command":"emacs-tab.reindentCurrentLine",
        "when":"editorTextFocus && !inlineSuggestionVisible"
     }

- `Rewrap extension <https://github.com/stkb/Rewrap>`_ for long line reflowing
  in text docs as ``Alt+q``.

- Stock Python mode comes with linter, with overrides to stop it from complaining
  about not being able to find the source for imports.

  .. code-block:: json

      "python.analysis.diagnosticSeverityOverrides": {
       "reportMissingModuleSource": false,
       "reportMissingImports": false
     }

- Can get something like doom-modeline, left hand side of status bar + `coloured
  status bar problems extension
  <https://github.com/bradzacher/vscode-coloured-status-bar-problems>`_.

Nicenesses
----------

- GH Copilot chat

- ``Ctrl/+`` and ``Ctrl/-`` to change UI scaling

- Multiple cursors (select a word, then ``Ctrl+Shift+L``)

Weirdnesses
-----------

- NixOS: recompile ssh with no-configfile-permcheck patch for git

- In Emacs, ``Ctrl-X 5 2`` makes a new frame. Can open a new window in code, but
  its relationship to the old window is questionable, and the explorer and git
  views may differ. Can drag tabs out so they become new windows in the same
  group as the primary, but can't figure out how to use a keyboard shortcut to
  do this.  But if we drag tabs, we can make it save all its window state at
  shutdown in user ``settings.json``.

  .. code-block:: json

     "window.restoreWindows": "all"

- Using escape as meta conflicts with too much for me but you can try it:

  .. code-block:: json

     "emacs-mcx.useMetaAsEscape": true

- None of the restructured text plugins are as good as rst-mode

Untried
-------

- Any other languages except Python and Nix and a smattering of shell/XML/JSON.

Other Useful Extensions
-----------------------

- `Trailing whitespace extension <https://github.com/jannek/tws>`_ .

- `Preview extension <https://github.com/searKing/preview-vscode>`_ (for rST,
  Markdown, etc.)

- `Reopen closed tab extension <https://github.com/xmile1/reopenclosedtab>`_

- `RedHat XML extension <https://github.com/redhat-developer/vscode-xml>`_.

- `Ruff Python linter/formatter extension
  <https://github.com/astral-sh/ruff-vscode>`.

My ~/.config/Code/User Files
-----------------------------

`keybindings.json <./keybindings.json>`_

`settings.json <./settings.json>`_

- Editor user settings, see ``@modified`` filter in user settings.
