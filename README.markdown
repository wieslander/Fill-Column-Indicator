Many modern editors and IDEs can graphically indicate the location of the
fill column by drawing a thin line (in design parlance, a "rule") down the
length of the editing window.  Fill-column-indicator implements this
facility in Emacs:

![screenshot](https://github.com/alpaker/Fill-Column-Indicator/raw/master/FciScreenshot.png)

Installation and Usage
======================

Put the package file in your load path and put

`(require 'fill-column-indicator)`

in your .emacs.

To toggle graphical indication of the fill column in a buffer, use the
command `fci-mode`.

Configuration
=============

* On graphical displays the fill-column rule is drawn using a bitmap
  image.  Its color is controlled by the variable `fci-rule-color`, whose
  value can be any valid color name.  The rule's width in pixels is
  determined by the variable `fci-rule-width`; the default value is 2.

* The image formats fci-mode can use are XPM, PBM, and XBM.  If Emacs has
  been compiled with the appropriate library it uses XPM images by default;
  if not it uses PBM images, which are natively supported.  You can specify a
  particular format by setting `fci-rule-image-format` to either xpm, xpm, or
  xbm.

* On character terminals the rule is drawn using the character specified by
  `fci-rule-character`; the default is \`|' (ascii 124).  If
  `fci-rule-character-color` is nil, then it is drawn using fci-rule-color
  (or the closest approximation thereto that the terminal is capable of); if
  it is a color name, then that color is used instead.

* If you'd like the rule to be drawn using fci-rule-character even on
  graphical displays, set `fci-always-use-textual-rule` to a non-nil value.

* If you'd like the rule to be drawn at a column other than the fill-column, set
  `fci-fill-column` to a non-nil value.  This is useful if you want a
  fill-column of, say, 70 or 72 characters for wrapping flowing blocks of text
  such as code comments, but want a vertical rule at, say, 80 or 100 characters
  to indicate the maximum line length for lines of code.

These variables (as well as those in the next section) can be given
buffer-local bindings.


Other Options
=============

When `truncate-lines` is nil, the effect of drawing a fill-column rule is
very odd looking. Indeed, it makes little sense to use a rule to indicate
the position of the fill column in that case (the positions at which the
fill column falls in the visual display space won't in general be
collinear).  For this reason, fci-mode sets truncate-lines to t in buffers
in which it is enabled and restores it to its previous value when
disabled.  You can turn this feature off by setting
`fci-handle-truncate-lines` to nil.

If `line-move-visual` is t, then vertical navigation can behave oddly in
several edge cases while fci-mode is enabled (this is due to a bug in C
code).  Accordingly, fci-mode sets line-move-visual to nil in buffers in
which it is enabled and restores it to its previous value when
disabled.  This can be suppressed by setting `fci-handle-line-move-visual`
to nil.  (But you shouldn't want to do this.  There's no reason to use
line-move-visual if truncate-lines is t, and it doesn't make sense to use
something like fci-mode when truncate-lines is nil.)

Fci-mode needs free use of two characters (specifically, it needs the use
of two characters whose display table entries it can change
arbitrarily).  By default, it uses the first two characters of the Private
Use Area of the Unicode BMP, viz. U+E000 and U+E001.  If you need to use
those characters for some other purpose, set `fci-eol-char` and
`fci-blank-char` to different values.

Troubleshooting
===============

* Fci-mode is intended to be used with monospaced fonts.  If you're using
  a monospaced font and the fill-column rule is missing or misaligned on a
  few lines but otherwise appears normal, then most likely (a) there are
  non-ascii characters on those lines that are being displayed using a
  non-monospaced font, or (b) your font-lock settings use bold or italics
  and those font variants aren't monospaced.

* Although the XBM and PBM formats are natively supported by Emacs, the
  implementations are different in different ports and sometimes
  incomplete; for example, on some ports XBM images are always drawn in
  black.  Explicitly setting `fci-rule-image-format` to a different value
  will usually resolve such issues.

Known Issues
============

* The indicator extends only to end of the buffer contents (as opposed to
  running the full length of the editing window).

* When portions of a buffer are invisible, such as when outline mode is
  used to hide certain lines, the fill-column rule is hidden as
  well. 

* Fci-mode should work smoothly when simultaneously displaying the same
  buffer on both a graphical display and on a character terminal.  It does
  not currently support simultaneous display of the same buffer on window
  frames with different default font sizes. (It would be feasible to
  support this use case, but thus far there seems to be no demand for
  it.)

* An issue specific to the Mac OS X (NextStep) port, versions 23.0-23.2:
  Emacs won't, in these particular versions, draw a cursor on top of an
  image.  Thus on graphical displays the cursor will disappear when
  positioned directly on top of the fill-column rule.  The best way to deal
  with this is to upgrade to v23.3 or v24 (or downgrade to v22).  If that
  isn't practical, a fix is available via the mini-package `fci-osx-23-fix.el`,
  which can be downloaded from this page.  Directions for its use are given
  in the file header.

Todo
====

* Accommodate non-nil values of `hl-line-sticky-flag` and similar cases.

* Accommodate linum-mode more robustly.

* Compatibility with non-nil `show-trailing-whitespace`.