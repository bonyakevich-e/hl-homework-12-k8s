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

Для развёртывания стенда и деплоя в него веб-приложения Nextcloud достаточно запустить:
```
$ ansible-playbook -i hosts playbook.yml
```
> [!IMPORTANT]
> Предварительно, при необходимости, нужно изменить ip-адреса виртуальных машин в файлах Vagrantfile, hosts, templates/haproxy, templates/exports. В файле templates/50-vagrant.yaml нужно изменить адрес шлюза по умолчанию, который будет использоваться виртуальными машинами.

#### Cхема кластера Kubernetes после развёртывания стенда:

![otus highload scheme v6 kubernetes структурная схема](https://github.com/user-attachments/assets/adcc444f-c5d6-466b-a90b-77b9dea64d44)





#### Схема взаимодействия сервисов, которые используется в работе веб-приложения:


  
![otus highload scheme v6 kubernetes схема взаиможействия](https://github.com/user-attachments/assets/192b48ea-e92f-40f9-a113-ea26ee520ec9)


- В качестве Container Network Interface (CNI) используется __Calico__ - https://docs.tigera.io/calico/latest/about/
- В качестве ingress controller'а используется __Ingress-nginx__ - https://github.com/kubernetes/ingress-nginx
- В качестве веб-интерфейса для управления кластером используется __Kubernetes Dashboard__ - https://github.com/kubernetes/dashboard
- Для динамического провижена Persistent Volumes через Persistent Volume Claims используется __NFS Subdir External Provisioner__ - https://github.com/kubernetes-sigs/nfs-subdir-external-provisioner 
- Для развёртывания веб-приложения Nextcloud используется __Nextcloud Helm Chart__  - https://github.com/nextcloud/helm/tree/main/charts/nextcloud

Для доступа к веб-приложению Nextcloud нужно использовать адрес: https://otus.highload.com (предварительно нужно настроить резолвинг этого доменного имени во внешний ip адрес сервера balancer).

#### СКРИНШОТЫ РАЗВЁРНУТОГО СТЕНДА:

![Screenshot from 2025-03-22 17-37-46](https://github.com/user-attachments/assets/79af6261-033a-4555-8260-9997a88dd20c)

![Screenshot from 2025-03-22 17-36-34](https://github.com/user-attachments/assets/8b09aa5b-e016-4d4c-8c1f-b26dec173104)

![Screenshot from 2025-03-22 17-35-55](https://github.com/user-attachments/assets/090837ec-54de-47a9-a972-f7dfc3d48a8d)

![Screenshot from 2025-03-22 17-36-51](https://github.com/user-attachments/assets/5281a7e6-ff0a-4135-83f3-aeb2e25d72d5)

![Screenshot from 2025-03-22 17-37-07](https://github.com/user-attachments/assets/f243b432-cbb6-4bdf-8ad5-4200e8204e5c)

![Screenshot from 2025-03-22 17-37-21](https://github.com/user-attachments/assets/e1fc89b5-9411-4b97-ab64-93a8e46768af)

![Screenshot from 2025-03-22 17-38-02](https://github.com/user-attachments/assets/fddd735c-0e66-454a-927e-4a53bb416a48)

![Screenshot from 2025-03-22 17-38-19](https://github.com/user-attachments/assets/78b8c924-80f5-4963-ac62-054b4fd12ae5)

![Screenshot from 2025-03-22 17-38-52](https://github.com/user-attachments/assets/e4114d54-2060-4b94-b22d-2bb287338d7a)




