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

Подробнее здесь:
https://www.jenkins.io/doc/book/installing/linux/

**Для Ubuntu:**

1. Ставим Java 8

```
sudo apt update -y && sudo apt install openjdk-8-jdk
```

2. Устанавливаем Jenkins
```
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > \
    /etc/apt/sources.list.d/jenkins.list'
sudo apt-get update
sudo apt-get install jenkins
```


**Для Debian:**

1. Ставим Java 8

```
sudo apt update -y

wget -qO - https://adoptopenjdk.jfrog.io/adoptopenjdk/api/gpg/key/public | sudo apt-key add -

sudo add-apt-repository --yes https://adoptopenjdk.jfrog.io/adoptopenjdk/deb/

sudo apt-get update && sudo apt-get install adoptopenjdk-8-hotspot
```

2. Устанавливаем Jenkins
```
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > \
    /etc/apt/sources.list.d/jenkins.list'
sudo apt-get update
sudo apt-get install jenkins
```

3. ПРОВЕРЯЕМ службу после установки:  
```
sudo service jenkins status
```

4. Переходим по адресу:  
http://your_ip:8080

5. Активируем Jenkins - копируем адрес файла (/var/lib/jenkins...), 
В терминале вводим 
```
cat /var/lib/jenkins...
```
И вставляем пароль в в браузере в Jenkins

6. Выбираем установку с плагинами или же пустой (доставляем потом)

7. Устанавливаем логин и пароль 



<a name="admin"/>

### 3.Администрирование Jenkins  

1. Имя профиля -> Configure
   Тут можно изменить пароль, информацию, и тд
   В Description можно поменять описание
   
2. Рядом с поиском слева есть уведомление по Warning, которые можно настроить в основных опциях

3. Build Queue
Очередь ваших jobs

4. Building execution status
Статус ваших джобов

5. Manage Jenkins - Configure system  
   \# of executors - количество одновременных процессов, ставим 4  
   Lables - название сервера (оставляем пустым если не нужно)  
   Env variables - определить нужные переменные окружения  
   Administrative monitors - настроить апдейты на уведомления  

6. Configure Global Security:  
   Disable remember me - ставим галочку, чтобы не запоминало имя последнего логина в систему  
   Security realm - оставляем по умолчанию хранение данных о пользователях в Jenkins own users database  

7. System Informations - здесь нужно запомнить где находится самый главный запускаемый файл jenkins 
   ```
   /usr/share/jenkins/jenkins.war
   ```  
   Это нам понадобится потом для обновления
   
8. Sytem log - системные логи по дженкинсу

9. Jenkins CLI - полное управление jenkins через командную строку

10. Sctipt Console - пишем скрипты, которые что-то исправляют и меняют в нашем jenkins

11. Manage Node - управление дополнительными нодами (slave). Ноды нужны для обработки сложных джобов как доп сервера.

12.  Manage users - добавить нового пользователя
    
Для рестарта сервера Jenkins можно использовать команду:
http://your_ip:8080/restart

ОБНОВЛЕНИЕ!
В главом меню Manage Jenkins если показывает есть обновления, то правой кнопкой мыши по ..download.. и копируем ссылку  
Далее делаем бэкап файла /usr/share/jenkins/jenkins.war и вставляем по ссылке на исходное место
```
cd /usr/share/jenkins/ && mv jenkins.war jenkins2.150.1.war && wget https://...jenkins.io/jenkins.war
sudo service jenkins restart
```





<a name="plugins"/>

### 4.Управление Plugins  

Удобно посмотреть плагины можно здесь:  
https://plugins.jankins.io

В настройках Manage Jenkins - Manage Plugins - устанавливем эти плагины:
   Git, Credentials, GreenBalls, ChuckNorris
   И тутже ниже можно ставить галочку "Restart jenkins после установки плагинов"
   
   Для установки старой версии плагина можно выбрать ручную установку и скачать нужноу версию (выбрать с гитхаба)
   
   Директория установленных плагинов /var/lib/jenkins/plugins
   

<a name="simple_job"/>

### 5.Простейшие jobs, включая Deployment  

**1. Создаем простой jobs**
   **Create new jobs** -> указываем имя и выбираем **Freestyle project** -> указываем **Description** -> в **Build** создаем **Jankins Shell**
   и вводим стандартные команды линукса к примеру echo "Hello'
   Жмем **SAVE** и в главном окне **Dashboard** справа от нашего job жмем **Выполнить** (зеленую кнопку)
   Чтобы посмотреть лог - жмем рядом с именем джоба Console output

**2. Создаем полный Jobs:**
   Наши jobs хранятся в /var/lib/jenkins/jobs
   Создаем  job **DEPLOY TO TEST** и ставим галочку **Discard of build** (удалять старые билды) и ставим 5 в Max # builds of keeps и Aply SAVE
   
   Добавляем **Execute shell**
   ```
   echo "------------------------BUILD IS STARTED--------------------"
   cat <<EOF >index.html
   <html>
   <head>
   <title>Пример 1</title>
   </head>
   <body>
   <H1>Привет!</H1>
   <P> Это простейший пример HTML-документа. </P>
   <P> Этот *.html-файл может быть одновременно открыт и в Notepad, и в Netscape. Сохранив изменения в Notepad, просто нажмите кнопку Reload ('перезагрузить') в        Netscape, чтобы увидеть эти изменения реализованными в HTML-документе. </P>
   </body>
   </html>
   EOF
   echo "------------------------BUILD IS FINISHED--------------------"
   ```
   
   Добавляем второй Execute shell
   
   ```
   echo "------------------------TEST IS STARTED--------------------"
   result=`grep "Привет!" index.html | wc -l`
   if [ "$result" = "1" ]
   then 
      echo "PASSED"
   else
      echo "ERROR"
      exit 1
   fi
   echo "------------------------TEST IS FINISHED--------------------"
   ```
   SAVE
   
   Доставляем плагин **Publish Over SSH** и идем в **Configure System** - **Publish over SSH** - указываем pub key текстом, **SSH Servers** жмем **Add**, и добавляем 2 сервер (**TEST** и **PRODUCTION**) заполняем все поля и в **Remote Directory** указываем конечную папку **/var/www/html**
   
   Возвращаемся в нашу джобу и внизу после **Execution Shell** выбираем в **Post Build Actions** -> **Send build artefacts over ssh** -> выбираем сервер
   **Source files** - ставим *
   Exec command - команда после билда указываем **sudo service httpd restart**
   
   И добавляем еще один **Post Build Actions** - **ChuckNOrris**
   
   **APPLY SAVE**
   
   
   Создаем ВТОРОЙ джоб **DEPLOY TO PRODUCTION** и указываем внизу **Copy from** - **DEPLOY TO TEST** и мы склонируем 
   И в этой джобе меняем сервер деплоя
   
   Если нужно отключить джоб в **PRODUCTION** временно, то в настройках ставим галочку **Disable this job**
    
   

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


  

