# Morozan Nichita IA2303 
# Лабораторная работа №6. Балансирование нагрузки в облаке и авто-масштабирование
## Цель работы
Закрепить навыки работы с AWS EC2, Elastic Load Balancer, Auto Scaling и CloudWatch, создав отказоустойчивую и автоматически масштабируемую архитектуру.  
Студент развернёт:  
VPC с публичными и приватными подсетями;  
Виртуальную машину с веб-сервером (nginx);  
Application Load Balancer;  
Auto Scaling Group (на основе AMI);  
нагрузочный тест с использованием CloudWatch.  


## Шаг 1. Создание VPC и подсетей
Воспользуемся уже созданной vpc из прошлой работы:
<img width="1809" height="841" alt="image" src="https://github.com/user-attachments/assets/f480f660-6005-4668-ad7f-d26b5d62a199" />  

и interner gateway  
<img width="1665" height="135" alt="image" src="https://github.com/user-attachments/assets/14fad566-b0c9-4fcd-bcaa-64bde50b9e57" />
## Шаг 2. Создание и настройка виртуальной машины
Воспользуемся уже созданной в прошлой работе машиной. Система Ubuntu с уже рабочим и настроенным nginx проектом. С выбранной ранее vpc и доступным публичным адресом.  

<img width="1809" height="841" alt="image" src="https://github.com/user-attachments/assets/74a5635c-4a95-4dfd-902c-16f082d24c1a" />

## Шаг 3. Создание AMI
В EC2 выберите Instance → Actions → Image and templates → Create image.  
Назовите AMI, например: project-web-server-ami.  
Дождитесь появления AMI в разделе AMIs.  
Вопрос :Что такое image и чем он отличается от snapshot? Какие есть варианты использования AMI?  
Ответ: это готовый шаблон (образ), который содержит всё необходимое для запуска EC2-инстанса в AWS. Снэпшот это бэкап отдельного диска. Создание темплейтов для быстрого запуска истансов без необходиомсти долгой настройки.  
<img width="1438" height="512" alt="image" src="https://github.com/user-attachments/assets/ca29ecf2-d5e5-4dbf-967f-8fa940bf93cd" />  

## Шаг 4. Создание Launch Template

В разделе EC2 выберите Launch Templates → Create launch template.  
Укажите следующие параметры:  
Название: project-launch-template  
AMI: выберите созданную ранее AMI (My AMIs -> project-web-server-ami).  
Тип инстанса: t3.micro.  
Security groups: выберите ту же группу безопасности, что и для виртуальной машины.  
Нажмите Create launch template.  
В разделе Advanced details -> Detailed CloudWatch monitoring выберите Enable. Это позволит собирать дополнительные метрики для Auto Scaling.  
Вопрос :Что такое Launch Template и зачем он нужен? Чем он отличается от Launch Configuration? 
Ответ:  Launch Template — это современный шаблон для запуска EC2-инстансов.Launch Configuration — устаревший неверисионируемый вариант, который AWS больше не развивает и не рекомендует.

<img width="1909" height="826" alt="image" src="https://github.com/user-attachments/assets/4fe9e4ad-f11e-4c1f-8a25-14f359c42a8c" />  

## Шаг 5. Создание Target Group

Параметры:
Название: project-target-group
Тип: Instances
Протокол: HTTP
Порт: 80
VPC: выберите созданную VPC

<img width="1909" height="826" alt="image" src="https://github.com/user-attachments/assets/8a06fdc9-a605-46b6-960f-11438e89eea3" />

## Шаг 6. Создание Application Load Balancer
Название: project-alb  
Scheme: Internet-facing.  
Вопрос: В чем разница между Internet-facing и Internal?  
Ответ: Internet-facing — балансировщик имеет публичные IP и DNS, Internal- приватные IP, доступен только внутри VPC.
Subnets: выберите созданные 2 публичные подсети.  
Security Groups: выберите ту же группу безопасности, что и для виртуальной машины.  
Listener: протокол HTTP, порт 80.  
Default action: выберите созданную Target Group project-target-group.  
Вопрос :Что такое Default action и какие есть типы Default action?  
Ответ:  Default action — это то действие, которое ALB выполнит, если ни одно из правил listener'а не подошло к запросу.  
Forward пересылает запрос в выбранную Target Group. Redirect делает HTTP-редирект. Fixed-response возвращает статический ответ.  

<img width="1909" height="826" alt="image" src="https://github.com/user-attachments/assets/dc2a95da-fe25-4464-8d01-2841e088b834" />  
<img width="1909" height="826" alt="image" src="https://github.com/user-attachments/assets/86bfe025-1ca7-4018-9b3e-dcb6435ae0d5" />  


## Шаг 7. Создание Auto Scaling Group

Название: project-auto-scaling-group  
Launch template: выбераем созданный ранее Launch Template (project-launch-template).  
В разделе Choose instance launch options .  
В разделеNetwork:  
выбираем созданную VPC и две приватные подсети.  
Availability Zone distribution: выбераем Balanced best effort.  
В разделе Integrate with other services:  
 Выбрать Attach to an existing load balancer  
 Выберать созданную Target Group (project-target-group).  
В разделе Configure group size and scaling:  
Минимальное количество инстансов: 2  
Максимальное количество инстансов: 4  
Желемое количество инстансов: 2  
Укажите Target tracking scaling policy и настройте масштабирование по CPU (Average CPU utilization — 50% / Instance warm-up period — 60 seconds).  

В разделе Additional settings ставим галочку на Enable group metrics collection within CloudWatch, чтобы собирать метрики Auto Scaling Group в CloudWatch. Этот пункт позволит нам отслеживать состояние группы и её производительность.  

Вопрос: Что такое Instance warm-up period и зачем он нужен?  
Ответ: это время (в секундах), в течение которого новый запущенный инстанс не учитывается в метриках Auto Scaling Group и не получает трафик от Load Balancer.
Вопрос: Почему для Auto Scaling Group выбираются приватные подсети?  
Ответ: Потому что это лучшая и рекомендуемая практика безопасности в 2025 году.
Вопрос: Зачем нужна настройка: Availability Zone distribution?  
Ответ:  Эта настройка определяет, как Auto Scaling Group будет распределять инстансы по зонам доступности (AZ).


<img width="1909" height="826" alt="image" src="https://github.com/user-attachments/assets/d75cd26e-7add-4770-8ff3-1f670dbe05c4" />  
<img width="1909" height="826" alt="image" src="https://github.com/user-attachments/assets/9105643a-1be4-4680-9e07-6fb9ebea2506" />  

