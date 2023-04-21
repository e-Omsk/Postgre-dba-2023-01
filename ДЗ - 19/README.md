GCE   
Проект: postgres197605.  
VM postgres-0  
БД otus

# Базовое задание 
Варриант 1: Создать индексы на БД, которые ускорят доступ к данным.

Таблица anytable пять полей. Первая id, остальные текстовые. 

1. Создать индекс к какой-либо из таблиц вашей БД.
 
> Смотрим:    
> explain  
> select id from anytable where id = 60000;    
> Получилось сканирование по последовательности. Второй cost = 2070 машиновремени:   
> Seq Scan on anytable  (cost=0.00..2070.00 rows=1 width=4)   
> Создаем индекс: 
> create index idx_anytable_id on anytable(id);  
> Смотрим explain. Сканирование по индексу. Воторой cost = 4,31. Значительно уменьшился:   
> Index Only Scan using idx_anytable_id on anytable  (cost=0.29..4.31 rows=1 width=4)  

2. Реализовать индекс для полнотекстового поиска.

> Смотрим:  
> explain select * from anytable where colum1='300004.031835575529453';  
> Получилось  сканирование по последовательности. Второй cost = 2070 машиновремени:       
> Seq Scan on anytable  (cost=0.00..2070.00 rows=1 width=77)    
> Создаем индекс для полнотекстового поиска:    
> create index idx_anytable_text on anytable(colum1);     
> Проверяем explain. Воторой cost = 8,44. Значительно уменьшился:      
> Index Scan using idx_anytable_text on anytable  (cost=0.42..8.44 rows=1 width=77)
> 
