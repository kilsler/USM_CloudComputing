# Лабораторная №4
## Цели работы
Целью работы является познакомиться с сервисом Amazon S3 (Simple Storage Service) и отработать основные операции:

- создание публичного и приватного бакетов;
- загрузку и организацию объектов;
- работу с S3 через AWS CLI (копирование, перемещение, синхронизация);
- настройку версионирования и шифрования;
- использование S3 Static Website Hosting;
- применение Lifecycle-правил для архивирования старых данных.

## Условие задания
В этой лабораторной будут созданы два бакета:

- Публичный бакет - для хранения аватаров пользователей и статического контента;
- Приватный бакет - для логов и служебных файлов (с Lifecycle-политикой).  

  
## Выполнение 

### Шаг 1 Подготовка
Выбран регион: eu-central-1 (Frankfurt).  

Выбраны форматы имён бакетов:  

Публичный: cc-lab4-pub-k19-default-was-already-taken  
Приватный: cc-lab4-priv-k19-default-was-already-taken  
Локально (на своем компьютере) создана структура каталогов и файлов, как показано ниже:
```
s3-lab/
 ├── public/
 │   ├── avatars/
 │   │   ├── user1.jpg
 │   │   └── user2.jpg
 │   └── content/logo.png
 ├── private/
 │   └── logs/
 │       └── activity.csv
 └── README.md
```

### Шаг 2 Создание бакетов
Выбран способ - использовать ACL (упрощённый, наглядный),
Публичный бакет:

Имя: cc-lab4-pub-k19-default-was-already-taken  
Region: eu-central-1  
Object Ownership: ACLs enabled (Can be configured using ACLs)  
Block all public access: снять галочку (разрешить публичность)  

Приватный бакет:
Имя: cc-lab4-priv-k19-default-was-already-taken  
Region: eu-central-1  
Object Ownership: ACLs enabled (Can be configured using ACLs)  
Block all public access: оставить галочку  
<img width="1911" height="782" alt="image" src="https://github.com/user-attachments/assets/b7716dcc-46ea-4910-b5e4-4f4579d9b626" />

### Шаг 3 Загрузка объектов через AWS Management Console


### Шаг 4 Загрузка объектов через AWS CLI


### Шаг 5 Проверка доступа к объектам


### Шаг 6 Версионирование объектов


### Шаг 7 Создание Lifecycle-правил для приватного бакета


### Шаг 8 Создание статического веб-сайта на базе S3


### Шаг 9 Дополнительное задание. Загрузка файлов через AWS SDK


