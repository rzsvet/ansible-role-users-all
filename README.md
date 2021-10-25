# Ansible role for users

## Переменные

debug - если ключено, тогда отображает информацию , но ничего не создавать
users - массив json данных содержащих с себе имя пользователя, массив групп в которые необходимо добавить публичный ключ, массив групп в которые необходимо добавить приватный ключ
add_private - нужно добавить приватный ключ
add_public - нужно добавить публичный ключ

```YAML
debug: false
users: { name: user, private: ["none"], public: ["none"] }
add_private: false
add_public: false
```

## Установка

Добавить в файл requirements.yml
```
- src: https://github.com/rzsvet/ansible-role-users-all.git
```
Перед выполнением запустить
```
ansible-galaxy install -r requirements.yml
```

## Применение

```YAML
  - name: Initial users role
    with_items:
      - { name: admin, private: ["srv"], public: ["none"] }
      - { name: user, private: ["none"], public: ["app", "db"] }
    include_role:
      name: users
    vars:
      debug: false
      user: "{{ item }}"
      add_private: "{{ item.private | intersect(group_names) | length > 0 }}"
      add_public: "{{ item.public | intersect(group_names) | length > 0 }}"
```

## Примечание

Для сохранности с помощью ansible-vault создайте файл содержащий приватный и публичный ключ хранящихся в group_vars переменных ssh_public_key, ssh_private_key
Данная роль тестовая и врядли пригодится для использования в реальных проектах. Старайтесь не использовать сторонние роли, чтобы случайно не загрузить вредный код.
