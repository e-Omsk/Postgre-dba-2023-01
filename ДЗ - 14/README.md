GCE
Проект: postgres197605.

# Базовое задание 

На 1 ВМ создаем таблицы test для записи, test2 для запросов на чтение. Создаем публикацию таблицы test и подписываемся на публикацию таблицы test2 с ВМ №2. На 2 ВМ создаем таблицы test2 для записи, test для запросов на чтение. Создаем публикацию таблицы test2 и подписываемся на публикацию таблицы test1 с ВМ №1:

> Создал четыре ВМ на ресурсе от Google cloud. Установил postgresql 14 версии.
> На первой ВМ создал БД Otus. Создал таблицу test для записи и test2 для чтения.
> Смотрю уровень реликации: show wal_level; Устанавливаю: ALTER SYSTEM SET wal_level = logical, если не logical.
> Выходим. Перегружаю кластер: sudo pg_ctlcluster 14 main restart
> Опять подключаемся к клиенту БД otus: sudo -u postgres psql -p 5432 -d otus
> Создаем публикацию: CREATE PUBLICATION test_pub FOR TABLE test;
> Задаем пароль: \password 
> 
