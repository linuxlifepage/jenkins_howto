# Конспект видео-курса по Jenkins от Дениса Астахова (ADV-IT)

https://www.youtube.com/watch?v=cyb10iplv7U&list=PLg5SS_4L6LYvQbMrSuOjTL1HOiDhUE_5a&index=1


## Содержание

<!--ts-->
* [1.Теория о CI/CD](#theory)
    * [1.1 Термин](#termin)
* [2.Установка Jenkins на Ubuntu/Debian](#install)  
* [3.Администрирование Jenkins](#admin)  
* [4.Управление Plugins](#plugins)  
* [5.Простейшие jobs, включая Deployment](#simple_job)  
* [6.Добавление Slave Node](#slave_node)  
* [7.Удаленнои и локальное управление через Jenkins CLI](#cli)  
* [8.Deployments из GitHub](#deploy_github)  
* [9.Автоматизация запуска Build Job - Jenkins Build Triggers](#automate_run)  
* [10.Автоматизация запуска Build из Github - Jenkins Build Triggers from Github](#automate_run_github)  
* [11.Build с параметрами](#build_parameters)  
* [12.Deploy в AWS Elastic Beanstalk](#aws_elastic)  
* [13.Запуск Groovy Script - Обнуление счетчика Jenkins Build](#groovy)  
* [14.Основы Jenkins Pipeline и Jenkinsfile](#pipeline)  
<!--te-->

<a name="theory"/>

### 1.Теория о CI/CD 

CI - Continuous integration

Это DevOps модель, в которой разработчики делают Commit кода в Repository и автоматически запускаются Build или компиляция этого кода, после этого запускаются автоматические тесты кода: Unit Test, Integration Test, Functionality Test

CD - Continuous Delivery and Deployment

Это продолжение CI, где уже готовый скомпилированный код (Artifact) деплоится в Staging и Production

<a name="install"/>

### 2.Установка Jenkins на Ubuntu/Debian  

Сначала ставим java 8 версии:

Для Ubuntu:

```
sudo apt update -y && sudo apt install openjdk-8-jdk
```

Для Debian:
```
wget -qO - https://adoptopenjdk.jfrog.io/adoptopenjdk/api/gpg/key/public | sudo apt-key add -

sudo add-apt-repository --yes https://adoptopenjdk.jfrog.io/adoptopenjdk/deb/

sudo apt-get update && sudo apt-get install adoptopenjdk-8-hotspot
```

<a name="admin"/>

### 3.Администрирование Jenkins  

asd

<a name="plugins"/>

### 4.Управление Plugins  

asd

<a name="simple_job"/>

### 5.Простейшие jobs, включая Deployment  

asd

<a name="slave_node"/>

### 6.Добавление Slave Node  

asd

<a name="cli"/>

### 7.Удаленнои и локальное управление через Jenkins CLI 

asd

<a name="deploy_github"/>

### 8.Deployments из GitHub  

asd

<a name="automate_run"/>

### 9.Автоматизация запуска Build Job - Jenkins Build Triggers  

asd

<a name="automate_run_github"/>

### 10.Автоматизация запуска Build из Github - Jenkins Build Triggers from Github  

asd

<a name="build_parameters"/>

### 11.Build с параметрами  

asd

<a name="aws_elastic"/>

### 12.Deploy в AWS Elastic Beanstalk 

asd

<a name="groovy"/>

### 13.Запуск Groovy Script - Обнуление счетчика Jenkins Build 

asd

<a name="pipeline"/>

### 14.Основы Jenkins Pipeline и Jenkinsfile 

asd


  

