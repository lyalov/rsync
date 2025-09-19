«Резервное копирование» 
Ялова Л.В


```bash
Задание 1
Составьте команду rsync, которая позволяет создавать зеркальную копию домашней директории пользователя в директорию /tmp/backup
Необходимо исключить из синхронизации все директории, начинающиеся с точки (скрытые)
Необходимо сделать так, чтобы rsync подсчитывал хэш-суммы для всех файлов, даже если их время модификации и размер идентичны в источнике и приемнике.
На проверку направить скриншот с командой и результатом ее выполнения``
```
Составьте команду rsync
Исключаем директори с точкой командой 
```bash
rsync -avc  --exclude='.*' /home/$USER/ /tmp/backup/
```
Папка  /home/$USER/ 
у меня состоит  папки  с . (точкой) в начале названия.


```bash lyalov@RedFox:/mnt/d/gitlab/rsync$ ls -lah /home/$USER/
total 56K
drwx------ 7 lyalov lyalov 4.0K Sep 18 19:28 .
drwxr-xr-x 3 root   root   4.0K Sep 16 17:44 ..
-rw------- 1 lyalov lyalov 4.0K Sep 18 00:29 .bash_history
-rw-r--r-- 1 lyalov lyalov  220 Sep 16 17:44 .bash_logout
-rw-r--r-- 1 lyalov lyalov 3.8K Sep 17 13:04 .bashrc
drwx------ 3 lyalov lyalov 4.0K Sep 17 22:31 .cache
drwx------ 3 lyalov lyalov 4.0K Sep 17 22:31 .config
```

и данная команда покажет пустоту !
Я создал три папки и в каждой папке файлы. Ниже структура. 

```bash
lyalov@RedFox:/mnt/d/gitlab/rsync$ tree /home/$USER/
/home/lyalov/
└── test_home
    ├── folder1
    │   ├── file1.txt
    │   ├── file2.txt
    │   ├── file3.txt
    │   ├── file4.txt
    │   └── file5.txt
    ├── folder2
    │   ├── file1.txt
    │   ├── file2.txt
    │   ├── file3.txt
    │   ├── file4.txt
    │   ├── file5.txt
    │   ├── file6.txt
    │   ├── file7.txt
    │   └── file8.txt
    └── folder3
        ├── file1.txt
        ├── file2.txt
        ├── file3.txt
        ├── file4.txt
        ├── file5.txt
        └── file6.txt
```
Запускаю ещё раз 
``` bash lyalov@RedFox:/mnt/d/gitlab/rsync$ rsync -avc --delete --exclude='.*' /home/$USER/ /tmp/backup/
sending incremental file list
./
test_home/
test_home/folder1/
test_home/folder1/file1.txt  
test_home/folder1/file2.txt  
test_home/folder1/file3.txt  
test_home/folder1/file4.txt  
test_home/folder1/file5.txt  
test_home/folder2/
test_home/folder2/file1.txt  
test_home/folder2/file2.txt  
test_home/folder2/file3.txt  
test_home/folder2/file4.txt  
test_home/folder2/file5.txt  
test_home/folder2/file6.txt  
test_home/folder2/file7.txt  
test_home/folder2/file8.txt  
test_home/folder3/
test_home/folder3/file1.txt  
test_home/folder3/file2.txt  
test_home/folder3/file3.txt  
test_home/folder3/file4.txt  
test_home/folder3/file5.txt  
test_home/folder3/file6.txt  

sent 2,416 bytes  received 400 bytes  5,632.00 bytes/sec
total size is 839  speedup is 0.30
lyalov@RedFox:/mnt/d/gitlab/rsync$ rsync -avc --delete --exclude='.*' /home/$USER/ /tmp/backup/
sending incremental file list

sent 911 bytes  received 16 bytes  1,854.00 bytes/sec
total size is 839  speedup is 0.91
lyalov@RedFox:/mnt/d/gitlab/rsync$ 
```
Ключ в команде -с считывает хеш суммы , и даже повторное выполнение ничего не копирует , но хеш сумму перечитывает


ОТВЕТ 2
Скрипт 

```bash
#!/bin/bash
BACKUP_DIR="/tmp/backup"
LOG_FILE="$HOME/backup_home.log"

rsync -avc --delete --exclude='.*' "$HOME/" "$BACKUP_DIR/" >> "$LOG_FILE" 2>&1

if [ $? -eq 0 ]; then
    echo "$(date) - Backup completed successfully" >> "$LOG_FILE"
else
    echo "$(date) - Backup failed" >> "$LOG_FILE"
fi
```

Crontab фаил 

``` bash
# Edit this file to introduce tasks to be run by cron.
# 
# Each task to run has to be defined through a single line
# indicating with different fields when the task will be run
# and what command to run for the task
# 
# To define the time you can provide concrete values for
# minute (m), hour (h), day of month (dom), month (mon),
# and day of week (dow) or use '*' in these fields (for 'any').
# 
# Notice that tasks will be started based on the cron's system
# daemon's notion of time and timezones.
# 
# Output of the crontab jobs (including errors) is sent through
# email to the user the crontab file belongs to (unless redirected).
# 
# For example, you can run a backup of all your user accounts
# at 5 a.m every week with:
# 0 5 * * 1 tar -zcf /var/backups/home.tgz /home/
# 
# For more information see the manual pages of crontab(5) and cron(8)
# 
# m h  dom mon dow   command
0 2 * * * /home/lyalov/backup_home.sh
```
Лог фаил выполнения задания 
```bash
lyalov@RedFox:/mnt/d/gitlab/rsync$ cat ~lyalov/backup_home.log 
sending incremental file list
./
backup_home.log
backup_home.sh

sent 1,429 bytes  received 61 bytes  2,980.00 bytes/sec
total size is 1,167  speedup is 0.78
Fri Sep 19 04:10:36 PM MSK 2025 - Backup completed successfully

```
