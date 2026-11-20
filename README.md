# CV Match — рабочее окружение

## 1. Что здесь есть и чего нет

Что запускается на этом этапе:
- база данных PostgreSQL 16.4 (сервис `db`).

Чего в проекте пока нет:
- API, интерфейса, модели.

## 2. Что нужно установить

- Git
- Docker Desktop
- Docker Compose

Команды проверки версий:
git --version
docker --version
docker compose version
docker info

## 3. Как создать локальные настройки

cp .env.example .env

Что менять можно, а что нельзя:
Можно: POSTGRES_PORT (если 55432 занят).
Нельзя: имена переменных — они читаются в compose.yaml.

## 4. Как проверить конфигурацию

docker compose config

Ожидаемо: YAML без ошибок, все переменные подставлены.

## 5. Как запустить базу

docker compose --env-file .env up -d --wait --wait-timeout 120 db

Если версия Compose не поддерживает --wait:
docker compose --env-file .env up -d db
docker compose --env-file .env ps

Ждать, пока в колонке STATUS появится (healthy).
Ожидаемо: Up ... (healthy), порт 127.0.0.1:55432->5432/tcp.

## 6. Как проверить, что база работает

docker compose ps
docker compose port db 5432
docker compose exec db psql -U edu_user -d edu_course -P pager=off -c "SELECT version();"

Ожидаемо:
Up ... (healthy)
127.0.0.1:55432
PostgreSQL 16.4 ... (1 row)

## 7. Как остановить

Остановить без удаления данных:
docker compose stop

Полностью удалить контейнер (том с данными сохраняется):
docker compose down

Удалить том с данными (осторожно — данные пропадут):
docker compose down -v

## 8. Типичные ошибки

| Симптом | Причина | Что сделать |
|---|---|---|
| `port is already allocated` | Порт 55432 занят | `docker ps --filter "publish=55432"`, остановить контейнер или поменять `POSTGRES_PORT` |
| `permission denied` | Нет прав на Docker | Запустить Docker Desktop, проверить `docker info` |
| `cannot connect to Docker daemon` | Docker Desktop не запущен | Открыть Docker Desktop, дождаться иконки «running» |
| `.env` не подхватывается | Файл в другой папке | Проверить `pwd`, что `.env` лежит в корне проекта |
| `psql: role does not exist` | Переменные не применились | `docker compose down`, `docker compose --env-file .env up -d` |


## 9. Ограничения по данным
В проекте используются только синтетические данные. 
Реальные резюме, персональные данные, логи и скриншоты в репозиторий не помещаются.


