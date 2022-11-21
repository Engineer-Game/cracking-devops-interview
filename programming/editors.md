# Editors

## To comment out blocks in vim:

press Esc (to leave editing or other mode)

hit ctrl+v (visual block mode)

use the up/down arrow keys to select lines you want (it won't highlight everything - it's OK!)

Shift+i (capital I)

insert the text you want, i.e. '% '

press Esc

## To uncomment out blocks in vim:

press Esc (to leave editing or other mode)

hit ctrl+v (visual block mode)

use the up/down arrow keys to select the lines to uncomment.

If you want to select multiple characters, use one or combine these methods:

use the left/right arrow keys to select more text

to select chunks of text use shift + left/right arrow key

you can repeatedly push the delete keys below, like a regular delete button

press d or x to delete characters, repeatedly if necessary

press Esc

## [How to partially replace text in a selected text-block?](http://vi.stackexchange.com/questions/2758/how-to-partially-replace-text-in-a-selected-text-block)

Ctrl-v-- select block --c-- insert whatever --Esc

'm aware of inserting in front of a text-block:

Ctrl-v\_select lines\_I\_type text\_ESC

Now I would like to do this but also with replacing a part in my block selection. Currently I'm doing two operations

Ctrl-v_select block\_x\_go back to start_
