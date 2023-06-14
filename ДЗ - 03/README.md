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
•  создаем внутреннюю  сеть для контейнера:    
> sudo docker network create pg-net   
> подключаем созданную сеть к контейнеру сервера Postgres    
•  (при этом создаестя каталог /var/lib/postgres и    
•      разворачиваем контейнер с PostgreSQL 15 смонтировав в него /var/lib/postgresql):        
> sudo docker run --name pg-server --network pg-net -e POSTGRES_PASSWORD=123 -d -p 5432:5432 -v     
> /var/lib/postgres:/var/lib/postgresql/data postgres:15     
> 
> 
> 
