# Traefik

В данном репозитории содержится минимальная конфигурация для запуска прокси, как на локальной машине, так и на боевом сервере.
Также, используется менеджер контейнеров - [Portainer](https://www.portainer.io/).

Для корректной работы прокси обязательно ознакомьтесь с [документацией](https://doc.traefik.io/traefik/).

## Инструкция по установке

### 1. Клонировать репозиторий и зайти в него
```text
git clone https://github.com/Woody174/traefik.git

cd traefik
```

### 2. Создать файл `acme.json` и изменить права на файл. Также создать файлы для логов

> В файле будут храниться все сертификаты Let's Encrypt

```console
$ touch acme.json log/traefik.log log/access.log
$ chmod 600 acme.json
```

### 3. Устанавливаем начальные данные для запуска прокси
Для этого перейдите в раздел `config` и из файла `traefik.yml.example` создайте `traefik.yml`:
```console
cp traefik.yml.example traefik.yml
```
Выполните нужные настройки

### 4. Создаём сеть в Docker
```console
docker network create traefik_proxy
```

### 5. Запускаем контейнер
```console
docker compose up -d
```
___

## Добавление хостов

### Существует 2 метода добавления хостов в traefik:

1. В файле `docker-compose.yml` добавить следующие строки, заменив <ROUTE_NAME>, <HOST_NAME> на свои названия:
    ```text
    services:
        container:
            ....
            networks:
                - traefik_proxy
            labels:
                - "traefik.enable=true"
                - "traefik.http.routers.<ROUTE_NAME>.entrypoints=websecure"
                - "traefik.http.routers.<ROUTE_NAME>.rule=Host(`<HOST_NAME>`)"
                - "traefik.http.routers.<ROUTE_NAME>.tls=true"
    
    networks:
      traefik_proxy:
        external: true
    ```
    > Более подробно читайте в документации

2. Через динамические файлы. В разделе `config/dynamic`, из файла `dynamic.yml.example` создайте файл с произвольным названием, с расширением `.yml`, и заполните его.

___

#### Инструкция не идеальная, но будет упрощаться =)
