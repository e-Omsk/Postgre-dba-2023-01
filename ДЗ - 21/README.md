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
> 
> В файл "demo_small.sql" на хосте через редактор nano добавил в строку где создается таблица (ticket_flights)
> в самом конце команды перед точкой запятой:                                   
> partition by list (fare_conditions):              
> CREATE TABLE ticket_flights (       
    ticket_no character(13) NOT NULL,       
    flight_id integer NOT NULL,      
    fare_conditions character varying(10) NOT NULL,
    amount numeric(10,2) NOT NULL,
    CONSTRAINT ticket_flights_amount_check CHECK ((amount >= (0)::numeric)),
    CONSTRAINT ticket_flights_fare_conditions_check CHECK (((fare_conditions)::text = ANY (ARRAY[('Economy'::character varying)::text, ('Comfort'::character varying)::text, ('Business'::character varying)::text])))            
) partition by list (fare_conditions);    
> туда же в файл добавил после создания таблицы  секционирование по списку: Business, Comfort, Economy     
> CREATE TABLE ticket_flights_Business partition of ticket_flights for values in ('Business');    
> CREATE TABLE ticket_flights_Comfort partition of ticket_flights for values in ('Comfort');        
> CREATE TABLE ticket_flights_Economy partition of ticket_flights for values in ('Economy');        
> CREATE TABLE ticket_flights_Default partition of ticket_flights default ;        
>
> Затягиваем файл: sudo -u postgres psql -f /home/e-Omsk/demo_small.sql                 
> Результат:        
> select count(*) from bookings.ticket_flights;         
> count  1.045.726         
> select count(*) from bookings.ticket_flights_Business;           
> count 107.642          
> select count(*) from bookings.ticket_flights_Comfort;       
> count 17.291       
> select count(*) from bookings.ticket_flights_Economy;       
> count 920.793       
> select count(*) from bookings.ticket_flights_Default;       
> count 0         
> Таблица секционировалась. Лвинная доля данных попала в секцию (таблицу) ticket_flights_Economy   
 
