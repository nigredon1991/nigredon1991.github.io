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

|c|	c	change

|d|	d	delete

|y|	y	yank into register (does not change the text)

|~|	~	swap case (only if 'tildeop' is set)

|g~|	g~	swap case

|gu|	gu	make lowercase

|gU|	gU	make uppercase

|!|	!	filter through an external program

|=|	=	filter through 'equalprg' or C-indenting if empty


|gq|	gq	text formatting

|g?|	g?	ROT13 encoding

|>|	>	shift right

|<|	<	shift left

|zf|	zf	define a fold

|g@|	g@	call function set with the 'operatorfunc' option

# Вставка в Insert Mode

`Esc` or `Ctrl-[` - выйти из Insert mode

`Cntrl-r0` - вставка из 0 регистра (стандартный)

`Cntrl-r=6*20<CR>` - вставка значения выражения 6*20

`Cntrl-v<code>` - вставка unicode символа(065,u00bf)

`Cntrl-k{char1}{char2}` - вставка unicode символа из диграфа (`:help digraph-table`)

В Normal mode `ga` на символе даёт код символа

`ث ` -  <ﺙ> 1579, Hex 062b, Oct 3053, Digr tk

# Visual mode gists

`Esc` or `Ctrl-[` - выйти из Visual mode(такой же, как Insert)

`gv` - повторить последнее выделение

Также во время выделения работают операторы.

`Vr-` - заменить все символы в строке на `-`

`o` - переход курсора наверх выделения, либо вниз выделения

`:’<,’>s//bar/g` - заменить все вхождения последнего поиска на `bar` в выделенной области

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
