## Домашнее задание к занятию "13.3 работа с kubectl" </br>
### Задание 1: проверить работоспособность каждого компонента </br>
Для проверки работы можно использовать 2 способа: port-forward и exec. Используя оба способа, проверьте каждый компонент: </br>
* сделайте запросы к бекенду;
* сделайте запросы к фронту;
* подключитесь к базе данных.
Создал nsmaspace stage: `kubectl create namespace stage` </br>
Запустил 2 pvc, 3 деплоя, 3 сервиса из файла: [deploy_in_stage.yaml](https://github.com/murzinvit/13.03_kubernetes_config_kubectl/blob/e9e1be417bea3dc9e804d7fd0c585f89c63bcf13/deploy_in_stage.yaml) </br>
![kubectl_get_depl](https://github.com/murzinvit/screen/blob/b786bfa6b4fd7e26abfeff0f5d3e99bcfedc9586/Kuber_kubectl_get%20deployment_3.jpg) </br>

### Задание 2: ручное масштабирование </br>
При работе с приложением иногда может потребоваться вручную добавить пару копий. Используя команду kubectl scale, попробуйте увеличить количество бекенда и фронта до 3. После уменьшите количество копий до 1. Проверьте, на каких нодах оказались копии после каждого действия (kubectl describe).
