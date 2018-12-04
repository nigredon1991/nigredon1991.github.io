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

##### Основы

# Правило суммы
$$m+n$$

`Если некоторый объект A можно выбрать m способами, а другой объект B можно выбрать n способами, то выбор "Либо A, либо B" можно осуществить m+n способами`
Если способы начинают пересекаться k раз, то получаем `m+n-k` способов

# Правило произведения
$$m*n$$
`Если объект A можно выбрать m способами и если после каждого такого выбора объект B можно выбрать n способами, то выбор пары (A, B) в указанном порядке можно осуществить mn способами`

##### Размещения

k-расстановки(размещения) -- набор предметов из k элементов

# Размещения с повторениями
n видов, по k предметов 
$$\overline{A_n^k}=n^k$$
