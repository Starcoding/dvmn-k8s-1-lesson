# dvmn-k8s-1-lesson
Данный проект предназначается для 1 урока модуля Kubernutes от DVMN.  
Данный проект представляет из себя сайт на Django, который разворачивается в локальном кластере kubernutes через kubectl и minikube.  
Главная цель - научиться разврачивать локальный кластел k8s, познать азы работы с k8s и научиться создавать манифест файлы.

## Деплой
- Установите ```minikube``` и ```kubectl``` (Опционально установите docker или иное средство визуализации)
- Скачайте данный репозиторий и перейдите в консоли в папку с ним  
- Для ```локальной БД Postgres``` установите [Helm](https://helm.sh/) и [Helm Chart for Postgres](https://artifacthub.io/packages/helm/bitnami/postgresql)  
- В файле ```django-config.yaml``` измените переменные окружения (Для использования БД Postgres установленной в локальном кластере k8s следуйте инструкциям из предыдущего шага)  

### Поочередно запустите следующие команды
- ```kubectl apply -f django-config.yaml  ```
- ```kubectl apply -f django-service-and-deployment.yaml  ```
- ```kubectl apply -f ingress-star-burger.yaml  ```
- ```kubectl apply -f clear-sessions.yaml  ```

Также при первом запуске (или при обновлении ```image``` взятого за основу) необходимо совершить миграции, для этого запустите команду:  
```kubectl apply -f clear-sessions.yaml``` - данная команда запустит работу, которая совершит все необходимые миграции в БД  

Создайте суперпользователя для Django выполнив команды:  
- ```kubectl exec <название вашего пода с django-deployment> -it -- bash```  - название пода можно узнать с помощью ```kubectl get pods```
- ```python3 manage.py createsuperuser```

В файле ```/etc/hosts``` добавьте строку ```127.0.0.1 star-burger.test``` для переадресации в браузере на ваш локальный кластер k8s.  
Запустите команду ```minikube tunnel``` и перейдите в браузере по адресу ```star-burger.test```. Вы должны увидеть страничку администратора Django.  
