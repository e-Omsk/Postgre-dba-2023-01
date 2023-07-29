GCE   
Проект: postgres197605.  
VM postgres-dz-13

# Цель:    
Применить логический бэкап. Восстановиться из бэкапа      

# Описание/Пошаговая инструкция выполнения домашнего задания:   

1. Создаем ВМ/докер c ПГ.:    
> Создал ВМ на Google cloud и установил Ubuntu 22.04 через web интерфейс. Postgres 15.        

2. Создаем БД, схему и в ней таблицу:         
> создал БД backupdb: CREATE DATABASE backupdb;          
> подключаемся к БД: \c backupdb;     
> создаем схему backup: CREATE SCHEMA backup;     
> создадим таблицу table1 и заполним случайными данными: create table table1 as select generate_series(1,100) as id, md5(random()::text)::char(20) as name;         
> смотрим текущую схему SELECT current_schema(); - видим public;     
> смотрим нашу таблицу в схеме backup \dt backup.* - ее там нет, значит она в public - e;     
> переносим нашу таблицу в схему backup: ALTER TABLE table1 SET SCHEMA backup;     
> смотрим снова \dt backup.* - на месте.    

3. Заполним таблицы автосгенерированными 100 записями:     
> заполнили в предыдущем пункте. 

4. Под линукс пользователем Postgres создадим каталог для бэкапов:
> под root - ом создаю каталог pg_backup: sudo mkdir pg_backup;
> меняю владельца на postgres: sudo chown postgres.postgres /pg_backup;
> проверяем: ls -la;   

5. Сделаем логический бэкап используя утилиту COPY:
>      
