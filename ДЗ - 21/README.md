GCE   
Проект: postgres197605.  
VM postgres-dz-21

Секционирование таблицы

# Цель:
Научиться секционировать таблицы.

1. Секционировать большую таблицу из демо базы flights.    
> Скопировал БД "Авиаперевозки" по ссылке:     
> https://edu.postgrespro.ru/demo_small.zip     
> Разархивировал и скопировал в GCE на VM postgres-dz-21 с помощью scp:       
> scp demo_small.sql e-Omsk@35.224.144.159:/home/e-Omsk       
> раздал полномочия директории где лежал файл: chmod 755 /home/e-Omsk      
> затянул данные в базу:       
> "sudo -u postgres psql -f /home/e-Omsk/demo_small.sql"          
> посмотрел какие таблицы есть: select * from pg_catalog.pg_tables;        
> выбрал самую большую таблицу (получилась ticket_flights ):      
> select pg_size_pretty(pg_total_relation_size('"bookings"."ticket_flights"'));               
> 109 MB            
> количестов строк: select count(*) from bookings.ticket_flights;       
> 1045726
> Сециониование по списку:       
> По колонке: fare_conditions. Три значения: Business, Comfort, Economy
> Удалил таблицу: DROP table ticket_flights;     
> CREATE TABLE ticket_flights (     
>    ticket_no character(13) NOT NULL,    
>    flight_id integer NOT NULL,     
>    fare_conditions character varying(10) NOT NULL,     
>    amount numeric(10,2) NOT NULL,      
>    CONSTRAINT ticket_flights_amount_check CHECK ((amount >= (0)::numeric)),                 
>    CONSTRAINT ticket_flights_fare_conditions_check CHECK (((fare_conditions)::text = ANY (ARRAY[('Economy'::character varying)::text, ('Comfort'::character varying)::text,('Business'::character varying)::text])))              
> );    
> CREATE TABLE ticket_flights_Business partition of ticket_flights for values in ('Business');    
> CREATE TABLE ticket_flights_Comfort partition of ticket_flights for values in ('Comfort');     
> CREATE TABLE ticket_flights_Economy partition of ticket_flights for values in ('Economy');     
> CREATE TABLE ticket_flights_Default partition of ticket_flights default ;
> 
