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
> воспользовался утилитой pgtune для генерации параметров под мою ВМ:

> исходные данные:
>  DB Version: 15    
> OS Type: linux    
> DB Type: web    
> Total Memory (RAM): 2 GB    
> CPUs num: 2    
> Connections num: 20    
> Data Storage: ssd 
   
> утилита выдала следующие значения для настройки:     
> max_connections = 20; было 100; поставил минимальное число, т.к малое количество ядер CPU;    
> shared_buffers = 512MB; было 128Mb;  
> effective_cache_size = 1536MB; было 4Gb;   
> maintenance_work_mem = 128MB; было 64Mb;   
> checkpoint_completion_target = 0.9; было 0.9;   
> wal_buffers = 16MB; было -1;    
> default_statistics_target = 100; было то же значение        
> random_page_cost = 1.1; было 4.0;   
> effective_io_concurrency = 200; было 1;    
> work_mem = 13107kB; было 4Мб.      
> min_wal_size = 1GB; было 80Мб;    
> max_wal_size = 4GB; было 1Гб;       

4. нагрузить кластер через утилиту через утилиту pgbench (https://postgrespro.ru/docs/postgrespro/14/pgbench):
> создал и наполнил четыре таблицы () стандартным образом: /usr/bin$ sudo -u postgres pgbench -i postgres   
> применил утилиту pgbench для теста производительности со стандартными значениями по умолчанию при установке кластера и значениями от pgtune:   
> sudo -u postgres pgbench -c 50 -j 2 -P 10 -T 60  postgres
> где -с 50 число имитируемых клиентов (одновременных сеансов БД)
> -j 2 число рабочих потоков
> -P 10 выводит отчет о прогрессе через 10 секунд
> -T 60 число транзакций, которые будут выполняться каждым клиентом
> postgres тестируемая БД  

5. написать какого значения tps удалось достичь, показать какие параметры в
какие значения устанавливали и почему:    
> при стандартных значениях tps = 729.130982
> при значениях от pgtune 


