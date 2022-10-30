# Прокси сервер

## 1. Клонировать репозиторий и зайти в него

```text
git clone https://github.com/Woody174/traefik.git

cd traefik
```

## 2. Создать файл `acme.json` и изменить права на файл. Также создать файлы для логов

> В файле будут храниться все сертификаты Let's Encrypt

```console
touch acme.json log/traefik.log log/access.log

chmod 600 acme.json
```

## 3. Заполняем переменные окружения
```console
cp .env.example .env
```
Указываем Email для сертификатов Let's Encrypt


## 4. Создаём сеть
```console
docker network create traefik_proxy
```

## 5. Запускаем контейнер
```console
docker compose up -d
```

___

## Добавление хостов

В файле `docker-compose.yml` добавить следущие строки, заменив <ROUTE_NAME>, <HOST_NAME> на свои названия:

```text
services:
    container:
    
        ....

        networks:
            - proxy
        labels:
            - "traefik.enable=true"
            - "traefik.http.routers.<ROUTE_NAME>.entrypoints=websecure"
            - "traefik.http.routers.<ROUTE_NAME>.rule=Host(`<HOST_NAME>`)"

networks:
  proxy: {}
```
