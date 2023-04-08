GCE
Проект: postgres197605.

# Базовое задание 

На 1 ВМ создаем таблицы test для записи, test2 для запросов на чтение. Создаем публикацию таблицы test и подписываемся на публикацию таблицы test2 с ВМ №2. На 2 ВМ создаем таблицы test2 для записи, test для запросов на чтение. Создаем публикацию таблицы test2 и подписываемся на публикацию таблицы test1 с ВМ №1:

> Создал четыре ВМ на ресурсе от Google cloud. Установил postgresql 14 версии.  
> На первой ВМ создал БД Otus. Создал таблицу test с данными для записи и test2 пустая для чтения.  
> Смотрю уровень реликации: show wal_level; Устанавливаю: ALTER SYSTEM SET wal_level = logical, если не logical.  
> Выходим. Перегружаю кластер: sudo pg_ctlcluster 14 main restart  
> Опять подключаемся к клиенту БД otus: sudo -u postgres psql -p 5432 -d otus  
> Создаем публикацию: CREATE PUBLICATION test_pub FOR TABLE test;  
> Задаем пароль: \password   
> На второй ВМ создаю в БД otus таблицу test2 с данными для записи и test пустая для чтения.  
> создадим подписку к БД по Порту с Юзером и Паролем и Копированием данных=true  
> CREATE SUBSCRIPTION test_sub   
> CONNECTION 'host=10.128.0.8 port=5432 user=postgres password=qwerty dbname=otus'   
> PUBLICATION test_pub WITH (copy_data = true);    
> требует прописать pg_hba.conf entry for host "10.128.0.12", user "postgres", database "otus", SSL encryption connection to server a.....   
> Прописываем в pg_hba.conf (host    otus            postgres        10.128.0.12/32          scram-sha-256) на первой ВМ  
> Повторяем команду. CREATE SUBSCRIPTION УРА! Подписка создана по таблице test.   
>   
>   
