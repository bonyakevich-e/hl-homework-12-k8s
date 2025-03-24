### OTUS High Load Homework | Subject: Kubernetes. Деплой в k8s
-----------------------------
### ЦЕЛЬ:
- инсталляция k8s на виртуальные машины и скрипты автоматического деплоя конфигурации кластера веб портала из предыдущих занятий в k8s
- бэкап конфигурации кластера
-----------------------------
#### ВЫПОЛНЕНИЕ:

Стенд состоит из 8-ми виртуальных машин:
- `control1`, `control2`, `control3` - control-plane кластера Kubernetes
- `node1`, `node2`, `node3` - воркеры (workers)
- `balancer` - балансировщик/прокси HAProxy. Используется как единая точка входа для управления кластером Kubernetes и для доступа к развёрнутому приложению Nextcloud извне
- `storage` - NFS-хранилище. Используется для хранения данных Nextcloud и PostgreSQL

Для развёртнывания стенда и деплоя в него веб-приложения Nextcloud достаточно запустить:
```
$ ansible-playbook -i hosts playbook.yml
```
> [!IMPORTANT]
> Предварительно, при необходимости, нужно изменить ip-адреса виртуальных машин в файлах Vagrantfile, hosts, templates/haproxy, templates/exports. В файле templates/50-vagrant.yaml нужно изменить адреса шлюза по умолчанию, который будет использоваться виртуальными машинами.

#### Cхема кластера Kubernetes после развёртнывания стенда:

![otus highload scheme v6 kubernetes структурная схема](https://github.com/user-attachments/assets/20aea254-3ade-423b-a660-bc75d0f5745c)



#### Схема взаимодействия сервисов, которые используется в работе веб-приложения:


  
![otus highload scheme v6 kubernetes схема взаиможействия](https://github.com/user-attachments/assets/192b48ea-e92f-40f9-a113-ea26ee520ec9)

