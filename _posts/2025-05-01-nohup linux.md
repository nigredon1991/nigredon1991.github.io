2025-05-01_18:45
# Использование nohup
Утилита, чтобы отвязать запускаемый процесс от терминала.
По умолчанию выводит stdout в файл `nohup.out`. Можно использовать в скриптах.

В целом, nohup удобен когда надо запустить какое-то gui-приложение из консоли и закрыть:
```sh
# без вывода
QT_AUTO_SCREEN_SCALE_FACTOR=1 nohup  /var/lib/snapd/snap/bin/fbreader
# запуск, потом руками убить консоль и сразу убрать вывод, чтобы ничего лишнего не создавалось
QT_AUTO_SCREEN_SCALE_FACTOR=1 nohup  /var/lib/snapd/snap/bin/fbreader >/dev/null
# сразу с закрытием
(QT_AUTO_SCREEN_SCALE_FACTOR=1 nohup  /var/lib/snapd/snap/bin/fbreader >/dev/null & ) ; exit
```
Если убить shell , то процесс остаётся только с  nohup и а с & убивается.

<!-- :public: -->
