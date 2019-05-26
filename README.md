# lab2

# Установка ОС и настройка LVM, RAID
1.
1. Установка Linux
![](screens/VirtualBox_Raid_26_03_2019_16_55_33.png)

![](screens/VirtualBox_raid_06_04_2019_10_53_40.png)

![](screens/VirtualBox_raid_06_04_2019_10_58_16.png)

![](screens/VirtualBox_raid_06_04_2019_11_02_44.png)

![](screens/VirtualBox_raid_06_04_2019_11_08_35.png)

![](screens/VirtualBox_raid_06_04_2019_11_09_12.png)

![](screens/VirtualBox_raid_06_04_2019_11_12_23.png)

![](screens/VirtualBox_raid_06_04_2019_11_16_37.png)

![](screens/VirtualBox_raid_06_04_2019_11_17_06.png)

Поставил grub

![](screens/VirtualBox_raid_06_04_2019_11_29_23.png "Поставил grub")

![](screens/VirtualBox_raid_06_04_2019_11_29_49.png "Вывод mount")

С помощью этих команд выводятся данные об physical volumes, volume groups, logical volumes, примонтированных устройствах.

# Вывод

В этой работе я научился устанавливать ОС Linux, настраивать LVM и RAID, а также ознакомился с командами: 

* lsblk -o NAME,SIZE,FSTYPE,TYPE,MOUNTPOINT

* fdisk -l
*+pvs,lvs,vgs

* cat /proc/mdstat

* mount

* dd if=/dev/xxx of=/dev/yyy

* grub-install /dev/XXX

В результате получил виртуальную машину с дисками ssd1, ssd2.


# Задание 2 (Эмуляция отказа одного из дисков)

1.1 Был удален 1 диск и добавлен новый
Проверяем что новый диск приехал в систему с помощью fdisk -l

![](https://github.com/badpocyk/lab2/blob/master/screens/2.PNG?raw=true)

Копирование таблиц разделов на новый диск с помощью sfdisk -d /dev/XXXX | sfdisk /dev/YYY

![](https://github.com/badpocyk/lab2/blob/master/screens/3.PNG?raw=true)

Добавление в рейд массив нового диска: mdadm --manage /dev/md0 --add /dev/YYY 

![](https://github.com/badpocyk/lab2/blob/master/screens/5.PNG?raw=true)

Результат cat /proc/mdstat/

![](https://github.com/badpocyk/lab2/blob/master/screens/6.PNG?raw=true)

Синхронизация разделов 

![](https://github.com/badpocyk/lab2/blob/master/screens/7.PNG?raw=true)

Установка grub на новый диск

![](https://github.com/badpocyk/lab2/blob/master/screens/8.PNG?raw=true)

Проверка что все работает

![](https://github.com/badpocyk/lab2/blob/master/screens/9.PNG?raw=true)


# Вывод


В этом задании научился:


* Удалять диск ssd1

* Проверять статус RAID-массива

* Копировать таблицу разделов со старого диска на новый

* Добавлять в рейд массив новый диск

* Выполнять синхронизацию разделов, не входящих в RAID

Изучил новые команды:


* sfdisk -d /dev/XXXX | sfdisk /dev/YYY

* mdadm --manage /dev/md0 --add /dev/YYY

Результат: Удален диск ssd1, добавлен диск ssd3, ssd2 сохранен


# Задание 3 (Добавление новых дисков и перенос раздела)

Удалили один диск и проверили состояние

![](https://github.com/badpocyk/lab2/blob/master/screens/21.PNG?raw=true)

Добавили новый диск

![](https://github.com/badpocyk/lab2/blob/master/screens/22.PNG?raw=true)

Копирование со старого диска на новый

![](https://github.com/badpocyk/lab2/blob/master/screens/23.PNG?raw=true)

Копирование /boot

![](https://github.com/badpocyk/lab2/blob/master/screens/24.PNG?raw=true)

Перемонтировка /boot на живой диск

![](https://github.com/badpocyk/lab2/blob/master/screens/26.PNG?raw=true)

Установка grub

![](https://github.com/badpocyk/lab2/blob/master/screens/27.PNG?raw=true)

Создание нового RAID-массива с включением туда только одного нового ssd диска: 

![](https://github.com/badpocyk/lab2/blob/master/screens/28.PNG?raw=true)

Проверка состояния RAID

![](https://github.com/badpocyk/lab2/blob/master/screens/29.PNG?raw=true)

Выполнение команды pvs для просмотра информации о текущих физических томах 

![](https://github.com/badpocyk/lab2/blob/master/screens/210.PNG?raw=true)

Создание нового физического тома, включив в него ранее созданный RAID массив: 

![](https://github.com/badpocyk/lab2/blob/master/screens/211.PNG?raw=true)

Выполнение -vgdisplay system -v
-pvs
-vgs
-lvs -a -o+devices

![](https://github.com/badpocyk/lab2/blob/master/screens/212.PNG?raw=true)

![](https://github.com/badpocyk/lab2/blob/master/screens/213.PNG?raw=true)

Перемещение данных со старого диска на новый

![](https://github.com/badpocyk/lab2/blob/master/screens/214.PNG?raw=true)

-vgdisplay system -v
-pvs
-vgs
-lvs -a -o+devices
-lsblk -o NAME,SIZE,FSTYPE,TYPE,MOUNTPOINT

![](https://github.com/badpocyk/lab2/blob/master/screens/214.PNG?raw=true)

![](https://github.com/badpocyk/lab2/blob/master/screens/215.PNG?raw=true)

![](https://github.com/badpocyk/lab2/blob/master/screens/216.PNG?raw=true)

![](https://github.com/badpocyk/lab2/blob/master/screens/217.PNG?raw=true)

Изменение VG, удалив из него диск старого raid. 

![](https://github.com/badpocyk/lab2/blob/master/screens/218.PNG?raw=true)

--lsblk -o NAME,SIZE,FSTYPE,TYPE,MOUNTPOINT
--pvs
--vgs
В выводе команды pvs у /dev/md0 исчезли VG и Attr. В выводе команды vgs #PV - уменьшилось на 1, VSize, VFree - стали меньше

![](https://github.com/badpocyk/lab2/blob/master/screens/219.PNG?raw=true)

Перемонтировка /boot на второ диск

![](https://github.com/badpocyk/lab2/blob/master/screens/220.PNG?raw=true)

Добавились новые диски

![](https://github.com/badpocyk/lab2/blob/master/screens/221.PNG?raw=true)

Скопировал таблицу разделов, установил grub

![](https://github.com/badpocyk/lab2/blob/master/screens/222.PNG?raw=true)

Изменение размера нового диска (второй раздел)

![](https://github.com/badpocyk/lab2/blob/master/screens/223.PNG?raw=true)

![](https://github.com/badpocyk/lab2/blob/master/screens/224.PNG?raw=true)

![](https://github.com/badpocyk/lab2/blob/master/screens/225.PNG?raw=true)

![](https://github.com/badpocyk/lab2/blob/master/screens/226.PNG?raw=true)

Перечитывание таблицы разделов

![](https://github.com/badpocyk/lab2/blob/master/screens/227.PNG?raw=true)

Добавление нового диска

![](https://github.com/badpocyk/lab2/blob/master/screens/228.PNG?raw=true)

Расширение кол-ва дисков в массиве до 2

![](https://github.com/badpocyk/lab2/blob/master/screens/229.PNG?raw=true)

Запуск fdisk /dev/sda

![](https://github.com/badpocyk/lab2/blob/master/screens/230.PNG?raw=true)

Таблица разделов

![](https://github.com/badpocyk/lab2/blob/master/screens/231.PNG?raw=true)

Расширение raid

![](https://github.com/badpocyk/lab2/blob/master/screens/232.PNG?raw=true)

--Вывод команды pvs
--Расширение размера PV
--Вывод команды pvs 

![](https://github.com/badpocyk/lab2/blob/master/screens/233.PNG?raw=true)

![](https://github.com/badpocyk/lab2/blob/master/screens/234.PNG?raw=true)

Имена новых дисков

![](https://github.com/badpocyk/lab2/blob/master/screens/235.PNG?raw=true)

Создание raid

![](https://github.com/badpocyk/lab2/blob/master/screens/236.PNG?raw=true)

Создание нового PV, в этом PV группы с названием data, логического тома с размером всего свободного пространства var_log 

![](https://github.com/badpocyk/lab2/blob/master/screens/237.PNG?raw=true)

Форматирование созданного раздела

![](https://github.com/badpocyk/lab2/blob/master/screens/238.PNG?raw=true)

Новое хранилище для логов

![](https://github.com/badpocyk/lab2/blob/master/screens/239.PNG?raw=true)

Синхронизация разделов и меняние их местами

![](https://github.com/badpocyk/lab2/blob/master/screens/240.PNG?raw=true)

Измнение /etc/fstab

![](https://github.com/badpocyk/lab2/blob/master/screens/241.PNG?raw=true)

Итоговая проверка

![](https://github.com/badpocyk/lab2/blob/master/screens/242.PNG?raw=true)