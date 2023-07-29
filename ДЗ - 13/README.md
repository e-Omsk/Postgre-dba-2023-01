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
> создадим таблицу table1: CREATE TABLE table1 (i int);      
> смотрим текущую схему SELECT current_schema(); - видим public;     
> смотрим нашу таблицу в схеме backup \dt backup.* - ее там нет, значит она в public - e;     
> переносим нашу таблицу в схему backup: ALTER TABLE table1 SET SCHEMA backup;     
> смотрим снова \dt backup.* - на месте.    

3. Заполним таблицы автосгенерированными 100 записями:     

