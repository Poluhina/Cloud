# Лабораторная работа №2. Введение в AWS. Вычислительные сервисы

# Цель работы

Познакомиться с основными вычислительными сервисами AWS, научиться создавать и настраивать виртуальные машины (EC2), а также развёртывать простые веб-приложения.

# Условие
# Задание 0. Подготовка среды

1) Зарегистрировалась в AWS и создала бесплатный аккаунт (Free Tier).

 - Перешла по ссылке: https://aws.amazon.com/

 - Нажала "Create an AWS Account".
   
2) Вошла в консоль управления под root-пользователем.

3) В правом верхнем углу выберала регион EU (Frankfurt) eu-central-1.


# Задание 1. Создание IAM группы и пользователя

1) Открыла сервис IAM.

2) СоздалаIAM группу Admins.

3) Создала IAM пользователя.

# Задание 2. Настройка Zero-Spend Budget

1) Открыла сервис Billing and Cost Management.

2) В меню слева выбрала Budgets → Create budget.

3) Выбрала "Zero spend budget" шаблон и указала параметры:

 - Budget name: ZeroSpend
 
 - Email recipients: мой email
 
 - Нажала "Create budget" внизу страницы.

# Задание 3. Создание и запуск EC2 экземпляра (виртуальной машины)

Для запуска и настройки виртуальной машины используется сервис Amazon EC2 (Elastic Compute Cloud).

1) Открыла сервис EC2.

2) В меню слева выбрала Instances → Launch instances и заполнила параметры для запуска виртуальной машины: 

 - Name and tags: webserver.

 - AMI: Выбрала Amazon Linux 2023 AMI. Это образ, который будет использоваться для создания виртуальной машины.
   
 - Instance type: t3.micro.
  
 - Key pair. Это криптографическая пара ключей (приватный и публичный). Она нужна для безопасного входа на сервер по SSH.

   - Выбрала "Create a new key pair".
   - Ввела имя для ключа в формате yournickname-keypair.
   - Нажала "Create key pair" и скачала файл с приватным ключом (расширение .pem) и сохранила его.
     
 - Security group. Это набор правил, которые определяют, какой трафик разрешен к вашему экземпляру.

    - Выбрала "Create a new security group".
      
    - Ввела имя группы webserver-sg.
      
    - Добавила два правила для входящего трафика (Inbound rules).
  

- Разрешила входящий HTTP трафик с любого IP-адреса.

- Разрешила входящий SSH трафик только с вашего текущего IP-адреса.

- Network settings. Оставила настройки по умолчанию. AWS автоматически создаст виртуальную сеть (VPC) и подсеть (subnet).

- Configure Storage. Оставила настройки по умолчанию.

Ввнизу в Advanced details → User Data и вставила следующий скрипт:

```
#!/bin/bash
dnf -y update
dnf -y install htop
dnf -y install nginx
systemctl enable nginx
systemctl start nginx
```

В Launch instance появился статус Running и Status checks: 2/2. После того, как виртуальная машина запустилась, можно посмотреть её публичный IP-адрес в колонке "IPv4 Public IP".

Открыла в браузере и проверила что она работает.

<img width="571" height="269" alt="клауд" src="https://github.com/user-attachments/assets/0d4a17c7-4a88-4f0c-98b6-d7405e0feb81" />

(скриншот из браузера)

# Задание 4. Логирование и мониторинг

Открыла вкладку Status checks.

В карточке инстанса EC2 во вкладке Status checks, можно быстро определить,  выявил ли Amazon EC2 какие-либо проблемы, которые могут помешать работе приложений.

Amazon EC2 выполняет автоматические проверки для каждого работающего экземпляра:

   - System reachability check — проверяет инфраструктуру AWS (железо и гипервизор).
   - Instance reachability check — проверяет, доступна ли операционная система на уровне инстанса.


Убедилась, что обе проверки прошли успешно (2/2 checks passed).

<img width="173" height="100" alt="клауд2" src="https://github.com/user-attachments/assets/66c1a5d6-cb3d-4f33-9e46-141e38f9a93c" />


Открыла вкладку Monitoring.

На этой вкладке отображаются метрики Amazon CloudWatch для инстанса.

Так как инстанс был создан недавно, метрик пока немного.

Просмотр системного лога (System Log)

В верхнем меню нажала Actions → Monitor and troubleshoot → Get system log.

В меню выберите Actions → Monitor and troubleshoot → Get instance screenshot.

<img width="1620" height="763" alt="клауд3" src="https://github.com/user-attachments/assets/01aef006-72f8-46f1-b4c9-2feaf8772a2b" />

# Задание 5. Подключение к EC2 инстансу по SSH

Открыла терминал на компьютере.

Перейдите в директорию, где сохранён файл приватного ключа .pem.

Подключилась к инстансу по SSH:

```
ssh -i yournickname-keypair.pem ec2-user@<Public-IP>
```

После успешного подключения увидела приглашение командной строки:

[ec2-user@ip-xx-xx-xx-xx ~]$

# Задание 6a. Развёртывание статического веб-сайта (Для специализаций Frontend & Backend & Security)

Начальный уровень

Создайте на локальном компьютере 3 HTML-файла:
index.html — главная страница сайта.
about.html — страница "О нас".
contact.html — страница "Контакты".
Подключитесь к вашему инстансу EC2 по SSH (см. задание 5).
Перейдите в директорию веб-сервера Nginx:

cd /usr/share/nginx/html
Скопируйте созданные HTML-файлы на сервер с помощью scp (в отдельном терминале на вашем локальном компьютере):

scp -i yournickname-keypair.pem index.html ec2-user@<Public-IP>:/usr/share/nginx/html
scp -i yournickname-keypair.pem about.html ec2-user@<Public-IP>:/usr/share/nginx/html
scp -i yournickname-keypair.pem contact.html ec2-user@<Public-IP>:/usr/share/nginx/html
Что делает команда scp?

Проверьте, что файлы успешно скопированы на сервер:

ls -l /usr/share/nginx/html
Откройте в браузере URL: http://<Public-IP> и убедитесь, что ваш сайт доступен.

# Контрольные вопросы

Что делает политика AdministratorAccess?

Что такое User Data и какую роль выполняет данный скрипт? Для чего используется nginx?

В каких случаях важно включать детализированный мониторинг?

Детализированный мониторинг можно включать в случаях, когда нужно реагировать на какие то проблемы в реальном времени или можно включать, когда запускают производственные приложения, где важно знать точное состояние сервера каждую минууту.

Почему в AWS нельзя использовать пароль для входа по SSH?

Что делает команда scp?

Что делает данный конфигурационный файл?

Чем «Stop» отличается от «Terminate»

