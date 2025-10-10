## Задание 0. Подготовка среды
Уже было настроено во время предыдущих лабораторных работ.
## Задание 1. Создание IAM группы и пользователя
Уже было создано в время предыдущих лабораторных работ.  
Политика AdministratorAccess - предоставляет полные права на все ресурсы AWS(Создавать пользователей, инстансы и пользоваться любыми сервисами AWS).
## Задание 2. Настройка Zero-Spend Budget
Уже было создано во время предыдущих лабораторных работ.
## Задание 3. Создание и запуск EC2 экземпляра (виртуальной машины)
User Data — это специальный раздел настроек при создании EC2-инстанса, куда можно вставить скрипт, который автоматически выполняется при первом запуске .  
nginx - веб-сервер.   
Полученный инстанс  
<img width="1656" height="158" alt="image" src="https://github.com/user-attachments/assets/b95e9d64-80a3-4bf0-88de-fef0061fd91b" />  
<img width="1670" height="797" alt="image" src="https://github.com/user-attachments/assets/2114ea8d-4744-4e61-b856-1ba283c6e70c" />  
Результат захода на публичный ipv4 подключения.
<img width="1230" height="941" alt="image" src="https://github.com/user-attachments/assets/abb6c096-4e12-460b-8f39-3e05c5e7cbb9" />  

Name:webserver101025  
Application and OS image : ubuntu  
Instance type: t3-micro  
Key-pair name: Vladimir-101025-keypair
Security group: webserver-sg  
Advanced details → User Data:  
```  
#!/bin/bash  
apt update -y  
apt upgrade -y  
apt install -y htop nginx  
systemctl enable nginx  
systemctl start nginx  
```
## Задание 4. Логирование и мониторинг
Проверка статуса:  
<img width="1625" height="385" alt="image" src="https://github.com/user-attachments/assets/f7de7e1a-d76c-4836-9e2e-6ca0637a2749" />  
Детализированный мониторинг нужен там, где важна быстрая реакция на изменения.  
Instance screenshot
<img width="922" height="427" alt="image" src="https://github.com/user-attachments/assets/5032c378-32ee-4bc4-bd64-b6084c383cfa" />  
## Задание 5. Подключение к EC2 инстансу по SSH
Почему в AWS нельзя использовать пароль для входа по SSH?- Использование пароля для публично доступных EC2-инстансов делает их уязвимыми для атак из интернета.  
<img width="955" height="498" alt="image" src="https://github.com/user-attachments/assets/21fe015f-3c26-4144-8a04-f38ed20625c7" />  
## Задание 6a. Развёртывание статического веб-сайта (Для специализаций Frontend & Backend & Security)























