2025-09-16_08:13

Надо было поставить yandexmusic в Manjaro.
Нашёл утилиту debtap

```sh
debtap Yandex_Music_amd64_5.68.0.deb
sudo pacman -U yandexmusic-5.68.0.r17465047-1-x86_64.pkg.tar.zst
loading packages...
resolving dependencies...
warning: cannot resolve "gtk", a dependency of "yandexmusic"
:: The following package cannot be upgraded due to unresolvable dependencies:
      yandexmusic

:: Do you want to skip the above package for this upgrade? [y/N] y
looking for conflicting packages...
 there is nothing to do
```
Не ставится, потому что требуется gtk.
Но он в системе у меня есть.
Открываешь zst - файл, открываешь .PKGINFO, удаляешь все строки содержащие 'gtk'. Сохранение.
Далее установка идёт без проблем.

Скрипт для пересборки **repack-yandex-music.sh**:
```sh
#!/bin/bash

set -eu

remove_gtk_from_archive() {
        SOURCE_ARCHIVE=${1?Need path to archive}

        SOURCE_ARCHIVE="$(readlink -f "${SOURCE_ARCHIVE}")"
        [ -f "${SOURCE_ARCHIVE}" ] || {
                echo "Need path to archive"
                exit 1
        }

        NEW_FILE="${SOURCE_ARCHIVE/tar/new.tar}"

        TEMPD=$(mktemp -d)
        cd "$TEMPD"
        tar -I unzstd -xvf "${SOURCE_ARCHIVE}"
        sed -i '/gtk/d' .PKGINFO
        find . -type f -printf '%P\0' | tar -I zstd -c -f "${NEW_FILE}" --null --files-from -
        echo "New file: ${NEW_FILE}"
        echo "=========================="
        echo "sudo pacman -U ${NEW_FILE}"
}

SOURCE_DEB="${1?Need path to deb}"
SOURCE_DEB="$(readlink -f "${SOURCE_DEB}")"
DIR_SOURCE="$(dirname "$SOURCE_DEB")"

debtap -Q -o "$DIR_SOURCE" "$SOURCE_DEB"
source_archive="$(find "$DIR_SOURCE" -type f -name '*.zst' -exec readlink -f {} \;)"

remove_gtk_from_archive "$source_archive"
```

#linux

<!-- :public: -->
