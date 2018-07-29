---
layout: post
title:  "bash dialog example"
date:   2018-07-27 23:09:01 +0500
categories: jekyll update
---

Примеры кода для создания интерактивных интерфейсов в bash с помощью dialog.

### Примеры

* `--backtitle <TEXT>` - название в левом верхнем углу экрана
* `--checklist <TEXT> <WIND_HEIGHT> <LENGTH> <LIST_HEIGHT>` - выбор нескольких вариантов
* `--radiolist <TEXT> <WIND_HEIGHT> <LENGTH> <LIST_HEIGHT>` - выбор одного варианта
* `--inputbox <TEXT> <WIND_HEIGHT> <LENGTH> <LIST_HEIGHT>`
* `yesno <TEXT> <WIND_HEIGHT> <LENGTH>` - запрос на согласие, даёт код возврата


# Добавление дополнительной кнопки в меню

`dialog --extra-button --extra-label 123 --menu 0 0 0 0 0 0`

# Захват вывода

Если надо ловить вывод, того что ввёл пользователь, то надо выкатывать error_out в файл(2>"MyFile")

Если вывод имеет код возврата, то ловим его.
Код 255 - означает, что клиент ввёл `Esc`. `CTRL-C` то же самое что нет в `yesno`

Из checklist вывод в файл происходит в ковычках, поэтому можно удялять их `sed -i 's/"//g'`
$###TODO - но надо разобраться, как работает по нормальному вывод из dialog

$TODO  - надо разобраться, как использовать питон, либо другой ЯП для отображения этих менюшек
