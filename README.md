# Дипломный практикум. Марков Федор.

### Порядок разворота инфраструктуры

Шаг 1. С помощью репозитория [aws_creation](https://github.com/shhhowtime/aws_creation "Ссылка на репозиторий") развернуть инфраструктуру на AWS. Данные действия нужно выполнять вручную, так как в репозитории содержатся крупные файлы, которые не хотелось бы добавлять в Git репозиторий (готовые конфигурации Gitlab, папка со служебными образами, хранящимися в Docker Registry). В случае изменения уже готовой инфраструктуры можно воспользоваться Terraform Cloud:
![Terraform Cloud](https://raw.githubusercontent.com/shhhowtime/diplom/main/1.png)

Шаг 2. Во время создания инфраструктуры будет создан EC2 инстанс для Gitlab, 3 EC2 инстанса для тестового окружения (1 master и 2 child) и 6 EC2 инстансов для рабочего окружения (3 master и 3 child). При этом во время инициализации инстанса для gitlab будет запущен скрипт для следующих задач:

- [Установка docker, registry и gitlab на данных хост.](https://github.com/shhhowtime/aws_creation/blob/91d7968e30cd56abc17af748d19593d7995e90fa/git_hosts.tf#L24) Установка производится с помощью репозитория [gitlab](https://github.com/shhhowtime/gitlab "Ссылка на репозиторий");
- [Настройка kubernetes кластера для окружений stage и prod](https://github.com/shhhowtime/aws_creation/blob/91d7968e30cd56abc17af748d19593d7995e90fa/git_hosts.tf#L31). Данный репозиторий является клоном репозитория kubespray с добавлением возможности деплоя на динамический инвентарь ec2.

Шаг 3. После создания gitlab появится доступ к [тестовому приложению](http://elb-markov-gitlab-712017750.eu-central-1.elb.amazonaws.com/gitlab-instance-1257b7f2/myapp "Ссылка на репозиторий"). Учётные данные для доступа к данному приложению будут в ответе на сайте неталогии. Для данного приложения также настроен Gitlab CI для веток stage и prod, соответствующих окружению.

Шаг 4. Также в gitlab имеется ссылка на [конфигурацию для мониторинга](http://elb-markov-gitlab-712017750.eu-central-1.elb.amazonaws.com/gitlab-instance-1257b7f2/deploy-monitoring "Ссылка на репозиторий"). Ссылки на готовые панели мониторинга: [stage](http://elb-markov-kube-stage-1341621630.eu-central-1.elb.amazonaws.com:3000/d/TNADuWTmz/node-exporter?orgId=1) и [prod](http://elb-markov-kube-prod-645643444.eu-central-1.elb.amazonaws.com:3000/d/TNADuWTmz/node-exporter?orgId=1&from=now-5m&to=now)  Учётные данные совпадают с учётными данными для gitlab. Настроены дашборды и тестовый алерт в discord для prod окружения. Примерный вид уведомления:
