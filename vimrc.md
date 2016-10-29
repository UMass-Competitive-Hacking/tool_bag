# .vimrc file

[Vim command cheat sheet](http://vim.rtorr.com/)

Commands are used via a color (:) while not in insert mode (press *esc* to *escape* insert mode [clever vim devs are clever])

vimrc is a config file for vim.  In this file, you can do everything from set line numbers, color schemes, mappings, and most important, **expand tabs into spaces**.

Configuring is simple:

1. Open your .vimrc from the home/[user] directory

```
$ cd ~/
$ vim ./.vimrc
```

2. Edit
  1. For line numbers:
  ```vim
  set number
  ```

  2. Tabs (4 spaces)
  ```
  set tabstop=4
  set expandtab
  ```

  3. Mapping (convenience mappings)
  	1. Map 'jk' to the esc key
  	```
  	imap jk <Esc>
  	```
  	2. Map 'ww' to Escape insert-mode and write
  	```
  	imap ww <Esc>:w<Cr>
  	```
  	3. Map 'xx' to Escape insert-mode and write/quit
  	```
  	imap xx <Esc>:wq<Cr>
  	```
  	4. Color scheme example (Changes comments to cyan)
  	```
  	hi Comment ctermfg=cyan
  	```

3. Save with :wq and reopen the file to make sure things worked

