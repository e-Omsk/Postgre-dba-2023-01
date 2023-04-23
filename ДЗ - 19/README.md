GCE   
Проект: postgres197605.  
VM postgres-0  
БД otus

# Базовое задание 
Варриант 1: Создать индексы на БД, которые ускорят доступ к данным.

Таблица anytable пять полей. Первая id, остальные текстовые. 

1. Создать индекс к какой-либо из таблиц вашей БД.
 
> Смотрим:    
> explain select id from anytable where id = 60000;    
> Получилось сканирование по последовательности. Второй cost = 2070 машиновремени:   
> Seq Scan on anytable  (cost=0.00..2070.00 rows=1 width=4)   
> Создаем индекс: 
> create index idx_anytable_id on anytable(id);  
> Смотрим explain. Сканирование по индексу. Воторой cost = 4,31. Значительно уменьшился:   
> Index Only Scan using idx_anytable_id on anytable  (cost=0.29..4.31 rows=1 width=4)   
>  Index Cond: (id = 60000)    

2. Реализовать индекс для полнотекстового поиска.

> Смотрим:  
> explain select * from anytable where colum1='300004.031835575529453';  
> Получилось  сканирование по последовательности. Второй cost = 2070 машиновремени:       
> Seq Scan on anytable  (cost=0.00..2070.00 rows=1 width=77)    
> Создаем индекс для полнотекстового поиска:    
> create index idx_anytable_text on anytable(colum1);     
> Проверяем explain. Уже индекстное сканирование(Index Scan using idx_anytable_text). Воторой cost = 8,44. Значительно уменьшился:      
> Index Scan using idx_anytable_text on anytable  (cost=0.42..8.44 rows=1 width=77)  
> Index Cond: (colum1 = '300004.031835575529453'::text)

3. Реализовать индекс на часть таблицы или индексна поле с функцией.  

> Создаем индекс:  
> create index idx_anytable_id_1000 on anytable(id) where id < 1000;  
> Проверяем explain:   
> explain select id from anytable where id < 100;    
> Получаем индекстное сканирование(Index Only Scan using idx_anytable_id_1000 on anytable):   
> Index Only Scan using idx_anytable_id_1000 on anytable  (cost=0.28..6.01 rows=99 width=4)  
> Index Cond: (id < 100)   

4. Создать индекс на несколько полей.

> Смотрим:   
> explain select * from anytable where colum2 = '300008.557095817138523' and colum3 ='300001.843707564357473';   
> Получилось сканирование по последовательности. Второй cost = 2070 машиновремени:   
> Seq Scan on anytable  (cost=0.00..2270.00 rows=1 width=77)   
> Создаем индекс:  
> create index idx_anytable_text_text on anytable(colum2, colum3);    
> Проверяем explain. Уже индекстное сканирование(Index Scan using idx_anytable_text_text). Воторой cost = 8,44. Значительно уменьшился:   
> Index Scan using idx_anytable_text_text on anytable  (cost=0.42..8.44 rows=1 width=77)    
> Index Cond: ((colum2 = '300008.557095817138523'::text) AND (colum3 = '300001.843707564357473'::text))  
> Сделаем индекс на одно поле:  
> create index idx_anytable_text2 on anytable(colum2);   
> Проверим. Отличия от индеска на два поля нет. Видимо потому, что значение в колонке colum2 уникально:  
> Index Scan using idx_anytable_text2 on anytable  (cost=0.42..8.44 rows=1 width=77)   
>  Index Cond: (colum2 = '300008.557095817138523'::text)  

2 вариант: Различные варианты соединения таблиц.  
  
Создал и заполнил две таблицы:     
> create table author (id serial, name text);      
> insert into author values (1,'Стругацкие'),(2,'Ефремов'),(3,'Лем'),(4,'Бредбери');      
> create table books (id serial, book_title text, author int);     
> insert into books values (1,'Обитаемый остров',1), (2,'Град обреченный',1),(3,'Туманность Андромеды',2),(4,'Час Быка',2),(5,'Солярис',3),
					                    (6,'Марсианские хроники',4),(6,'Повесть временых лет',null),(7,'Народные сказки',null);     
                         
 1. Реализовать прямое соединение двух или более таблиц:   
 
> select *   
> from books b  
> join author auth  
>      on b.author = auth.id;  
>  Или:      
> select *   
> from books b, author auth  
> where b.author = auth.id;   
> верулись строки только точное совпадение по ключу из обеих таблиц.       

2. Реализовать левостороннее (или правостороннее) соединение двух или более таблиц.   

> левостороннее: 
> select *  
> from books b    
> left join author auth   
>         on b.author = auth.id;  
> вернулись строки из левой таблицы и строки из правой таблицы, те что совпали по ключу и строки с null значением ключа.        

> правостороннее:   
> select *    
> from books b     
> right join author auth    
>           on b.author = auth.id;    
> вернулись строки из правой таблицы и строки из левой таблицы, те что совпали по ключу и строки с null значением ключа.  

3. Реализовать кросс соединение двух или более таблиц.

> 

4. Реализовать полное соединение двух или более таблиц.   

> select *  
> from books b   
> full join author auth on b.author = auth.id;   
> выводит все строки из обеих таблиц        
> сначало выводит данные как в прямом соединении, затем добавляет оставшиеся данные из левой таблицы, затем из правой.         
