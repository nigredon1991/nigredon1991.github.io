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
`cis`


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
