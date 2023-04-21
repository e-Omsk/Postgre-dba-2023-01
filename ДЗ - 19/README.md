GCE   
Проект: postgres197605.  
VM postgres-0  
БД otus

# Базовое задание 
Варриант 1: Создать индексы на БД, которые ускорят доступ к данным.

1. Создать индекс к какой-либо из таблиц вашей БД.

> Создал таблицу anytable пять полей. Первая id, остальные текстовые.  
> Смотрим:    
> explain
> select id from anytable where id = 60000;    
> Получилось, сканирование по последовательности. Второй cost = 2070 машиновремени:   
>  Seq Scan on anytable  (cost=0.00..2070.00 rows=1 width=4)
   Filter: (id = 60000)
> Создаем индекс: 
> create index idx_anytable_id on anytable(id);  
> Смотрим explain. Сканирование по индексу. Воторой cost = 4,31. Значительно уменьшился: 
> Index Only Scan using idx_anytable_id on anytable  (cost=0.29..4.31 rows=1 width=4)  
> 
