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
> Cоздадим подписку к БД по Порту с Юзером и Паролем и Копированием данных=true  
> CREATE SUBSCRIPTION test_sub   
> CONNECTION 'host=10.128.0.8 port=5432 user=postgres password=qwerty dbname=otus'   
> PUBLICATION test_pub WITH (copy_data = true);    
> требует прописать pg_hba.conf entry for host "10.128.0.12", user "postgres", database "otus", SSL encryption connection to server a.....   
> Прописываем в pg_hba.conf (host    otus            postgres        10.128.0.12/32          scram-sha-256) на первой ВМ  
> Повторяем команду. CREATE SUBSCRIPTION УРА! Подписка создана по таблице test.   
> Проверяем на всякий случай: \dRs . Представления: SELECT * FROM pg_stat_subscription \gx .   
> Вносим значение в test первой ВМ и смотрим изменения в test второй ВМ.    
>        
> C таблицей test2 аналогично: 
> На второй ВМ:  
> ALTER SYSTEM SET wal_level = logical;   
> CREATE PUBLICATION test2_pub FOR TABLE test2;   
> Прописываем в pg_hba.conf (host    otus            postgres        10.128.0.8/32          scram-sha-256)
> Перегружаю кластер: sudo pg_ctlcluster 14 main restart   
> Задаем пароль: \password .   
> Как оказалось надо было прописать (не слушал): listen_addresses = 'localhost, 10.128.0.12' в /etc/postgresql/14/main/postgresql.conf.   
> На первой ВМ:  
> CREATE SUBSCRIPTION test2_sub   
> CONNECTION 'host=10.128.0.12 port=5432 user=postgres password=qwerty1 dbname=otus'   
> PUBLICATION test2_pub WITH (copy_data = true);  
> Проверяем: \dRs . Представления: SELECT * FROM pg_stat_subscription \gx . Так же select-ом. Все в порядке.  


Третья ВМ использовать как реплику для чтения и бэкапов (подписаться на таблицы из ВМ №1 и №2 ). Небольшое описание, того, что получилось.

> Создал БД otus.  
> Создал таблички test и test2  
> Прописываем в pg_hba.conf (host    otus            postgres        10.128.0.13/32          scram-sha-256) на обеих ВМ чтобы пускало.  
> При создании подписки выругалась: "could not create replication slot "test2_sub": ERROR:  replication slot "test2_sub" already exists".  
> Пришлось создать новую публикацию: CREATE PUBLICATION test3_pub FOR TABLE test2; для второй ВМ.  
> И подписаться на третьей ВМ:   
> CREATE SUBSCRIPTION test3_sub     
> CONNECTION 'host=10.128.0.12 port=5432 user=postgres password=qwerty1 dbname=otus'   
> PUBLICATION test3_pub WITH (copy_data = true);  
> То же самое с таблицей test c первой ВМ.

Реализовать горячее реплицирование для высокой доступности на 4ВМ. Источником должна выступать ВМ №3. Написать с какими проблемами столкнулись.

> На третьей ВМ:  
> Раскамитил synchronous_commit = on в postgresql.conf.  
> Под аккаунтом postgres необходимо создать пользователя для репликации: CREATE ROLE rep_user  WITH REPLICATION PASSWORD '12345' LOGIN; .  
> Смотрю уровень реликации: show wal_level; Устанавливаю: ALTER SYSTEM SET wal_level =  replica, если не  replica.  
> Добовляем в файл pg_hba.conf, имя пользователя для репликаций и IP адрес сервера:  
> host    replication     rep_user        10.128.0.13/32           scram-sha-256  
> Перезапускаем Postgresql: sudo pg_ctlcluster 14 main restart.  
> На четвертой ВМ: 
> Разремарил: hot_standby_feedback = off, max_standby_streaming_delay = 30s в postgresql.conf. 
> Останавливаем кластер.
> В режиме подчиненного сервера у нас все данные реплицируются с основного, нам необходимо удалить файлы из каталога main:  
> sudo rm -rf /var/lib/postgresql/14/main/*    
> Делаем бэкап с опцией восстановления(-R) c третьего хоста:  
> sudo -u postgres pg_basebackup -h 10.128.0.13 -p 5432 -U rep_user -R -D /var/lib/postgresql/14/main
> Запускаем кластер.  
> Проверяем. Горячее реплецирование работает.
