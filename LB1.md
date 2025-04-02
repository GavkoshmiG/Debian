# 1.	Подготовка среды Установить на виртуальной машине Debian 11/12 (VMware\VirtualBox\HyperV\WSL). Убедиться, что система обновлена (команда apt-getupdate && apt-get upgrade – обязательно понимать разницу команд).

apt-get update – уведомляет о доступных пакетах. Проверяет репозитории на наличие новых версий, но не устанавливает их.

apt-get upgrade - устанавливает обновления для уже установленных пакетов.

apt install – для установки новых пакетов

apt – пакетный менеджер для управления пакетами

![image](https://github.com/user-attachments/assets/efc31d56-9239-4846-bcd3-62b862098e96)

![image](https://github.com/user-attachments/assets/ee184b25-1ffc-4750-86f9-0d5ce58c959c)

![image](https://github.com/user-attachments/assets/a225b666-3352-4fd9-91d0-c21b8cb630c8)

# 2. Установка PostgreSQL С помощью пакетного менеджера apt установить PostgreSQL (последнюю доступную версию из репозиториев). Привести команды установки и их объяснение.

Для установки PostgreSQL использовалась команда:

      sudo apt install postgresql

sudo: Выполнение команд, требующих прав root

![image](https://github.com/user-attachments/assets/83ae032a-15e7-4906-891c-8a934e84ef97)

После установки произведена проверка установки PostgreSQL при помощи команды:

      sudo systemctl status postgresql

![image](https://github.com/user-attachments/assets/f2451af0-07b8-443d-983b-247571fd314e)

# 3.	Создание служебной учётной записи. Проверить, что при установке создана учётная запись postgres. Объяснить её назначение и права в системе.

Переключились на пользователя postgres командой:

      sudo -i -u postgres

Проверили информацию о postgres пользователе при помощи команды:

      id postgres

Был запущен терминал PostgreSQL при помощи команды:

      psql

Назначение пользователя postgres:

  + Пользователь postgres создаётся автоматически при установке PostgreSQL. Он является владельцем всех файлов, связанных с PostgreSQL.
    
  + Пользователь postgres используется для запуска и остановки сервера PostgreSQL.
    
  + Все процессы PostgreSQL (например, postmaster) запускаются от имени этого пользователя.
    
  + Пользователь postgres имеет полный доступ ко всем базам данных, таблицам и другим объектам в PostgreSQL.
    
  + Он может создавать, изменять и удалять базы данных, пользователей и роли.


Права пользователя postgres:

  + Пользователь postgres является владельцем каталога данных PostgreSQL (например, /var/lib/postgresql).
    
  + Он имеет права на чтение, запись и выполнение в этом каталоге и его подкаталогах.
    
  + Внутри PostgreSQL пользователь postgres имеет роль суперпользователя (superuser).
    
  + Пользователь postgres обычно не имеет прав суперпользователя (root) в системе. Это сделано для безопасности. Однако он может выполнять команды, связанные с управлением PostgreSQL, такие как запуск и остановка сервера.


![image](https://github.com/user-attachments/assets/c11ae0fc-e0e8-4a55-b1a7-ddb35d39dc06)

# 4.	Первичная настройка конфигурационных файлов. Найти и изучить основные файлы конфигурации PostgreSQL (например, postgresql.conf, pg_hba.conf). Внести изменения (например, задать порт, при необходимости изменить метод аутентификации) и перезапустить сервис PostgreSQL.

Открываем файл postgresql.conf при помощи команды:

      sudo nano /etc/postgresql/15/main/postgresql.conf

Далее в файле раскомментируем и строку с listen_adress и установим туда "*", дабы иметь возможность подключения с различных адресов.

После всех изменений необходимо перезапустить postgresql при помощи команды:

      sudo systemctl restart postgresql

![image](https://github.com/user-attachments/assets/b59ee7ef-9d78-4dd0-a440-1b400eb0b2a8)

![image](https://github.com/user-attachments/assets/d414b1e7-1798-463d-9ee0-1cc6d583e836)

![image](https://github.com/user-attachments/assets/4dfa1810-aaf5-4d17-84ba-820ea13c1371)

# 5.	Управление сервисом Использовать инструменты управления сервисами Debian (systemd) — проверить статус сервиса PostgreSQL, включить автозапуск.

Статус сервиса PostgreSQL был получен при помощи команды:

      sudo systemctl status postgresql

Для включения автозапуска была использована команда:

      sudo systemctl enable postgresql 
*enable - подкоманда systemctl, которая включает автозапуск сервиса при загрузке системы

![image](https://github.com/user-attachments/assets/97551507-4c0a-4f45-b530-21ebe515d953)

# 6.	Создание тестовой базы данных Создать отдельного пользователя (логин задать в формате ФИО, например, alpopov) в PostgreSQL, новую базу данных (нейминг аналогичен dbalpopov), задать ему пароль. Использовать psql, чтобы проверить.

Для перехода в терминал пользователя postgres использована команда:

      sudo -u postgres psql

Для создания нового пользователя использовалась запись:

      CREATE USER ivkonshin WITH PASSWORD ‘ivkonshin’;

Для создания базы данных для пользователя ivkonshin использовали запись:

      CREATE DATABASE aastarikova OWNER ivkonshin;

Для просмотра списка пользователей в PostgreSQL использовалась команда:

      \du

Для посмотра списка баз данных на сервере PostgreSQL использовалась команда:

      \l

![image](https://github.com/user-attachments/assets/d8c724bb-d235-4383-8d26-e1660fbd238a)

![image](https://github.com/user-attachments/assets/b8c1950f-de9c-469c-927e-453237f0002f)

Для того чтобы любой пользователь PostgreSQL мог зайти по паролю был поменян параметр peer (параметр, говорящий о том, что подключаться к PostgreSQL могут только пользователи, что есть в системе) на md5.

![image](https://github.com/user-attachments/assets/edbe6d00-e41f-4fa1-a3a4-5c29a0675711)

Для подключения к базе от лица пользователя ivkonshin была использована команда:

      psql -U ivkonshin -d aastarikova -W

Для проверки текущего юзера, под которым был осуществлен вход, использована команда:

      SELECT current_user;

![image](https://github.com/user-attachments/assets/2d933d2c-344b-4e6f-ac1e-f451f91117bb)

# 7.	Знакомство со схемами. Объяснить, что такое схема в PostgreSQL, в чём разница между базой данных и схемой. По умолчанию существует схема public. Создайте ещё одну схему, например, test_schema. Настройте у пользователя, созданного выше, права на использование этой схемы (команда GRANT). Продемонстрируйте, как работать с разными схемами (смена search_path или обращение к объектам через test_schema.table_name).

База данных — это основное хранилище, в котором могут находиться схемы. 
В ней хранятся:
1. Схемы (как папки для структурирования данных).
2. Пользователи и их права доступа.
3. Журналы транзакций.
4. Настройки базы данных.
   
Схема — это область внутри базы данных, в которой хранятся объекты, в том числе и таблицы, в которых хранится информация.

Для создания новой схемы используется команда:

      CREATE SCHEMA test_schema;

Для предоставления прав на использование объектов схемы используется команда:

        GRANT USAGE ON SCHEMA test_schema TO ivkonshin;

Для предоставления прав на создание объектов в схеме пользователем используется:

      GRANT CREATE ON SCHEMA test_schema TO ivkonshin;

Для упрощения дальнейшей работы используем команду:

        SET search_path TO test_schema, public;
*позволяет указать, в каких схемах PostgreSQL будет искать объекты, если схема не указана явно. search_path — это список схем, которые PostgreSQL проверяет при поиске объектов. По умолчанию search_path включает схему public, а также схему, соответствующую имени текущего пользователя. Если объект не указан с явным именем схемы (например, schema_name.table_name), PostgreSQL ищет его в схемах, перечисленных в search_path, в порядке их указания.

![image](https://github.com/user-attachments/assets/c40c6a64-ef3e-414f-8193-45f60fdd0c60)

Для проверки прав пользователей использована команда:

      \dn+

После ее использования видно, что пользователь ivkonshin в схеме test_schema может взаимодействовать с объектами, схемами, а также может создавать объекты в схеме.

![image](https://github.com/user-attachments/assets/16713718-2e14-4011-90d5-c5311426f73c)

Создана табилица при помощи команды:

       CREATE TABLE test_table (id SERIAL PRIMARY KEY, name VARCHAR(50));

Получилось проверить содержимое схемы при помощи команды:

      \dt test_schema.*

![image](https://github.com/user-attachments/assets/5e844490-f73d-4ab1-b5e8-36c3e3e53cdd)

# 8.	Использование утилиты psql для базовых операций. В схеме public создать тестовую таблицу, внести несколько записей, выполнить основные SQL-запросы (SELECT, INSERT, UPDATE, DELETE). В схеме из 7 задания test_schema создать другую таблицу и внести несколько записей. Покажите, как к ней обращаться. Использовать скрипт, который демонстрирует создание таблицы с привязкой к конкретной схеме.

Создана табилица при помощи команды:

      CREATE TABLE public.test (id SERIAL PRIMARY KEY, name VARCHAR(50), age INT, salary FLOAT);

![image](https://github.com/user-attachments/assets/9394ecbe-ba1b-49f0-878e-b05ddc42bcb0)

Получены все поля с типами данных, которые они ожидают у определенной таблицы при помощи команды:

      \d public.test

![image](https://github.com/user-attachments/assets/a34ba280-ed28-45f6-8242-4b54f2f1ad52)

Данные в таблицу были добавлены командой:

      INSERT INTO public.test (id, name, age, salary) VALUES ('1', 'Ivan', 21, 777777);

Для получения всех столбцов таблицы использовалась команда:

      SELECT * FROM public.test;

![image](https://github.com/user-attachments/assets/b4de6d33-f455-42dd-90f1-5348cb228af7)

Для получения конкретных столбцов таблицы использовалась команда:

      SELECT id, name, salary FROM public.test;

![image](https://github.com/user-attachments/assets/691362cc-5b64-4e08-a774-f438fed40ce3)

Изменить поля в существующей таблице можно при помощи команды:

      UPDATE public.test SET salary=999999 WHERE name='Ivan';

![image](https://github.com/user-attachments/assets/73d94b0b-8774-49c6-ac04-efc4889906c6)

![image](https://github.com/user-attachments/assets/48568f56-3036-4a3c-a4d9-660919c7a733)

Удалить запись из таблицы можно при помощи команды:

      DELETE FROM public.test WHERE name='Vasya';

![image](https://github.com/user-attachments/assets/ed4f2783-6754-4c5a-83c1-99f700d926e5)

![image](https://github.com/user-attachments/assets/428f07fb-c9d1-4d8f-8d93-9f219b63a150)

![image](https://github.com/user-attachments/assets/06e6b11c-63df-49d4-b861-500d738048e9)

![image](https://github.com/user-attachments/assets/5c4ce0e8-4eb6-4154-ad8b-ab3929ff6f58)

# 9. Настройка локальных и сетевых подключений Настроить доступ к базе данных по локальной сети (например, разрешить подключаться к PostgreSQL с другого хоста). Объяснить параметры listen_addresses и формат строки в pg_hba.conf. Произвести подключение через pgadmin или dbeaver с локальной машины.

Изменили файл конфигурации postgresql.conf, параметр listen_address.

Проверили ip машины, к которой будет осуществлено подключение при помощи команды:

      ip a

![image](https://github.com/user-attachments/assets/1a3e1c7b-851a-4b15-84f8-115e6088101b)

Было разрешено подключение с любых хостов, для этого добавили строку:

      host          all            0.0.0.0/0            md5

host — указывает, что подключение происходит через TCP/IP.

all — означает, что разрешены подключения ко всем базам данных.

all — разрешение для всех пользователей.

0.0.0.0/0 позволяет подключаться к серверу со всех адресов.

![image](https://github.com/user-attachments/assets/40bcc152-7de0-4bb3-a52e-c4f7b9ec2701)

В настройках сети для виртуальной машины был изменен NAT на Bridged Adapter

В pgAdmin создан сервер Debian (при созданиие указали ip адрес, полученный ранее при помощи команды ip a), к которому и было произведено подключение.

![image](https://github.com/user-attachments/assets/39e08fb0-e9f8-441c-903f-ca8f09c606a4)

# 10. Журналирование (logging). Изменить настройки журналирования в postgresql.conf, перезапустить сервис и проверить, как новые логи записываются. Найти логи в Debian и вывести примеры строк лога.

Внесены изменения в файл postgresql.conf 

Если этот параметр выключен (off), логи могут записываться в syslog или journald (в зависимости от системы), но не в отдельный файл, изменено на on:

      logging_collector = on

Нужно для того чтобы PostgreSQL записывал логи в файлы, а не только в системный журнал:

      log_directory = 'log'

Определяет каталог, куда PostgreSQL будет сохранять файлы логов. По умолчанию log, но можно изменить, например:

      log_filename = 'postgresql.log'

![image](https://github.com/user-attachments/assets/2a6a9495-4750-4559-b52c-cad1e66898d4)

Определяет имя файла лога:

      log_statement = 'all'

Определяет, какие SQL-запросы логировать. Возможные значения:

none — не записывать SQL-запросы.
ddl — записывать только изменения структуры БД (CREATE, ALTER, DROP).
mod — записывать изменения данных (INSERT, UPDATE, DELETE, TRUNCATE).
all — записывать все запросы.

![image](https://github.com/user-attachments/assets/f635f49b-be3b-4398-9be0-a251c3387382)

Определяет минимальный уровень сообщений, которые будут записываться в лог. Возможные значения:

      log_min_messages = info

debug5, debug4, debug3, debug2, debug1 — детальное отладочное логирование.
info — информационные сообщения.
notice — важные, но не критические уведомления.
warning — предупреждения.
error — ошибки, но сервер продолжает работать.
fatal — критические ошибки, приводящие к завершению соединения.
panic — серверная ошибка, требующая перезапуска.

Определяет минимальный уровень ошибок, при котором в лог будет записываться сам SQL-запрос, вызвавший ошибку. Нужно чтобы видеть не только ошибки, но и какие запросы их вызвали:

      log_min_error_statement = error

![image](https://github.com/user-attachments/assets/d7aebc80-0d3a-4a9c-9cfd-530f677e8d5a)

Логирует все новые подключения к базе данных. Позволяет отслеживать, кто и когда подключался:

      log_connections = on

Логирует отключения пользователей от базы данных. Позволяет видеть, как долго длилась сессия:

      log_disconnections = on

![image](https://github.com/user-attachments/assets/0cfc9672-b04b-4760-afb5-8521fcacf198)

Пример логирования:

![image](https://github.com/user-attachments/assets/8099886b-9850-4634-8a05-b5b6b47927df)

# 11. Назначение ролей и прав Создать роль с ограниченными привилегиями и протестировать, какие операции она может выполнять (роль назвать limited_user). Продемонстрировать, как в PostgreSQL наследуются права между ролями. Объяснить выдачу прав через GRANT.

Для создания пользователя использована команда:

      CREATE ROLE test_user WITH LOGIN PASSWORD 'test_user';

![image](https://github.com/user-attachments/assets/803e09eb-d142-4239-afa4-f5797270f58d)

Для добавления прав пользователя использована команда:

        GRANT SELECT ON public.test TO test_user;

Возможные права:

SELECT — право на чтение данных (выполнение запросов SELECT).

INSERT — право на вставку новых строк (выполнение запросов INSERT).

UPDATE — право на обновление существующих строк (выполнение запросов UPDATE).

DELETE — право на удаление строк (выполнение запросов DELETE).

TRUNCATE — право на очистку таблицы (выполнение команды TRUNCATE).

REFERENCES — право на создание внешних ключей (FOREIGN KEY), ссылающихся на эту таблицу.

ALL PRIVILEGES — все доступные права на таблицу.

![image](https://github.com/user-attachments/assets/aeef922c-d36a-4e59-8495-ae99d9e918ec)

Из примеров видно, что некоторые действия ограничены для пользователя.

![image](https://github.com/user-attachments/assets/e63debfa-34f4-4fbc-85bc-cefadac6685e)

Создан еще один юзер, а далее наследуем все права старой роли при помощи команды:

      GRANT test_user_2 TO test_user;

![image](https://github.com/user-attachments/assets/1b1ccd36-a669-4549-980c-2d969c2bcf7d)

