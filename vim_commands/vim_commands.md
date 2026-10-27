## Vim Commands

### Navigation

- `h` `j` `k` `l`: Left, Down, Up, Right
- `w`: Jump to the start of next word
- `b`: Jump to the start of previous word
- `0`: Jump to the start of the line
- `$`: Jump to the end of the line
- `gg`: Jump to the start of the file
- `G`: Jump to the end of the file
- `:<number>`: Jump to the specific line
- `C-u`: page up
- `C-d`: Page down

### Edit

- `i`: Insert before the cursor
- `a`: Insert after the cursor
- `o`: Open a new line
- `x`: Delete a single char
- `dd`: Delete the line (cut)
- `yy`: Yank the line (copy)
- `p`: Paste
- `u`: Undo
- `C+r`: Redo
- `ciw`: Change inner word (deletes the word you're on and puts you in Insert mode)
- `diw`: Delete inner word
- `dw`: Delete from cursor to the start of next word
- `ci"`: Change inside double quotes (keep the quotes, enter Insert mode)
- `ca"`: Change around double quotes (including the quotes)
- `di"`: Delete inside double quotes
- `da"`: Delete around double quotes (including the quotes)
- `yi"`: Yank (copy) inside double quotes
- `vi"`: Visually select inside double quotes

### Search

- `\<pattern>`: Search in file
    - `n`: Go next
    - `N`: Go previous
- `:%s/old/new/g`: Replace old to new globally
- `:%s/old/new/gc`: Same as above but need confirmation

### Others

- `.`: Repeat the last command
- `sp`: Split window horizontally
- `vsp`: Split window vertically
