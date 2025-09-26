
# Домашнее задание к занятию 4 «Оркестрация группой Docker контейнеров на примере Docker Compose»

### Инструкция к выполению

1. Для выполнения заданий обязательно ознакомьтесь с [инструкцией](https://github.com/netology-code/devops-materials/blob/master/cloudwork.MD) по экономии облачных ресурсов. Это нужно, чтобы не расходовать средства, полученные в результате использования промокода.
2. Практические задачи выполняйте на личной рабочей станции или созданной вами ранее ВМ в облаке.
3. Своё решение к задачам оформите в вашем GitHub репозитории в формате markdown!!!
4. В личном кабинете отправьте на проверку ссылку на .md-файл в вашем репозитории.

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


                    Ответ:
                    https://hub.docker.com/repository/docker/a7lex777a0lexx/custom-nginx/general



## Задача 2
1. Запустите ваш образ custom-nginx:1.0.0 командой docker run в соответвии с требованиями:
- имя контейнера "ФИО-custom-nginx-t2"


    Ответ:
            скачаем если образ 
            docker pull a7lex777a0lexx/custom-nginx:1.0.0

            Запуск контейнера:
            docker run -d \ --name alex-gubanov-custom-nginx-t2 \ -p 127.0.0.1:8080:80 \ custom-nginx:1.0.0


              Проверить логи контейнера
              docker logs alex-ubanov-custom-nginx-t2


- контейнер работает в фоне

                Прверка статуса контейнера:
                                docker ps
                Проверьте правильность команды запуска:
                                docker port alex-gubanov-custom-nginx-t2
 
 



- контейнер опубликован на порту хост системы 127.0.0.1:8080


                        Проверить доступность веб-сервера
                        curl http://127.0.0.1:8080

2. Не удаляя, переименуйте контейнер в "custom-nginx-t2"
                      Остановить контейнер (если он запущен)
                      docker stop alex-Gubanov-custom-nginx-t2

                     Переименовать
                    docker rename alex-Gubanov-custom-nginx-t2 custom-nginx-t2

                      Запустить с новым именем
                      docker start custom-nginx-t2


3. Выполните команду ```date +"%d-%m-%Y %T.%N %Z" ; sleep 0.150 ; docker ps ; ss -tlpn | grep 127.0.0.1:8080  ; docker logs custom-nginx-t2 -n1 ; docker exec -it custom-nginx-t2 base64 /usr/share/nginx/html/index.html```

          Полный скрипт для PowerShell:

            Get-Date -Format "dd-MM-yyyy HH:mm:ss.ffffff K"; Start-Sleep -Milliseconds 150; docker ps; netstat -an | Select-String "127.0.0.1:8080"; docker logs custom-nginx-t2 -n 1; docker exec -it custom-nginx-t2 base64 /usr/share/nginx/html/index.html



4. Убедитесь с помощью curl или веб браузера, что индекс-страница доступна.
                            ccurl http://127.0.0.1:80url

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.



## Задача 3
1. Воспользуйтесь docker help или google, чтобы узнать как подключиться к стандартному потоку ввода/вывода/ошибок контейнера "custom-nginx-t2".

                           


2. Подключитесь к контейнеру и нажмите комбинацию Ctrl-C.

                            подключение к запущенному контейнеру
                             docker attach custom-nginx-t2

                             Подключимся к контейнеру 
                             docker exec -it custom-nginx-t2 /bin/bash


3. Выполните ```docker ps -a``` и объясните своими словами почему контейнер остановился.

      Контейнер остановился потому, что веб-сервер nginx работает как фоновыф процесс  внутри контейнера, и отправка сигнала SIGINT (Ctrl+C) этому процессу приводит к  (PID 1) завершениюго процесса- контейнер автоматически останавливается


4. Перезапустите контейнер
                    docker start custom-nginx-t2

                    Проверяем результат
                      docker ps -a

5. Зайдите в интерактивный терминал контейнера "custom-nginx-t2" с оболочкой bash.


6. Установите любимый текстовый редактор(vim, nano итд) с помощью apt-get.
                      определим тип пакетного менеджера: 
                      cat /etc/os-release

                      Обновим списки пакетов и установим редактор:
                      apt-get update 
                      apt-get install -y nano


7. Отредактируйте файл "/etc/nginx/conf.d/default.conf", заменив порт "listen 80" на "listen 81".


            Редактируем файл 
            nano /etc/nginx/conf.d/default.conf


8. Запомните(!) и выполните команду ```nginx -s reload```, а затем внутри контейнера ```curl http://127.0.0.1:80 ; curl http://127.0.0.1:81```.

                  Перезапустим nginx в proxy контейнере:
                   
                  nginx -s reload

                  Проверяем работу
                  curl http://localhost:81


9. Выйдите из контейнера, набрав в консоли  ```exit``` или Ctrl-D.

10. Проверьте вывод команд: ```ss -tlpn | grep 127.0.0.1:8080``` , ```docker port custom-nginx-t2```, ```curl http://127.0.0.1:8080```. Кратко объясните суть возникшей проблемы.

          Проверяем доступность на старом порту 80:
          curl http://127.0.0.1:80
         
          на новом порту 81:
          curl http://127.0.0.1:81

        Если возникнут  проверим через ss
        ss -tulpn | grep nginx

 После изменения порта nginx внутри контейнера с 80 на 81, проброс портов с хоста перестал работать корректно.
 Внутри контейнера nginx теперь слушает порт 81
  Но проброс портов настроен как 127.0.0.1:8080:80 (хост:8080 → контейнер:80)
   Порт 80 внутри контейнера больше не слушается nginx
  Решение проблемы: Нужно либо: вернуть порт 80 в конфигурации nginx внутри контейнера или изменить проброс портов при запуске контейнера: -p 127.0.0.1:8080:81

Проверка проброса портов: 
docker port custom-nginx-t2 

Результат: 80/tcp -> 127.0.0.1:8080


вывод проблемы:изменили порт nginx внутри контейнера с 80 на 81, но не обновили проброс портов. Docker успешно принимает соединения на порту 8080 хоста и перенаправляет их на порт 80 контейнера, но там nginx не слушает этот порт. 
Решение:Вернуть порт 80 в контейнере заходим в контейнер и исправляем порт 
docker exec -it custom-nginx-t2 /bin/bash nano /etc/nginx/conf.d/default.conf 

Меняем `listen 81` обратно на `listen 80` nginx -s reload exit

 Проверяем curl http://127.0.0.1:8080



11. * Это дополнительное, необязательное задание. Попробуйте самостоятельно исправить конфигурацию контейнера, используя доступные источники в интернете. Не изменяйте конфигурацию nginx и не удаляйте контейнер. Останавливать контейнер можно. [пример источника](https://www.baeldung.com/linux/assign-port-docker-container)

                            Установка socat
          docker exec custom-nginx-t2 sh -c "apt-get update && apt-get install -y socat"

          Запуск проброса
          docker exec -d custom-nginx-t2 socat TCP-LISTEN:80,fork TCP:127.0.0.1:81

          Проверка
          curl http://127.0.0.1:8081

          проверим что nginx слушает порт 81: 
          docker exec custom-nginx-t2 sh -c "netstat -tuln | grep :81"

12. Удалите запущенный контейнер "custom-nginx-t2", не останавливая его.(воспользуйтесь --help или google)
              docker rm -f custom-nginx-t2

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.

## Задача 4


- Запустите первый контейнер из образа ***centos*** c любым тегом в фоновом режиме, подключив папку  текущий рабочий каталог ```$(pwd)``` на хостовой машине в ```/data``` контейнера, используя ключ -v.

            Скачиваем образ CentOS
            docker pull centos:latest


            Команда для запуска:
            docker run -d -v ${PWD}:/data centos:latest tail -f /dev/null

            Проверяем что контейнер запущен
            docker ps

            Проверяем монтирование
            docker exec centos-data ls -la /data

- Запустите второй контейнер из образа ***debian*** в фоновом режиме, подключив текущий рабочий каталог ```$(pwd)``` в ```/data``` контейнера. 

                Скачаем образ Debian
                docker pull debian:latest

            Запустим контейнер
            docker run -d --name debian-data -v ${PWD}:/data debian:latest tail -f /dev/null


- Подключитесь к первому контейнеру с помощью ```docker exec``` и создайте текстовый файл любого содержания в ```/data```.

            подключимся к контейнеру и создадим файл
            docker exec <centos_id> sh -c "echo 'Этот файл создан из CentOS контейнера' > /data/centos-file.txt"
            проверим что файл создан из контейнера:
            docker exec <centos_id> cat /data/centos-file.txt

            Проверим что файлы видны и в Debian контейнере:
            Проверим из Debian контейнера
            docker exec <debian_id> ls -la /data
            docker exec <debian_id> cat /data/centos-file.txt

- Добавьте ещё один файл в текущий каталог ```$(pwd)``` на хостовой машине.

          создадим файл на хостовой машине:
        Перейдем в каталог DockerData
        cd C:\DockerData

        Создадим новый файл
            host-created-file.txt
     Проверим что файл виден в контейнерах:

      Проверим из CentOS контейнера
      docker exec 99889c91b63b ls -la /data/host-created-file.txt
      docker exec 99889c91b63b cat /data/host-created-file.txt

      Проверим из Debian контейнера
      docker exec 75f41c2b3d9e ls -la /data/host-created-file.txt
      docker exec 75f41c2b3d9e cat /data/host-created-file.txt

- Подключитесь во второй контейнер и отобразите листинг и содержание файлов в ```/data``` контейнера.
       Найдем ID второго контейнера (Debian):
          docker ps
         Подключимся к Debian контейнеру и посмотрим содержимое /data:
          Посмотрим список всех файлов в /data
          docker exec 75f41c2b3d9e ls -la /data/
              Подключимся к интерактивной сессии Debian
              docker exec -it 75f41c2b3d9e /bin/bash



В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.





## Задача 5

1. Создайте отдельную директорию(например /tmp/netology/docker/task5) и 2 файла внутри него.
"compose.yaml" с содержимым:
                                    
                  Создадим полный путь директорий
                mkdir -Force C:\tmp\netology\docker\task5
                    Перейдем в созданную директорию
                    cd C:\tmp\netology\docker\task5



```
version: "3"
services:
  portainer:
    network_mode: host
    image: portainer/portainer-ce:latest
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock


                                    Создаем compose.yaml с указанным содержимым
                                    @"
                                    version: "3"
                                    services:
                                      portainer:
                                        network_mode: host
                                        image: portainer/portainer-ce:latest
                                        volumes:
                                          - /var/run/docker.sock:/var/run/docker.sock
                                    "@ | Out-File -FilePath compose.yaml -Encoding UTF8



```
"docker-compose.yaml" с содержимым:
```
version: "3"
services:
  registry:
    image: registry:2

    ports:
    - "5000:5000"
                                              @"
                                              version: "3"
                                              services:
                                                registry:
                                                  image: registry:2
                                                  ports:
                                                    - "5000:5000"
                                              "@ | Out-File -FilePath docker-compose.yaml -Encoding UTF8


``

И выполните команду "docker compose up -d". Какой из файлов был запущен и почему? (подсказка: https://docs.docker.com/compose/compose-application-model/#the-compose-file )


                cd C:\tmp\netology\docker\task5 
                docker compose up -d
                     Проверим что запустилось: 
                      docker ps
                      docker-compose ps

               Остановим
              docker compose down

              Будет запущен Portainer из файла compose.yaml, так как он имеет высший приоритет согласно официальной документации Docker.
                  Был запущен файл compose.yaml, потому что Docker Compose использует следующий порядок приоритета при поиске конфигурационных файлов: 
                  compose.yaml 
                  docker-compose.yml

2. Отредактируйте файл compose.yaml так, чтобы были запущенны оба файла. (подсказка: https://docs.docker.com/compose/compose-file/14-include/)

                  Создаем объединенный compose.yaml
                 Проверим статус контейнера Portainer
            docker compose logs portainer --tail=20

            Проверим что контейнер работает
            docker compose ps portainer
            Останавливаем текущий Registry
            docker compose -f docker-compose.yaml down

                  Создаем объединенный compose.yaml
                  @"
                  services:
                    portainer:
                      image: portainer/portainer-ce:latest
                      ports:
                        - "9000:9000"
                      volumes:
                        - /var/run/docker.sock:/var/run/docker.sock
                        - portainer_data:/data

                    registry:
                      image: registry:2
                      ports:
                        - "5000:5000"

                  volumes:
                    portainer_data:
                  "@ | Out-File -FilePath compose.yaml -Encoding UTF8

                        Запускаем оба сервиса
                        docker compose up -d
                                                            
                                Проверим что оба сервиса запущены:
                                docker ps
                                docker compose ps



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


Проверим что есть в registry: 
curl http://127.0.0.1:5000/v2/_catalog

gроверим какие образы custom-nginx есть локально: 
docker images | findstr custom-nginx 
docker images | findstr nginx

Тегируем образ для registry 
docker tag a7lex777a0lexx/custom-nginx:1.0.0 127.0.0.1:5000/custom-nginx:latest 

Загружаем в registry 
docker push 127.0.0.1:5000/custom-nginx:latest

Проверим что образ загружен:
curl http://127.0.0.1:5000/v2/_catalog
curl http://127.0.0.1:5000/v2/custom-nginx/tags/list

развернем стек в Portainer
нажмите "Деплой стека"


проверим запущенные контейнеры:
docker ps | findstr nginx

Проверим доступность nginx:
curl http://127.0.0.1:9090
откроем в браузере:
 http://127.0.0.1:9090


Проверим в Portainer:
Перейдите в Containers - должен быть контейнер custom-nginx-stack_nginx_1




6. Перейдите на страницу "http://127.0.0.1:9000/#!/2/docker/containers", выберите контейнер с nginx и нажмите на кнопку "inspect". В представлении <> Tree разверните поле "Config" и сделайте скриншот от поля "AppArmorProfile" до "Driver".


7. Удалите любой из манифестов компоуза(например compose.yaml).  Выполните команду "docker compose up -d". Прочитайте warning, объясните суть предупреждения и выполните предложенное действие. Погасите compose-проект ОДНОЙ(обязательно!!) командой.


В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод, файл compose.yaml , скриншот portainer c задеплоенным компоузом.

---
              Удалим compose.yaml: 
              Remove-Item compose.yaml

 предупреждение:  "Found orphan containers ([task5-portainer-1]) for this project. If you removed or renamed this service in your compose file, you can run this command with the --remove-orphans flag to clean it up."
  Docker Compose не нашел файлов compose.yaml/docker-compose.yaml и будет использовать:
  Имя проекта по умолчанию (имя текущей директории) 
  Базовую конфигурацию


  Удалили compose.yaml, но остался docker-compose.yaml
   В docker-compose.yaml только сервис registry
    Сервис portainer остался (orphan), так как он был определен в удаленном файле

                  Запускаем с удалением orphan-контейнеров
                  docker compose up -d --remove-orphans

                  Проверяем что осталось
                  docker compose ps

                  Гасим проект ОДНОЙ командой
                  docker compose down

                  После docker compose down проверяем 
                  docker ps -a | findstr task5




После docker compose down проверяем docker ps -a | findstr task5
команда docker compose down останавливает и удаляет все сервисы текущего compose-проекта, используя метаданные которые Docker Compose сохранил при запуске.

 Удален compose.yaml 
    Остался только docker-compose.yaml с сервисом registry

Запущен docker compose up -d --remove-orphans 
    Удален orphan-контейнер task5-portainer-1 
    Запущен сервис task5-registry-1 
Выполнена команда docker compose down 
    Остановлен и удален контейнер task5-registry-1 
    Удалена сеть task5_default 
    Проект полностью "погашен" одной командой 

Проверка результата 
     ps -a | findstr task5 
не выводит ничего - все контейнеры удалены



### Правила приема

Домашнее задание выполните в файле readme.md в GitHub-репозитории. В личном кабинете отправьте на проверку ссылку на .md-файл в вашем репозитории.


