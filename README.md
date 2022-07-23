
# **Тестовое задание Python**

Необходимо разработать скрипт на языке Python 3, 

который будет выполнять следующие функции:

1. Получать данные с документа при помощи Google API, сделанного в [Google Sheets](https://docs.google.com/spreadsheets/d/1f-qZEX1k_3nj5cahOzntYAnvO4ignbyesVO7yuBdv_g/edit) (необходимо копировать в свой Google аккаунт и выдать самому себе права).
2. Данные должны добавляться в БД, в том же виде, что и в файле –источнике, с добавлением колонки «стоимость в руб.»
    
    a. Необходимо создать DB самостоятельно, СУБД на основе PostgreSQL.
    
    b. Данные для перевода $ в рубли необходимо получать по курсу [ЦБ РФ](https://www.cbr.ru/development/SXML/).
    
3. Скрипт работает постоянно для обеспечения обновления данных в онлайн режиме (необходимо учитывать, что строки в Google Sheets таблицу могут удаляться, добавляться и изменяться).

Дополнения, которые дадут дополнительные баллы и поднимут потенциальный уровень оплаты труда:

1. a. Упаковка решения в docker контейнер
    
    b. Разработка функционала проверки соблюдения «срока поставки» из таблицы. В случае, если срок прошел, скрипт отправляет уведомление в Telegram.
    
    c. Разработка одностраничного web-приложения на основе Django или Flask. Front-end React.
    
    ![Untitled](https://kanalservis.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F6ee6a638-c52e-46a0-9c2d-cb518c955fb1%2FUntitled.png?table=block&id=b1d9d345-46fe-49b7-8909-2884086d4be1&spaceId=dbcc5cf8-15c2-4d75-bb66-44a130d346fa&width=2000&userId=&cache=v2)
    

1. Решение на проверку передается в виде ссылки на проект на Github.
В описании необходимо указать ссылку на ваш Google Sheets документ (открыть права чтения и записи для пользователя [irbispro10@gmail.com](mailto:irbispro10@gmail.com)), а также инструкцию по запуску разработанных скриптов.
******************************

### Ссылка на Google table с выданым доступом
[Google table](https://docs.google.com/spreadsheets/d/1ki8CrRI7vUo0f4JqWiopPh4y7ffxX2BP2uvRg6eqb_0/edit#gid=0)

### Настройки

Для корректного подключения к локальной базе данных PostgreSQL, необходимо в файле script/project.ini в секции [postgresql] указать свои значения для переменных.
Желательно изменять только значения password=123456, host=localhost, port=5432. В главном скрипте предусмотрено 2 режима старта:

1) 'первый запуск'. Устанавливается соединение с pgAdmin4, cоздается БД (согласно настройкам в project.ini) и таблица.
2) 'повторный запуск'. Устанавливается соединение с pgAdmin4, БД и таблица уже созданы, скрипт будет обновлять данные с google sheet. 

Цикл обновления устанавливается глобальной константой WORK_PERIOD = 10сек. в файле script/main_script.py

[Не баг, а фича] Вносить изменения в google-таблицу необходимо только во время работы скрипта.

В случае изменения предустановленных параметров базы данных, продублировать эти изменения для DATABASES в backend/djangoPage/settings.py в секции [DATABASES]

Для получения уведомлений в Telegramm о вышедших сроках поставки необходимо добавиться в [канал](https://t.me/db_notice).

### Зависимости
Скачиваем архив.
Разархивируем.
В полученной папке kanal_test_task-master открываем папку frontend.

В папке frontend открываем консоль и устанавливаем зависимости командой:
```sh
npm install --force
```
После установки зависимостей для frontend, переходим в основную директорию командой:

```sh
cd ..
```
Создаем виртуальное окружение командой:
```sh
pipenv shell
```
Список необходимых библиотек находится в файле Pipfile.lock.
Устанавливаем зависимости командой:
```sh
pipenv install
```
Переходим в папку с основным скриптом командой: 
```sh
cd script/
```


## Запуск скриптa
Запускаем скрипт с активированным окружением командой
```sh
py main_script.py
```
Создается БД и таблица и первично наполняется данными с google_sheet, отправляется уведомление в Telegram.

[не баг, а фича] Уведомления будут отправляться в Telegram с периодичностью раз в сутки работы скрипта.

Предполагается, что 'сегодня' в БД не будут вноситься контракты с уже проваленным сроком поставки, ввиду отсутствия смысла.

## Запуск веб-приложения
В директории kanal_test_task-master открываем новую консоль.
Активируем виртуальное окружение командой:
```sh
pipenv shell
```
Переходим в директорию backend командой:
```sh
cd backend/
```
[Важный повтор] В случае изменения предустановленных параметров базы данных, продублировать эти изменения для БД в файле backend/djangoPage/settings.py в секции [DATABASES]

Далее необходимо провести миграцию django c существующей и предварительно наполненной БД. В консоли вводим команду:

```sh
 py manage.py migrate
```

После связывания существующей БД с Django, стартуем веб-сервер командой:
```sh
 py manage.py runserver
```

После этого открываем новую третью консоль в директории frontend и стартуем командой:
```sh
npm start
```
ps. Для выхода из основного скрипта несколько раз нажимаем Escape.

   Для выхода из backend Ctrl+Break.

   Для выхода из frontend CtrL+C.








