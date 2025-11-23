# Лабораторная работа №4. Облачное хранилище данных. Amazon S3

# Шаг 1. Подготовка
Создала структуру каталогов и файлов.

Регион: eu-central-1 (Frankfurt).

Формат имён бакетов:

Публичный: cc-lab4-pub-kXX

Приватный: cc-lab4-priv-kXX

# Шаг 2. Создание бакетов
Перешла в консоль управления AWS S3 и создала два бакета с указанными именами.

Публичный бакет:

Имя: cc-lab4-pub-k055

Region: eu-central-1

Object Ownership: ACLs enabled (Can be configured using ACLs)

Block all public access: снять галочку (разрешить публичность)

Необходимо подтвердить предупреждение. Далее, нажала Create bucket.

Аналогично создала второй, приватный бакет. С именем cc-lab4-priv-k055, но оставила все настройки по умолчанию (Block all public access — включен).

Скриншот публичного и приватного бакетов.
<img width="1095" height="311" alt="image" src="https://github.com/user-attachments/assets/5cfa74ad-9d79-4a4e-b6c8-fa70d3048d0c" />

1) Что означает опция “Block all public access” и зачем нужна данная настройка?

Это важная настройка безопасности в S3. Если она включена, никто в интернете не сможет получить доступ к файлам. Если же, она выключена, файлы могут бытьь доступными через URL.

# Шаг 3. Загрузка объектов через AWS Management Console

Перешла в бакет cc-lab4-pub-k055.

Перешла в директорию avatars/ и нажала Upload.

Загрузила файл user1.jpg из локальной папки s3-lab/public/avatars/.

После загрузки в пункте Permissions выберала Grant public-read access.

Завершила загрузку нажав Upload.
<img width="1651" height="619" alt="image" src="https://github.com/user-attachments/assets/efe75250-c71f-4998-8c53-c7ee0ae4d684" />


2) Чем отличается ключ (object key) от имени файла?

# Шаг 4. Загрузка объектов через AWS CLI

Установила и настроила AWS CLI.

Публичные файлы (user1.jpg, user2.jpg, logo.jpg) загружены в публичный бакет и доступны по ссылке.

Приватный файл (activity.csv) загружен в приватный бакет и не доступен публично.

Использованы правильные пути и параметры (--acl publc-read для публичных файлов).

Скриншот терминала
<img width="1097" height="247" alt="image" src="https://github.com/user-attachments/assets/b01624a3-9099-44f8-910a-9d0c97ea1d39" />


# Шаг 5. Проверка доступа к объектам
