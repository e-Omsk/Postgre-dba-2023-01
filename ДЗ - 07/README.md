GCE   
Проект: postgres197605.  
VM postgresql-1

# Цель:
создание новой базы данных, схемы и таблицы   
создание роли для чтения данных из созданной схемы созданной базы данных   
создание роли для чтения и записи из созданной схемы созданной базы данных    

# Базовое задание 

1. создайте новый кластер PostgresSQL 14:    
> Создал ВМ на Google cloud и установил Ubuntu 22.04 через web интерфейс.   

2. зайдите в созданный кластер под пользователем postgres:   
> sudo -u postgres psql

3. создайте новую базу данных testdb:   
> CREATE DATABASE testdb;

4. зайдите в созданную базу данных под пользователем postgres:   
> \c testdb    
> будет сообщение: You are now connected to database "testdb" as user "postgres".  
> и курсор: testdb=#     

5. создайте новую схему testnm:  
> CREATE SCHEMA testnm;    
> сообщение: CREATE SCHEMA    

6. создайте новую таблицу t1 с одной колонкой c1 типа integer:  
> CREATE TABLE t1(c1 integer);    
> сообщение: CREATE TABLE    

7. вставьте строку со значением c1=1:   
> INSERT INTO t1 values(1);   
> сообщение: INSERT 0 1   

8. создайте новую роль readonly:  
> CREATE role readonly;  
> сообщение: CREATE ROLE  

9. дайте новой роли право на подключение к базе данных testdb:
> grant connect on DATABASE testdb TO readonly;   
> сообщение: GRANT   

10. дайте новой роли право на использование схемы testnm:   
> grant usage on SCHEMA testnm to readonly;   
> сообщение: GRANT   

11. дайте новой роли право на select для всех таблиц схемы testnm:    
> grant SELECT on all TABLEs in SCHEMA testnm TO readonly;
> сообщение: GRANT      

12. создайте пользователя testread с паролем test123:     
> CREATE USER testread with password 'test123';
> сообщение: CREATE ROLE    

13. дайте роль readonly пользователю testread:     
> grant readonly TO testread;
> сообщение: CREATE ROLE 

14. зайдите под пользователем testread в базу данных testdb:     
> \c testdb testread    
> сообщение: connection to server on socket "/var/run/postgresql/.s.PGSQL.5432" failed: FATAL:  Peer authentication failed for user
> "testread"        
> Previous connection kept     
> вышел из консоли \q     
> зашел в pg_hba.conf: nano /etc/postgresql/15/main/pg_hba.conf    
> поменял Peer на scram-sha-256 для локального пользователя.    
> сохранился. Вышел. Рестартовал кластер: pg_ctlcluster 15 main restart.    
> выдал ошибку и предложил воспользоваться другой командой: Error: cluster is running from systemd, can only restart it as root. Try
> instead: sudo systemctl restart postgresql@15-main     
> воспользовался: sudo systemctl restart postgresql@15-main     
> снова зашел в БД testdb под пользователем "postgres"    
> на этот раз коменда отработала корректно     
> сообщение: You are now connected to database "testdb" as user "testread".      

15. сделайте select * from t1;        
> SELECT * FROM t1;
> сообщение: ERROR:  permission denied for table t1 (нет полномичий на эту команду для таблицы t1)

16. получилось? (могло если вы делали сами не по шпаргалке и не упустили один существенный момент про который позже):
> не получилось.

17. напишите что именно произошло в тексте домашнего задания:     
> 

18. у вас есть идеи почему? ведь права то дали?    
>

19. посмотрите на список таблиц:
>

20. подсказка в шпаргалке под пунктом 20:
>

21. а почему так получилось с таблицей (если делали сами и без шпаргалки то может у вас все нормально):    
>

22. вернитесь в базу данных testdb под пользователем postgres:   
>

23. удалите таблицу t1:
>

24. создайте ее заново но уже с явным указанием имени схемы testnm:  
>

25. вставьте строку со значением c1=1:
>

26. зайдите под пользователем testread в базу данных testdb:
>

27. сделайте select * from testnm.t1;
>

28. получилось?
>

29. есть идеи почему? если нет - смотрите шпаргалку:
>

30. как сделать так чтобы такое больше не повторялось? если нет идей - смотрите шпаргалку:
>

31. сделайте select * from testnm.t1:
>

32. получилось?
>

33. есть идеи почему? если нет - смотрите шпаргалку:
>

31. сделайте select * from testnm.t1;
>

32. получилось?
>

33. ура!
>

34. теперь попробуйте выполнить команду create table t2(c1 integer); insert into t2 values (2); 
>

35. а как так? нам же никто прав на создание таблиц и insert в них под ролью readonly?
>

36. есть идеи как убрать эти права? если нет - смотрите шпаргалку:
>

37. если вы справились сами то расскажите что сделали и почему, если смотрели шпаргалку - объясните что сделали и почему выполнив указанные в ней команды:
>

38. теперь попробуйте выполнить команду create table t3(c1 integer); insert into t2 values (2);
>

39. расскажите что получилось и почему:
>





