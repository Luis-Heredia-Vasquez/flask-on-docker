# Flask on Docker

[![Build](https://github.com/Luis-Heredia-Vasquez/flask-on-docker/actions/workflows/build.yml/badge.svg)](https://github.com/Luis-Heredia-Vasquez/flask-on-docker/actions/workflows/build.yml)

This repository contains a Dockerized Flask web service built with Flask, Postgres, Gunicorn, and Nginx. The app connects to a Postgres database, serves static files through Nginx, and supports user-uploaded media files.

## Demo

![Demo of Flask image upload app](docs/demo.gif)

## Development

Build and start the development containers:

```bash
docker compose up -d --build
```

Create the database table:

```bash
docker compose exec web python manage.py create_db
```

Seed the database with a sample user:

```bash
docker compose exec web python manage.py seed_db
```

Open the app:

```bash
curl localhost:1135
```

Expected output:

```json
{"hello":"world"}
```

Stop the development containers:

```bash
docker compose down -v
```

## Production

Production environment files are ignored by Git. Create `.env.prod` and `.env.prod.db` locally before running production.

Build and start the production stack:

```bash
docker compose -f docker-compose.prod.yml up -d --build
```

Create the database table:

```bash
docker compose -f docker-compose.prod.yml exec web python manage.py create_db
```

Open the app through Nginx:

```bash
curl localhost:1135
```

Test the static file:

```bash
curl localhost:1135/static/hello.txt
```

Open the upload form:

```bash
curl localhost:1135/upload
```

Upload and view a file from the terminal:

```bash
echo "test upload" > test.txt
curl -F "file=@test.txt" localhost:1135/upload
curl localhost:1135/media/test.txt
```

Stop the production containers:

```bash
docker compose -f docker-compose.prod.yml down -v
```

## Notes

This version uses `python:3.11-slim-bookworm` and `netcat-openbsd` because the tutorial's original Debian `buster` image no longer builds cleanly with `apt-get update`.

On the lambda server, this project maps the app to port `1135`. If that port is already in use, change the left side of the port mapping in the compose file.















