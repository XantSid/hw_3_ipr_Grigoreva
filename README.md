## Шаблон для третьего ДЗ по ИПР

Условие дз доступно по [ссылке](https://github.com/Nikfilk2030/missing_semester_hse_cs/blob/main/2026/amcs/year_1/hw_3/README.md)

# Запуск приложения с помощью Docker

## Сборка Docker-образа

Для сборки образа приложения выполните команду из корневой директории проекта:

```bash
docker build -t counter-app .
```

## Запуск приложения без Docker Compose

```bash
docker run -d --name counter -p 8080:8000 \
  -e GREETING="Привет из Docker!" \
  -e STORAGE=sqlite \
  counter-app
```

После запуска приложение будет доступно по адресу `http://localhost:8080`.

Остановить контейнер:

```bash
docker stop counter
```

Удалить контейнер:

```bash
docker rm counter
```

## Запуск с помощью Docker Compose

Для запуска приложения и Redis выполните:

```bash
docker compose up -d --build
```

В проекте используются два сервиса:

- `app` — веб-приложение;
- `redis` — хранилище счётчика.

Приложение подключается к Redis по адресу `redis://redis:6379/0`. Для сохранения данных Redis используется именованный том `redis-data`, а запись на диск включена с помощью Append Only File (AOF).

Проверить состояние контейнеров:

```bash
docker compose ps
```

Посмотреть логи:

```bash
docker compose logs
```

Остановить и удалить контейнеры и сеть:

```bash
docker compose down
```

Именованный том с данными Redis при этом сохраняется. Не используйте `docker compose down -v`, если хотите сохранить счётчик.

## Проверка работы приложения

Получить текущее значение счётчика:

```bash
curl http://localhost:8080/
```

Увеличить счётчик:

```bash
curl -X POST http://localhost:8080/increment
```

Проверить работоспособность HTTP-сервера:

```bash
curl -i http://localhost:8080/healthz
```

Проверить готовность хранилища:

```bash
curl -i http://localhost:8080/readyz
```

## Переменные окружения

| Переменная | Назначение | Значение в Compose |
|---|---|---|
| `PORT` | Порт приложения внутри контейнера | `8000` |
| `GREETING` | Приветственное сообщение | Задаётся через `.env` |
| `STORAGE` | Тип хранилища | `redis` |
| `REDIS_URL` | Адрес Redis | `redis://redis:6379/0` |
| `APP_PORT` | Порт приложения на компьютере | `8080` |