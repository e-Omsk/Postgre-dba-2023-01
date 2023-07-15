GCE   
Проект: postgres197605.  
VM postgres-dz-12

# Цель:
сделать нагрузочное тестирование PostgreSQL    
настроить параметры PostgreSQL для достижения максимальной производительности   

# Базовое задание 

1. развернуть виртуальную машину любым удобным способом:    
> Создал ВМ на Google cloud и установил Ubuntu 22.04 через web интерфейс. Параметры: 2 vCPU + 2 GB memory, диск 10 GB SSD persistent disk.  

2. поставить на неё PostgreSQL 15 любым способом:   
> через ssh в командной строке: sudo apt update && sudo apt upgrade -y -q && sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list' && wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add - && sudo apt-get update && sudo apt -y install postgresql-15

3. настроить кластер PostgreSQL 15 на максимальную производительность не обращая внимание на возможные проблемы с надежностью в случае аварийной перезагрузки виртуальной машины:     
> воспользовался утилитой pgtune для генерации параметров под мою ВМ.
