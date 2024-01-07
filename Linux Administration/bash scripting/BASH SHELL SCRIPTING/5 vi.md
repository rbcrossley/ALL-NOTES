`ps -eo pid,args | grep vi`
to kill unresponsive vi.

<kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>O</kbd>

To save a file with different name

`:w! filename`

This means "I don't mind if you overwrite the file".

`a` cursor jumps to the start of line.
`A` cursor jumps to the end of line.

Beginning of line
<kbd>Home</kbd>, `0` and pipe key takes to beginning of line.

<kbd>End</kbd> key takes to end of line.
You can also use `$` key to go to end of line.
`carat ^` to go the first non-blank character in line.


To set line numbers in vi editor.

`:set nu`

To go to certain line number after doing that.

`:145`

`b` - cursor goes back 1 word
`e`- cursor goes forward 1 word
`u` to  undo.
`dd` to delete entire line.
`dw` to delete word.
`8dd` to delete 8 lines.
`dG` to delete all lines until the end of file
`dgg` to delete all the lines till the beginning of file.
`yy` to copy entire line into buffer.
`p` to paste.
`4yy` to copy 4 lines.
<kbd>Ctrl</kbd>+<kbd>r</kbd> to redo last undo.
`o` to create a line below the cursor.
`O` to create a line above the cursor.

:`set nonu` for no line numbering.