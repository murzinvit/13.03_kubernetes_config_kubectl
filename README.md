## Домашнее задание к занятию "13.3 работа с kubectl" </br>
### Задание 1: проверить работоспособность каждого компонента </br>
Для проверки работы можно использовать 2 способа: port-forward и exec. Используя оба способа, проверьте каждый компонент: </br>
* сделайте запросы к бекенду; </br>
* сделайте запросы к фронту; </br>
* подключитесь к базе данных. </br>
--------------------------------------------------------------
## Запуск приложения: </br>
Запустил приложене: [microservices-demo](https://github.com/microservices-demo/microservices-demo) </br>
![kubectl_get_depl](https://github.com/murzinvit/screen_1/blob/c684d6d406141eb02a53752080bd4c84ef579e28/Kuber_deploy_list_sock_shop.jpg) </br>
## Тестирование frontend: </br>
Ответ на - curl frontend: `kubectl exec deploy/frontend -it -- curl localhost:80` </br>
![exec_frontend]() </br>

Ответ от frontend после forward: `kubectl port-forward deploy/frontend 3000:80`</br>
![Kuber_curl_list]() </br>

## Тестирование backend: </br>
Ответ на - curl backend: `kubectl exec deploy/backend -it -- curl localhost:9000` </br>
![exec_backend](https://github.com/murzinvit/screen_1/blob/190c9899800f01f083f7aaf9aaee993bc1f003d2/Kuber_curl_backend_faild.jpg) </br>
При запуске приложения в бекенде выдаёт ошибку: [log_file]() из backend </br>
![exec_backend_faild](https://github.com/murzinvit/screen_1/blob/190c9899800f01f083f7aaf9aaee993bc1f003d2/Kuber_curl_backend_faild.jpg) </br>

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
https://github.com/netology-code/devkub-homeworks </br>
https://www.dmosk.ru/miniinstruktions.php?mini=postgresql-users </br>
https://github.com/microservices-demo/microservices-demo </br>
`kubectl expose deployment frontend -n stage --type=NodePort --name=front-svc` </br>
`kubectl expose deployment backend -n stage --type=NodePort --name=back-svc` </br>
`kubectl expose deployment db -n stage --type=NodePort --name=db-svc` </br>
