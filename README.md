![image](https://private-user-images.githubusercontent.com/188659446/405283539-83cfa9c5-2a02-41db-bbd7-559ab54600e5.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3Mzc1OTI4ODEsIm5iZiI6MTczNzU5MjU4MSwicGF0aCI6Ii8xODg2NTk0NDYvNDA1MjgzNTM5LTgzY2ZhOWM1LTJhMDItNDFkYi1iYmQ3LTU1OWFiNTQ2MDBlNS5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjUwMTIzJTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI1MDEyM1QwMDM2MjFaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1iOTdjZTVjODFkNmJiOGJiN2UzMjM1OTBhYjM3OWIzYmIxMTE3ZTIyYjE4OTcyMTA4MzdmYzkyZTg2ZmU3N2RlJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.zal3gw9T7NEkuNQ4wB8YzbiEz5Ocpc-m6KSDV0vq8_w)

# Установить Linux Oracle на VirtualBox:


Установить образ Linux

![image](https://github.com/user-attachments/assets/b75f1e46-b7d8-47dc-bf7d-49d867dde538)


Выделить 4 ядра

Выделить 4096 МБ оперативной памяти

![image](https://github.com/user-attachments/assets/fde546eb-76ce-41ac-8ed2-ec3d954e4a48)


# Установка docker

Устанавливает утилиту wget на систему

     sudo yum install wget

![image](https://github.com/user-attachments/assets/05024b7a-ef82-4193-a916-5c098613ae11)


Скачиваем файл репозитория

     sudo wget -P /etc/yum.repos.d/ https://download.docker.com/linux/centos/docker-ce.repo

![image](https://github.com/user-attachments/assets/37d52cf8-1d79-445c-9fcc-78c2a442d32e)

Устанавливаем docker

     sudo yum install docker-ce docker-ce-cli containerd.io
     
![image](https://github.com/user-attachments/assets/232a4eba-5bbb-468d-81b9-650252c64f59)


Запускаем его и разрешаем автозапуск

     sudo systemctl enable docker --now

![image](https://github.com/user-attachments/assets/d61997df-9e15-4703-86c5-59f60a7325ff)


# Установка compose

Для начала нужно убедиться в наличии пакета curl

     sudo yum install curl

![image](https://github.com/user-attachments/assets/0f8d9396-dfa6-4d0f-a848-dbef5d3bb16d)



Объявление переменной COMVER, полученной в результате curl запроса, хранящей в себе номер последней
версии Docker Compose

     COMVER=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep 'tag_name' | cut -d\" -f4)

Теперь скачиваем скрипт docker-compose последней версии, используя объявленную ранее переменную и помещаем его в каталог /usr/bin

     sudo curl -L "https://github.com/docker/compose/releases/download/$COMVER/docker-compose-$(uname -s)-$(uname -m)" -o /usr/bin/docker-compose

![image](https://github.com/user-attachments/assets/eb01d532-4948-42aa-9c8b-3ca772049bd9)



Предоставление прав на выполнение файла docker-compose

     sudo chmod +x /usr/bin/docker-compose

Проверка установленной версии Docker Compose

     sudo docker-compose --version

![image](https://github.com/user-attachments/assets/cb31e25a-447d-411b-9d5d-23f5830f2512)


# Делаем grafana

Установка git

     sudo yum install git

![image](https://github.com/user-attachments/assets/4d201493-0e0d-4513-8fb3-08dc93d53ff8)



Этот код скачивает содержимое репозитория skl256/grafana_stack_for_docker

     sudo git clone https://github.com/skl256/grafana_stack_for_docker.git



Заходит в папку - cd

     cd grafana_stack_for_docker

cd .. - возвращает в папку выше

![image](https://github.com/user-attachments/assets/af9bdae9-45b4-42fa-898c-7aa62558e3dc)

(После этого можно вставлять готовый docker-compose)

Cоздаем папки двумя разными способами

     sudo mkdir -p /mnt/common_volume/swarm/grafana/config

     sudo mkdir -p /mnt/common_volume/grafana/{grafana-config,grafana-data,prometheus-data,loki-data,promtail-data}

Выдаем права

     sudo chown -R $(id -u):$(id -g) {/mnt/common_volume/swarm/grafana/config,/mnt/common_volume/grafana}

Создаем файл

     sudo touch /mnt/common_volume/grafana/grafana-config/grafana.ini

Копирование файлов

     sudo cp config/* /mnt/common_volume/swarm/grafana/config/

Переименовывание файла

     sudo mv grafana.yaml docker-compose.yaml

![image](https://github.com/user-attachments/assets/9821a39a-1064-4220-bf31-7bf0f02df5e8)


Собрать докер (нужно запускать из папки где docker-compose.yaml)

     sudo docker compose up -d
     
![image](https://github.com/user-attachments/assets/56a95968-942b-4421-b67f-c2af12dccb08)

Опустить докер - sudo docker compose stop





# Начинаем чистку файлов

Команда открывает файл docker-compose.yaml в текстовом редакторе vi с правами суперпользователя

     sudo vi docker-compose.yaml

Что-бы что-то изменить в тесковом редакторе нужно нажать insert на клавиатуре

Затем в docker-compose нужно вставить node-exporter и удалить ненужные файлы (но у нас уже вставлен готовый докер)

![image](https://github.com/user-attachments/assets/771ba1c1-9b5f-47b1-b6dd-d86911dd41fd)

выйти не сохраняясь из vim - esc -> :q!

выйти сохраняясь из vim - esc -> :wq!

Заходим в другую папку 

     sudo cd /mnt/common_volume/swarm/grafana/config

Открываем файл prometheus.yaml в текстовом редакторе vi с правами суперпользователя

     sudo vi prometheus.yaml

![image](https://github.com/user-attachments/assets/172d29ef-ab0e-4ae7-8cff-bb8ebcfd7d46)



Далее нужно исправить targets: на exporter:9100

![image](https://github.com/user-attachments/assets/67113037-c873-4c8d-90a6-922f14350ecd)


# Делаем grafana на сайте

   •	переходим на сайт localhost:3000

                User & Password GRAFANA: admin

                Код графаны: 3000

                Код прометеуса: http://prometheus:9090

   •	в меню выбираем вкладку Dashboards и создаем Dashboard

                ждем кнопку +Add visualization, а после "Configure a new data source"

                выбираем Prometheus

                Connection

                http://prometheus:9090

   •	Authentication

                Basic authentication

                User: admin

                Password: admin

                Нажимаем на Save & test и должно показывать зелёную галочку

   •	в меню выбираем вкладку Dashboards и создаем Dashboard

                ждем кнопку "Import dashboard"

                Find and import dashboards for common applications at grafana.com/dashboards: 1860 //ждем кнопку Load

                Select Prometheus ждем кнопку "Import"



![image](https://github.com/user-attachments/assets/0c2c1e9e-a2e9-484b-909c-03f10e7282f6)







# Делаем VictoriaMetrics

Для начала зайдем в нужную папку

     cd grafana_stack_for_docker

Открываем docker-compose

     sudo vi docker-compose.yaml

После prometheus вставляем vmagent (но у нас уже вставлен готовый докер)

![image](https://github.com/user-attachments/assets/715909e4-f85b-4665-afb6-6a4f7d202db7)



Захом в connection там где мы писали http://prometheus:9090 пишем http://victoriametrics:8428 И заменяем имя из "Prometheus-2" в "Vika" нажимаем на dashboards add visualition выбираем "Vika" снизу меняем на "code" Переходим в терминал и пишем

     echo -e "# TYPE OILCOINT_metric1 gauge\nOILCOINT_metric1 0" | curl --data-binary @- http://localhost:8428/api/v1/import/prometheus

• команда отправляет бинарные данные (например, метрики в формате Prometheus) на локальный сервер, который слушает на порту 8428.

     curl -G 'http://localhost:8428/api/v1/query' --data-urlencode 'query=OILCOINT_metric1'

• команда делает запрос к API для получения данных по метрике OILCOINT_metric1

• команда выводит текст, который может быть использован для определения метрики в формате, совместимом с Prometheus

• команда выводит информацию о типе и значении этой метрики в формате, который может быть использован системой мониторинга Prometheus.



Значение 0 меняем на любое другое

Копируем переменную OILCOINT_metric1 и вставляем в query

Нажимаем run

![image](https://github.com/user-attachments/assets/03954f3d-1782-4658-9475-8d089be587c4)


Копируем переменную OILCOINT_metric1 и вставляем в code

![image](https://github.com/user-attachments/assets/8dff93c2-2a70-4a9a-8302-4670f4bd6f44)
