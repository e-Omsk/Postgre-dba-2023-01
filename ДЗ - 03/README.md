GCE   
Проект: postgres197605.  
VM postgresql-docker 
БД otus

# Цель:
установить PostgreSQL в Docker контейнере
настроить контейнер для внешнего подключения

# Базовое задание 

• Создать ВМ с Ubuntu 20.04/22.04 или развернуть докер любым удобным способом
> Создал ВМ на Google cloud и установил Ubuntu 22.04 через web интерфейс.   

• Поставить на нем Docker Engine
> curl -fsSL https://get.docker.com -o get-docker.sh && sudo sh get-docker.sh && rm get-docker.sh && sudo usermod -aG docker $USER   
>  создаем внутреннюю  сеть для контейнера:    
> sudo docker network create pg-net  

• Сделаем каталог /var/lib/postgres и    
• развернем контейнер с PostgreSQL 15 смонтировав в него /var/lib/postgresql:   
> (подключаем созданную сеть к контейнеру сервера Postgres)           
> sudo docker run --name pg-server --network pg-net -e POSTGRES_PASSWORD=123 -d -p 5432:5432 -v     
> /var/lib/postgres:/var/lib/postgresql/data postgres:15        
> Постгрис запущен.

• Развернем контейнер с клиентом postgres,    
• подключимся из контейнера с клиентом к контейнеру с сервером:
> sudo docker run -it --rm --network pg-net --name pg-client postgres:15 psql -h pg-server -U postgres    
> получается запущен один контейнер с постгрисом и контейнер с клиентом.  

• Создадим БД otus и  таблицу с парой строк:  
> create database otus;   
> create table test  as select  generate_series(1,10) as id;

• Подключимся к контейнеру с сервером с ноутбука/компьютера извне инстансов GCP/ЯО/места установки докера:   
> 

• Останвливаем контейнер с сервером:   
> sudo docker stop 8b218c9f3f22

• Удаляем контейнер с сервером:  
> sudo docker rm 8b218c9f3f22

• Создаем его заново:
> udo docker run --name pg-server --network pg-net -e POSTGRES_PASSWORD=postgres -d -p 5432:5432 -v   /var/lib/postgres:/var/lib/postgresql/data postgres:15

• Подключился снова из контейнера с клиентом к контейнеру с сервером:   
> sudo docker run -it --rm --network pg-net --name pg-client postgres:15 psql -h pg-server -U postgres

• Проверил, что данные остались на месте:    
> \l    
> \c otus     
> select * from test;    


