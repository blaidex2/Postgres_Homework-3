## Домашнее задание №3


•	создайте виртуальную машину c Ubuntu 20.04/22.04 LTS в GCE/ЯО/Virtual Box/докере

![image](https://user-images.githubusercontent.com/130083589/235294936-2b940b51-a837-48a0-a8f6-26669774ab59.png)

 
•	поставьте на нее PostgreSQL 15 через sudo apt
>sudo apt update && sudo DEBIAN_FRONTEND=noninteractive apt upgrade -y -q && 
>sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > 
>/etc/apt/sources.list.d/pgdg.list' && wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | 
>sudo apt-key add - && sudo apt-get update && sudo DEBIAN_FRONTEND=noninteractive apt -y install postgresql-15
 
 ![image](https://user-images.githubusercontent.com/130083589/235294979-e1a5b94d-897d-46e8-be1d-d0c3e6307136.png)


•	проверьте что кластер запущен через sudo -u postgres pg_lsclusters

![image](https://user-images.githubusercontent.com/130083589/235294986-ae1017bb-042a-4029-b1c6-9c4d5e3355cb.png)

 
•	зайдите из под пользователя postgres в psql и сделайте произвольную таблицу с произвольным содержимым
postgres=# create table test(c1 text);
postgres=# insert into test values('1');
\q

![image](https://user-images.githubusercontent.com/130083589/235294996-50bbb057-5d38-4816-b49f-8be9266f4ab6.png)

 
•	остановите postgres например через sudo -u postgres pg_ctlcluster 15 main stop
 
 
 ![image](https://user-images.githubusercontent.com/130083589/235295009-1a60424b-c1ce-4817-bcca-a081cae60d30.png)


•	создайте новый диск к ВМ размером 10GB
•	добавьте свеже-созданный диск к виртуальной машине - надо зайти в режим ее редактирования и дальше выбрать пункт attach existing disk
 
 ![image](https://user-images.githubusercontent.com/130083589/235295019-9deb8c03-2013-488f-ad86-2e00800fc4ed.png)

 
•	проинициализируйте диск согласно инструкции и подмонтировать файловую систему, только не забывайте менять имя диска на актуальное, 
в вашем случае это скорее всего будет /dev/sdb - https://www.digitalocean.com/community/tutorials/how-to-partition-and-format-storage-devices-in-linux
 
 ![image](https://user-images.githubusercontent.com/130083589/235295029-6e0138f6-0443-4d9c-bedd-4395a52c1b67.png)

![image](https://user-images.githubusercontent.com/130083589/235295033-ec337ef7-48ce-4d31-b85d-9e07d4b7e22d.png)


 
•	перезагрузите инстанс и убедитесь, что диск остается примонтированным (если не так смотрим в сторону fstab)
 
 ![image](https://user-images.githubusercontent.com/130083589/235295040-b41f7845-b2b7-42b0-84a4-37e7b1c9d34b.png)


•	сделайте пользователя postgres владельцем /mnt/data - chown -R postgres:postgres /mnt/data/
 
 
 ![image](https://user-images.githubusercontent.com/130083589/235295047-c7528d0b-3730-498c-a90d-c62c0d75f495.png)


•	перенесите содержимое /var/lib/postgres/15 в /mnt/data - mv /var/lib/postgresql/15/mnt/data
 
 ![image](https://user-images.githubusercontent.com/130083589/235295065-8da89131-ff32-4d9e-a085-cd7f54d939e2.png)


•	попытайтесь запустить кластер - sudo -u postgres pg_ctlcluster 15 main start
 
 ![image](https://user-images.githubusercontent.com/130083589/235295095-c3dc2f3a-896f-4be9-9cd5-74e1cae760a0.png)


•	напишите получилось или нет и почему

> Запустить кластер не удалось, т.к. данные были перемещены

•	задание: найти конфигурационный параметр в файлах раположенных в /etc/postgresql/15/main который надо поменять и поменяйте его
 
 ![image](https://user-images.githubusercontent.com/130083589/235295127-7f075050-e1c9-47ce-a8cc-d304500108d2.png)

![image](https://user-images.githubusercontent.com/130083589/235295136-b0514492-b929-4e6e-bae0-3fa2aabde3cb.png)

![image](https://user-images.githubusercontent.com/130083589/235295146-fe2b314d-42a3-44df-adf7-a8cea3478487.png)


•	напишите что и почему поменяли

> Был изменен путь с «/var/lib/postgresql/15/» на «/mnt/data/15/», т.к. ранее были перемещены данные кластера.

•	попытайтесь запустить кластер - sudo -u postgres pg_ctlcluster 15 main start
 
 ![image](https://user-images.githubusercontent.com/130083589/235295151-bf183c7f-39cc-4472-bdb8-c26124b77f9f.png)


•	напишите получилось или нет и почему

> Удалось запустить кластер, т.к. был указан верный путь в конфигурационном файле.

•	зайдите через через psql и проверьте содержимое ранее созданной таблицы
 
 ![image](https://user-images.githubusercontent.com/130083589/235295166-5f6e61fb-6d86-47a9-857d-b95925fbdf06.png)


•	задание со звездочкой *: не удаляя существующий инстанс ВМ сделайте новый, поставьте на его PostgreSQL, удалите файлы с данными из /var/lib/postgres, перемонтируйте внешний диск который сделали ранее от первой виртуальной машины ко второй и запустите PostgreSQL на второй машине так чтобы он работал с данными на внешнем диске, расскажите как вы это сделали и что в итоге получилось.

![image](https://user-images.githubusercontent.com/130083589/235295176-95b5ba00-7b52-45c5-9213-fead025921b0.png)

![image](https://user-images.githubusercontent.com/130083589/235295181-d32dd284-f8f2-4dfa-bc91-c75a3b0ec533.png)

![image](https://user-images.githubusercontent.com/130083589/235295184-7e9e01b7-c3bf-4330-bf80-f536e5f76b0c.png)

![image](https://user-images.githubusercontent.com/130083589/235295202-f91e6184-46ce-4c91-9686-864e2cadc603.png)

![image](https://user-images.githubusercontent.com/130083589/235295205-2a9159c9-4200-4061-8f06-42fb2b9f357b.png)

 
> Создана ВМ2, на ней установлен PostgreSQL, после чего в папку «/var/lib/postgresql/» ВМ2 был примонтирован диск с данными, ранее примонтированный к ВМ1. 
PostgreSQL запущен на ВМ2, данные с ВМ1 присутствуют.

