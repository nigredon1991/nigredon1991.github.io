
---
layout: post
title:  "Python for windows on linux"
date: 2022-05-17 11:22:00 +0500
categories: jekyll update
---

# Workflow для создания приложений под linux на windows с помощью wine.

В директории уже есть какой-то код, который запускается под linux.
Уже настроен wine, в нём установлен python (лучше сразу 3 и последний возможный)

```
ls src/main.py
wine python -m venv venv
wine pip install pyinstaller
wine pyinstaller -F -c src/main.py
# -F - создать один файл
# -c - создать консольное приложение
```

После этого в директории build появится файла main.exe, который будет совершенно нормально запускаться под windows.

GUI-приложения с таким workflow не делал, но возможно заработает.
#windows
#python

<!-- :public: -->
