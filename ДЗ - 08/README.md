GCE   
Проект: postgres197605.  
VM postgres-dz-08

Настройка autovacuum с учетом особеностей производительности

# Цель:
запустить нагрузочный тест pgbench
настроить параметры autovacuum
проверить работу autovacuum  

# Базовое задание 

1. Создать инстанс ВМ с 2 ядрами и 4 Гб ОЗУ и SSD 10GB:    
> Создал ВМ на Google cloud и установил Ubuntu 22.04 через web интерфейс. Параметры: 2 vCPU + 4 GB memory, диск 10 GB SSD persistent disk.    

2. Установить на него PostgreSQL 15 с дефолтными настройками:
> Установил командой:    
> sudo apt update && sudo apt upgrade -y -q && sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list' && wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add - && sudo apt-get update && sudo apt -y install postgresql-15    

3. Создать БД для тестов: выполнить pgbench -i postgres:
> Зашел в консоль: sudo -u postgres psql    
> Создал БД test: CREATE DATABASE test; 
> Вышел \q, запустил под ползователем postgres  программу для тестов pgbench: pgbench -i test;                
> Получилось:            
> creating tables...    
> generating data (client-side)...     
> 100000 of 100000 tuples (100%) done (elapsed 0.11 s, remaining 0.00 s)    
> vacuuming...    
> creating primary keys...     
> done in 0.54 s (drop tables 0.00 s, create tables 0.01 s, client-side generate 0.41 s, vacuum 0.05 s, primary keys 0.07 s).      

4. Запустить pgbench -c8 -P 6 -T 60 -U postgres postgres:
5. Применить параметры настройки PostgreSQL из прикрепленного к материалам занятия файла:
6. Протестировать заново:
7. Что изменилось и почему?
8. Создать таблицу с текстовым полем и заполнить случайными или сгенерированными данным в размере 1млн строк:
9. Посмотреть размер файла с таблицей:
10. 5 раз обновить все строчки и добавить к каждой строчке любой символ:
11. Посмотреть количество мертвых строчек в таблице и когда последний раз приходил автовакуум:
12. Подождать некоторое время, проверяя, пришел ли автовакуум:
13. 5 раз обновить все строчки и добавить к каждой строчке любой символ:
14. Посмотреть размер файла с таблицей:
15. Отключить Автовакуум на конкретной таблице:
16. 10 раз обновить все строчки и добавить к каждой строчке любой символ:
17. Посмотреть размер файла с таблицей:
18. Объясните полученный результат:
19. Не забудьте включить автовакуум.
