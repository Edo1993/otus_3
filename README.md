# otus_3
lvm homework

На имеющемся образе centos/7 - v. 1804.21 (приложенный Vagrantfile) 

- Уменьшить том под / до 8G
- Выделить том под /home
- Выделить том под /var -  сделать в mirror
- /home - сделать том для снапшотов
- Прописать монтирование в fstab. Попробовать с разными опциями и разными файловыми системами ( на выбор)

Работа со снапшотами:- сгенерить файлы в /home/
- снять снапшот
- удалить часть файлов
- восстановится со снапшота
Залоггировать работу можно с помощью утилиты script

Перед началом работ запускаем утилиту script (homework.log - приложен к репозиторию)
```script homework.log```

Разворачиваем Vagrantfile, подключаемся к машине lvm
```
vagrant up
vagrant ssh
```
На vm устанавливаем утилиты, кот. помогут в дальнейшей работе
```
sudo yum install xfsdump vim
```

Смотрим список дисков и их разделов сразу после старта vm
```
lsblk
```
Подготовим временный том для / раздела
```
pvcreate /dev/sdb
vgcreate vg_root /dev/sdb
lvcreate -n lv_root -l +100%FREE /dev/vg_root
```
Создадим на нем файловую систему и смонтируем его, чтобы перенести туда данные:
```
mkfs.xfs /dev/vg_root/lv_root
mount /dev/vg_root/lv_root /mnt
```
![Image alt](https://github.com/Edo1993/otus_3/raw/master/1.png)
Этой командой скопируем все данные с / раздела в /mnt - итог вывода на скрине:
```
xfsdump -J - /dev/VolGroup00/LogVol00 | xfsrestore -J - /mnt
```

![Image alt](https://github.com/Edo1993/otus_3/raw/master/3.png)
