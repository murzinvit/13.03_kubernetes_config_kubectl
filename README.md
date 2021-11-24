## Домашнее задание к занятию "13.3 работа с kubectl" </br>
### Задание 1: проверить работоспособность каждого компонента </br>
Для проверки работы можно использовать 2 способа: port-forward и exec. Используя оба способа, проверьте каждый компонент: </br>
* сделайте запросы к бекенду; </br>
* сделайте запросы к фронту; </br>
* подключитесь к базе данных. </br>
--------------------------------------------------------------
Создал nsmaspace stage: `kubectl create namespace stage` </br>
Установил nfs через helm: `helm install nfs-server stable/nfs-server-provisioner` </br>
Запустил 2 pvc, 3 деплоя, 3 сервиса из файла: [deploy_in_stage.yaml](https://github.com/murzinvit/13.03_kubernetes_config_kubectl/blob/e9e1be417bea3dc9e804d7fd0c585f89c63bcf13/deploy_in_stage.yaml) </br>
![kubectl_get_depl](https://github.com/murzinvit/screen/blob/b786bfa6b4fd7e26abfeff0f5d3e99bcfedc9586/Kuber_kubectl_get%20deployment_3.jpg) </br>
Зайти в контейнер в frontend: `kubectl exec -n stage deploy/frontend -it -- bash` </br>
![exec_frontend](https://github.com/murzinvit/screen/blob/17fe33b395b9936f88da1315b64e91faad37992e/Kuber_exec_frontend.jpg) </br>
Ответ от frontend: `kubectl port-forward -n stage deployment/frontend 30000:80`</br>
![Kuber_curl_list](https://github.com/murzinvit/screen/blob/aa254c577e166c513da28479b4f8ab8fe50d02e4/Kuber_curl_list_30000.jpg) </br>
Зайти в контейнер в backend: `kubectl exec -n stage deploy/backend -it -- bash` </br>
![exec_backend](https://github.com/murzinvit/screen/blob/5744dbea9d8ee0f27f9d568b31eb587153d3c861/Kuber_exec_backend.jpg) </br>
Ответ от backend: `kubectl port-forward -n stage deployment/backend 30000:9000`</br>
![backend_answer](https://github.com/murzinvit/screen/blob/36f3a07c8fdf971cc60f860cb48d552ece71bc5a/Kuber_curl_backend.jpg) </br>
Зайти в контейнер в db: `kubectl exec -n stage deploy/db -it -- bash` </br>
![db_answer](https://github.com/murzinvit/screen/blob/eff71e5445754d45a38055080e995fcee2ccd349/Kuber_exec_postres_news.jpg) </br>
Ответ от db: `kubectl port-forward -n stage deployment/db 30000:5432`</br>
![Kuber_curl_postgres](https://github.com/murzinvit/screen/blob/eff71e5445754d45a38055080e995fcee2ccd349/Kuber_curl_postgres.jpg) </br>

### Задание 2: ручное масштабирование </br>
При работе с приложением иногда может потребоваться вручную добавить пару копий. Используя команду kubectl scale, попробуйте увеличить количество бекенда и фронта до 3. После уменьшите количество копий до 1. Проверьте, на каких нодах оказались копии после каждого действия (kubectl describe).
Масштабировать frontend: `kubectl scale --replicas=2 -n stage deployment/frontend` </br>
![scale_frontend](https://github.com/murzinvit/screen/blob/f7cca75d9fd7718eb6f937451c408dd6e0a673be/Kuber_scale_frontend.jpg) </br>
На какой ноде запустился pod: `kubectl describe pod -n stage`</br>
![run_in_ingress_node](https://github.com/murzinvit/screen/blob/114e58c86985191bbab70a7116f685a5df20c7ea/Kuber_run_in_ingress_node.jpg) </br>
Масштабировать backend: `kubectl scale --replicas=2 -n stage deployment/backend` </br>
![scale_frontend](https://github.com/murzinvit/screen/blob/6eda183769bc79136042f880a92282b51828066f/Kuber_scale_backend.jpg) </br>
На какой ноде запустился pod: `kubectl describe pod -n stage`</br>
![run_in_ingress_node](https://github.com/murzinvit/screen/blob/4411b02c777564d18ee03b2335e0e49abbdf3a45/Kuber_backend_run_in_ingress.jpg) </br>

### Рабочие заметки: </br>
`kubectl expose deployment frontend -n stage --type=NodePort --name=front-svc` </br>
`kubectl expose deployment backend -n stage --type=NodePort --name=back-svc` </br>
`kubectl expose deployment db -n stage --type=NodePort --name=db-svc` </br>
