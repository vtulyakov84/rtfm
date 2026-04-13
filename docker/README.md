### Запуск контейнера Docker с postgresql

Docker-compose считывает значение переменных окружения из файла `.env`, располагаемого в одном каталоге с файлом `docker-compose.yml`.

`./env`
```ini
POSTGRES_CONTAINER_NAME=cnt_postgres
POSTGRES_DB=postgres
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
POSTGRES_PORT=5432

DB_URI=postgres://${POSTGRES_USER}:${POSTGRES_PASSWORD}@${POSTGRES_CONTAINER_NAME}:${POSTGRES_PORT}/${POSTGRES_DB}
MIGRATIONS_DIR=./migrations
```

`./docker-compose.yml`
```yml
services:
  postgres:
    image: postgres:15
    container_name: ${POSTGRES_CONTAINER_NAME}
    environment:
      POSTGRES_USER: ${POSTGRES_USER}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
      POSTGRES_DB: ${POSTGRES_DB:-postgres}  # рекомендуется добавить
    ports:
      - "${POSTGRES_PORT}:5432"
    volumes:
      - ./postgres/data:/var/lib/postgresql/data
    restart: unless-stopped  # ← исправлено: на той же строке
    networks:
      - app-network

networks:
  app-network:
    driver: bridge
```

```bash
# Запуск контейнера
$ sudo docker compose up -d

# Просмотр запушенного контейнера
$ sudo docker ps

# Остановка контейнера по id или имена
$ sudo docker stop <container-id>
$ sudo docker stop <container-name>
```

Запущенный контейнер доступен для подключения: `localhost:5432`, `postgres/postgres`.