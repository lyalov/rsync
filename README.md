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
rsync -avc --delete --exclude='.*' /home/$USER/ /tmp/backup/
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

#данная команда покажет пустоту !
Я создал три папки и в каждой папке файлы. Ниже структура. 

```bash
lyalov@RedFox:/mnt/d/gitlab/rsync$ tree /home/$USER/
/home/lyalov/
├── create.sh
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



![alt text](https://github.com/username/reponame/blob/branch/path/image.png)