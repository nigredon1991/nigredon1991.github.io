---
layout: post
title:  "bash-setuid"
date:   2020-07-19 21:09:22 +0500
categories: jekyll update
---

Небольшие заметки.


# Задача: требуется сделать скрипт, который будет запускаться с правами рута от обычных пользователей

Есть uid флаг на файлах в Unix, который позволяет запускать программы пользователям с правами рута, но в Linux, он не работает на любых shell-скриптах. Нужен запуск в виде скомпилированной программы(все варианты я не проверял, знаю что на C будет работать)
Проблема в том, что 

1. Пишем скрипт на баше по абсолютному пути с правами редактирования только от рута

/usr/local/bin/echo_command
```bash
#!/bin/bash
echo $UID
echo $*
```

1. Создаём враппер на C, который будет запускать скрипт с правами рута
/usr/local/bin/echo_command_wrapper
```C
#include <stdio.h>

int main(int argc, char* argv[])
{
  setuid(0);
  char buffer[100];
  snprintf(buffer, sizeof buf, "%s%s%s", "/usr/local/bin/echo_command \"", argv[1], "\"")
  system(buffer);
  return 0;
}
```


```bash
$ sudo chmod a+s /usr/local/bin/echo_command_wrapper
$ sudo chown root:root /usr/local/bin/echo_command_wrapper
$ sudo chown root:root /usr/local/bin/echo_command
```


1. Test

```bash
$ echo $UID
1000
$ ./usr/local/bin/echo_command
1000
$ ./usr/local/bin/echo_command_wrapper lolo
0
lolo
```
