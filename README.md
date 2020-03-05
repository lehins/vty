[![Build Status](https://travis-ci.org/jtdaugherty/vty.png)](https://travis-ci.org/jtdaugherty/vty)

`vty` is a terminal interface library. It provides a high-level
interface for doing terminal I/O. Vty is supported on GHC versions
7.10.1 and up.

Install via `git` with:

```
git clone git://github.com/jtdaugherty/vty.git
```

Install via `cabal` with:

```
cabal install vty
```

# Features

* Supports a large number of terminals, i.e., vt100, ansi, hurd, linux,
  `screen`, etc., or anything with a sufficient terminfo entry.

* Automatically handles window resizes.

* Supports Unicode output on terminals with UTF-8 support.

* Provides an efficient output algorithm. Output buffering and terminal
  state changes are minimized.

* Minimizes repaint area, which virtually eliminates the flicker
  problems that plague ncurses programs.

* Provides a pure, compositional interface for efficiently constructing
  display images.

* Automatically decodes keyboard keys into (key,[modifier]) tuples.

* Automatically supports refresh on Ctrl-L.

* Supports a keypress timeout after for lone ESC. The timeout is
  customizable.

* Provides extensible input and output interfaces.

* Supports ANSI graphics modes (SGR as defined in `console_codes(4)`)
  with a type-safe interface and graceful fallback for terminals
  with limited or nonexistent support for such modes.

* Properly handles cleanup (but not due to signals).

* Provides a comprehensive test suite.

* Supports "normal" and "extended" (SGR) mouse modes as described at
  http://invisible-island.net/xterm/ctlseqs/ctlseqs.html#h2-Mouse-Tracking

* Supports bracketed paste mode as described at
  http://cirw.in/blog/bracketed-paste

* Supports multi-column Unicode characters such as emoji characters. In
  cases where Vty and your terminal emulator disagree on character
  widths, Vty provides a tool `vty-build-width-table` and library
  functionality to build a width table that will work for your terminal
  and load it on application startup.

# Development Notes

Vty uses threads internally, so programs made with Vty need to be
compiled with the threaded runtime using the GHC `-threaded` option.

# Platform Support

## Posix Terminals

For the most part, Vty uses `terminfo` to determine terminal protocol
with some special rules to handle some omissions from `terminfo`.

## Windows

Windows is not supported.

# Contributing

If you decide to contribute, that's great! Here are some guidelines you
should consider to make submitting patches easier for all concerned:

 - If you want to take on big things, talk to me first; let's have a
   design/vision discussion before you start coding. Create a GitHub
   issue and we can use that as the place to hash things out.
 - If you make changes, make them consistent with the syntactic
   conventions already used in the codebase.
 - Please provide Haddock documentation for any changes you make.

# Development Notes

## Code Coverage

To evaluate coverage, configure as follows:

~~~
rm -rf dist ; cabal configure --enable-tests --enable-library-coverage \
  --disable-library-profiling \
  --disable-executable-profiling
~~~

## Profiling

~~~
rm -rf dist ; cabal configure --enable-tests --disable-library-coverage \
  --enable-library-profiling \
  --enable-executable-profiling
~~~

# Known Issues

* Terminals have numerous quirks and bugs, so mileage may vary. Please
  report issues as you encounter them and provide details on your
  terminal emulator, operating system, etc.

* STOP, TERM and INT signals are not handled.

* The character encoding of the terminal is assumed to be UTF-8 if
  unicode is used.

* Terminfo is assumed to be correct unless there is an override
  configured. Some terminals will not have correct special key support
  (shifted F10 etc). See `Config` for customizing vty's behavior for a
  particular terminal.

* Vty uses the `TIOCGWINSZ` ioctl to find the current window size, which
  appears to be limited to Linux and BSD.

# Further Reading

Good sources of documentation for terminal programming are:

* https://github.com/b4winckler/vim/blob/master/src/term.c
* http://invisible-island.net/xterm/ctlseqs/ctlseqs.html
* http://ulisse.elettra.trieste.it/services/doc/serial/config.html
* http://www.leonerd.org.uk/hacks/hints/xterm-8bit.html
* http://www.unixwiz.net/techtips/termios-vmin-vtime.html
* http://vt100.net/docs/vt100-ug/chapter3.html
