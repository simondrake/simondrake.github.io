---
layout: page
permalink: /cheatsheets/vim/
---

<h1>Vim </h1>
<h3><small class="text-muted">Vim is a highly configurable text editor</small></h3>

#### Verbs

|Verb|Description|
|:---|:----------|
|`v`|Visual|
|`c`|Change|
|`d`|Delete|
|`y`|Yank|
|`m`|Mark|

#### Modifiers

|Modifier|Description|
|:-------|:----------|
|`i`|Inside|
|`a`|Around|
|`t`|Till (stop before)|
|`f`|Find (put cursor on)|

#### Nouns

|Noun|Description|
|:---|:----------|
|`w`|Word|
|`s`|Sentence|
|`p`|Paragraph|
|`b`|Block/Parentheses|
|`t`|Tag|

#### Searching/Navigating

|Command|Description|
|:------|:----------|
|`f{c}`|Jump forward and put cursor on `{c}`|
|`F{c}`|Jump back and put cursor on `{c}`|
|`t{c}`|Jump forward and put cursor before `{c}`|
|`T{c}`|Jump back and put cursor after `{c}`|
|`/{s}`|Search for `{s}`|
|`/{s}/e`|(In visual mode) Select text up to and including `{s}`|
|`*`|Search for other instances of the word under cursor|
|`n`|Next occurence|
|`N`|Previous occurence|
|`0`|Go to beginning of line|
|`$`|Go to end of line|
|`^`|Go to first non-blank character in line|
|`_`|Go to first non-blank character in line|
|`w`|Move forward one word|
|`b`|Move back one word|
|`e`|Move to the end of your word|
|`gg`|Go to top of file|
|`G`|Go to bottom of file|
|`&ltC-i>`|Previous location|
|`&ltC-o>`|Forward location|
|`:{l}`|Move to line number `{l}`|
|`%`|Go to block close|

#### Editing

|Command|Description|
|:------|:----------|
|`i`|Insert before cursor|
|`a`|Append after cursor|
|`I`|Insert at the beginning of line|
|`A`|Insert at the end of line|
|`o`|New line below|
|`O`|New line above|
|`r`|Replace character|
|`R`|Replace but keep typing|
|`C`|Change rest of line|
|`~`|Change case|
|`x`|Delete under cursor|
|`X`|Delete before cursor|
|`dd`|Delete line|
|`D`|Delete to end of line|
|`J`|Join the current line with the next|
|`u`|Undo|
|`&ltC-r>`|Redo|
|`.`|Repeat command|
|`yy`|Yank line|
|`{d}yy`|yank `{d}` lines|
|`p`|Paste after cursor|
|`P`|Paste before cursor|
|`edit!`|Reload buffer|

#### Macros

|Command|Description|
|:------|:----------|
|`q{c}`|Record macro and store in `{c}`|
|`q`|Stop recording|
|`@{c}`|Replay macro stored in `{c}`|
|`:5,10norm! @{c}`|Execute the macro stored in `{c}` on lines 5 through 10|
|`:5,$norm! @{c}`|Execute the macro stored in `{c}` on lines 5 through the end of the file|
|`:%norm! @{c}`|Execute the macro stored in `{c}` on all lines|
|`:g/pattern/norm! @{c}`|Execute the macro stored in `{c}` on all lines matching pattern|
|`:'<,'>norm! @{c}`|Execute the macro stored in `{c}` on visually selected lines|

#### Search and Replace

|Command|Description|
|:------|:----------|
|`:%s/foo/bar/g`|Find each occurrence of `foo` (all lines), and replace it with `bar`|
|`:s/foo/bar/g`|Find each occurrence of `foo` (current line), and replace it with `bar`|
|`:%s/foo/bar/gc`|Change each `foo` to `bar`, but ask for confirmation first|
|`:%s/\/bar/gc`|Change only whole words exactly matching `foo` to `bar`; ask for confirmation|
|`:%s/foo/bar/gci`|Change each `foo` (case insensitive) to `bar`; ask for confirmation|
|`:%s/foo/bar/gcI`|Change each `foo` (case sensitive) to `bar`; ask for confirmation.|

#### Splits

|Command|Description|
|:------|:----------|
|`:vsp`|Vertical split|
|`:sp`|Horizontal split|
|`:resize 60`|Change height of window|
|`:res +5`|Change height of window|
|`:res -5`|Change height of window|
|`:vertical resize 80`|Change width of window|
|`:vertical resize +5`|Change width of window|
|`:vertical resize -5`|Change width of window|

#### Terminal

|Command|Description|
|:------|:----------|
|`:term`|Open terminal|
|`:vsp | term`|Open terminal in a vertical split|
|`:sp | term`|Open terminal in a horizontal split|

#### Tips and Tricks

|Command|Description|
|:------|:----------|
|`:noautocmd w`|Save file without executing Autocommand events|
|`"_d`|Use "black hole register" and delete|
|`:map`|Show mappings|
|`:map <leader>rn`|Show mapping for `<leader>rn`|
|`:map j`|Show mapping for `j`|
|`:map <F5>`|Show mapping for `<F5>`|

#### Plugins

|Plugin|Command|Description|
|:-----|:------|:----------|
|[multiple-cursors][multiple-cursors]|`Shit + i`|Use when vim freezes|
|[multiple-cursors][multiple-cursors]|`&ltC-n>`|Start multicursor|
|[multiple-cursors][multiple-cursors]|`&ltC-n>`|Select next occurence|
|[multiple-cursors][multiple-cursors]|`&ltC-x>`|Skip next occurence|
|[multiple-cursors][multiple-cursors]|`&ltC-p>`|Previous occurence (go back)|
|[multiple-cursors][multiple-cursors]|`&ltA-n>`|Select all occurences|
|[vim-gitgutter][vim-gitgutter]|`]c`|Next hunk|
|[vim-gitgutter][vim-gitgutter]|`[c`|Previous hunk|
|[coc][coc]|`K`|Show documentation|
|[coc][coc]|`&ltC-w-w>`|Move to floating window (e.g. documentation)|
|[coc][coc]|`:CocUpdate`|Update plugins|
|[NerdTree][NerdTree]|`t`|Open file in new tab|
|[NerdTree][NerdTree]|`i`|Open file in horizontal split window|
|[NerdTree][NerdTree]|`s`|Open file in vertical split window|
|[NerdTree][NerdTree]|`I`|Toggle hidden files|
|[NerdTree][NerdTree]|`m`|Show menu|
|[NerdTree][NerdTree]|`R`|Refresh the tree|
|[NerdTree][NerdTree]|`?`|Toggle quick help|
|[NerdTree][NerdTree]|`m-a`|Create file|
|[NerdTree][NerdTree]|`m-a{name}/`|Create directory (append `/` to name when creating)|
|[vim-surround][vim-surround]|`ysiw"`|Surround word with quotes (`"`)|
|[vim-surround][vim-surround]|`cs"'`|Change surrounded `"` to `'`|
|[vim-surround][vim-surround]|`cs<{`|Change surrounded `<>` to `{}`|
|[vim-surround][vim-surround]|`cs<}`|Change surrounded `<>` to `{}` (without spaces)|
|[vim-plug][vim-plug]|`:PlugUpdate`|Update plugins|
|[vim-test][vim-test]|`:TestNearest`|Runs the test nearest to the cursor|
|[vim-test][vim-test]|`:TestFile`|Runs all tests in the current file|
|[vim-test][vim-test]|`:TestSuite`|Runs all tests in a test suite|
|[vim-test][vim-test]|`:TestLast`|Runs the last test|
|[vim-test][vim-test]|`:TestVisit`|Visits the test file from which you last run your tests|
|[vim-rhubarb][vim-rhubarb]|`:GBrowse`|Open Git URL|
|[vim-rhubarb][vim-rhubarb]|`:GBrowse!`|Copy Git URL to clipboard|
|[vim-fugitive][vim-fugitive]|`:Git [command]`|Run git command|
|[vim-fugitive][vim-fugitive]|`:Gdiffsplit (or :Gvdiffsplit)`|Open a diff split|

#### Examples

|Command|Description|
|:------|:----------|
|`d2w`|Delete two words|
|`cis`|Change inside sentence|
|`100i{s}`|Writes `{s}` one hundred times|
|`3.`|Repeat command three times|
|`:%!uniq`|Remove duplicate lines|
|`:%!jq .`|Prettify a JSON file|
|`{d}df{x}`|Deletes up to the `{d}`th character of `{x}`|
|`$A,`|Add a comma to the end of the line|
|`:g/^$/d`|Delete empty lines|


[multiple-cursors]: https://github.com/terryma/vim-multiple-cursors
[vim-gitgutter]: https://github.com/airblade/vim-gitgutter
[coc]: https://github.com/neoclide/coc.nvim
[NerdTree]: https://github.com/preservim/nerdtree
[vim-surround]: https://github.com/tpope/vim-surround
[vim-plug]: https://github.com/junegunn/vim-plug
[vim-test]: https://github.com/vim-test/vim-test
[vim-rhubard]: https://github.com/tpope/vim-rhubarb
[vim-fugitive]: https://github.com/tpope/vim-fugitive
