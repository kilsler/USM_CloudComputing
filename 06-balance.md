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
