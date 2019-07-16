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

Домашнаяя работа №18

0. Подготовил машину в GCP
1. Произвел настройку GitLab, создал группу и проект
2. Зарегистрировал раннер
3. Настроил файл .gitlab-ci.yml в котором указываются последовательность шагов для пайплана из описание и настройки.
4. Закомитил в гитлаб и посмотрел как работают пайплайны

Домашняя работа №19

1. Создал правила фаервола для Prometheus и Puma

gcloud compute firewall-rules create prometheus-default --allow tcp:9090
gcloud compute firewall-rules create puma-default --allow tcp:9292

2. Создал docker-host start_docker_machine.sh
3. Запустил Prometheus

docker run --rm -p 9090:9090 -d --name prometheus prom/prometheus:v2.1.0

4. Конфигурация
	4.1 В директории monitoring/prometheus создал файл prometheus.yml c настройками
	4.2 В директории prometheus собрал Docker образ:

		export USER_NAME=zemond
		docker build -t $USER_NAME/prometheus .

5. Собрал images при помощи скриптов docker_build.sh в директории каждого сервиса

	for i in post-py ui comment; do cd src/$i; bash docker_build.sh; cd -; done

6. Запустил наш Prometheus совместно с микросервисами. 

	docker-compose up -d

Ссылка на докер хаб

https://cloud.docker.com/u/zemond/repository/list

Запушенные контейнеры:

zemond / prometheus
zemond / post
zemond / comment
zemond / ui

Домашняя работа №20

Сделал основное задание
Создал docker-host start_docker_machine.sh

Через gcp настроил правила для firewall

gcloud compute firewall-rules create grafana-default --allow tcp:3000
gcloud compute firewall-rules create cadvisor-default --allow tcp:8080
gcloud compute firewall-rules create cloudprober-default --allow tcp:9313
gcloud compute firewall-rules create prometheus-alertmanager --allow tcp:9093
gcloud compute firewall-rules create prometheus-default --allow tcp:9090

Выполнил docker-compose up -d 
Запустил мониторинг и алерты через docker-compose docker/docker-compose-monitoring.yml

Настроил сбор метрик докера с помощью cAdvisor
Настроил  Grafana и AlertManager
Настроил дашборды для сбора метрик приложения и бизнес метрик
Настроил алерты на остановку сервисов
Настроил интеграцию с тестовым Slack-чатом 

Домашняя работа №21

1. Скачал архивы
	wget https://github.com/express42/reddit/archive/logging.zip && unzip -x logging.zip
2. Сделал сборку
	export USER_NAME=zemond && for i in ui post-py comment; do cd src/$i; bash docker_build.sh; cd -; done
3. Настроил окружение 
4. Создал отдельный docker-compose-logging.yml
5. В отдельной директории создал докер файл для fluentd:v0.12 с файлом конфигурации fluent.conf
6. Собрал образ
	docker build -t $USER_NAME/fluentd .
7. Поднял инфраструтуру 
8. Посмотрел работу логов и парсинга
9. Посмотрел на трейсинг через zipkin

Домашняя работа №22

1. Описал манифесты:

	post-deployment.yml
	ui-deployment.yml
	comment-deployment.yml
	mongo-deployment.yml

2. Прошел Kubernetes The Hard Way. Файлы сложил в директорию the_hard_way.

Домашняя работа №23

1. Установил kubectl 
	
	sudo apt-get install kubectl

2.Установил Minikube, запустил ui

	kubectl apply -f ui-deployment.yml

3. пробросим сетевые порты что бы открыть в браузере http://localhost:8080

	kubectl get pods --selector component=ui
	kubectl port-forward <pod-name> 8080:9292

4. Выполнил:

	kubectl apply -f comment-deployment.yml kubectl get pods --selector component=comment -
	kubectl apply -f post-deployment.yml kubectl get pods --selector component=post kubectl get deployment
	kubectl apply -f mongo-deployment.yml

5. Создал объекты service для ui-service.yml, post-service.yml и comment-service.yml

6. Создал кластер в гугл

	gcloud container clusters get-credentials standard-cluster-1 --zone europe-west1-d --project docker-239201
	kubectl config view 
	kubectl apply -f reddit/dev-namespace.yml -n dev -f reddit/ 

7. Настроил брендмауэр для портов tcp:30000-32767

8. Для определения порта публикации сервиса ui

	kubectl describe service ui -n dev | grep NodePort

Домашняя работа №24

Проскейлим в 0 сервис, который следит, чтобы dns-kube подов всегда хватало 

	kubectl scale deployment --replicas 0 -n kube-system kube-dnsautoscaler

Проскейлим в 0 сам kube-dns 

	kubectl scale deployment --replicas 0 -n kube-system kube-dns

Вернем kube-dns-autoscale в исходную

	kubectl scale deployment --replicas 1 -n kube-system kube-dnsautoscaler
	kubectl apply -f ui-service.yml -n dev
	kubectl get service  -n dev --selector component=ui
	kubectl get service -n dev --selector component=ui

Ingress

Создадим новый конфиг ui-ingress.yml 

	kubectl apply -f ui-ingress.yml -n dev
	kubectl get ingress -n dev 

Поправим ui-service.yml

	kubectl apply -f ui-service.yml -n dev

secret

	kubectl get ingress -n dev

Далее подготовим сертификат используя IP как CN

	openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout tls.key -out tls.crt -subj "/CN=35.245.128.95" 

И загрузим сертификат в кластер kubernetes
	
	kubectl create secret tls ui-ingress --key tls.key --cert tls.crt -n dev

Проверим командой

	kubectl describe secret ui-ingress -n dev

Правим ui-ingress.yml 

	kubectl apply -f ui-ingress.yml -n dev

NetworkPolicy

	gcloud beta container clusters list
	gcloud beta container clusters update cluster-3 --zone=us-central1-a --update-addons=NetworkPolicy=ENABLED
	gcloud beta container clusters update cluster-3 --zone=us-central1-a --enable-network-policy

Создаем mongo-network-policy.yml

	kubectl apply -f mongo-network-policy.yml -n dev

Создадим mongo-deployment.yml

Volume

	gcloud compute disks create --size=25GB --zone=us-central1-a reddit-mongo-disk
	kubectl apply -f mongo-deployment.yml -n dev
	kubectl delete deploy mongo -n dev
 
PersistentVolume

	kubectl apply -f mongo-volume.yml -n dev

создадим mongo-claim.yml

	kubectl apply -f mongo-claim.yml -n dev
	kubectl describe storageclass standard -n dev 

Обновим mongo-deployment.yml

	kubectl apply -f mongo-deployment.yml -n dev

StorageClass

создадим storage-fast.yml
	
	kubectl apply -f storage-fast.yml -n dev

создадим mongo-claim-dynamic.yml

	kubectl apply -f mongo-claim-dynamic.yml -n dev
	kubectl apply -f mongo-deployment.yml -n dev

kubectl get persistentvolume -n dev
NAME                                       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM                   STORAGECLASS   REASON   AGE
pvc-m01885e8-9783-12e9-7cd2-43010a84020f   15Gi       RWO            Delete           Bound       dev/mongo-pvc           standard                12m40s
pvc-a20sd404-9784-12e9-7cd2-43010a84020f   10Gi       RWO            Delete           Bound       dev/mongo-pvc-dynamic   fast                    56s
reddit-mongo-disk                          25Gi       RWO            Retain           Available                                                   7m

Домашняя работа №25

Helm
Создал файл tiller.yml
	
	kubectl apply -f tiller.yml
	helm init --service-account tiller
	kubectl get pods -n kube-system --selector app=helm

Сделал переконфигурацию

	helm install --name test-ui-1 ui/

Создал файл _helpers.tpl в папках templates сервисов ui, post и comment
Вставил функцию “.fullname” в каждый _helpers.tpl файл.

GitLab + Kubernetes

	helm repo add gitlab https://charts.gitlab.io 
	helm fetch gitlab/gitlab-omnibus --version 0.1.37 --untar
	cd gitlab-omnibus
	helm install --name gitlab . -f values.yaml

В директории Gitlab_ci/ui:
Инициализировал локальный git-репозиторий
Добавил удаленный репозиторий
Закоммитил и отправил в gitlab

	git init
	git remote add origin http://gitlab-gitlab/chromko/ui.git
	git add .
	git commit -m “init”
	git push origin master

Перенес содержимое директории Charts (папки ui, post, comment, reddit) в Gitlab_ci/reddit-deploy
Запушил reddit-deploy в gitlab-проект reddit-deploy
Скопировал полученный файл .gitlab-ci.yml для ui в репозитории для post и comment.
Проверил, что динамическое создание и удаление окружений работает и с ними как ожидалось
Файлы .gitlab-ci.yml, полученные в ходе работы, поместил в папку с исходниками для каждой компоненты приложения.
Файл .gitlab-ci.yml для reddit-deploy поместил в charts.
Все изменения, которые были внесены в Chart’ы - перенес в папку charts, созданную вначале.

Домашняя работа №26

Из Helm-чарта установил ingress-контроллер nginx

	helm install stable/nginx-ingress --name nginx

По плану изучил следующие:

-Развернул Prometheus в k8s
-Настроил Prometheus и Grafana для сбора метрик
-Настроил EFK для сбора логов

Prometheus поставил с помощью Helm чарта. Загрузил prometheus локально в Charts каталог

	cd kubernetes/charts && helm fetch —-untar stable/prometheus 

Запустил Prometheus в k8s из charsts/prometheus

	helm upgrade prom . -f custom_values.yml --install
	helm upgrade prom . -f custom_values.yml --install
 
Запустил приложение из helm чарта reddit

	helm upgrade reddit-test ./reddit —install
	helm upgrade production --namespace production ./reddit --install
	helm upgrade staging --namespace staging ./reddit —install

Поставил также grafana с помощью helm (ссылка на gist)

	helm upgrade --install grafana stable/grafana --set "adminPassword=admin" \
	--set "service.type=NodePort" \
	--set "ingress.enabled=true" \
	--set "ingress.hosts={reddit-grafana}"

	kubectl label node gke-cluster-1-big-pool-b4209075-tvn3 elastichost=true
 
Запустил стек в моем k8s

	kubectl apply -f ./efk
	
Kibana поставил из helm чарта (ссылка на gist)
	
	helm upgrade --install kibana stable/kibana \
	--set "ingress.enabled=true" \
	--set "ingress.hosts={reddit-kibana}" \
	--set "env.ELASTICSEARCH_URL=http://elasticsearch-logging:9200" \
	--version 0.1.1
