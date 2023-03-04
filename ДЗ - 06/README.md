GCE
Проект: postgres197605.

# Базовое задание 

Запустить кластер после переноса постгриса на примонтированный диск (udo -u postgres pg_ctlcluster 14 main start):

> Кластер не запустился. Ошибка: postgres is not in the sudoers file.  
> This incident will be reported. Т.е. пострига нет в файле sudoers.

 Найти конфигурационный параметр в файлах раположенных в /etc/postgresql/14/main который надо поменять и поменяйте его:
> Редактируем конфигурационный файл  sudo nano /etc/postgresql/14/main/postgresql.conf.
> Меняем значение data_directory = '/mnt/data/14/main'. Ставим путь, куда перенесли постгрис.
> Запускаем кластер sudo -u postgres pg_ctlcluster 14 main start. Запускается.
> Проверяем: pg_lsclusters. Горит зеленым.
> Заходим в sql и проверяем наличие данных в таблице test командой SELECT * FROM test;
> Данные на месте.


# Задание со * звездочкой:

#в первой сессии добавить новую запись insert into persons(first_name, second_name) values('sveta', 'svetova');
сделать select * from persons во второй сессии
видите ли вы новую запись и если да то почему?
#Новую запись не вижую.
