# Домашнее задание к занятию "`Система мониторинга Zabbix`" - `Федоров Дмитрий`


### Инструкция по выполнению домашнего задания

   1. Сделайте `fork` данного репозитория к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/git-hw или  https://github.com/имя-вашего-репозитория/7-1-ansible-hw).
   2. Выполните клонирование данного репозитория к себе на ПК с помощью команды `git clone`.
   3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
      - впишите вверху название занятия и вашу фамилию и имя
      - в каждом задании добавьте решение в требуемом виде (текст/код/скриншоты/ссылка)
      - для корректного добавления скриншотов воспользуйтесь [инструкцией "Как вставить скриншот в шаблон с решением](https://github.com/netology-code/sys-pattern-homework/blob/main/screen-instruction.md)
      - при оформлении используйте возможности языка разметки md (коротко об этом можно посмотреть в [инструкции  по MarkDown](https://github.com/netology-code/sys-pattern-homework/blob/main/md-instruction.md))
   4. После завершения работы над домашним заданием сделайте коммит (`git commit -m "comment"`) и отправьте его на Github (`git push origin`);
   5. Для проверки домашнего задания преподавателем в личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
   6. Любые вопросы по выполнению заданий спрашивайте в чате учебной группы и/или в разделе “Вопросы по заданию” в личном кабинете.
   
Желаем успехов в выполнении домашнего задания!
   
### Дополнительные материалы, которые могут быть полезны для выполнения задания

1. [Руководство по оформлению Markdown файлов](https://gist.github.com/Jekins/2bf2d0638163f1294637#Code)

---

### Задание 1
Установите Zabbix Server с веб-интерфейсом.

Выбрана версия установки Zabbix 7.4 на Ubuntu 24.04

1. Откроем новую сессию с root-привилегиями.
   ```
   sudo -s  
   ```
2. Установим PostgresSQL
   ```
   apt install postgresql postgresql-contrib
   ```
2. Установим репозиторий Zabbix
   ```
   wget https://repo.zabbix.com/zabbix/7.4/release/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_7.4+ubuntu24.04_all.deb
   dpkg -i zabbix-release_latest_7.4+ubuntu24.04_all.deb
   apt update
   ```
3. Установим Zabbix сервер, веб-интерфейс и агент
   ```
   apt install zabbix-server-pgsql zabbix-frontend-php php8.3-pgsql zabbix-apache-conf zabbix-sql-scripts zabbix-agent
   ```
4. Создадим базу данных. Установим и запустим сервер базы данных.
   ```
   sudo -u postgres createuser --pwprompt zabbix
   sudo -u postgres createdb -O zabbix zabbix
   ```
   Импортируем начальную схему и данные
   ```
   zcat /usr/share/zabbix/sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix
   ```
6. Настроим базу данных для Zabbix сервера
   Введем раннее созданный пароль к БД в файле /etc/zabbix/zabbix_server.conf
   DBPassword=password
7. Запустим процессы Zabbix сервера и агента и настроим их запуск при загрузке ОС
   ```
   systemctl restart zabbix-server zabbix-agent apache2
   systemctl enable zabbix-server zabbix-agent apache2
   ```

Авторизация в админке Zabbix.
![alt text](https://github.com/fin4457/zabbix_1/blob/main/img/zabbix.jpg)



---

### Задание 2

**Что нужно сделать:**

1. Запушьте [репозиторий](https://github.com/netology-code/sdvps-materials/tree/main/gitlab) на GitLab, изменив origin. Это изучалось на занятии по Git.
2. Создайте .gitlab-ci.yml, описав в нём все необходимые, на ваш взгляд, этапы.

В качестве ответа в шаблон с решением добавьте: 
   
 * файл gitlab-ci.yml для своего проекта или вставьте код в соответствующее поле в шаблоне; 
 * скриншоты с успешно собранными сборками.
### Решение 2

.gitlab-ci.yml
```
stages:
  - test
  - build

test:
  stage: test
  image: golang:1.17
  script: 
   - go test .

build:
  stage: build
  image: docker:latest
  script:
   - docker build .
```
![alt text](https://github.com/fin4457/8-03-hw/blob/main/build.jpg)
 
 
---
## Дополнительные задания* (со звёздочкой)

Их выполнение необязательное и не влияет на получение зачёта по домашнему заданию. Можете их решить, если хотите лучше разобраться в материале.

---

### Задание 3*

Измените CI так, чтобы:

 - этап сборки запускался сразу, не дожидаясь результатов тестов;
 - тесты запускались только при изменении файлов с расширением *.go.

В качестве ответа добавьте в шаблон с решением файл gitlab-ci.yml своего проекта или вставьте код в соответсвующее поле в шаблоне.
