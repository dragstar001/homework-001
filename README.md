# homework-001
## Разворачивание базы данных (PostgreSQL + Patroni + HAProxy)
0. [Установка Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) установка производится на одном из улов кластера или деплой-узле. (Может отличаться в зависимости от используемой ОС)

```
sudo apt update && sudo apt install -y python3-pip sshpass git
pip3 install ansible
```

1. Скачиваем или клонируем этот репозиторий

```
git clone https://github.com/dragstar001/homework-001.git
```

2. Переходим в директорию с плейбуками 

```
cd homework-001/postgresql_cluster
```

3. Заполняем файл inventory

```
nano inventory
```

4. Изменяем переменные в файле vars/[main.yml](./postgresql_cluster/vars/main.yml)

```
nano vars/main.yml
```

5. Проверяем соединение со всеми хостами

```
ansible all -m ping
```

6. Запускаем плейбук разворачивания кластера

```
ansible-playbook deploy_pgcluster.yml
```

## Разворачивание приложения ( sre-course/api )
0. [Установка Helm](https://helm.sh/docs/intro/install/) установка производится на одном из улов кластера или деплой-узле. (Может отличаться в зависимости от используемой ОС)

```
curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
sudo apt-get install apt-transport-https --yes
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm
```

1. Скачиваем или клонируем этот репозиторий

```
git clone https://github.com/dragstar001/homework-001.git
```

2. Переходим в директорию проекта

```
cd homework-001
```

4. Изменяем переменные в файле weather-api-servers/[values.yaml](./weather-api-servers/values.yaml)

```
nano weather-api-servers/values.yaml
```

5. Проверяем чарт на наличие ошибок

```
helm install weather-api-servers weather-api-servers --dry-run --debug
```

6. Запускаем установку приложения

```
helm install weather-api-servers weather-api-servers
```

## Обновление приложения ( sre-course/api )
1. Изменяем версию контейнера в файле weather-api-servers/[values.yaml](./weather-api-servers/values.yaml)

```
nano weather-api-servers/values.yaml
```

2. Запускаем обновление приложения

```
helm upgrade weather-api-servers weather-api-servers
```
