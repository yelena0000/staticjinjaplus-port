# Порт для StaticJinjaPlus

Этот репозиторий содержит порт для построения Docker-образов из проекта [StaticJinjaPlus](https://github.com/MrDave/StaticJinjaPlus). Порт позволяет собрать Docker-образы с разными базовыми образами и версиями StaticJinjaPlus.

## Требования
- Установленный Docker
- Аккаунт на Docker Hub для публикации образов

## Доступные базовые образы
- `ubuntu:24.04` (большой образ с полным окружением Ubuntu)
- `python:3.11-slim` (лёгкий образ на базе Python Slim)

## Инструкции по сборке

### 1. Клонирование репозитория
```bash
git clone https://github.com/yelena0000/staticjinjaplus-port.git
cd staticjinjaplus-port
```
### 2. Сборка образа
Укажите версию (`VERSION`) StaticJinjaPlus и выберите соответствующий Dockerfile (`Dockerfile.ubuntu` или `Dockerfile.python-slim`) в зависимости от базового образа (без указания VERSION версия по умолчанию - 0.1.1).

Допустимые значения для `VERSION`:
- тег `0.1.0`: `VERSION=0.1.0`
- тег `0.1.1`: `VERSION=0.1.1`
- ветка `main`: `VERSION=main`

#### Пример сборки с базовым образом Ubuntu (с версией по умолчанию 0.1.1)
```bash
docker build --platform linux/amd64 -f Dockerfile.ubuntu -t yourusername/static-jinja-plus .
```
#### Пример сборки с базовым образом Ubuntu (версия 0.1.0)
```bash
docker build --platform linux/amd64 -f Dockerfile.ubuntu -t yourusername/static-jinja-plus:0.1.0 --build-arg VERSION=0.1.0 .
```
#### Пример сборки с базовым образом Python Slim (ветка main)
```bash
docker build --platform linux/amd64 -f Dockerfile.python-slim -t yourusername/static-jinja-plus:main --build-arg VERSION=main .
```

### 3. Публикация на Docker Hub
```bash
docker login
docker push yourusername/static-jinja-plus:0.1.0
```
Аналогично повторите для других тегов (`0.1.1`, `latest`, `develop`, `0.1.0-slim`, `0.1.1-slim`, `slim`, `develop-slim`).
### Примечания
- Все контрольные суммы уже встроены в Dockerfile - не нужно вычислять их вручную.
- Замените `yourusername` на ваш логин на Docker Hub.
- Размеры образов: Ubuntu (~175 МБ) больше, чем Python Slim (~52 МБ), что важно для оптимизации.
