---
layout: post
title:  "Kombinatorics"
date:   2018-11-26 23:09:01 +0500
categories: jekyll update
---
<!-- mathjax config similar to math.stackexchange -->
<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  jax: ["input/TeX", "output/HTML-CSS"],
  tex2jax: {
    inlineMath: [ ['$', '$'] ],
    displayMath: [ ['$$', '$$']],
    processEscapes: true,
    skipTags: ['script', 'noscript', 'style', 'textarea', 'pre', 'code']
  },
  messageStyle: "none",
  "HTML-CSS": { preferredFont: "TeX", availableFonts: ["STIX","TeX"] }
});
</script>
<script src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS_HTML" type="text/javascript"></script>

Конспект по книге Комбинаторика Виленкина

# Основы

## Правило суммы
$$m+n$$

`Если некоторый объект A можно выбрать m способами, а другой объект B можно выбрать n способами, то выбор "Либо A, либо B" можно осуществить m+n способами`

Если способы начинают пересекаться k раз, то получаем `m+n-k` способов

## Правило произведения

$$m*n$$

`Если объект A можно выбрать m способами и если после каждого такого выбора объект B можно выбрать n способами, то выбор пары (A, B) в указанном порядке можно осуществить mn способами`

## Формула включений и исключений

$$N(a_ia_j...a_k)$$ - количество предметов обладающих свойствами $a_i$,$a_j$,...,$a_k$( и возможно ещё другими свойствами)

$$a'_j$$ - не обладает свойством $a_j$

$$N(a'_1a'_2...a'_n) = N - N(a_1)- N(a_2)- ... - N(a_n) + \\
N(a_1a_2) + N(a_1a_3) + ... + N(a_1a_n) + ... + N(a_{n-1}a_n) -  \\
N(a_1a_2a_3) - ... - N(a_{n-2}a_{n-2}a_n) + ... + (-1)^nN(a_1a_2...a_n)
$$
Формула доказывается по индукции.

# Размещения, перестановки, сочетания

k-расстановки(размещения) -- набор предметов из k элементов

## Размещения с повторениями
n видов, по k предметов 
$$\overline{A_n^k}=n^k$$
