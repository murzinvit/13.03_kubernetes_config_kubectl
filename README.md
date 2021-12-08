## Домашнее задание к занятию "13.3 работа с kubectl" </br>
### Задание 1: проверить работоспособность каждого компонента </br>
Для проверки работы можно использовать 2 способа: port-forward и exec. Используя оба способа, проверьте каждый компонент: </br>
* сделайте запросы к бекенду; </br>
* сделайте запросы к фронту; </br>
* подключитесь к базе данных. </br>
--------------------------------------------------------------
## Запуск приложения: </br>
Запустил приложене: [deploy_app.yaml](https://github.com/murzinvit/13.03_kubernetes_config_kubectl/blob/4df2fa1c427a9890efa560d56cc3a8025eb15f9e/deploy_app.yaml) </br>
![kubectl_get_depl](https://github.com/murzinvit/screen_1/blob/6230c64bc7d0beeb9857e02530fd3e783d0aa9cb/Kuber_deploy_app_last.jpg) </br>
## Тестирование frontend: </br>
Ответ на - curl frontend: `kubectl exec deploy/frontend -it -- curl localhost:80` </br>
![exec_frontend](https://github.com/murzinvit/screen_1/blob/89ff2d0c91b05822e5458604bae9a236e4e4feba/Kuber_curl_front_last.jpg) </br>

Ответ от frontend после port-forward на порт 3000: `kubectl port-forward deploy/frontend 3000:80`</br>
![Kuber_curl_list](https://github.com/murzinvit/screen_1/blob/0d839c304aa53215021b871e776f0ccd9151b5c3/Kuber_forward_front_last.jpg) </br>

## Тестирование backend: </br>
Ответ на - curl backend: `kubectl exec deploy/backend -it -- curl localhost:9000` </br>
![exec_backend](https://github.com/murzinvit/screen_1/blob/c7f5405f04459820f5373dc9d8114b4d70c374fc/Kuber_curl_backend_last.jpg) </br>
Ответ от backend после port-forward на порт 3001: `kubectl port-forward deploy/backend 3001:9000` </br>
![exec_backend_faild](https://github.com/murzinvit/screen_1/blob/0d839c304aa53215021b871e776f0ccd9151b5c3/Kuber_farward_backend_last.jpg) </br>

Т.к базу данных через curl не проверить. Можно проверить в контейнере в psql: `kubectl exec deploy/db -it -- bash` </br>
![db_answer](https://github.com/murzinvit/screen/blob/eff71e5445754d45a38055080e995fcee2ccd349/Kuber_exec_postres_news.jpg) </br>
Через port-forward тоже не проверить т.к действуйт в пределах localhost, поэтому можно проверить через expose: </br>
`kubectl expose deploy/db --type=NodePort --name=db-svc`</br>
Порт для доступа 30807: </br>
![expose_last](https://github.com/murzinvit/screen_1/blob/84f19ff2151dda0915e3ef7d2b26b3ac8fb00e14/Kuberr_db_expose_last.jpg) </br>
Можно проверить из HeidiSQL: </br>
![heidi_last](https://github.com/murzinvit/screen_1/blob/ee886709fb7294566cd077afdbd88b97a3959f8f/Kuber_heidi_last.jpg) </br>
Заходим в СУБД: </br>
![heidi_last](https://github.com/murzinvit/screen_1/blob/b017526433c6c10611ffe5899158f50b35b267b6/Kuber_heidi_result_last.jpg) </br>

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
