---
layout: post
title:  "vim-practice"
date:   2018-03-28 08:32:22 +0500
categories: jekyll update
---

Новые заметки по командам vim

На данный момент буду накидывать сюда примеры, которые не видел ранее.
Когда-нибудь отсортирую их либо внесу в основную статью.

# Text object selection ( :help object-select )

Можем действовать на объект в тексте внутри которого сейчас находится курсор.
Начинается в Visual mode или после оператора (d,c ...),

потом `i`(без пробелов вокруг) или `a` (с пробелами), а в конце команда передвижения

`daw` -  удалить слово

`vi"` - выделить то, что в `"`

`vit` - выделить tag block

`w` - word

`s` - sentence(предложение)

`p` - paragraph

Выделить блок текста ограниченный чем-то (i-внутри скобок, a-вместе со скобками)

`{B}(b)[]<>t''...` - разные блоки внутри скобок, кавычек

{123}

(123)

# Operators (:help operator)

|c|		change
|d|		delete
|y|		yank into register (does not change the text)
|~|		swap case (only if 'tildeop' is set)
|g~|	g~	swap case
|gu|	gu	make lowercase
|gU|	gU	make uppercase
|!|	    filter through an external program
|=|		filter through 'equalprg' or C-indenting if empty
|gq|	gq	text formatting
|g?|	g?	ROT13 encoding
|>|		shift right
|<|		shift left
|zf|	zf	define a fold
|g@|	g@	call function set with the 'operatorfunc' option

# Insert Mode

## Вставка

`Esc` or `Ctrl-[` - выйти из Insert mode

`Cntrl-r0` - вставка из 0 регистра (стандартный)

`Cntrl-r=6*20<CR>` - вставка значения выражения 6*20

`Cntrl-v<code>` - вставка unicode символа(065,u00bf)

`Cntrl-k{char1}{char2}` - вставка unicode символа из диграфа (`:help digraph-table`)

В Normal mode `ga` на символе даёт код символа

`ث ` -  <ﺙ> 1579, Hex 062b, Oct 3053, Digr tk

## Особенности

`Cntrl-w` - удалить слово
`Cntrl-u` - удалить до начала строки

# Visual mode gists

`Esc` or `Ctrl-[` - выйти из Visual mode(такой же, как Insert)

`gv` - повторить последнее выделение

Также во время выделения работают операторы.

`Vr-` - заменить все символы в строке на `-`

`o` - переход курсора наверх выделения, либо вниз выделения

`:’<,’>s//bar/g` - заменить все вхождения последнего поиска на `bar` в выделенной области
`Ctrl-r <name register>` - вставить в командную строку текст из регистра
Например:
  `Ctrl-R + \ ` - последнее выделение при поиске
Очерёдность:
* - поиск
cw<text>- заменить
:%s//<C-r><C-w>/g - вставить последнюю замену с последнего поиска

# Полезные регистры
	
1.  The unnamed register "" - все операции копирования, удаления
2.  10 numbered registers "0 to "9 - те же самые операции, но в порядке последнем
3.  The small delete register "- только первая строка копирования
4.  26 named registers "a to "z or "A to "Z - в эти можно без проблем писать, что захочешь
5.  Three read-only registers
	": - last command
	". - last inserted line
	"%  - name of current file
6.  Alternate buffer register "# - регистр для плагинов
7.  The expression register "= - не разобрался
8.  The selection registers "* and "+ - регистры для копирования из системы
9.  The black hole register "_ - в него можно всё удалять и другие регистры не будут задействованы
10. Last search pattern register "/

# Ex-commands

`Cntrl-w Cntrl-w` -  вставить в командную строку слово под курсором

`Cntrl-w Cntrl-a` - вставить в командную строку WORD под курсором

`q:` - история команд, которую можно редактировать и запускать из неё

`%normal A;` - вызвать normal-команду для каждой строки

Для справки:
| Command | Effect
|:[range]delete [x] |Delete specified lines [into register x]
|:[range]yank [x] |Yank specified lines [into register x]
|:[line]put [x] |Put the text from register x after the specified line
|:[range]copy {address}|  Copy the specified lines to below the line specified by {address}
|:[range]move {address}|  Move the specified lines to below the line specified by {address}
|:[range]join | Join the specified lines
|:[range]normal {commands} | Execute Normal mode {commands} on each specified line
|:[range]substitute/{pattern}/{string}/[flags]| Replace occurrences of {pattern} with {string} on each specified line
|:[range]global/{pattern}/[cmd] | Execute the Ex command [cmd] on all specified lines where the {pattern} matches


# Windows

`<C-w>s` or `:sp[lit] {file}` - разделить окно горизонтально
`<C-w>v` or `:vsp[lit] {file}` - разделить окно вертикально
`<C-w>w` - ходить по окнам циклически
`<C-w>[hjkl]` -  перейти на окно в зависимости от положения
`<C-w>с` or `:cl[ose]` - закрыть текущее окно
`<C-w>o` or `:on[ly]`- закрыть все окна кроме текущего
`<C-w>=` - сравнять размеры всех окон
`<C-w>_` - максимизировать текущее окно снизу вверх
`<C-w>|` - максимизировать текущее окно слева направо
`[N]<C-w>_` - сделать текущее окно равным N строк
`[N]<C-w>|` - сделать текущее окно равным N колонок


# Кодировка

`:e ++enc=cp1251` - поменять кодировку

## Tabs(вкладки)
`tabe[edit] {filename}` - открыть файл во вкладке
`<C-w>T` - переместить текущее окно в свою новую вкладку
`tabc[lose]` - закрыть вкладку
`tabo[nlye]` - закрыть вкладки кроме текущей
`[N]gt` - переместится на N владку
`[N]gt` - переместится на следующую владку
`[N]gt` - переместится на предыдущую владку

# Auto-marks


# Vimscript

Пока примеры функций, которые хочется использовать в своих скриптах
```
function! test_write_tab()
    let full_cur_line = getline(line('.'))
    let target_pattern  = '\(.*\)[^(]*\(.*\)'
    let target_line_num = search(target_pattern)
    finded = split(matchstr(getline(line('.')), '(.*)[^(]*(.*)'), '==')
    execute(':tabprevious')
    setbufline(1,line('.'), '123'.'132')
    append(line('.'), ['123:', '\n'.'132'])
    execute(':tabprevious')
```

# Slow Vim

`:syntime on` - включить сбор статистики по скорости работы
`:syntime report` - показать собранную статистику

# Vim session

Create:

`:mks[!] ~/.vim/sessions/<file>`
`:source ~/.vim/sessions/article.vim`

Open:

`:source ~/.vim/sessions/article.vim`


<!-- :public: -->

#vim
