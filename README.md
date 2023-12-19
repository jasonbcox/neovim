README
======


          NVIM REFERENCE MANUAL


Differences between Nvim and Vim             *vim-differences*

Nvim differs from Vim in many ways, although editor and Vimscript (not
Vim9script) features are mostly identical.  This document is a complete and
centralized reference of the differences.

              Type |gO| to see the table of contents.

==============================================================================
# Configuration                *nvim-config*

User configuration and data files are found in standard |base-directories|
(see also |$NVIM_APPNAME|).  Note in particular:

- Use `$XDG_CONFIG_HOME/nvim/init.vim` instead of `.vimrc` for your |config|.
- Use `$XDG_CONFIG_HOME/nvim` instead of `.vim` to store configuration files.
- Use `$XDG_STATE_HOME/nvim/shada/main.shada` instead of `.viminfo` for persistent
  session information.  |shada|

==============================================================================
# Defaults                      *nvim-defaults*

- Filetype detection is enabled by default. This can be disabled by adding
  ":filetype off" to |init.vim|.
- Syntax highlighting is enabled by default. This can be disabled by adding
  ":syntax off" to |init.vim|.
- Default color scheme has been updated. This can result in color schemes
  looking differently due to them relying on how highlight groups are defined
  by default. Add ":colorscheme vim" to |init.vim| or your color scheme file to
  restore the old default links and colors.

- 'autoindent' is enabled
- 'autoread' is enabled (works in all UIs, including terminal)
- 'background' defaults to "dark" (unless set automatically by the terminal/UI)
- 'backspace' defaults to "indent,eol,start"
- 'backupdir' defaults to .,~/.local/state/nvim/backup// (|xdg|), auto-created
- 'belloff' defaults to "all"
- 'comments' includes "fb:•"
- 'commentstring' defaults to ""
- 'compatible' is always disabled
- 'complete' excludes "i"
- 'define' defaults to "". The C ftplugin sets it to "^\\s*#\\s*define"
- 'directory' defaults to ~/.local/state/nvim/swap// (|xdg|), auto-created
- 'display' defaults to "lastline"
- 'encoding' is UTF-8 (cf. 'fileencoding' for file-content encoding)
- 'fillchars' defaults (in effect) to "vert:│,fold:·,foldsep:│"
- 'formatoptions' defaults to "tcqj"
- 'hidden' is enabled
- 'history' defaults to 10000 (the maximum)
- 'hlsearch' is enabled
- 'include' defaults to "". The C ftplugin sets it to "^\\s*#\\s*include"
- 'incsearch' is enabled
- 'isfname' does not include ":" (on Windows). Drive letters are handled
  correctly without it. (Use |gF| for filepaths suffixed with ":line:col").
- 'joinspaces' is disabled
- 'langnoremap' is enabled
- 'langremap' is disabled
- 'laststatus' defaults to 2 (statusline is always shown)
- 'listchars' defaults to "tab:> ,trail:-,nbsp:+"
- 'mouse' defaults to "nvi"
- 'mousemodel' defaults to "popup_setpos"
- 'nrformats' defaults to "bin,hex"
- 'path' defaults to ".,,". The C ftplugin adds "/usr/include" if it exists.
- 'ruler' is enabled
- 'sessionoptions' includes "unix,slash", excludes "options"
- 'shortmess' includes "CF", excludes "S"
- 'showcmd' is enabled
- 'sidescroll' defaults to 1
- 'smarttab' is enabled
- 'startofline' is disabled
- 'switchbuf' defaults to "uselast"
- 'tabpagemax' defaults to 50
- 'tags' defaults to "./tags;,tags"
- 'termguicolors' is enabled by default if Nvim can detect support from the
  host terminal
- 'ttimeoutlen' defaults to 50
- 'ttyfast' is always set
- 'undodir' defaults to ~/.local/state/nvim/undo// (|xdg|), auto-created
- 'viewoptions' includes "unix,slash", excludes "options"
- 'viminfo' includes "!"
- 'wildmenu' is enabled
- 'wildoptions' defaults to "pum,tagfile"

- |editorconfig| plugin is enabled, .editorconfig settings are applied.
- |man.lua| plugin is enabled, so |:Man| is available by default.
- |matchit| plugin is enabled. To disable it in your config: >vim
    :let loaded_matchit = 1

- |g:vimsyn_embed| defaults to "l" to enable Lua highlighting

DEFAULT MOUSE
            *default-mouse* *disable-mouse*
By default the mouse is enabled, and <RightMouse> opens a |popup-menu| with
standard actions ("Cut", "Copy", "Paste", …). Mouse is NOT enabled in
|command-mode| or the |more-prompt|, so you can temporarily disable it just by
typing ":".

If you don't like this you can disable the mouse in your |config| using any of
the following:
- Disable mouse completely by unsetting the 'mouse' option: >vim
  set mouse=
- Pressing <RightMouse> extends selection instead of showing popup-menu: >vim
  set mousemodel=extend
- Pressing <A-LeftMouse> releases mouse until the cursor moves:  >vim
  nnoremap <A-LeftMouse> <Cmd>
    \ set mouse=<Bar>
    \ echo 'mouse OFF until next cursor-move'<Bar>
    \ autocmd CursorMoved * ++once set mouse&<Bar>
    \ echo 'mouse ON'<CR>
<
To remove the "How-to disable mouse" menu item and the separator above it: >vim
  aunmenu PopUp.How-to\ disable\ mouse
  aunmenu PopUp.-1-
<
DEFAULT MAPPINGS
              *default-mappings*
Nvim creates the following default mappings at |startup|. You can disable any
of these in your config by simply removing the mapping, e.g. ":unmap Y".

- Y |Y-default|
- <C-U> |i_CTRL-U-default|
- <C-W> |i_CTRL-W-default|
- <C-L> |CTRL-L-default|
- & |&-default|
- # |v_#-default|
- * |v_star-default|
- Nvim LSP client defaults |lsp-defaults|
  - K |K-lsp-default|

DEFAULT AUTOCOMMANDS
              *default-autocmds*
Default autocommands exist in the following groups. Use ":autocmd! {group}" to
remove them and ":autocmd {group}" to see how they're defined.

nvim_terminal:
- BufReadCmd: Treats "term://" buffers as |terminal| buffers. |terminal-start|
- TermClose: A |terminal| buffer started with no arguments (which thus uses
  'shell') and which exits with no error is closed automatically.

nvim_cmdwin:
- CmdwinEnter: Limits syntax sync to maxlines=1 in the |cmdwin|.

nvim_swapfile:
- SwapExists: Skips the swapfile prompt (sets |v:swapchoice| to "e") when the
  swapfile is owned by a running Nvim process. Shows |W325| "Ignoring
  swapfile…" message.

==============================================================================
New Features                   *nvim-features*

MAJOR COMPONENTS

- API                             |API|
- Job control                     |job-control|
- LSP framework                   |lsp|
- Lua scripting                   |lua|
- Parsing engine                  |treesitter|
- Providers
  - Clipboard                     |provider-clipboard|
  - Node.js plugins               |provider-nodejs|
  - Python plugins                |provider-python|
  - Ruby plugins                  |provider-ruby|
- Remote plugins                  |remote-plugin|
- Shared data                     |shada|
- Terminal emulator               |terminal|
- UI                              |ui| |--listen| |--server|
- Vimscript parser                |nvim_parse_expression()|
- XDG base directories            |xdg|

USER EXPERIENCE

Working intuitively and consistently is a major goal of Nvim.

              *feature-compile*
- Nvim always includes ALL features, in contrast to Vim (which ships various
  combinations of 100+ optional features).  |feature-compile| Think of it as
  a leaner version of Vim's "HUGE" build. This reduces surface area for bugs,
  and removes a common source of confusion and friction for users.

- Nvim avoids features that cannot be provided on all platforms; instead that
  is delegated to external plugins/extensions. E.g. the `-X` platform-specific
  option is "sometimes" available in Vim (with potential surprises:
  https://stackoverflow.com/q/14635295).

- Vim's internal test functions (test_autochdir(), test_settime(), etc.) are
  not exposed (nor implemented); instead Nvim has a robust API.

- Behaviors, options, documentation are removed if they cost users more time
  than they save.

Usability details have been improved where the benefit outweighs any
backwards-compatibility cost. Some examples:

- Directories for 'directory' and 'undodir' are auto-created.
- Terminal features such as 'guicursor' are enabled where possible.
- Various "nvim" |cli-arguments| were redesigned.

Some features are built in that otherwise required external plugins:

- Highlighting the yanked region, see |vim.highlight|.

ARCHITECTURE

The Nvim UI is "decoupled" from the core editor: all UIs, including the
builtin |TUI| are just plugins that connect to a Nvim server (via |--server|
or |--embed|). Multiple Nvim UI clients can connect to the same Nvim editor
server.

External plugins run in separate processes. |remote-plugin| This improves
stability and allows those plugins to work without blocking the editor. Even
"legacy" Python and Ruby plugins which use the old Vim interfaces (|if_pyth|,
|if_ruby|) run out-of-process, so they cannot crash Nvim.

Platform and I/O facilities are built upon libuv. Nvim benefits from libuv
features and bug fixes, and other projects benefit from improvements to libuv
by Nvim developers.

FEATURES

Command-line:
  The expression prompt (|@=|, |c_CTRL-R_=|, |i_CTRL-R_=|) is highlighted
  using a built-in Vimscript expression parser. |expr-highlight|
          *E5408* *E5409*
  |input()|, |inputdialog()| support custom highlighting. |input()-highlight|
          *g:Nvim_color_cmdline*
  (Experimental) Command-line (|:|) is colored by callback defined in
  `g:Nvim_color_cmdline` (this callback is for testing only, and will be
  removed in the future).

Commands:
  |:checkhealth|
  |:drop| is always available
  |:Man| is available by default, with many improvements such as completion
  |:match| can be invoked before highlight group is defined
  |:source| works with Lua
  User commands can support |:command-preview| to show results as you type
  |:write| with "++p" flag creates parent directories.

Events:
  |RecordingEnter|
  |RecordingLeave|
  |SearchWrapped|
  |Signal|
  |TabNewEntered|
  |TermClose|
  |TermOpen|
  |UIEnter|
  |UILeave|

Functions:
  |dictwatcheradd()| notifies a callback whenever a |Dict| is modified
  |dictwatcherdel()|
  |menu_get()|
  |msgpackdump()|, |msgpackparse()| provide msgpack de/serialization
  |stdpath()|
  |system()|, |systemlist()| can run {cmd} directly (without 'shell')
  |matchadd()| can be called before highlight group is defined
  |tempname()| tries to recover if the Nvim |tempdir| disappears.
  |writefile()| with "p" flag creates parent directories.

Highlight groups:
  |highlight-blend| controls blend level for a highlight group
  |expr-highlight| highlight groups (prefixed with "Nvim")
  |hl-NormalFloat| highlights floating window
  |hl-FloatBorder| highlights border of a floating window
  |hl-FloatTitle| highlights title of a floating window
  |hl-FloatFooter| highlights footer of a floating window
  |hl-NormalNC| highlights non-current windows
  |hl-MsgArea| highlights messages/cmdline area
  |hl-MsgSeparator| highlights separator for scrolled messages
  |hl-Substitute|
  |hl-TermCursor|
  |hl-TermCursorNC|
  |hl-WinSeparator| highlights window separators
  |hl-Whitespace| highlights 'listchars' whitespace
  |hl-WinBar| highlights 'winbar'
  |hl-WinBarNC| highlights non-current window 'winbar'

Input/Mappings:
  ALT (|META|) chords always work (even in the |TUI|). Map |<M-| with any key:
  <M-1>, <M-BS>, <M-Del>, <M-Ins>, <M-/>, <M-\>, <M-Space>, <M-Enter>, etc.
  Case-sensitive: <M-a> and <M-A> are two different keycodes.

  ALT may behave like <Esc> if not mapped. |i_ALT| |v_ALT| |c_ALT|

Normal commands:
  |gO| shows a filetype-defined "outline" of the current buffer.
  |Q| replays the last recorded macro instead of switching to Ex mode (|gQ|).

Options:
  Local values for global-local number/boolean options are unset when the
  option is set without a scope (e.g. by using |:set|), similarly to how
  global-local string options work.

  'autoread'    works in the terminal (if it supports "focus" events)
  'background'  colorscheme is only reloaded if value is changed, not every
                time it is set
  'cpoptions'   flags: |cpo-_|
  'diffopt'     "linematch" feature
  'exrc'        searches for ".nvim.lua", ".nvimrc", or ".exrc" files. The
                user is prompted whether to trust the file.
  'fillchars'   flags: "msgsep", "horiz", "horizup", "horizdown",
                "vertleft", "vertright", "verthoriz"
  'foldcolumn'  supports up to 9 dynamic/fixed columns
  'guicursor'   works in the terminal (TUI)
  'inccommand'  shows interactive results for |:substitute|-like commands
                and |:command-preview| commands
  'jumpoptions' "view" tries to restore the |mark-view| when moving through
  the |jumplist|, |changelist|, |alternate-file| or using |mark-motions|.
  'laststatus'  global statusline support
  'mousescroll' amount to scroll by when scrolling with a mouse
  'pumblend'    pseudo-transparent popupmenu
  'scrollback'
  'shortmess'   "F" flag does not affect output from autocommands
  'signcolumn'  supports up to 9 dynamic/fixed columns
  'statuscolumn' full control of columns using 'statusline' format
  'tabline'     %@Func@foo%X can call any function on mouse-click
  'termpastefilter'
  'ttimeout', 'ttimeoutlen' behavior was simplified
  'winblend'    pseudo-transparency in floating windows |api-floatwin|
  'winhighlight' window-local highlights

Providers:
  If a Python interpreter is available on your `$PATH`, |:python| and
  |:python3| are always available. See |provider-python|.

Shell:
  Shell output (|:!|, |:make|, …) is always routed through the UI, so it
  cannot "mess up" the screen. (You can still use "chansend(v:stderr,…)" if
  you want to mess up the screen :)

  Nvim throttles (skips) messages from shell commands (|:!|, |:grep|, |:make|)
  if there is too much output. No data is lost, this only affects display and
  improves performance. |:terminal| output is never throttled.

  |:!| does not support "interactive" commands. Use |:terminal| instead.
  (GUI Vim has a similar limitation, see ":help gui-pty" in Vim.)

  :!start is not special-cased on Windows.

  |system()| does not support writing/reading "backgrounded" commands. |E5677|

Signs:
  Signs are removed if the associated line is deleted.
  Signs placed twice with the same identifier in the same group are moved.

Startup:
  |-e| and |-es| invoke the same "improved Ex mode" as -E and -Es.
  |-E| and |-Es| read stdin as text (into buffer 1).
  |-es| and |-Es| have improved behavior:
    - Quits automatically, don't need "-c qa!".
    - Skips swap-file dialog.
  |-s| reads Normal commands from stdin if the script name is "-".
  Reading text (instead of commands) from stdin |--|:
    - works by default: "-" file is optional
    - works in more cases: |-Es|, file args

TUI:
      *:set-termcap*
  Start Nvim with 'verbose' level 3 to show terminal capabilities: >
  nvim -V3
<
      *'term'* *E529* *E530* *E531*
  'term' reflects the terminal type derived from |$TERM| and other environment
  checks.  For debugging only; not reliable during startup. >vim
    :echo &term
<  "builtin_x" means one of the |builtin-terms| was chosen, because the expected
  terminfo file was not found on the system.

  Nvim will use 256-colour capability on Linux virtual terminals.  Vim uses
  only 8 colours plus bright foreground on Linux VTs.

  Vim combines what is in its |builtin-terms| with what it reads from terminfo,
  and has a 'ttybuiltin' setting to control how that combination works.  Nvim
  uses one or the other, it does not attempt to merge the two.

UI/Display:
  |Visual| selection highlights the character at cursor. |visual-use|

  messages: When showing messages longer than 'cmdheight', only
  scroll the message lines, not the entire screen. The
  separator line is decorated by |hl-MsgSeparator| and
  the "msgsep" flag of 'fillchars'. *msgsep*

Variables:
  |v:progpath| is always absolute ("full")
  |v:windowid| is always available (for use by external UIs)
  |OptionSet| autocommand args |v:option_new|, |v:option_old|,
  |v:option_oldlocal|, |v:option_oldglobal| have the type of the option
  instead of always being strings. |v:option_old| is now the old global value
  for all global-local options, instead of just string global-local options.

Vimscript:
  |:redir| nested in |execute()| works.

==============================================================================
Upstreamed features           *nvim-upstreamed*

These Nvim features were later integrated into Vim.

- 'fillchars' flags: "eob"
- 'jumpoptions' "stack" behavior
- 'wildoptions' flags: "pum" enables popupmenu for wildmode completion
- |<Cmd>|
- |WinClosed|
- |WinScrolled|
- |:sign-define| "numhl" argument
- |:source| works with anonymous (no file) scripts
- 'statusline' supports unlimited alignment sections

==============================================================================
Other changes             *nvim-changed*

This section documents various low-level behavior changes.

|mkdir()| behaviour changed:
1. Assuming /tmp/foo does not exist and /tmp can be written to
   mkdir('/tmp/foo/bar', 'p', 0700) will create both /tmp/foo and /tmp/foo/bar
   with 0700 permissions. Vim mkdir will create /tmp/foo with 0755.
2. If you try to create an existing directory with `'p'` (e.g. mkdir('/',
   'p')) mkdir() will silently exit. In Vim this was an error.
3. mkdir() error messages now include strerror() text when mkdir fails.

|string()| and |:echo| behaviour changed:
1. No maximum recursion depth limit is applied to nested container
   structures.
2. |string()| fails immediately on nested containers, not when recursion limit
   was exceeded.
3. When |:echo| encounters duplicate containers like >vim

       let l = []
       echo [l, l]
<
   it does not use "[...]" (was: "[[], [...]]", now: "[[], []]"). "..." is
   only used for recursive containers.
4. |:echo| printing nested containers adds "@level" after "..." designating
   the level at which recursive container was printed: |:echo-self-refer|.
   Same thing applies to |string()| (though it uses construct like
   "{E724@level}"), but this is not reliable because |string()| continues to
   error out.
5. Stringifyed infinite and NaN values now use |str2float()| and can be evaled
   back.
6. (internal) Trying to print or stringify VAR_UNKNOWN in Vim results in
   nothing, E908, in Nvim it is internal error.

|json_decode()| behaviour changed:
1. It may output |msgpack-special-dict|.
2. |msgpack-special-dict| is emitted also in case of duplicate keys, while in
   Vim it errors out.
3. It accepts only valid JSON.  Trailing commas are not accepted.

|json_encode()| behaviour slightly changed: now |msgpack-special-dict| values
are accepted, but |v:none| is not.

Viminfo text files were replaced with binary (messagepack) |shada| files.
Additional differences:

- |shada-c| has no effect.
- |shada-s| now limits size of every item and not just registers.
- 'viminfo' option got renamed to 'shada'. Old option is kept as an alias for
  compatibility reasons.
- |:wviminfo| was renamed to |:wshada|, |:rviminfo| to |:rshada|.  Old
  commands are still kept.
- ShaDa file format was designed with forward and backward compatibility in
  mind. |shada-compatibility|
- Some errors make ShaDa code keep temporary file in-place for user to decide
  what to do with it.  Vim deletes temporary file in these cases.
  |shada-error-handling|
- ShaDa file keeps search direction (|v:searchforward|), viminfo does not.

|printf()| returns something meaningful when used with `%p` argument: in Vim
it used to return useless address of the string (strings are copied to the
newly allocated memory all over the place) and fail on types which cannot be
coerced to strings. See |id()| for more details, currently it uses
`printf("%p", {expr})` internally.

|c_CTRL-R| pasting a non-special register into |cmdline| omits the last <CR>.

|CursorMoved| triggers when moving between windows.

Lua interface (|lua.txt|):

- `:lua print("a\0b")` will print `a^@b`, like with `:echomsg "a\nb"` . In Vim
  that prints `a` and `b` on separate lines, exactly like
  `:lua print("a\nb")` .
- `:lua error('TEST')` emits the error: >
  E5108: Error executing lua: [string "<Vimscript compiled string>"]:1: TEST
<  whereas Vim emits only "TEST".
- Lua has direct access to Nvim |API| via `vim.api`.
- Lua package.path and package.cpath are automatically updated according to
  'runtimepath'. |lua-module-load|

Commands:
  |:doautocmd| does not warn about "No matching autocommands".
  |:wincmd| accepts a count.
  `:write!` does not show a prompt if the file was updated externally.
  |:=| does not accept |ex-flags|. With an arg it is equivalent to |:lua=|

Command-line:
  The meanings of arrow keys do not change depending on 'wildoptions'.

Functions:
  |input()| and |inputdialog()| support for each other’s features (return on
  cancel and completion respectively) via dictionary argument (replaces all
  other arguments if used), and "cancelreturn" can have any type if passed in
  a dictionary.
  |input()| and |inputdialog()| support user-defined cmdline highlighting.

Highlight groups:
  |hl-ColorColumn|, |hl-CursorColumn| are lower priority than most other
  groups
  |hl-CurSearch| highlights match under cursor instead of last match found
  using |n| or |N|
  |hl-CursorLine| is low-priority unless foreground color is set
  |hl-VertSplit| superseded by |hl-WinSeparator|
  Highlight groups names are allowed to contain `@` characters.
  It is an error to define a highlight group with a name that doesn't match
  the regexp `[a-zA-Z0-9_.@-]*` (see |group-name|).

Macro/|recording| behavior
  Replay of a macro recorded during :lmap produces the same actions as when it
  was recorded. In Vim if a macro is recorded while using :lmap'ped keys then
  the behaviour during record and replay differs.

  'keymap' is implemented via :lmap instead of :lnoremap so that you can use
  macros and 'keymap' at the same time. This also means you can use |:imap| on
  the results of keys from 'keymap'.

Mappings:
- Creating a mapping for a simplifiable key (e.g. <C-I>) doesn't replace an
  existing mapping for its simplified form (e.g. <Tab>).
- "#" followed by a digit doesn't stand for a function key at the start of the
  lhs of a mapping.

Motion:
  The |jumplist| avoids useless/phantom jumps.

Performance:
  Folds are not updated during insert-mode.

Syntax highlighting:
  syncolor.vim has been removed. Nvim now sets up default highlighting groups
  automatically for both light and dark backgrounds, regardless of whether or
  not syntax highlighting is enabled. This means that |:syntax-on| and
  |:syntax-enable| are now identical. Users who previously used an
  after/syntax/syncolor.vim file should transition that file into a
  colorscheme. |:colorscheme|

Vimscript compatibility:
  `count` does not alias to |v:count|
  `errmsg` does not alias to |v:errmsg|
  `shell_error` does not alias to |v:shell_error|
  `this_session` does not alias to |v:this_session|

Working directory (Vim implemented some of these after Nvim):
- |DirChanged| and |DirChangedPre| can be triggered when switching to another
  window or tab.
- |getcwd()| and |haslocaldir()| may throw errors if the tab page or window
  cannot be found.  *E5000* *E5001* *E5002*
- |haslocaldir()| checks for tab-local directory if and only if -1 is passed as
  window number, and its only possible returns values are 0 and 1.
- `getcwd(-1)` is equivalent to `getcwd(-1, 0)` instead of returning the global
  working directory. Use `getcwd(-1, -1)` to get the global working directory.

Autocommands:
- Fixed inconsistent behavior in execution of nested autocommands:
  https://github.com/neovim/neovim/issues/23368
- |TermResponse| is fired for any OSC sequence received from the terminal,
  instead of the Primary Device Attributes response. |v:termresponse|

==============================================================================
Missing features           *nvim-missing*

These legacy Vim features are not yet implemented:

- *:gui*
- *:gvim*
- *'completepopup'*
- *'previewpopup'*

==============================================================================
Removed legacy features           *nvim-removed*

These Vim features were intentionally removed from Nvim.

Aliases:
  ex        (alias for "nvim -e")
  exim      (alias for "nvim -E")
  gex       (GUI)
  gview     (GUI)
  gvim      (GUI)
  gvimdiff  (GUI)
  rgview    (GUI)
  rgvim     (GUI)
  rview
  rvim
  view      (alias for "nvim -R")
  vimdiff   (alias for "nvim -d" |diff-mode|)

Commands:
  :behave
  :fixdel
  *hardcopy* `:hardcopy` was removed. Instead, use `:TOhtml` and print the
  resulting HTML using a web browser or other HTML viewer.
  :helpfind
  :mode (no longer accepts an argument)
  :open
  :Print
  :promptfind
  :promptrepl
  :scriptversion (always version 1)
  :shell
  :sleep! (does not hide the cursor; same as :sleep)
  :smile
  :tearoff
  :cstag
  :cscope
  :lcscope
  :scscope
  :Vimuntar

Compile-time features:
  Emacs tags support
  X11 integration (see |x11-selection|)

Cscope:
                                                                      *cscope*
  Cscope support was removed in favour of plugin-based solutions such as:
  https://github.com/dhananjaylatkar/cscope_maps.nvim

Eval:
  Vim9script
  *cscope_connection()*
  *err_teapot()*
  *js_encode()*
  *js_decode()*
  *v:none* (used by Vim to represent JavaScript "undefined"); use |v:null| instead.
  *v:sizeofint*
  *v:sizeoflong*
  *v:sizeofpointer*

Events:
  *SafeStateAgain*
  *SigUSR1* Use |Signal| to detect `SIGUSR1` signal instead.

Highlight groups:
  *hl-StatusLineTerm* *hl-StatusLineTermNC* are unnecessary because Nvim
    supports 'winhighlight' window-local highlights.
    For example, to mimic Vim's StatusLineTerm:  >vim
      hi StatusLineTerm ctermfg=black ctermbg=green
      hi StatusLineTermNC ctermfg=green
      autocmd TermOpen,WinEnter * if &buftype=='terminal'
        \|setlocal winhighlight=StatusLine:StatusLineTerm,StatusLineNC:StatusLineTermNC
        \|else|setlocal winhighlight=|endif
<

Options:
  *'aleph'* *'al'*
  antialias
  'backspace' no longer supports number values. Instead:
    - for `backspace=0` set `backspace=` (empty)
    - for `backspace=1` set `backspace=indent,eol`
    - for `backspace=2` set `backspace=indent,eol,start` (default behavior in Nvim)
    - for `backspace=3` set `backspace=indent,eol,nostop`

  *'balloondelay'* *'bdlay'*
  *'ballooneval'* *'beval'* *'noballooneval'* *'nobeval'*
  *'balloonexpr'* *'bexpr'*
  bioskey (MS-DOS)
  conskey (MS-DOS)
  *'cp'* *'nocompatible'* *'nocp'* *'compatible'* (Nvim is always "nocompatible".)
  'cpoptions' (gjkHw<*- and all POSIX flags were removed)
  *'cryptmethod'* *'cm'* *'key'* (Vim encryption implementation)
  cscopepathcomp
  cscopeprg
  cscopequickfix
  cscoperelative
  cscopetag
  cscopetagorder
  cscopeverbose
  *'ed'* *'edcompatible'* *'noed'* *'noedcompatible'*
  'encoding' ("utf-8" is always used)
  esckeys
  'guioptions' "t" flag was removed
  *'guifontset'* *'gfs'* (Use 'guifont' instead.)
  *'guipty'* (Nvim uses pipes and PTYs consistently on all platforms.)
  'highlight' (Names of builtin |highlight-groups| cannot be changed.)
  *'hkmap'* *'hk'* use `set keymap=hebrew` instead.
  *'hkmapp'* *'hkp'* use `set keymap=hebrewp` instead.
  keyprotocol

  *'pastetoggle'* *'pt'* Just Paste It.™ |paste| is handled automatically when
  you paste text using your terminal's or GUI's paste feature (CTRL-SHIFT-v,
  CMD-v (macOS), middle-click, …).

  *'imactivatefunc'* *'imaf'*
  *'imactivatekey'* *'imak'*
  *'imstatusfunc'* *'imsf'*
  *'insertmode'* *'im'* Use the following script to emulate 'insertmode':
>vim
    autocmd BufWinEnter * startinsert
    inoremap <Esc> <C-X><C-Z><C-]>
    inoremap <C-C> <C-X><C-Z>
    inoremap <C-L> <C-X><C-Z><C-]><Esc>
    inoremap <C-Z> <C-X><C-Z><Cmd>suspend<CR>
    noremap <C-C> <Esc>
    snoremap <C-C> <Esc>
    noremap <C-\><C-G> <C-\><C-N><Cmd>startinsert<CR>
    cnoremap <C-\><C-G> <C-\><C-N><Cmd>startinsert<CR>
    inoremap <C-\><C-G> <C-X><C-Z>
    autocmd CmdWinEnter * noremap <buffer> <C-C> <C-C>
    autocmd CmdWinEnter * inoremap <buffer> <C-C> <C-C>

    lua << EOF
      vim.on_key(function(c)
        if c == '\27' then
          local mode = vim.api.nvim_get_mode().mode
          if mode:find('^[nvV\22sS\19]') and vim.fn.getcmdtype() == '' then
            vim.schedule(function()
              vim.cmd('startinsert')
            end)
          end
        end
      end)
    EOF
<
  *'macatsui'*
  *'maxcombine'* *'mco'*
    Nvim counts maximum character sizes in bytes, not codepoints. This is
    guaranteed to be big enough to always fit all chars properly displayed
    in vim with 'maxcombine' set to 6.

    You can still edit text with larger characters than fits in the screen buffer,
    you just can't see them. Use |g8| or |ga|. See |mbyte-combining|.

    NOTE: the rexexp engine still has a hard-coded limit of considering
    6 composing chars only.

  *'maxmem'* Nvim delegates memory-management to the OS.
  *'maxmemtot'* Nvim delegates memory-management to the OS.
  printoptions
  *'printdevice'*
  *'printencoding'*
  *'printexpr'*
  *'printfont'*
  *'printheader'*
  *'printmbcharset'*
  *'prompt'* *'noprompt'*
  *'remap'* *'noremap'*
  *'restorescreen'* *'rs'* *'norestorescreen'* *'nors'*
  *'secure'*
    Everything is allowed in 'exrc' files since they must be explicitly marked
    trusted.
  *'shelltype'*
  'shortmess' flags: *shm-f* *shm-n* *shm-x* *shm-i* (behave like always on)
  *'shortname'* *'sn'* *'noshortname'* *'nosn'*
  *'swapsync'* *'sws'*
  *'termencoding'* *'tenc'* (Vim 7.4.852 also removed this for Windows)
  *'terse'* *'noterse'* (Add "s" to 'shortmess' instead)
  textauto
  textmode
  *'toolbar'* *'tb'*
  *'toolbariconsize'* *'tbis'*
  *'ttybuiltin'* *'tbi'* *'nottybuiltin'* *'notbi'*
  *'ttyfast'* *'tf'* *'nottyfast'* *'notf'*
  *'ttymouse'* *'ttym'*
  *'ttyscroll'* *'tsl'*
  *'ttytype'* *'tty'*
  weirdinvert

Plugins:

- logiPat
- rrhelper
- *vimball*

Providers:

- *if_lua* : Nvim |Lua| API is not compatible with Vim's "if_lua".
- *if_mzscheme*
- |if_pyth|: *python-bindeval* *python-Function* are not supported.
- *if_tcl*

Startup:
  --literal (file args are always literal; to expand wildcards on Windows, use
    |:n| e.g. `nvim +"n *"`)
  Easy mode: eview, evim, nvim -y
  Restricted mode: rview, rvim, nvim -Z
  Vi mode: nvim -v

Test functions:
  test_alloc_fail()
  test_autochdir()
  test_disable_char_avail()
  test_feedinput()
  test_garbagecollect_soon
  test_getvalue()
  test_ignore_error()
  test_null_blob()
  test_null_channel()
  test_null_dict()
  test_null_function()
  test_null_job()
  test_null_list()
  test_null_partial()
  test_null_string()
  test_option_not_set()
  test_override()
  test_refcount()
  test_scrollbar()
  test_setmouse()
  test_settime()
  test_srand_seed()

TUI:
        *t_xx* *termcap-options* *t_AB* *t_Sb* *t_vb* *t_SI*
  Nvim does not have special `t_XX` options nor <t_XX> keycodes to configure
  terminal capabilities. Instead Nvim treats the terminal as any other UI,
  e.g. 'guicursor' sets the terminal cursor style if possible.

        *termcap*
  Nvim never uses the termcap database, only |terminfo| and |builtin-terms|.

        *xterm-8bit* *xterm-8-bit*
  Xterm can be run in a mode where it uses true 8-bit CSI.  Supporting this
  requires autodetection of whether the terminal is in UTF-8 mode or non-UTF-8
  mode, as the 8-bit CSI character has to be written differently in each case.
  Vim issues a "request version" sequence to the terminal at startup and looks
  at how the terminal is sending CSI.  Nvim does not issue such a sequence and
  always uses 7-bit control sequences.

