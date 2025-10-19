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















