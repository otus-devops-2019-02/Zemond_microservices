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
