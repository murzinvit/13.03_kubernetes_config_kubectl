## Домашнее задание к занятию "13.3 работа с kubectl" </br>
### Задание 1: проверить работоспособность каждого компонента </br>
Для проверки работы можно использовать 2 способа: port-forward и exec. Используя оба способа, проверьте каждый компонент: </br>
* сделайте запросы к бекенду; </br>
* сделайте запросы к фронту; </br>
* подключитесь к базе данных. </br>
Создал nsmaspace stage: `kubectl create namespace stage` </br>
Установил nfs через helm: `helm install nfs-server stable/nfs-server-provisioner` </br>
Запустил 2 pvc, 3 деплоя, 3 сервиса из файла: [deploy_in_stage.yaml](https://github.com/murzinvit/13.03_kubernetes_config_kubectl/blob/e9e1be417bea3dc9e804d7fd0c585f89c63bcf13/deploy_in_stage.yaml) </br>
![kubectl_get_depl](https://github.com/murzinvit/screen/blob/b786bfa6b4fd7e26abfeff0f5d3e99bcfedc9586/Kuber_kubectl_get%20deployment_3.jpg) </br>
Зайти в контейнер в deploy frontend: `kubectl exec -n stage deploy/frontend -it -- bash` </br>
Доступ к deploy: `kubectl port-forward deployment/frontend 80 80`</br>



### Задание 2: ручное масштабирование </br>
При работе с приложением иногда может потребоваться вручную добавить пару копий. Используя команду kubectl scale, попробуйте увеличить количество бекенда и фронта до 3. После уменьшите количество копий до 1. Проверьте, на каких нодах оказались копии после каждого действия (kubectl describe).

### Рабочие заметки: </br>
`kubectl expose deployment frontend -n stage --type=NodePort --name=front-svc` </br>
`kubectl expose deployment backend -n stage --type=NodePort --name=back-svc` </br>
`kubectl expose deployment db -n stage --type=NodePort --name=db-svc` </br>
