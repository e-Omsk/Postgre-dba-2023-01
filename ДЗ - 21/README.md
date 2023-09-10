GCE   
Проект: postgres197605.  
VM postgres-dz-21

Секционирование таблицы

# Цель:
Научиться секционировать таблицы.

1. Секционировать большую таблицу из демо базы flights.    
> Скопировал БД "Авиаперевозки" по ссылке: https://edu.postgrespro.ru/demo_small.zip    
> Разархивировал и скопировал в GCE на VM postgres-dz-21 с помощью scp: "scp demo_small.sql e-Omsk@35.224.144.159:/home/e-Omsk"
> раздал полномочия директории где лежал файл: chmod 755 /home/e-Omsk
> затянул данные в базу: "sudo -u postgres psql -f /home/e-Omsk/demo_small.sql"
> посмотрел какие таблицы есть: select * from pg_catalog.pg_tables;
> выбрал самую большую таблицу (получилась ticket_flights ): select pg_size_pretty(pg_total_relation_size('"bookings"."ticket_flights"'));
>  pg_size_pretty    
  ----------------   
   109 MB   
  (1 row)      
> количестов строк: select count(*) from bookings.ticket_flights;
> count    
  ---------   
   1045726   
  (1 row)  
