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
> Запуситл для БД test: pgbench -c8 -P 6 -T 60 -U postgres test    
> Получилось:     
> pgbench (15.4 (Ubuntu 15.4-1.pgdg22.04+1))    
> starting vacuum...end.    
> progress: 6.0 s, 894.3 tps, lat 8.897 ms stddev 3.621, 0 failed     
> progress: 12.0 s, 912.2 tps, lat 8.770 ms stddev 3.632, 0 failed      
> progress: 18.0 s, 978.7 tps, lat 8.175 ms stddev 3.327, 0 failed       
> progress: 24.0 s, 984.3 tps, lat 8.124 ms stddev 3.340, 0 failed      
> progress: 30.0 s, 983.5 tps, lat 8.135 ms stddev 3.160, 0 failed      
> progress: 36.0 s, 957.0 tps, lat 8.358 ms stddev 7.414, 0 failed          
> progress: 42.0 s, 974.2 tps, lat 8.210 ms stddev 3.452, 0 failed       
> progress: 48.0 s, 987.0 tps, lat 8.104 ms stddev 3.300, 0 failed       
> progress: 54.0 s, 1002.2 tps, lat 7.984 ms stddev 3.143, 0 failed       
> progress: 60.0 s, 989.0 tps, lat 8.085 ms stddev 3.127, 0 failed      
> transaction type: <builtin: TPC-B (sort of)>       
> scaling factor: 1      
> query mode: simple       
> number of clients: 8      
> number of threads: 1      
> maximum number of tries: 1      
> duration: 60 s      
> number of transactions actually processed: 57982      
> number of failed transactions: 0 (0.000%)       
> latency average = 8.275 ms      
> latency stddev = 3.950 ms      
> initial connection time = 26.491 ms       
> tps = 966.512965 (without initial connection time)       

5. Применить параметры настройки PostgreSQL из прикрепленного к материалам занятия файла:    
> 

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
