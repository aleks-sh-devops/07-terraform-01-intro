# Домашнее задание к занятию "7.1. Инфраструктура как код"

## Задача 1. Выбор инструментов. 
 
### Легенда
 
Через час совещание на котором менеджер расскажет о новом проекте. Начать работу над которым надо 
будет уже сегодня. 
На данный момент известно, что это будет сервис, который ваша компания будет предоставлять внешним заказчикам.
Первое время, скорее всего, будет один внешний клиент, со временем внешних клиентов станет больше.

Так же по разговорам в компании есть вероятность, что техническое задание еще не четкое, что приведет к большому
количеству небольших релизов, тестирований интеграций, откатов, доработок, то есть скучно не будет.  
   
Вам, как девопс инженеру, будет необходимо принять решение об инструментах для организации инфраструктуры.
На данный момент в вашей компании уже используются следующие инструменты: 
- остатки Сloud Formation, 
- некоторые образы сделаны при помощи Packer,
- год назад начали активно использовать Terraform, 
- разработчики привыкли использовать Docker, 
- уже есть большая база Kubernetes конфигураций, 
- для автоматизации процессов используется Teamcity, 
- также есть совсем немного Ansible скриптов, 
- и ряд bash скриптов для упрощения рутинных задач.  

Для этого в рамках совещания надо будет выяснить подробности о проекте, что бы в итоге определиться с инструментами:

1. Какой тип инфраструктуры будем использовать для этого проекта: изменяемый или не изменяемый?
2. Будет ли центральный сервер для управления инфраструктурой?
3. Будут ли агенты на серверах?
4. Будут ли использованы средства для управления конфигурацией или инициализации ресурсов? 
 
В связи с тем, что проект стартует уже сегодня, в рамках совещания надо будет определиться со всеми этими вопросами.

### В результате задачи необходимо

1. Ответить на четыре вопроса представленных в разделе "Легенда". 
1. Какие инструменты из уже используемых вы хотели бы использовать для нового проекта? 
1. Хотите ли рассмотреть возможность внедрения новых инструментов для этого проекта? 

Если для ответа на эти вопросы недостаточно информации, то напишите какие моменты уточните на совещании.


## Решение  
1. Скорее всего буде использоваться контейнеризация, поэтому предлагаю неизменяемый, но с обезательной средой для разработчиков, тестировщиков (помимо прода), т.к. поскольку скучно не будет нужно минимизировать риски попадание багов на прод.  
2. От маштабов проекта зависит. Вообще не стоит множить сущее без крайней на то необходимости. Я бы ключевое условие поставил в возможности маштабирования. В начале можно обойтись одним центральным сервером, но по мере разростания микросервисного проекта нужно предусмотреть беспроблемный переход для обеспечения большей надежности  
3. Думаю, что без агентов  
4. Конечно! Terraform и Ansible уже стандарт, пожалуй.  

Инструменты для проекта:
- Terraform
- Docker
- Ansible
- Kubernetes
- Packer

В качестве CI/CD на начальном этапе лучше использовать Teamcity, т.к. есть наработки, но я бы, в перспективе, рассматривал GitLab. Документация очень важна, поэтому я бы присмотрелся к внедрению Конфлюенса.
По поводу информации. В рамках этой задачи совсем непонятно что это за сервис. Где он будет жить на земле или в облаке. Какие сервисы будут. Веб сервис? БД? Какая нагрузка ожидается? Может нужен балансировщик? И т.д. И т.п... 

## Задача 2. Установка терраформ. 

Официальный сайт: https://www.terraform.io/

Установите терраформ при помощи менеджера пакетов используемого в вашей операционной системе.
В виде результата этой задачи приложите вывод команды `terraform --version`.

## Решение  
```
[root@vmtester2 ~]# terraform -v
Terraform v1.3.6
on linux_amd64
```
<p align="center">
  <img width="1200" height="600" src="./pic1.png">
</p>

## Задача 3. Поддержка легаси кода. 

В какой-то момент вы обновили терраформ до новой версии, например с 0.12 до 0.13. 
А код одного из проектов настолько устарел, что не может работать с версией 0.13. 
В связи с этим необходимо сделать так, чтобы вы могли одновременно использовать последнюю версию терраформа установленную при помощи
штатного менеджера пакетов и устаревшую версию 0.12. 

В виде результата этой задачи приложите вывод `--version` двух версий терраформа доступных на вашем компьютере 
или виртуальной машине.

## Решение  
А вторую версию терраформа я запустил в докере с официального имеджа: https://hub.docker.com/r/hashicorp/terraform/
```
[root@vmtester2 ~]# docker run -i -t --name tf_old hashicorp/terraform:1.3.4 version
Emulate Docker CLI using podman. Create /etc/containers/nodocker to quiet msg.
Resolved "hashicorp/terraform" as an alias (/var/cache/containers/short-name-aliases.conf)
Trying to pull docker.io/hashicorp/terraform:1.3.4...
Getting image source signatures
Copying blob 46cb13f000b3 done  
Copying blob 213ec9aee27d done  
Copying blob a059b1a180e3 done  
Copying config 61b09dee22 done  
Writing manifest to image destination
Storing signatures
Terraform v1.3.4
on linux_amd64
```
