# Zemond_microservices
Zemond microservices repository

Домашняя работа №14

1. Установил докер на убунту. Настроил ДНС, так как докер падал с ошибкой.
2. Попробовал следующие команды:
	2.1 docker run hello-world
	2.2 docker ps && docker ps -a
	2.3 docker images
	2.4 docker run -it ubuntu:16.04 /bin/bash
	2.5 docker ps -a --format "table {{.ID}}\t{{.Image}}\t{{.CreatedAt}}\t{{.Names}}" 
	2.6 docker start
	2.7 docker attach
	2.8 docker exec -it 2a697363a870 bash
	2.9 docker commit 2a697363a870 pankratov/ubuntu-tmp-file
	2.10 docker kill $(docker ps -q)
	2.11 docker system df
	2.12 docker rm $(docker ps -a -q)
	2.13 docker rmi $(docker images -q)
3. Вывод команды docker images направил в файл docker-1.log
4. В файле docker-1.log описал чем отличается контейнер от образа. 

Домашняя работа №15

1. Создал новый проект в GCP. Инициализировал gcloud.
2. Установил docker-mashine.
3. Создал структуру репозитория.
4. Собрал образ. 
	docker build -t reddit:latest .
5. Запустил контейнер.
	docker run --name reddit -d --network=host reddit:latest
6. Настроил файрвол.
7. Зарегестрировался на докер хабе.
8. Запушил свой образ
	 docker tag reddit:latest zemond/otus-reddit:1.0
9. Сделал необходимые проверки.

Задание со * не делал. 

Домашняя работа №16

1. Скачал и распаковал архив по ссылке, переименовал директорию.
2. Создал докер файлы в следующих директориях:
	
	post-py
	comment
	ui

3. Скачал последний образ монги и собрал образы

	docker pull mongo:latest
	docker build -t zemond/post:1.0 ./post-py
	docker build -t zemond/comment:1.0 ./comment
	docker build -t zemond/ui:1.0 ./ui

4. Создал сеть и запустил контейнеры

	docker network create reddit
	docker run -d --network=reddit --network-alias=post_db --network-alias=comment_db mongo:latest
	docker run -d --network=reddit --network-alias=post zemond/post:1.0
	docker run -d --network=reddit --network-alias=comment zemond/comment:1.0
	docker run -d --network=reddit -p 9292:9292 zemond/ui:1.0

5. Задание со * не делеал
6. Поменял содержимое ui, для уменьшения образа
7. Задание со * выполнил частично, добавил сборку образа из Alpine Linux тем самым уменьшим размер образа
8. Создал докер вольюм

	docker volume create reddit_db

9. Выключил старые контейнеры и собрал новые

	docker kill $(docker ps -q)
	docker run -d --network=reddit --network-alias=post_db --network-alias=comment_db -v reddit_db:/data/db mongo:latest
	docker run -d --network=reddit --network-alias=post zemond/post:1.0
	docker run -d --network=reddit --network-alias=comment zemond/comment:1.0
	docker run -d --network=reddit -p 9292:9292 zemond/ui:2.0

Домашняя работа №17

1. Познакомился с bridge network driver
2. Установил docker-compose
3. Создал ямлик dockercompose.yml
4. Запустил контейнеры

	export USERNAME=zemond
	docker-compose up -d 
	docker-compose ps

5. Изменил docker-compose под кейс с множеством сетей, сетевых алиасов.
6. Параметризовал с помощью переменных окружений:

	• порт публикации сервиса ui
	• версии сервисов

7. Параметризовал параметры и записал их в отдельный файл c расширением .env
8. Без использования команд source и export

9. Имя проекта можно задать через через параметр project_name в ямлике docker-compose.yml
10. Задание со * не выполнил.
