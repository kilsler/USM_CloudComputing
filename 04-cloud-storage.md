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
Имя: c-lab4-priv-k19-default-was-already-taken  
Region: eu-central-1  
Object Ownership: ACLs enabled (Can be configured using ACLs)  
Block all public access: оставить галочку  
<img width="1911" height="782" alt="image" src="https://github.com/user-attachments/assets/b7716dcc-46ea-4910-b5e4-4f4579d9b626" />

### Шаг 3 Загрузка объектов через AWS Management Console
Перейдите в бакет cc-lab4-pub-k19-default-was-already-taken.  
Перейдите в директорию avatars/ и нажмите Upload.  
Загрузите файл 1.jpg из локальной папки s3-lab/public/avatars/.  
После загрузки в пункте Permissions выберите Grant public-read access.  
Завершите загрузку нажав Upload.  

<img width="1726" height="727" alt="image" src="https://github.com/user-attachments/assets/08badea0-1505-45dd-873d-51db927e4530" />

### Шаг 4 Загрузка объектов через AWS CLI

Выполняем команду для загрузку файла user2.jpg в публичный бакет:

``` aws s3 cp s3-lab/public/avatars/2.jpg s3://cc-lab4-pub-k19-default-was-already-taken/avatars/2.jpg --acl public-read ```  
Загрузите файл logo.png в публичный бакет, в директорию content/, также сделав его публичным.  
``` aws s3 cp s3-lab/public/content/logo.png s3://cc-lab4-pub-k19-default-was-already-taken/content/logo.png --acl public-read ```  
Загрузите файл activity.csv в приватный бакет, не делая его публичным.  
``` aws s3 cp s3-lab/private/logs/activity.csv s3://c-lab4-priv-k19-default-was-already-taken/logs/activity.csv --acl private ```  
<img width="930" height="544" alt="image" src="https://github.com/user-attachments/assets/15c77a62-d4c1-4890-b603-59bf3b98a072" />
<img width="1899" height="382" alt="image" src="https://github.com/user-attachments/assets/c78e4d8a-a7fc-4152-8561-db7af0d482ca" />
<img width="1858" height="679" alt="image" src="https://github.com/user-attachments/assets/a9bc184d-d042-48a2-b3d1-d7e2932d8d54" />  



<img width="936" height="132" alt="image" src="https://github.com/user-attachments/assets/93626a0c-566d-4be5-b96d-cd7c2e5f8290" />
<img width="1917" height="507" alt="image" src="https://github.com/user-attachments/assets/33c9d87b-cc16-4d1b-87e0-335bcd017668" />

### Шаг 5 Проверка доступа к объектам
https://cc-lab4-pub-k19-default-was-already-taken.s3.eu-central-1.amazonaws.com/avatars/1.jpg  
https://cc-lab4-pub-k19-default-was-already-taken.s3.eu-central-1.amazonaws.com/avatars/2.jpg  
<img width="1918" height="1079" alt="image" src="https://github.com/user-attachments/assets/605a558e-640c-4353-931b-96b49731501d" />  
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/b0773456-593c-47ff-92bc-e2cb2f7aa935" />

### Шаг 6 Версионирование объектов


### Шаг 7 Создание Lifecycle-правил для приватного бакета


### Шаг 8 Создание статического веб-сайта на базе S3


### Шаг 9 Дополнительное задание. Загрузка файлов через AWS SDK


