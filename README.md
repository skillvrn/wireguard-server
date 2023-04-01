# wireguard-server

Данный плейбук настроит VPN-сервер на основе протокола Wireguard. Запуск производится в Docker'е на основе публичного Docker Image'а.
Предполагается, что сервер будет находиться на DigitalOcean.
Для нормальной работы достаточно:

- 1 vCPU
- 1 GB RAM
- 25 GB Disk
- 1 IPv4 WAN IP

*Важно! Версия Ubuntu - 22.04!*

## Порядок действий

### Клонировать репозиторий

git clone git@github.com:skillvrn/wireguard-server.git

### Отредактировать необходимые файлы

- Добавить IP-адрес удаленной машины, на которой требуется поднять VPN-сервер в файл `inventory/hosts.yaml`, в директиву `hosts`
- Для дальнейшей авторизации на сервере на локальной машине создать пару ssh-ключей, публичный ключ зашифровать командой

```bash
ansible-vault encrypt_string 'SOMEW_SSH_ID_RSA_PUB' --name ssh_public_key
```

(На локальной машине для этого должен быть [установлен ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html))

Вывод команды вставить в файл `group_vars/all.yaml`

- В файле `group_vars/digitalocean.yaml` задать параметры
+ wg_user - пользователь от которого будет запуск сервера
+ vpn_path - рабочая директория
+ wg_peers_count - количество преднастроенных конфигураций для подключения к серверу (конфигурационные файлы и QR-коды)
+ docker_compose_version - версия docker-compose
+ wg_image - образ контейнера
+ wg_container_name - имя для контейнера
+ wg_internal_subnet - подсеть для выдачи IP клиентам и работы в локальной сети

### Выполнить команду

```bash
ansible-playbook playbook.yaml
```
