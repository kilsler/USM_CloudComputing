# Лабораторная №3
## Цели работы
Научиться вручную создавать виртуальную сеть (VPC) в AWS, добавлять в неё подсети, таблицы маршрутов, интернет-шлюз (IGW) и NAT Gateway, а также настраивать взаимодействие между веб-сервером в публичной подсети и сервером базы данных в приватной.  

После выполнения работы студент:  
понимает, как формируется изоляция сетей в AWS (VPC);  
умеет создавать и связывать компоненты сети;  
знает, как EC2-инстансы разных подсетей взаимодействуют друг с другом;  
различает публичные и приватные маршруты.  

## Шаг 1. Создание VPC
Вопрос: Что обозначает маска /16? И почему нельзя использовать, например, /8? - 
Находим VPC среди сервисов амазона заходим и создаем новую сеть.  
Name tag: student-vpc-k19
IPv4 CIDR block: 10.19.0.0/16
Tenancy: Default
Нажмите Create VPC.
<img width="1703" height="812" alt="image" src="https://github.com/user-attachments/assets/e7c16845-8e65-4b80-b174-5ff00adfcf54" />  
## Шаг 2. Создание Internet Gateway (IGW)

В левой панели выбераем Internet Gateways → Create internet gateway.  
Укажите имя: student-igw-k19.  
Теперь нужно “прикрепить” (Attach) шлюз к сети:  
Выбераем созданный IGW.  
Нажмаем Actions → Attach to VPC.  
В списке выберираем student-vpc-kXX.  
Подтверждаем.  
<img width="1707" height="755" alt="image" src="https://github.com/user-attachments/assets/c9b8acbc-fe47-4408-9503-ec7a9a143337" />  
## Шаг 3. Создание подсетей
### Создание публичной подсети
В левой панели выберите Subnets → Create subnet.  
Укажите:  
VPC ID: выберите вашу сеть student-vpc-kXX  
Subnet name: public-subnet-kXX  
Availability Zone: us-central-1a  
AWS создаёт подсети в конкретных зонах доступности; в нашем случае выберем первую  
  
IPv4 CIDR block: 10.19.1.0/24  
Диапазон IP-адресов, которые будут выданы ресурсам в этой подсети
<img width="1684" height="747" alt="image" src="https://github.com/user-attachments/assets/f30d5c8f-208a-4b75-9dec-373eeeef4644" />  

Нажмите Create subnet.  
<img width="1701" height="867" alt="image" src="https://github.com/user-attachments/assets/51e1a99f-7254-4e84-b011-61e6e1770f9d" />  

### Создание приватной подсети

Подсеть называется приватной, если её трафик не направляется напрямую в Интернет.

Нажмите Create subnet ещё раз.
Укажите:
VPC ID: выберите вашу сеть student-vpc-kXX
Используем ту же VPC, чтобы обе подсети могли взаимодействовать между собой.

Subnet name: private-subnet-kXX
Availability Zone: Выберите любую другую зону
IPv4 CIDR block: 10.(k%30).2.0/24
Диапазон адресов не должен пересекаться с диапазоном публичной подсети.

Нажмите Create subnet.
<img width="1730" height="736" alt="image" src="https://github.com/user-attachments/assets/1ec1742a-1c46-4566-98fb-39bf8767bdc5" />  
<img width="1688" height="537" alt="image" src="https://github.com/user-attachments/assets/d9795620-9871-4dda-b33e-dc2cf2b6ffd6" />  

### Создание публичной таблицы маршрутов  
В левой панели выберите Route Tables → Create route table.  
Укажите:  
Name tag: public-rt-kXX  
VPC: student-vpc-kXX  
Нажмите Create route table  
Перейдите на вкладку Routes и нажмите Edit routes → Add route.  
Заполните:  
Destination: 0.0.0.0/0  
Это означает “весь остальной трафик, не относящийся к внутренним адресам VPC”.  

Target: выберите Internet Gateway (student-igw-kXX).  
Нажмите Save changes.  

<img width="1910" height="628" alt="image" src="https://github.com/user-attachments/assets/0029a9a8-c569-4714-83b3-c5c867b0d038" />

Перейдите на вкладку Subnet associations → Edit subnet associations.  
Зачем необходимо привязать таблицу маршрутов к подсети?  

Отметьте public-subnet-kXX и нажмите Save associations.  

<img width="1664" height="447" alt="image" src="https://github.com/user-attachments/assets/aaf1253e-badc-499f-99ac-d87b4015f141" />  

### Создание приватной таблицы маршрутов

Нажмите Create route table ещё раз.  
Укажите:  
Name tag: private-rt-kXX  
VPC: student-vpc-kXX  
Нажмите Create route table.  
Перейдите на вкладку Subnet associations → Edit subnet associations.  
Отметьте private-subnet-kXX и нажмите Save associations.  
На данный момент все ресурсы, которые будут созданы в приватной подсети, не смогут выходить в Интернет, так как у нас нет NAT Gateway и соответствующего маршрута.  

<img width="1670" height="413" alt="image" src="https://github.com/user-attachments/assets/ed73bcab-e669-4e3a-9ae0-35c7fdbc3416" />  

## Шаг 6. Создание NAT Gateway

### Шаг 6.1. Создание Elastic IP
Elastic IP — это статический публичный IPv4-адрес, закреплённый за вашим аккаунтом AWS. Он используется для NAT Gateway, чтобы тот мог представлять собой “точку выхода” в Интернет от имени всех приватных инстансов.  
В левой панели выберите Elastic IPs → Allocate Elastic IP address.  
Нажмите Allocate.  
<img width="1682" height="339" alt="image" src="https://github.com/user-attachments/assets/345b025c-b9d9-47ad-aa74-bd7d22137bdd" />  

### Шаг 6.2. Создание NAT Gateway  
В левой панели выберите NAT Gateways → Create NAT gateway.  
Укажите:  
Name tag: nat-gateway-kXX  
Subnet: выберите публичную подсеть (public-subnet-kXX)  
NAT Gateway всегда создаётся в публичной подсети, потому что ему нужен прямой выход в Интернет через IGW.  

Connectivity type: Public  
Elastic IP allocation ID: выберите EIP, созданный на предыдущем шаге.  
Нажмите Create NAT gateway.  
Подождите 2–3 минуты, пока статус изменится с Pending на Available. Это значит, что NAT Gateway готов к работе.  
<img width="1663" height="565" alt="image" src="https://github.com/user-attachments/assets/643681f9-0a97-47db-883f-79b75b5e2e57" />  

### Шаг 6.3. Изменение приватной таблицы маршрутов  
Вернитесь в Route Tables и выберите private-rt-kXX.  
Перейдите на вкладку Routes и нажмите Edit routes → Add route.  
Заполните:  
Destination: 0.0.0.0/0  
Target: выберите NAT Gateway (nat-gateway-kXX).  
Нажмите Save changes.  
Теперь ресурсы в приватной подсети смогут выходить в Интернет через NAT Gateway.  
<img width="1692" height="466" alt="image" src="https://github.com/user-attachments/assets/dacbb60d-4630-44d7-b29e-c6ec518eb1b3" />  

## Шаг 7. Создание Security Groups
Security Group (SG) — это виртуальный брандмауэр на уровне инстанса (EC2), который контролирует входящий (Inbound) и исходящий (Outbound) трафик.  

В левой панели выберите Security Groups → Create security group.  
Укажите:  
Security group name: web-sg-k19  
Description: Security group for web server  
VPC: выберите вашу VPC (student-vpc-k19)  
В разделе Inbound rules добавьте правила разрешающее следующие типы трафика:  
Тип: HTTP, Протокол: TCP, Порт: 80, Источник: 0.0.0.0/0  
Тип: HTTPS, Протокол: TCP, Порт: 443, Источник: 0.0.0.0/0  
Создайте еще две Security Groups:  
bastion-sg-kXX для bastion host с разрешением входящего трафика на порт 22 (SSH) только из вашего IP-адреса.  
db-sg-kXX для базы данных с разрешением входящего трафика:  
Тип: MySQL/Aurora, Протокол: TCP, Порт: 3306, Источник: web-sg-kXX (разрешаем доступ только с веб-сервера)  
Тип: SSH, Протокол: TCP, Порт: 22, Источник: bastion-sg-kXX (разрешаем доступ только с bastion host)  
Что такое Bastion Host и зачем он нужен в архитектуре с приватными подсетями?  
<img width="1135" height="358" alt="image" src="https://github.com/user-attachments/assets/b94c81d7-fd01-4420-8904-041cb546f374" />  

## Шаг 8. Создание EC2-инстансов
оздайте три EC2-инстанса, которые будут выполнять следующие роли:  

Веб-сервер (web-server) - в публичной подсети, доступен из Интернета по HTTP.  
Сервер базы данных (db-server) - в приватной подсети, недоступен напрямую извне.  
Bastion Host (bastion-host) - в публичной подсети, для безопасного доступа к приватным ресурсам.  
В строке поиска AWS Console введите EC2 и откройте консоль.  
Создадите 3 инстанса, следуя инструкциям ниже.  
Для всех инстансов используйте:  

AMI: Amazon Linux 2 AMI (HVM), SSD Volume Type  
Тип инстанса: t3.micro  
Ключ доступа (Key Pair): создайте новый ключ student-key-kXX и скачайте его.  
Хранилище: оставьте по умолчанию (8 ГБ).  
Теги: добавьте тег Name с соответствующим именем инстанса.  
Для web-server:  

Выберите сеть VPC: student-vpc-kXX  

Подсеть Subnet: public-subnet-kXX  

Auto-assign Public IP: Enable  

Security Group: выберите web-sg-kXX  

В разделе Advanced Details в поле User data вставьте следующий скрипт для автоматической установки веб-сервера:  

#!/bin/bash  
dnf install -y httpd php  
echo "<?php phpinfo(); ?>" > /var/www/html/index.php  
systemctl enable httpd  
systemctl start httpd  
<img width="1786" height="767" alt="image" src="https://github.com/user-attachments/assets/637be389-eec0-49c4-99cf-e370adf5bafd" />

Для db-server:  

Выберите сеть VPC: student-vpc-kXX  

Подсеть Subnet: private-subnet-kXX  

Auto-assign Public IP: Disable  

Security Group: выберите db-sg-kXX  

В разделе Advanced Details в поле User data вставьте следующий скрипт для автоматической установки MySQL сервера:  

#!/bin/bash  
dnf install -y mariadb105-server  
systemctl enable mariadb  
systemctl start mariadb  
mysql -e "ALTER USER 'root'@'localhost' IDENTIFIED BY 'StrongPassword123!'; FLUSH PRIVILEGES;"  
<img width="1779" height="774" alt="image" src="https://github.com/user-attachments/assets/7ea48ea0-d38b-428a-b192-b3815bf797d0" />

Для bastion-host:  

Выберите сеть VPC: student-vpc-kXX  

Подсеть Subnet: public-subnet-kXX  

Auto-assign Public IP: Enable  

Security Group: выберите bastion-sg-kXX  

В разделе Advanced Details в поле User data вставьте следующий скрипт для автоматической установки MySQL клиента:  

#!/bin/bash  
dnf install -y mariadb105  

<img width="1797" height="738" alt="image" src="https://github.com/user-attachments/assets/e0c446d9-712c-4710-89f4-7a5ce269c9d5" />


Итог:  
<img width="1704" height="399" alt="image" src="https://github.com/user-attachments/assets/0c29ef63-f02c-4f2a-b059-8b0c541b4395" />  






