# Ansible Role: Shadowsocks

Ansible-роль для установки и настройки Shadowsocks на хостах. Поддерживает дистрибутивы Gentoo, Debian и Ubuntu.

Используемый дистрибутив Shadowsocks - [shadowsocks-libev](https://github.com/shadowsocks/shadowsocks-libev)

## Требования
- Ansible 2.10+
- sudo-права на целевых хостах
## Структура инвентаря
Этот инвентарь Ansible предоставляет информацию о хостах, на которых будет устанавливаться и настраиваться Shadowsocks при использовании соответствующей роли.
```ini
[netherlands]
HOST_IP ansible_user=root

[poland]
HOST_IP ansible_user=root

[russia]
HOST_IP_1 ansible_user=root
HOST_IP_2 ansible_user=root

[vpnservers:children]
netherlands
poland
russia
```
## Переменные роли

Все чувствительные переменные роли определяются в файле `vars.yml`,
который должен содержать следующие переменные:
- `shadowsocks_port`: порт, который будет использоваться Shadowsocks-сервером (по умолчанию: 8388).
- `shadowsocks_password`: пароль для подключения к Shadowsocks-серверу (задайте свой уникальный пароль).

Остальные переменные задаются в playbook

## Шифрование переменных с использованием Ansible Vault

Для безопасного хранения чувствительных данных, таких как пароль Shadowsocks, используйте Ansible Vault для создания и редактирования файла `vars.yml`.
#### Пример:
```bash
ansible-vault create vars.yml
```

#### Содержимое файла vars.yml
```yml
shadowsocks_port: 8388
shadowsocks_password: "password"
```

## Пример использования

```yaml
- hosts: vpnservers
  vars_files:
    - vars.yml

  roles:
    - shadowsocks

  become: yes

  vars:
    shadowsocks_server_port: "{{ shadowsocks_port }}"
    shadowsocks_password: "{{ shadowsocks_password }}"
    shadowsocks_encryption_method: "aes-256-cfb"
    shadowsocks_timeout: 300
```

## Запуск
Заполните файл `inventory`:
`production.example` переименуйте в `production` и заполните своими данными.

Затем запустите `playbook`:
```bash
ansible-playbook -i production site.yml --ask-vault-pass
```
