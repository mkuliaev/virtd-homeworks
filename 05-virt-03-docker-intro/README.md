
# Домашнее задание к занятию 4 «Оркестрация группой Docker контейнеров на примере Docker Compose» Mikhail Kuliaev

## Задача 1

Сценарий выполнения задачи:
- Установите docker и docker compose plugin на свою linux рабочую станцию или ВМ.
- Если dockerhub недоступен создайте файл /etc/docker/daemon.json с содержимым: ```{"registry-mirrors": ["https://mirror.gcr.io", "https://daocloud.io", "https://c.163.com/", "https://registry.docker-cn.com"]}```
- Зарегистрируйтесь и создайте публичный репозиторий  с именем "custom-nginx" на https://hub.docker.com (ТОЛЬКО ЕСЛИ У ВАС ЕСТЬ ДОСТУП);
- скачайте образ nginx:1.21.1;
- Создайте Dockerfile и реализуйте в нем замену дефолтной индекс-страницы(/usr/share/nginx/html/index.html), на файл index.html с содержимым:
```
<html>
<head>
Hey, Netology
</head>
<body>
<h1>I will be DevOps Engineer!</h1>
</body>
</html>
```
- Соберите и отправьте созданный образ в свой dockerhub-репозитории c tag 1.0.0 (ТОЛЬКО ЕСЛИ ЕСТЬ ДОСТУП). 
- Предоставьте ответ в виде ссылки на https://hub.docker.com/<username_repo>/custom-nginx/general .

__________________

<details>
   
![Screnshot](https://github.com/mkuliaev/virtd-homeworks/blob/shvirtd-1/05-virt-03-docker-intro/png/docker_1.png)
   
</details>

[ссылка на hub.docker.com/kuliaev/custom-nginx/general](https://hub.docker.com/layers/mkuliaev/custom-nginx/1.0.0/images/sha256-395b4b6a90e1dde55847d4ccc63dc8c5f6fc8682a2b53f50aa0167caa038e852?context=repo)


<details>
   
![Screnshot](https://github.com/mkuliaev/virtd-homeworks/blob/shvirtd-1/05-virt-03-docker-intro/png/docker_2.png)
   
</details>


## Задача 2
1. Запустите ваш образ custom-nginx:1.0.0 командой docker run в соответвии с требованиями:
- имя контейнера "ФИО-custom-nginx-t2"
- контейнер работает в фоне
- контейнер опубликован на порту хост системы 127.0.0.1:8080
2. Не удаляя, переименуйте контейнер в "custom-nginx-t2"
3. Выполните команду ```date +"%d-%m-%Y %T.%N %Z" ; sleep 0.150 ; docker ps ; ss -tlpn | grep 127.0.0.1:8080  ; docker logs custom-nginx-t2 -n1 ; docker exec -it custom-nginx-t2 base64 /usr/share/nginx/html/index.html```
4. Убедитесь с помощью curl или веб браузера, что индекс-страница доступна.

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.

```
docker run -d --name "MYK-custom-nginx-t2" -p 127.0.0.1:8080:80 mkuliaev/custom-nginx:1.0.0
docker rename "MYK-custom-nginx-t2" custom-nginx-t2
```

```
08-08-2024 15:58:37.168684935 +12
CONTAINER ID   IMAGE                         COMMAND                  CREATED              STATUS              PORTS                    NAMES
4e3cffe6d256   mkuliaev/custom-nginx:1.0.0   "/docker-entrypoint.…"   About a minute ago   Up About a minute   127.0.0.1:8080->80/tcp   custom-nginx-t2
LISTEN 0      4096       127.0.0.1:8080       0.0.0.0:*    users:(("docker-proxy",pid=7046,fd=4))    
2024/08/08 03:57:21 [notice] 1#1: start worker process 42
PGh0bWw+CjxoZWFkPgpIZXksIE5ldG9sb2d5CjwvaGVhZD4KPGJvZHk+CjxoMT5JIHdpbGwgYmUg
RGV2T3BzIEVuZ2luZWVyITwvaDE+CjwvYm9keT4KPC9odG1sPgo=
```
```
kuliaev@docker:~/custom-nginx$ curl http://127.0.0.1:8080
<html>
<head>
Hey, Netology
</head>
<body>
<h1>I will be DevOps Engineer!</h1>
</body>
</html>
kuliaev@docker:~/custom-nginx$ 

```
<details>
   
![Screnshot](https://github.com/mkuliaev/virtd-homeworks/blob/shvirtd-1/05-virt-03-docker-intro/png/docker_2_2.png)
   
</details>

## Задача 3
1. Воспользуйтесь docker help или google, чтобы узнать как подключиться к стандартному потоку ввода/вывода/ошибок контейнера "custom-nginx-t2".
2. Подключитесь к контейнеру и нажмите комбинацию Ctrl-C.
3. Выполните ```docker ps -a``` и объясните своими словами почему контейнер остановился.
4. Перезапустите контейнер
5. Зайдите в интерактивный терминал контейнера "custom-nginx-t2" с оболочкой bash.
6. Установите любимый текстовый редактор(vim, nano итд) с помощью apt-get.
7. Отредактируйте файл "/etc/nginx/conf.d/default.conf", заменив порт "listen 80" на "listen 81".
8. Запомните(!) и выполните команду ```nginx -s reload```, а затем внутри контейнера ```curl http://127.0.0.1:80 ; curl http://127.0.0.1:81```.
9. Выйдите из контейнера, набрав в консоли  ```exit``` или Ctrl-D.
10. Проверьте вывод команд: ```ss -tlpn | grep 127.0.0.1:8080``` , ```docker port custom-nginx-t2```, ```curl http://127.0.0.1:8080```. Кратко объясните суть возникшей проблемы.
11. * Это дополнительное, необязательное задание. Попробуйте самостоятельно исправить конфигурацию контейнера, используя доступные источники в интернете. Не изменяйте конфигурацию nginx и не удаляйте контейнер. Останавливать контейнер можно. [пример источника](https://www.baeldung.com/linux/assign-port-docker-container)
12. Удалите запущенный контейнер "custom-nginx-t2", не останавливая его.(воспользуйтесь --help или google)

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.

```
Контейнер остановился, потому что при нажатии Ctrl-C я отправили сигнал прерывания  процессу, 
который работает в контейнере.
```

```
root@4e3cffe6d256:/# apt-get update && apt-get install -y nano
root@4e3cffe6d256:/# nano /etc/nginx/conf.d/default.conf
root@4e3cffe6d256:/# nginx -s reload
2024/08/08 04:40:27 [notice] 703#703: signal process started
root@4e3cffe6d256:/# curl http://127.0.0.1:80
curl: (7) Failed to connect to 127.0.0.1 port 80: Connection refused
root@4e3cffe6d256:/# curl http://127.0.0.1:81
<html>
<head>
Hey, Netology
</head>
<body>
<h1>I will be DevOps Engineer!</h1>
</body>
</html>
root@4e3cffe6d256:/# exit
exit
```
```
root@docker:/home/kuliaev/custom-nginx# ss -tlpn | grep 127.0.0.1:8080
LISTEN 0      4096       127.0.0.1:8080       0.0.0.0:*    users:(("docker-proxy",pid=9017,fd=4))    
root@docker:/home/kuliaev/custom-nginx# docker port custom-nginx-t2
80/tcp -> 127.0.0.1:8080
root@docker:/home/kuliaev/custom-nginx# curl http://127.0.0.1:8080
curl: (56) Recv failure: Соединение разорвано другой стороной
root@docker:/home/kuliaev/custom-nginx# 

Nginx теперь слушает порт 81 внутри контейнера, но Docker по-прежнему пробрасывает порт 8080 на хосте к порту 80 
внутри контейнера. Так как Nginx  не слушает порт 80, запросы на порт 8080 хоста просто не проходят.
```

```
root@docker:/home/kuliaev/custom-nginx# docker stop custom-nginx-t2
custom-nginx-t2
root@docker:/home/kuliaev/custom-nginx# docker commit custom-nginx-t2 custom-nginx-t2-fixed
sha256:8ec991007685aa0d59080e9198aed0f419719d456c76ddd63ab470133962768b
root@docker:/home/kuliaev/custom-nginx# docker run -d --name custom-nginx-t2-fixed -p 127.0.0.1:8080:81 custom-nginx-t2-fixed
fb41bb2121bb141f9fbf069c95489446dbcf5642fe64b6af82170b46cce3074a
root@docker:/home/kuliaev/custom-nginx# curl http://127.0.0.1:8080
<html>
<head>
Hey, Netology
</head>
<body>
<h1>I will be DevOps Engineer!</h1>
</body>
</html>
root@docker:/home/kuliaev/custom-nginx# docker rm -f custom-nginx-t2
custom-nginx-t2
root@docker:/home/kuliaev/custom-nginx# 
```

## Задача 4


- Запустите первый контейнер из образа ***centos*** c любым тегом в фоновом режиме, подключив папку  текущий рабочий каталог ```$(pwd)``` на хостовой машине в ```/data``` контейнера, используя ключ -v.
- Запустите второй контейнер из образа ***debian*** в фоновом режиме, подключив текущий рабочий каталог ```$(pwd)``` в ```/data``` контейнера. 
- Подключитесь к первому контейнеру с помощью ```docker exec``` и создайте текстовый файл любого содержания в ```/data```.
- Добавьте ещё один файл в текущий каталог ```$(pwd)``` на хостовой машине.
- Подключитесь во второй контейнер и отобразите листинг и содержание файлов в ```/data``` контейнера.


В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.
__________________

```
kuliaev@docker:~$ docker run -d --name centos-container -v $(pwd):/data centos:latest tail -f /dev/null
Unable to find image 'centos:latest' locally
latest: Pulling from library/centos
a1d0c7532777: Pull complete 
Digest: sha256:a27fd8080b517143cbbbab9dfb7c8571c40d67d534bbdee55bd6c473f432b177
Status: Downloaded newer image for centos:latest
9bfe5ce10d374632c2d8577144804c4bbd0b18c5fbd3f5613a29994feca5f01c
kuliaev@docker:~$ docker run -d --name debian-container -v $(pwd):/data debian:latest tail -f /dev/null
Unable to find image 'debian:latest' locally
latest: Pulling from library/debian
ca4e5d672725: Pull complete 
Digest: sha256:45f2e735295654f13e3be10da2a6892c708f71a71be845818f6058982761a6d3
Status: Downloaded newer image for debian:latest
6c671d35ec8feb4bc88736953306141992cfbad0cf65f3431cbcc2a8a7f661e2
kuliaev@docker:~$ docker exec -it centos-container /bin/bash
[root@9bfe5ce10d37 /]# echo "centos:latest tail -f /dev/null )))))))" > /data/centos_file.txt
[root@9bfe5ce10d37 /]# exit
exit
kuliaev@docker:~$ echo " host machine " > $(pwd)/host_file.txt
kuliaev@docker:~$ docker exec -it debian-container /bin/bash
root@6c671d35ec8f:/# ls /data
centos_file.txt  host_file.txt
root@6c671d35ec8f:/# cat /data/centos_file.txt
centos:latest tail -f /dev/null )))))))
root@6c671d35ec8f:/# cat /data/host_file.txt
 host machine 
root@6c671d35ec8f:/# 

```
<details>
   
![Screnshot](https://github.com/mkuliaev/virtd-homeworks/blob/shvirtd-1/05-virt-03-docker-intro/png/docker_4_1.png)
   
</details>


## Задача 5

1. Создайте отдельную директорию(например /tmp/netology/docker/task5) и 2 файла внутри него.
"compose.yaml" с содержимым:
```
version: "3"
services:
  portainer:
    image: portainer/portainer-ce:latest
    network_mode: host
    ports:
      - "9000:9000"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
```
"docker-compose.yaml" с содержимым:
```
version: "3"
services:
  registry:
    image: registry:2
    network_mode: host
    ports:
    - "5000:5000"
```

И выполните команду "docker compose up -d". Какой из файлов был запущен и почему? (подсказка: https://docs.docker.com/compose/compose-application-model/#the-compose-file )

2. Отредактируйте файл compose.yaml так, чтобы были запущенны оба файла. (подсказка: https://docs.docker.com/compose/compose-file/14-include/)

3. Выполните в консоли вашей хостовой ОС необходимые команды чтобы залить образ custom-nginx как custom-nginx:latest в запущенное вами, локальное registry. Дополнительная документация: https://distribution.github.io/distribution/about/deploying/
4. Откройте страницу "https://127.0.0.1:9000" и произведите начальную настройку portainer.(логин и пароль адмнистратора)
5. Откройте страницу "http://127.0.0.1:9000/#!/home", выберите ваше local  окружение. Перейдите на вкладку "stacks" и в "web editor" задеплойте следующий компоуз:

```
version: '3'

services:
  nginx:
    image: 127.0.0.1:5000/custom-nginx
    ports:
      - "9090:80"
```
6. Перейдите на страницу "http://127.0.0.1:9000/#!/2/docker/containers", выберите контейнер с nginx и нажмите на кнопку "inspect". В представлении <> Tree разверните поле "Config" и сделайте скриншот от поля "AppArmorProfile" до "Driver".

7. Удалите любой из манифестов компоуза(например compose.yaml).  Выполните команду "docker compose up -d". Прочитайте warning, объясните суть предупреждения и выполните предложенное действие. Погасите compose-проект ОДНОЙ(обязательно!!) командой.

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод, файл compose.yaml , скриншот portainer c задеплоенным компоузом.

---
```
kuliaev@docker:~$ mkdir -p /tmp/netology/docker/task5
kuliaev@docker:~$ cd /tmp/netology/docker/task5
kuliaev@docker:/tmp/netology/docker/task5$ nano compose.yaml
kuliaev@docker:/tmp/netology/docker/task5$ nano docker-compose.yaml
kuliaev@docker:/tmp/netology/docker/task5$ docker compose up -d
WARN[0000] Found multiple config files with supported names: /tmp/netology/docker/task5/compose.yaml, /tmp/netology/docker/task5/docker-compose.yaml 
WARN[0000] Using /tmp/netology/docker/task5/compose.yaml 
WARN[0000] /tmp/netology/docker/task5/compose.yaml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion 
[+] Running 12/12
 ✔ portainer Pulled                                                                                                                                                                                            10.0s 
   ✔ 57654d40e0a5 Pull complete                                                                                                                                                                                 0.6s 
   ✔ 1f476acfabd6 Pull complete                                                                                                                                                                                 0.7s 
   ✔ 5171176db7f2 Pull complete                                                                                                                                                                                 1.8s 
   ✔ 52e9438966a5 Pull complete                                                                                                                                                                                 2.7s 
   ✔ 43d4775415ac Pull complete                                                                                                                                                                                 3.3s 
   ✔ c1cad9f5200f Pull complete                                                                                                                                                                                 3.8s 
   ✔ a5e2b359b78b Pull complete                                                                                                                                                                                 3.9s 
   ✔ eb172612bcbb Pull complete                                                                                                                                                                                 6.0s 
   ✔ 6be7b2acffb5 Pull complete                                                                                                                                                                                 6.9s 
   ✔ 391dff0fb880 Pull complete                                                                                                                                                                                 7.3s 
   ✔ 4f4fb700ef54 Pull complete                                                                                                                                                                                 7.7s 
[+] Running 2/2
 ✔ Container task5-portainer-1                                          Started                                                                                                                                 1.0s 
 ! portainer Published ports are discarded when using host network mode                                                                                                                                         0.0s 
kuliaev@docker:/tmp/netology/docker/task5$ 

```
<details>
   
![Screnshot](https://github.com/mkuliaev/virtd-homeworks/blob/shvirtd-1/05-virt-03-docker-intro/png/docker_5_1.png)
   
</details>

```
Имя файла конфигурации Docker Compose по умолчанию — compose.yaml.
Если такого файла нет, он ищет файл с именем  docker-compose.yaml/yml.

```

```
kuliaev@docker:/tmp/netology/docker/task5$ nano compose.yaml 
kuliaev@docker:/tmp/netology/docker/task5$ docker compose up -d
WARN[0000] Found multiple config files with supported names: /tmp/netology/docker/task5/compose.yaml, /tmp/netology/docker/task5/docker-compose.yaml 
WARN[0000] Using /tmp/netology/docker/task5/compose.yaml 
WARN[0000] /tmp/netology/docker/task5/docker-compose.yaml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion 
WARN[0000] /tmp/netology/docker/task5/compose.yaml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion 
[+] Running 4/4
 ✔ Container task5-portainer-1                                          Started                                                                                                                                                                0.8s 
 ✔ Container task5-registry-1                                           Started                                                                                                                                                                1.0s 
 ! registry Published ports are discarded when using host network mode                                                                                                                                                                         0.0s 
 ! portainer Published ports are discarded when using host network mode                                                                                                                                                                        0.0s 
kuliaev@docker:/tmp/netology/docker/task5$ 


```
<details>
   
![Screnshot](https://github.com/mkuliaev/virtd-homeworks/blob/shvirtd-1/05-virt-03-docker-intro/png/docker_5_2_new1.png)
   
</details>



