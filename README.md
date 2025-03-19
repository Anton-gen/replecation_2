# Задание 1. Резервное копирование

Кейс

Финансовая компания решила увеличить надёжность работы баз данных и их резервного копирования.

Необходимо описать, какие варианты резервного копирования подходят в случаях:

1.1. Необходимо восстанавливать данные в полном объёме за предыдущий день.

1.2. Необходимо восстанавливать данные за час до предполагаемой поломки.

1.3.* Возможен ли кейс, когда при поломке базы происходило моментальное переключение на работающую или починенную базу данных.

Приведите ответ в свободной форме.

## ОТВЕТ:

1.1. Восстановление данных в полном объеме за предыдущий день:

Полное резервное копирование (Full Backup):  Это самый простой и надежный метод. Каждый день создается полная копия всей базы данных.  При восстановлении, вы просто восстанавливаете последнюю полную копию.

## Плюсы: Простота восстановления, охватывает все данные.

## Минусы: Требует много места для хранения резервных копий, длительное время выполнения резервного копирования.

Инкрементное резервное копирование (Incremental Backup) + Полное резервное копирование: Раз в неделю выполняется полная резервная копия, а каждый день создается инкрементная копия, содержащая только изменения, сделанные с момента последнего полного или инкрементного резервного копирования.

Плюсы: Быстрое ежедневное резервное копирование, занимает меньше места, чем полное резервное копирование каждый день.

Минусы: Восстановление занимает больше времени, так как нужно восстанавливать полную копию и все инкрементные копии, сделанные после нее. Более сложный процесс восстановления.

Дифференциальное резервное копирование (Differential Backup) + Полное резервное копирование:  Раз в неделю выполняется полная резервная копия, а каждый день создается дифференциальная копия, содержащая все изменения, сделанные с момента последнего полного резервного копирования.

## Плюсы: Более быстрое восстановление, чем с инкрементным резервным копированием.

## Минусы:  Занимает больше места, чем инкрементное, и ежедневное резервное копирование занимает больше времени, чем инкрементное.

Рекомендация для 1.1:  Для простоты и надежности, если позволяет время и место, лучше всего использовать ежедневное полное резервное копирование. Если время и место ограничены, используйте дифференциальное резервное копирование с еженедельным полным резервным копированием.

1.2. Восстановление данных за час до предполагаемой поломки:

Резервное копирование журналов транзакций (Transaction Log Backup): Этот метод позволяет восстановить базу данных до определенного момента времени. Журналы транзакций содержат запись обо всех изменениях, внесенных в базу данных. Регулярно (например, каждые 15 минут или час) сохраняйте журналы транзакций. Для восстановления базы данных на момент времени, нужно восстановить полную резервную копию, а затем применить все журналы транзакций, созданные после этой резервной копии, до нужного момента времени.

## Плюсы:  Высокая точность восстановления, минимальная потеря данных (возможность восстановления до последней транзакции перед сбоем).

## Минусы:  Более сложная настройка и процесс восстановления. Требует больше места для хранения журналов.

Комбинация полного/дифференциального/инкрементного резервного копирования с резервным копированием журналов транзакций:  Это наиболее надежный подход.  Например, вы можете делать полное резервное копирование раз в день, дифференциальное - несколько раз в день, и копировать журналы транзакций каждые 15-30 минут.

Рекомендация для 1.2:  Обязательно используйте резервное копирование журналов транзакций. В сочетании с полным/дифференциальным/инкрементным резервным копированием это обеспечит наилучшую защиту от потери данных.

1.3. Моментальное переключение на работающую или починенную базу данных (High Availability):

Да, это возможно и является стандартной практикой для критически важных систем. Для этого используются следующие технологии:

Кластеризация (Clustering):  Настройка нескольких серверов баз данных, работающих вместе.  В случае сбоя одного сервера, другой автоматически берет на себя его функции. Это обеспечивает минимальное время простоя.  Существуют различные типы кластеров, включая Active/Passive (один сервер активен, остальные в режиме ожидания) и Active/Active (все серверы обрабатывают запросы).

Репликация (Replication): Создание копий базы данных на нескольких серверах. Изменения, внесенные в основную базу данных (master), автоматически реплицируются на другие базы данных (slaves).  При сбое основного сервера, один из серверов-реплик может быть быстро переключен в режим master.  Существуют различные типы репликации: синхронная (гарантирует, что данные будут записаны на все реплики, прежде чем транзакция будет завершена) и асинхронная (данные реплицируются с некоторой задержкой).

Зеркалирование (Database Mirroring):  Схоже с репликацией, но обычно подразумевает более тесную интеграцию между серверами и более автоматизированный процесс переключения при сбое.

Always On Availability Groups (в SQL Server): это комплексное решение для обеспечения высокой доступности и аварийного восстановления. Always On позволяет создавать несколько реплик базы данных, которые могут находиться на разных серверах. В случае сбоя основного сервера, одна из реплик автоматически становится новой основной базой данных, что обеспечивает минимальное время простоя.

Рекомендация для 1.3:  Для финансовых компаний с критически важными базами данных, кластеризация или репликация являются обязательными.  Конкретный выбор зависит от бюджета, требуемого уровня доступности и сложности внедрения.  Важно учитывать, что синхронная репликация обеспечит наименьшую потерю данных, но может замедлить запись данных.  Асинхронная репликация быстрее, но может привести к некоторой потере данных в случае сбоя.

Общие рекомендации для всех сценариев:

Тестирование: Регулярно тестируйте процедуры восстановления из резервных копий, чтобы убедиться в их работоспособности и актуальности.

Мониторинг: Настройте мониторинг состояния баз данных и процессов резервного копирования, чтобы оперативно выявлять проблемы.

Автоматизация: Автоматизируйте процессы резервного копирования и восстановления, чтобы минимизировать человеческий фактор и ускорить время восстановления.

Размещение резервных копий: Храните резервные копии в нескольких местах, включая удаленные площадки, чтобы защититься от локальных катастроф.

Шифрование: Шифруйте резервные копии для защиты конфиденциальной информации.

Документация:  Подробно документируйте все процедуры резервного копирования и восстановления, включая инструкции, контактные данные и расписание.

# Задание 2. PostgreSQL

2.1. С помощью официальной документации приведите пример команды резервирования данных и восстановления БД (pgdump/pgrestore).

2.1.* Возможно ли автоматизировать этот процесс? Если да, то как?

Приведите ответ в свободной форме.

## ОТВЕТ:

Резервное копирование и восстановление PostgreSQL с помощью pg_dump/pg_restore

## 1. Резервное копирование (pg_dump):

Предположим, у вас есть база данных с именем mydatabase, и вы хотите создать её резервную копию в формате plain text (SQL script).


pg_dump -U myuser -h localhost -p 5432 -d mydatabase -f mydatabase_backup.sql

-U myuser:  Имя пользователя для подключения к базе данных.  Замените myuser на имя вашего пользователя PostgreSQL.

-h localhost:  Адрес хоста, на котором работает PostgreSQL (в данном случае, локальный компьютер).

-p 5432:  Порт, на котором работает PostgreSQL (по умолчанию 5432).

-d mydatabase:  Имя базы данных, которую нужно резервировать.

-f mydatabase_backup.sql:  Имя файла, в который будет сохранена резервная копия.

Другой распространенный формат резервной копии - custom format. Он более гибкий и позволяет делать параллельное восстановление:


pg_dump -U myuser -h localhost -p 5432 -d mydatabase -Fc -f mydatabase_backup.dump

-Fc: Указывает на создание резервной копии в custom format.

-f mydatabase_backup.dump: Имя файла, в который будет сохранена резервная копия.

Важно: Убедитесь, что пользователь, под которым вы запускаете pg_dump, имеет необходимые права для чтения всех объектов базы данных, которые вы хотите зарезервировать.

## 2. Восстановление (pg_restore):

Для восстановления из файла mydatabase_backup.sql (plain text):

psql -U myuser -h localhost -p 5432 -d mydatabase -f mydatabase_backup.sql

psql:  Инструмент командной строки для работы с PostgreSQL.

-U myuser:  Имя пользователя для подключения к базе данных.  Замените myuser на имя вашего пользователя PostgreSQL.  У этого пользователя должны быть права на создание и изменение объектов в базе данных.

-h localhost:  Адрес хоста, на котором работает PostgreSQL.

-p 5432:  Порт, на котором работает PostgreSQL.

-d mydatabase:  Имя базы данных, в которую нужно восстановить данные.  База данных должна существовать (если нет, её нужно создать).

-f mydatabase_backup.sql:  Имя файла с резервной копией.

Для восстановления из файла mydatabase_backup.dump (custom format):

pg_restore -U myuser -h localhost -p 5432 -d mydatabase mydatabase_backup.dump

pg_restore: Инструмент для восстановления резервных копий, созданных с помощью pg_dump в форматах, отличных от plain text.

-U myuser: Имя пользователя для подключения к базе данных.

-h localhost: Адрес хоста, на котором работает PostgreSQL.

-p 5432: Порт, на котором работает PostgreSQL.

-d mydatabase: Имя базы данных, в которую нужно восстановить данные.

mydatabase_backup.dump: Имя файла с резервной копией.

Важно:  Перед восстановлением рекомендуется убедиться, что база данных mydatabase существует и пуста (или вы готовы к перезаписи существующих данных).  Если база данных не существует, её нужно создать командой createdb mydatabase.  В некоторых случаях может потребоваться удаление существующих объектов в базе данных перед восстановлением (например, если схема базы данных изменилась).

## Автоматизация резервного копирования PostgreSQL

Да, автоматизировать процесс резервного копирования PostgreSQL очень желательно и возможно.  Это обеспечивает регулярность и уменьшает риск потери данных. Вот как это можно сделать:

## 1. Использование cron (Linux/Unix):

cron - это планировщик задач в Linux и других Unix-подобных системах.  Вы можете создать задание cron для автоматического запуска pg_dump через определенные интервалы (например, ежедневно или еженедельно).

Пример записи в crontab (открыть с помощью crontab -e):

0 2 * * * /usr/bin/pg_dump -U myuser -h localhost -p 5432 -d mydatabase -Fc -f /path/to/backup/mydatabase_$(date +\%Y\%m\%d).dump

0 2 * * *:  Это cron-выражение означает "запускать в 2:00 ночи каждый день".  Разберитесь с синтаксисом cron, чтобы настроить расписание под свои нужды.

/usr/bin/pg_dump:  Полный путь к исполняемому файлу pg_dump.  Убедитесь, что путь правильный (можно узнать с помощью which pg_dump).

-Fc:  Резервная копия в custom format (поддерживает параллельное восстановление).

/path/to/backup/mydatabase_$(date +\%Y\%m\%d).dump:  Путь к каталогу, где будут храниться резервные копии, и имя файла, включающее дату создания.  date +\%Y\%m\%d генерирует дату в формате YYYYMMDD, что позволяет создавать уникальные имена файлов для каждой резервной копии.

Важно:

Убедитесь, что у пользователя, от имени которого запускается cron (обычно это пользователь, от имени которого вы редактировали crontab), есть права на чтение базы данных и запись в каталог резервных копий.

Тестируйте cron-задание вручную, чтобы убедиться, что оно работает правильно.

Настройте ротацию резервных копий, чтобы старые резервные копии автоматически удалялись, чтобы не заполнять диск.  Это можно сделать с помощью утилиты find и rm в cron-задании.

Пример cron-задания с ротацией (хранить резервные копии за последние 7 дней):

0 2 * * * /usr/bin/pg_dump -U myuser -h localhost -p 5432 -d mydatabase -Fc -f /path/to/backup/mydatabase_$(date +\%Y\%m\%d).dump && find /path/to/backup/ -name "mydatabase_*" -mtime +7 -delete

&& find /path/to/backup/ -name "mydatabase_*" -mtime +7 -delete: Эта часть команды находит все файлы в каталоге /path/to/backup/, чьи имена начинаются с "mydatabase_" и которые были изменены более 7 дней назад (-mtime +7), и удаляет их (-delete).

## 2. Использование Task Scheduler (Windows):

В Windows для автоматизации задач используется Task Scheduler.  Вы можете создать задачу, которая будет запускать pg_dump.exe по расписанию.

Найдите Task Scheduler в меню "Пуск".

Создайте новую задачу (Create Basic Task).

Укажите имя и описание задачи.

Выберите триггер (расписание) для запуска задачи (например, ежедневно, еженедельно).

Выберите действие "Start a program".

В поле "Program/script" укажите путь к pg_dump.exe (например, C:\Program Files\PostgreSQL\15\bin\pg_dump.exe).

В поле "Add arguments" укажите параметры pg_dump (например, -U myuser -h localhost -p 5432 -d mydatabase -Fc -f C:\backup\mydatabase_%date:~0,4%%date:~5,2%%date:~8,2%.dump).  Обратите внимание, что синтаксис %date% для получения даты в Windows отличается от Linux.

Установите флажок "Run with highest privileges" (если необходимо).

Важно:

Убедитесь, что у пользователя, под которым выполняется задача, есть необходимые права.

Проверьте, что задача запускается и работает корректно.

## 3. Использование скриптов (Shell/Batch):

Вместо непосредственного запуска pg_dump в cron или Task Scheduler, можно создать скрипт (например, shell-скрипт для Linux или batch-файл для Windows), который выполняет дополнительные действия, такие как:

Проверка доступности базы данных перед резервным копированием.

Сжатие резервной копии (например, с помощью gzip или 7z).

Отправка резервной копии на удаленный сервер (например, с помощью scp или rsync).

Отправка уведомлений об успехе или неудаче резервного копирования (например, по электронной почте).

Это обеспечивает большую гибкость и контроль над процессом резервного копирования.

## 4. Использование специализированных инструментов резервного копирования:

Существуют специализированные инструменты для резервного копирования PostgreSQL, которые предоставляют расширенные возможности, такие как:

Инкрементное резервное копирование (резервируются только изменения с момента последнего резервного копирования).

Сжатие и шифрование резервных копий.

Управление резервными копиями (хранение, ротация, восстановление).

Интеграция с облачными хранилищами.

Примеры таких инструментов:

pgBackRest

Barman

WAL-G

Выбор метода автоматизации зависит от ваших потребностей, уровня опыта и инфраструктуры.  Для простых задач достаточно cron или Task Scheduler.  Для более сложных сценариев может потребоваться скрипт или специализированный инструмент.  Самое главное - регулярно тестировать процесс резервного копирования и восстановления, чтобы убедиться, что в случае необходимости вы сможете восстановить данные.

# Задание 3. MySQL

3.1. С помощью официальной документации приведите пример команды инкрементного резервного копирования базы данных MySQL.

3.1.* В каких случаях использование реплики будет давать преимущество по сравнению с обычным резервным копированием?

Приведите ответ в свободной форме.

## ОТВЕТ:

Пример команды инкрементного резервного копирования базы данных MySQL

В MySQL инкрементное резервное копирование можно реализовать с помощью бинарных логов. Для этого необходимо сначала включить бинарные логи в конфигурации MySQL. После этого можно использовать утилиту `mysqlbinlog` для создания инкрементных резервных копий.

Пример команды для создания инкрементного резервного копирования может выглядеть следующим образом:

## 1. Включите бинарные логи в конфигурации MySQL (файл `my.cnf`):

```ini
[mysqld]
log-bin=mysql-bin
```

## 2. После внесения изменений перезапустите сервер MySQL.

## 3. Для создания инкрементного резервного копирования используйте команду `mysqlbinlog`:

```bash
mysqlbinlog --start-datetime="2025-01-01 00:00:00" --stop-datetime="2025-01-02 00:00:00" mysql-bin.000001 > incremental_backup.sql
```

В этом примере мы создаем инкрементное резервное копирование, начиная с 1 января 2025 года и заканчивая 2 января 2025 года, сохраняя изменения в файл `incremental_backup.sql`.

Преимущества использования реплики по сравнению с обычным резервным копированием

Использование реплики базы данных может дать несколько преимуществ по сравнению с традиционным резервным копированием:

## 1. Непрерывная доступность: 

Реплики обеспечивают высокую доступность данных, так как они могут быть использованы для чтения в случае сбоя основной базы данных. Это позволяет минимизировать время простоя.

## 2. Снижение нагрузки на основную базу: 

Реплики могут использоваться для распределения нагрузки, особенно в сценариях с высокой частотой запросов на чтение. Это позволяет основной базе данных обрабатывать больше операций записи.

## 3. Быстрое восстановление: 

Восстановление данных из реплики может быть быстрее, чем восстановление из резервной копии, особенно если реплика настроена на синхронизацию с основной базой данных в реальном времени.

## 4. Тестирование и разработка: 

Реплики могут использоваться для тестирования и разработки, что позволяет разработчикам работать с актуальными данными без риска повредить основную базу.

## 5. Геораспределенность: 

Реплики могут быть размещены в разных географических регионах, что улучшает доступность и производительность для пользователей, находящихся в разных частях мира.

Таким образом, использование реплики может быть более эффективным решением для обеспечения доступности и производительности базы данных в условиях, когда требуется высокая надежность и минимальное время простоя.
