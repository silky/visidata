==========
User Guide
==========

Basic Movement and Essential Commands
=====================================

-  ``hjkl`` or arrow keys move the cursor around (left/down/up/right)
-  ``F1`` or ``z?`` opens command help for the current sheet
-  ``q`` quits the current sheet (backs out one level)
-  ``Ctrl-q`` aborts the program immediately

Advanced movement
=================

Scrolling
---------

"Scrolling" adjusts the onscreen visible slice, but will not change the
current cursor position.

-  ``Home`` and ``End`` move the cursor to the first and last row of the
   entire sheet, respectively. The column cursor position is maintained.
-  ``PageUp`` and ``PageDown`` move the cursor exactly one onscreen page
   up or down. ``Ctrl-b`` and ``Ctrl-f`` do the same

-  The ``g`` prefix modifies movement commands by making them go "all
   the way".

   -  ``gk`` and ``gj`` are the same as ``Home`` and ``End``,
      respectively.
   -  ``gh`` and ``gl`` move the cursor to the first and last column.
-  ``z`` prefix scrolling (most behave exactly like vim):

   -  ``zt`` scrolls current row to top of screen
   -  ``zz`` scrolls current row to middle of screen
   -  ``zb`` scrolls current row to bottom of screen
   -  ``zk`` scrolls one row up
   -  ``zj`` scrolls one row down
   -  ``zH`` moves cursor one page to left
   -  ``zL`` moves cursor one page to right
   -  ``zh`` moves cursor one column to left
   -  ``zl`` scrolls sheet one column to right
   -  ``zs`` scrolls sheet to leftmost column
   -  ``ze`` scrolls sheet to rightmost column
- ``c`` move to a column whose name matches a regex search
- ``r`` move to a specified row number
- ``<``/``>`` moves up/down this column to the next cell which has a different value from the current cell
- ``{``/``}`` moves up/down to the next selected row


Arranging Columns
=================

-  ``-`` (minus) hides a column
    - To unhide a column, go to the ``C``\olumns sheet and ``e``\dit the width.
-  ``_`` (underscore) adjusts a column's width so that the entire contents of its cells are visible.
-  ``!`` makes the current column a *key column*.

Column Types
------------

-  Columns start out untyped (unless the source data is typed).

-  You can set a column manually to a specific type:

   -  ``~`` : ``str`` (the default)
   -  ``#`` : ``int``
   -  ``%`` : ``float``
   -  ``$`` : ``currency`` (a float that strips leading and/or trailing non-numeric characters)
   -  ``@`` : ``date`` (datetime wrapper)

   The column type is shown at the right edge of the column header.

-  All values are stored in their original format, and only converted on
   demand and as needed.
-  Values that can't be properly converted are flagged with ``?`` at the right
   edge of the cell.
-  For commands like sort which require a correctly typed value, the
   default (0) value for that type is used.
-  Cell edits are rejected if they don't convert to the column type.

Creating new columns
--------------------

-  ``:`` split column (on any sheet)
-  ``=`` "add column expression" takes a Python expression as input,
   evaluates the expression, and appends the results into a new column.
   Column names can be supplied as variables, in order to have the
   expression performed on the column cell-by-cell.

   -  Example: Suppose there is a column named ``Foo`` and a column
      named ``Bar``. ``=Foo+Bar`` would result in a column named
      ``Foo+Bar`` whose first row is the sum of the first row of ``Foo``
      and the first row of ``Bar``. Its second row would be the sum of
      the second row of ``Foo`` and the second row of ``Bar``, and so
      on.

-  ``+`` join selected columns on Columns sheet
-  ``+`` on the source sheet allows you to select a statistical aggregator for this column
    - you can see and edit all existing aggregators in the ``C``\olumns sheet
    - see `tour 01 <http://github.com/saulpw/visidata/tree/stable/docs/tours.rst>`_ for an example usage
- ``;`` creates new columns from the capture groups of the given regex

Searching/Selecting/Modifying rows
==================================

- ``/`` search this column forward with regex search
- ``?`` search this column backwards with regex search
- ``d``\elete a row
- ``s``\elect a row
- ``u``\nselect a row
- ``|`` select rows whose contents within this column match a regex search
- ``,`` select rows whose contents within this column match the current cell
-  ``[``/``]`` sort asc/desc by one column
- ``"`` push a duplicate sheet which contains only the selected rows
- ``a``\ppend a blank row

Modifying data
==============

-  ``e``\ dit cell contents
   -  Edits made to a joined sheet are by design automatically reflected back to the source sheets.
- ``ge`` edits the cell contents of all of the selected rows
-  ``^`` allows you to edit a column's name
- ``HL`` allow you to move the current column (left/right).
- ``JK`` allow you to move the current row (up/down).
-  ``Ctrl-s``\ ave to ``.csv`` or ``.tsv`` (by extension)


Special Sheets
==============

-  ``Shift-F``\ requency table for current column with histogram
    - ``w`` toggles between bins with an even interval width and bins with an even frequency height
-  ``Ctrl-o`` to eval an expression and browse the result as a python
   object

Metasheets
----------

-  ``Shift-S``\ heets metasheet to manage/navigate multiple sheets
-  ``Shift-C``\ olumns metasheet

   -  On the ``Shift-C``\ olumns sheet, these commands apply to rows (the
      columns of the source sheet), instead of the columns on the
      Columns sheet

      -  ``-`` hides column (sets width to 0)
      -  ``_`` maximizes column width to fit longest value
      -  ``!`` marks column as a key column (pins to the left and
         matches on sheet joins)

-  ``Shift-O``\ ptions sheet to change the style or behavior
    - Note: currently the only way to set an option to ``False`` is to pass an empty string.
-  ``Ctrl-e``\ rror metasheet
-  ``Ctrl-t``\ hreads metasheet

.visidatarc
-----------
Allows you to customize VisiData settings across every session.

A sample .visidatarc is

::

    options.color_key_col = "cyan"

This configures the  key column to be colored 'cyan'.


Glossary
========

Definitions of terms used in the help and documentation:

-  'abort': exit program immediately
-  'drop': drop top (current) sheet
-  'go': move cursor
-  'jump': change to existing sheet
-  'load': reload an existing sheet from in-memory contents
-  'move': change layout of visible data
-  'open': create a new sheet from a file or url
-  'push': move a sheet to the top of the sheets list (thus making it
   immediately visible)
-  'scroll': change set of visible rows
-  'show': put on status line
-  'this': current [row/column/cell] ('current' is also used)

Here are slightly better descriptions of some non-obvious commands:

-  the "``g``\ lobal prefix": always applies to the next command only,
   but could mean "apply to all columns" (as with the regex search
   commands) or "apply to selected rows" (as with ``d``\ elete) or
   "apply to all sheets" (as with ``q``). The global\_action column on
   the Help Sheet shows the specific way the global prefix changes each
   command.

-  When sheets are joined, the rows are matched by the display values in
   the key columns. Different numbers of key columns cannot match (no
   partial keys and rollup yet). The join types are:

   -  ``&``: Join all selected sheets, keeping only rows which match
      keys on all sheets (inner join)
   -  ``+``: Join all selected sheets, keeping all rows from first sheet
      (outer join, with the first selected sheet being the "left")
   -  ``*``: Join all selected sheets, keeping all rows from all sheets
      (full join)
   -  ``~``: Join all selected sheets, keeping only rows NOT in all
      sheets (diff join)
