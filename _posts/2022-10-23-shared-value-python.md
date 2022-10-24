---
layout: post
title:  "Shared Value Python"
date: 2022-10-21 09:14:00 +0500
categories: jekyll update
---

# User shared memory in python

Когда пишешь в расшаренную пямять требуется смотреть, чтобы не было переполнения ( пишу и думаю, что знал об этом из нескольких книг и сам себе выстрелил в ногу).


Вот простенький пример программы:

##### python 1.py
```python
#!/usr/bin/env python

import argparse
import sys
from multiprocessing import Process
from multiprocessing.sharedctypes import Array


def set_value(value, new_value):
    value.value = bytes(new_value, "utf-8")


def main():
    parser = argparse.ArgumentParser(description="probe")
    parser.add_argument("--value", type=str, help="Value to set", default="value")
    args = parser.parse_args()
    old_value = "value"
    print(f"old value: {old_value}")

    value = Array(
        "c",
        bytes(old_value, "utf-8"),
    )
    thread1 = Process(
        target=set_value,
        args=(value, args.value),
    )
    thread1.start()

    thread1.join()
    print(f"new value: {value.value}")
    return True


if __name__ == "__main__":
    sys.exit(main())
```

То есть мы пишем в общую память одно значение и выходим.
Пример запуска:

```bash

# При таком запуске всё будет хорошо
❯ python 1.py --value=value
old value: value
new value: b'value'


# А здесь получаем переполнение
❯ python 1.py --value=value1
old value: value
Process Process-1:
Traceback (most recent call last):
  File "/usr/lib/python3.10/multiprocessing/process.py", line 314, in _bootstrap
    self.run()
  File "/usr/lib/python3.10/multiprocessing/process.py", line 108, in run
    self._target(*self._args, **self._kwargs)
  File "/home/nigredon1991/reps/monitor_radio/1.py", line 10, in set_value
    value.value = bytes(new_value, "utf-8")
ValueError: byte string too long
new value: b'value'
```

Меня здесь смутил текст ошибки, не сразу понял в чём проблема, потому что не писал такие приложения ранее.

<!-- :public: -->
