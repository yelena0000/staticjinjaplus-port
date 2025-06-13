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
- Укажите версию StaticJinjaPlus (например, тег, ветку или хэш коммита) и контрольную сумму (checksum).
- Используйте соответствующий Dockerfile в зависимости от базового образа.

#### Пример сборки с базовым образом Ubuntu (версия 0.1.0)
```bash
docker build --platform linux/amd64 -f Dockerfile.ubuntu -t yourusername/static-jinja-plus:0.1.0 --build-arg VERSION=0.1.0 --build-arg CHECKSUM=3555bcfd670e134e8360ad934cb5bad1bbe2a7dad24ba7cafa0a3bb8b23c6444 .
```
#### Пример сборки с базовым образом Python Slim (ветка develop)
```bash
docker build --platform linux/amd64 -f Dockerfile.python-slim -t yourusername/static-jinja-plus:develop --build-arg VERSION=refs/heads/main --build-arg CHECKSUM=9adccb8fe17a40252df1a3acdea7edef4633b4ecaa8ba2dd5e0270f87ae43eab .
```
### 3. Публикация на Docker Hub
```bash
docker login
docker push yourusername/static-jinja-plus:0.1.0
```
### Примечания
- Замените `yourusername` на ваш логин на `Docker Hub`.
- Контрольные суммы (checksum) можно вычислить с помощью команды `sha256sum` после скачивания файла `.tar.gz` из репозитория `StaticJinjaPlus` на `GitHub`.
- Для использования последней версии укажите ветку `main` или конкретный хэш коммита.
- Сравните размеры образов: Ubuntu (~175 МБ) больше, чем Python Slim (~52 МБ), что важно для оптимизации скачивания.

### Примеры версий
- `0.1.0`: Стабильная версия, контрольная сумма 3555bcfd670e134e8360ad934cb5bad1bbe2a7dad24ba7cafa0a3bb8b23c6444
- `0.1.1`: Обновлённая версия, контрольная сумма 30d9424df1eddb73912b0e2ad5375fa2c876c8e30906bec91952dfb75dcf220b
- `develop`: Разрабатываемая ветка, контрольная сумма 9adccb8fe17a40252df1a3acdea7edef4633b4ecaa8ba2dd5e0270f87ae43eab
