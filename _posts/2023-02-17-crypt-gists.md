layout: post
title:  "Crypt gists"
date: 2023-01-19 15:14:00 +0500
categories: jekyll update


# Создать сертификат

https://github.com/square/certstrap

Там инструкция

# Шифрование данных в репозитории с помощью sops и age

* sops.yaml: `mozilla sops <https://github.com/mozilla/sops>`

*.sops.yaml*
```
# Для генерации ключевой пары используется age (https://github.com/FiloSottile/age).
#   mkdir -p ~/.config/sops/age && age-keygen > ~/.config/sops/age/keys.txt
keys:
  - &user <age_key>

creation_rules:
  - path_regex: path/to/files/*/.*.enc$
    key_groups:
      - age:
          - *<user_public_key>
```

### Зашифровать файл

```
sops --encrypt -age <user_public_key> test.yaml > test.enc.yaml
```

Зашифровать файл для нескольких пользователей
```
sops --encrypt -age <user_public_key>,<user1_public_key> test.yaml > test.enc.yaml
```

<!-- :public: -->
#linux
