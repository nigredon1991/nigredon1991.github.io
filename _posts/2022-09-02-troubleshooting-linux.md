---
layout: post
title:  "Troobleshooting linux"
date: 2022-09-02 14:22:00 +0500
categories: jekyll update
---

# Разные полезные команды для разбора, что происходит в линуксе

##### Первое

 ⁃ Виртуалка или контейнер? cat /.dockerenv
 ⁃ Что это за система? cat /etc/os-release

#### htop 

  F5 - three view
 TAB - увидеть IO # В новых версиях есть возможность проверить IO
 
 ⁃ Насколько она загружена? Какие ресурсы используются сильнее?

#### Чистка лишних логов и по разным задачам

```
 journalctl _COMM=sshd
 journalctl --vacuum-time=2d
 journalctl --vacuum-size=500M
```

#### Проверка сети

 * mtr
 * tcpdump
 * lsof  Check port:  lsof -i :8080 , lsof -p <PID>
 * ss TCP -ta , UDP -ua , UNIX Sockets -xa , open ports -ntulp , ss -t -r state established,listening
 * ip
 * dig
 * nc -v -n 192.168.104.232 22
 * iotop -o ; iotop -p <PID> 
 * nload

# https://www.cyberciti.biz/datacenter/linux-unix-bsd-osx-cannot-write-to-hard-disk/

#### Проверка разных проблем с диском

```bash
  fallocate -l 1G test4.img
  df -h
  duf
# Fixing problem when the disk is full
  Compress uncompressed log and other files using gzip or bzip2 or tar command:
    gzip /ftpusers/tmp/*.log
    bzip2 /ftpusers/tmp/large.file.name
#  Delete unwanted files using rm command on a Unix-like system:
   rm -rf /ftpusers/tmp/*.bmp
#  Move files to other system or external hard disk using rsync command:
    rsync --remove-source-files -azv /ftpusers/tmp/*.mov /mnt/usbdisk/
    rsync --remove-source-files -azv /ftpusers/tmp/*.mov server2:/path/to/dest/dir/
#  Find out the largest directories or files eating disk space on a Unix-like systesm:
    du -a /ftpusers/tmp | sort -n -r | head -n 10
    du -cks * | sort -rn | head
    # check for the whole disk #
    du -cks / | sort -rn | head
#  Truncate a particular file. This is useful for log file:
    truncate -s 0 /ftpusers/ftp.upload.log
    ### bash/sh etc ##
    >/ftpusers/ftp.upload.log
    ## perl ##
    perl -e'truncate "filename", LENGTH'
#  Find and remove large files that are open but have been deleted on Linux or Unix:
    ## Works on Linux/Unix/OSX/BSD etc ##
    lsof -nP | grep '(deleted)'
     
    ## Only works on Linux ##
    find /proc/*/fd -ls | grep  '(deleted)'
#    To truncate it:

     ## works on Linux/Unix/BSD/OSX etc all ##
    > "/path/to/the/deleted/file.name"
    ## works on Linux only ##
    > "/proc/PID-HERE/fd/FD-HERE"

# Read-only mode
 cat > file
# You may end up getting an error such as follows when you try to create a file or save a file:
  $ cat > file
  -bash: file: Read-only file system

#  Run mount command to find out if the file system is mounted in read-only mode:
  $ mount
  $ mount | grep '/ftpusers'

#  To fix this problem, simply remount the file system in read-write mode on a Linux based system:
  mount -o remount,rw /ftpusers/tmp

#   Another example, from my FreeBSD 9.x server to remount / in rw mode:
  # mount -o rw /dev/ad0s1a /

#   Linux file system enters in read-only mode when it detects disk or filesystem problems. Hence, it is best to check if the disk is failing badly. Check for the overall health of your disk:
  $ sudo smartctl -d ata -H /dev/sda

#   Once the test completed, ﻿﻿run the following to see if disk is failing
  $ sudo smartctl --attributes --log=selftest /dev/sda

#  Out of inodes

  df -i

#  Is my hard drive is dying?
# I/O errors in log file (such as /var/log/messages) indicates that something is wrong with the hard disk and it may be failing. You can check hard disk for errors using smartctl command, which is control and monitor utility for SMART disks under Linux and UNIX like operating systems. The syntax is:

  smartctl -a /dev/DEVICE
  # check for /dev/sda on a Linux server
  smartctl -a /dev/sda

#  Is my hard drive and server is too hot?
#   High temperatures can cause server to function poorly. So you need to maintain the proper temperature of the server and disk. High temperatures can result into server shutdown or damage to file system and disk. Use hddtemp or smartctl utility to find out the temperature of your hard on a Linux or Unix based system by reading data from S.M.A.R.T. on drives that support this feature. Only modern hard drives have a temperature sensor. hddtemp supports reading S.M.A.R.T. information from SCSI drives too. hddtemp can work as simple command line tool or as a daemon to get information from all servers:
  hddtemp /dev/DISK
  hddtemp /dev/sg0
  # You can use the smartctl command as follows too:
  smartctl -d ata -A /dev/sda | grep -i temperature

#   How do I get the CPU temperature?
    sensors
#   Dealing with corrupted file systems
    umount /ftpusers
    fsck -y /dev/sda8
#   Dealing with software RAID on a Linux
    mdadm --detail /dev/md0
   
    ## Find status ##
    cat /proc/mdstat
    watch cat /proc/mdstat
```

<!-- :public: -->
