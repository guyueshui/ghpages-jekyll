# Welcome to my blog

I'm glad you are here. I plan to talk about ...

yychi love yukynn!

[My new page!](https://guyueshui.github.io)

---------------

Here comes a smaple article.

<font color="red">This article needs polish, do not truely trust it!</font>

Vim is so-called god's editor, but not so friendly to new users. Today we will cover some techniques and trick of vim, for further reference.

A vim operation consists of three parts, namely
```
[OPERATOR][NUMBER][MOTION]
```

where

- `OPERATOR` - what you want to do? This mainly covers copy, cut, paste, etc.
- `NUMBER` - how many times do you want? It's nothing but repeating the operation `NUMBER` times, and it's **optional**.
-	`MOTION` - where do you want to go? This point out the scope where the `OPERATOR` applies.

## Operators

### Copy, Cut, Paste

- v - visual mode, now you can select what you want
- V - visual in line, this is extremely useful when you want to copy a line, just `V`+`y`!
- y - yank, like <kbd>Ctrl</kbd>+<kbd>C</kbd>, copy the selected text to clipboard (`"`)
	> - `yy`: copy current line
	> - `5yy`: copy 5 lines below
	> - `y`+`MOTION`: copy the motion scope
	>	 + `y0`: copy from here to BOL (begining of line)
	>	 + `y$`: copy from here to EOL
	>	 + `y4G`: copy from here to line 4
	>	 + `y?bar`: copy from here to previous ocurrence of `bar`
- d - delete & yank, not only delete, but also yank
- p - paste after the cursor
- P - paste before the cursor

### Edit

- i - insert
- a - insert after the cursor
- o - insert a line below and insert
- O - insert a line above and insert
- r - replace, replace the character inplace
- x - delete current character
- s - delete the character and insert
- u - recall the last command
- `^r` - recall the last recall, namely redo
- `.` - repeat the last command

> Note: all deleted things were automatically yanked in buffers, i.e., register `"`

## Motions

### Basic

- h - move left
- l - move right
- j - move down
- k - move up
<br />
- G - jump to EOF (end of file)
- gg - jump to BOF
- `x` + (gg | G) - jump to line `x` (must be a valid line number)

> Note: view `(h, l)` and `(j, k)` as pairs

Some addtional movements:

- w - next word, points to the first letter
- b - back, previous word, points to the first letter
- e - end, jump to the end of the word
<br />
- `%` - find the bracket matches (`( )`,`{ }`, `[ ]`...)
<br />
- `^e` - scroll down
- `^y` - scroll up
- `^d` - half-screen down
- `^u` - half-screen up
<br />
- `*` - jump to next occurence of current word
- `#` - jump to previous occurence of current word

> Note: view `(w, b)` as a pair

### In Line

- `^`: jump to the first character which is not a blank (space, tab, `\n`, `\r`)
- `g_`: jump to the last character which isn't a blank
<br />
- `0`: jump to the begining
- `$`: jump to the end
<br />
- f`x`: find next `x` in current line
	> Note: you can use `;`(alongside) and `,`(reverse side) to repeat this in two directions
- F`x`: find previous `x` in current line
	> Note: same rules can be applied
<br />
- t`x`: find next `x` and move 1 backward
- T`x`: find previous `x` and move 1 backward

![Picture adapted from REF3](https://i0.wp.com/yannesposito.com/Scratch/img/blog/Learn-Vim-Progressively/line_moves.jpg?zoom=2)

-------------------

## Commands

### Search & Replace

- `/keyword` - search `keyword` after the cursor
- `?keyword` - search	`keyword` before the cursor
	> Note: use `n`(search next) or `N`(search previous) for quick search
- `:{search_scope}s/{target}/{replace}/{replace_flag}` - replace `{target}` to `{replace}`
	> - `s` stands for substitute.
	> - `:%s/a/b/g`: global (`%`) search `a`, replace it to `b` at every (`g`) occurence
	> - `:%s/a/b/gc`: interact with every replace

-------------------

## Advanced Tricks

In insert mode, press `^p`, vim will give you a list of all words you have typed, kind of auto complete.

![Image adapted from REF3](https://i2.wp.com/yannesposito.com/Scratch/img/blog/Learn-Vim-Progressively/completion.gif?zoom=2)

### Block Editing

One of the magic of vim is block editing. Just press `^V` to enter block mode. Then select some block you are interested, then make some modifications. Finally press `Esc`, then those modifications you have just made will be applied onto every line of the block.

See this magic:

1. `^` jump to BOL
2. `^V` enter block mode
3. `4j` move 4 lines down
4. `I` enter insert mode and add something
5. `Esc` to see the effect

![Picture adpated from REF3](https://i1.wp.com/yannesposito.com/Scratch/img/blog/Learn-Vim-Progressively/rectangular-blocks.gif?zoom=2)

Additionally, you can

1. `^v/v/V` enter visual mode
2. `J` joint them into one line
2. `<` or `>` modify indents
2. `=` auto indent (extremely powerful?)

![Image adapted from REF3](https://i1.wp.com/yannesposito.com/Scratch/img/blog/Learn-Vim-Progressively/autoindent.gif?zoom=2)

or

1. `^v` enter block mode
2. select some line
3. `$` jump to EOL
4. `A` append something
5. `Esc` see the effect

![Image adapted from REF3](https://i2.wp.com/yannesposito.com/Scratch/img/blog/Learn-Vim-Progressively/append-to-many-lines.gif?zoom=2)

### Macro Recording

Press `qx`, where `x` is the macro name, will enter macro recording mode, all actions will be recorded, just like a tape recorder. If you don't want to record anymore, press `q` to stop recording.

To replay the record, press `@x`. Moreover, `@@` will replay the last recored macro.

### Register Invocation

### Clipboard

Vim provides 12 clipborads (registers): `0`, `1`, `2` .. `9`, `a`, `"`. If your vim support system clipboard, there will be two additional register: `+` and `*`. Use `:reg` to see what are in your registers.

For X11 systems, things seleted or highlighted will be saved in register `*`, while things yanked or cutted will be saved in register `+`.

> To see whether your vim support system clipboard, type `$ vim --version`

In general, all your copy and paste operations are performed at register `"` by default. To use other register, add a prefix `"6` to your yank or paste commands. For example:
```
"6p		// put the buffer in register 6 to file
"8yy		// yank current line to register 8
:put {reg}	// put things in {reg} to file, <=> "{reg}p
```

### Multi-window

- `:sp <filename>` - split horizontally and open `<filename>`
- `:vsp` - split vertically <s>and open filename optionally, same file by default</s>
- `^w` + `h/j/k/l` - focus left/down/up/right window



## Reference

1. [Vim操作](https://github.com/ruanyf/articles/blob/master/dev/vim/operation.md)
2. [在 Vim 中优雅地查找和替换](https://harttle.land/2016/08/08/vim-search-in-file.html)
3. [简明 Vim 练级攻略](https://globalinheart.wordpress.com/2011/09/07/%E7%AE%80%E6%98%8E-vim-%E7%BB%83%E7%BA%A7%E6%94%BB%E7%95%A5/)
