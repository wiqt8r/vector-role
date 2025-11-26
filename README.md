# Ansible Role: vector-role

[![Ansible Role](https://img.shields.io/ansible/role/d/54134)](https://galaxy.ansible.com/ui/standalone/roles/wiqt8r/vector-role)
[![GitHub release](https://img.shields.io/github/v/release/wiqt8r/vector-role)](https://github.com/wiqt8r/vector-role/releases)

Роль для автоматизированной установки и настройки [Vector](https://vector.dev/) — высокопроизводительного инструмента для сбора, обработки и передачи логов и метрик.

## Описание роли

Роль выполняет следующие задачи:
- Создаёт системного пользователя и группу для Vector
- Скачивает и распаковывает архив с бинарным файлом Vector
- Создаёт символическую ссылку на бинарный файл
- Развертывает конфигурационный файл из шаблона
- Устанавливает unit-файл systemd
- Перезапускает и включает службу Vector

## Требования

- Ansible 2.12+
- Целевой хост с Linux (протестировано на AlmaLinux / CentOS-подобных системах)
- Доступ к скачиванию дистрибутива Vector по HTTP(S), либо предварительно скачанный архив

## Переменные роли

### Переменные по умолчанию (defaults/main.yml)

| Переменная | По умолчанию | Описание |
|------------|--------------|----------|
| `vector_version` | `"0.51.1"` | Версия Vector |
| `vector_download_url` | `"https://packages.timber.io/vector/{{ vector_version }}/vector-{{ vector_version }}-x86_64-unknown-linux-musl.tar.gz"` | URL архива с бинарным файлом Vector |
| `vector_install_dir` | `"/opt/vector"` | Каталог установки Vector |
| `vector_config_dir` | `"/etc/vector"` | Каталог для конфигурации Vector |
| `vector_data_dir` | `"/var/lib/vector"` | Каталог для данных Vector |
| `vector_bin_path` | `"/usr/local/bin/vector"` | Путь до бинарного файла (символическая ссылка) |
| `vector_user` | `"vector"` | Пользователь, от которого работает служба |
| `vector_group` | `"vector"` | Группа, от которой работает служба |
| `vector_service_enabled` | `true` | Включить автозапуск службы |
| `vector_service_state` | `"started"` | Целевое состояние службы (started, stopped и т.д.) |

### Внутренние переменные (vars/main.yml)

| Переменная | Описание |
|------------|----------|
| `vector_archive_path` | Путь на целевом хосте, куда будет загружен архив |

## Обработчики

| Обработчик | Описание |
|------------|----------|
| `restart vector` | Перезапускает службу Vector через ansible.builtin.service |

## Шаблоны

| Файл | Описание |
|------|----------|
| `templates/vector.yml.j2` | Конфигурационный файл Vector |
| `templates/vector.service.j2` | Unit-файл systemd для службы Vector |

## Пример использования

### Установка роли через requirements.yml

```yaml
---
- src: https://github.com/wiqt8r/vector-role.git
  scm: git
  version: "v0.1.0"
  name: vector-role
```

Установка роли:
```bash
ansible-galaxy install -r requirements.yml -p roles
```

### Пример playbook

```yaml
---
- name: Install Vector
  hosts: vector
  roles:
    - role: vector-role
  tags: [vector]
```

### Переопределение переменных

При необходимости параметры можно переопределить в `group_vars/vector/vars.yml` или непосредственно в playbook:

```yaml
vector_version: "0.51.1"
vector_install_dir: "/opt/vector"
```

## Зависимости

Явных зависимостей от других ролей нет. Роль может использоваться совместно с другими ролями, например, с ролью для ClickHouse.

## Лицензия

Этот проект распространяется под лицензией MIT. Подробности смотрите в файле LICENSE.

## Автор

**wiqt8r** - [GitHub](https://github.com/wiqt8r)

## Благодарности

- [Vector](https://vector.dev/) за отличный инструмент для сбора логов
