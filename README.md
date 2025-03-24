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

![otus highload scheme v6 kubernetes структурная схема](https://github.com/user-attachments/assets/adcc444f-c5d6-466b-a90b-77b9dea64d44)





#### Схема взаимодействия сервисов, которые используется в работе веб-приложения:


  
![otus highload scheme v6 kubernetes схема взаиможействия](https://github.com/user-attachments/assets/192b48ea-e92f-40f9-a113-ea26ee520ec9)


- В качестве Container Network Interface (CNI) используется __Calico__ - https://docs.tigera.io/calico/latest/about/
- В качестве ingress controller'а используется __Ingress-nginx__ - https://github.com/kubernetes/ingress-nginx
- В качестве веб-интерфейса для управления кластером используется __Kubernetes Dashboard__ - https://github.com/kubernetes/dashboard
- Для динамического провижена Persistent Volumes через Persistent Volume Claims используется __NFS Subdir External Provisioner__ - https://github.com/kubernetes-sigs/nfs-subdir-external-provisioner 
- Для развёртывания веб-приложения Nextcloud используется __Nextcloud Helm Chart__  - https://github.com/nextcloud/helm/tree/main/charts/nextcloud

Для доступа к веб-приложению Nextcloud нужно использовать адрес: https://otus.highload.com (предварительно нужно настроит резолвинг этого доменного имени во внешний ip адрес сервера balancer).

#### СКРИНШОТЫ РАЗВЁРНУТОГО СТЕНДА:
![Screenshot from 2025-03-22 17-38-52](https://github.com/user-attachments/assets/9092a780-fe14-467f-b7a5-b813cb224b3c)
![Screenshot from 2025-03-22 17-38-19](https://github.com/user-attachments/assets/79d85cac-a9cc-46c8-80ca-04ee52bcc1c2)
![Screenshot from 2025-03-22 17-38-02](https://github.com/user-attachments/assets/ee25b1dc-c3a5-4776-9cdc-3d1b0e851795)
![Screenshot from 2025-03-22 17-37-46](https://github.com/user-attachments/assets/6e9f4a8d-e702-481b-93c2-fbda1c6ce1ee)
![Screenshot from 2025-03-22 17-37-21](https://github.com/user-attachments/assets/0a08fde2-7a2f-46b6-8b34-293a33c94e89)
![Screenshot from 2025-03-22 17-37-07](https://github.com/user-attachments/assets/4c1ec5f9-b7b9-4754-88b8-dd6b92d8a598)
![Screenshot from 2025-03-22 17-36-51](https://github.com/user-attachments/assets/0cebed38-3d16-46c5-ac99-0e8d950c7195)
![Screenshot from 2025-03-22 17-36-34](https://github.com/user-attachments/assets/4af85065-5a1b-4f9f-a501-2c49495d0885)
![Screenshot from 2025-03-22 17-35-55](https://github.com/user-attachments/assets/2b914f05-c58b-4175-81ec-84acf90dd6db)
