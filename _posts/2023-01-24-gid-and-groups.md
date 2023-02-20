layout: post
title:  "Gid and name group"
date: 2023-01-13 15:24:00 +0500
categories: jekyll update

# Разные группы в linux с одним названием

Иногда у нас может быть несколько групп с одним названием.

Например, если одна группа была в системе, а вторая получена по ldap.

``` bash
sudo -u user1 id 
# uid=1806(user1) gid=1005(users) groups=1005(users)

sudo -u user2 id 
# uid=1001(runner) gid=1003(runner) groups=1003(runner)

# Такая команда может не сработать, потому что есть две группы с названием users
usermod -a -G users runner

# А когда указали точно gid
usermod -a -G 1005 -g 1005 runner

sudo -u runner id 
# uid=1001(runner) gid=1005(users) groups=1005(users),100(users)
```

<!-- :public: -->
#linux
