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
### 2. Получение контрольной суммы (SHA256)
Контрольная сумма (SHA256-хэш) необходима для проверки целостности загружаемого архива StaticJinjaPlus. Выполните следующие шаги для её вычисления:

1. Скачайте архив с нужной версией или веткой:
   
Для тега `0.1.0`:
```bash
wget https://github.com/MrDave/StaticJinjaPlus/archive/refs/tags/0.1.0.tar.gz
```
Для тега `0.1.1`:
```bash
wget https://github.com/MrDave/StaticJinjaPlus/archive/refs/tags/0.1.1.tar.gz
```
Для ветки `main` (для тега `develop`):
```bash
wget https://github.com/MrDave/StaticJinjaPlus/archive/refs/heads/main.tar.gz
```

2. Вычислите SHA256-хэш:
Используйте команду `sha256sum`:
```bash
sha256sum 0.1.0.tar.gz
````
```bash
sha256sum 0.1.1.tar.gz
````
```bash
sha256sum main.tar.gz
```
Пример результата для `0.1.0.tar.gz`: `3555bcfd670e134e8360ad934cb5bad1bbe2a7dad24ba7cafa0a3bb8b23c6444`.

3. Скопируйте полученный хэш (первые 64 символа) для использования в команде сборки (необходимо будет подставить это значение после `CHECKSUM=`).

4. Удалите временный файл (опционально):
```bash
rm *.tar.gz
```
Примечание: Хэш может меняться при обновлении репозитория. Всегда проверяйте его перед сборкой.
### 3. Сборка образа
Укажите версию (`VERSION`) StaticJinjaPlus и вычисленную в предыдущих шагах контрольную сумму (`CHECKSUM`).
Выберите соответствующий Dockerfile в зависимости от базового образа.

Допустимые значения для `VERSION`:
- тег `0.1.0`: `VERSION=tags/0.1.0`
- тег `0.1.1`: `VERSION=tags/0.1.1`
- ветка `main`: `VERSION=heads/main`

#### Пример сборки с базовым образом Ubuntu (версия 0.1.0)
```bash
docker build --platform linux/amd64 -f Dockerfile.ubuntu -t yourusername/static-jinja-plus:0.1.0 --build-arg VERSION=tags/0.1.0 --build-arg CHECKSUM=3555bcfd670e134e8360ad934cb5bad1bbe2a7dad24ba7cafa0a3bb8b23c6444 .
```
#### Пример сборки с базовым образом Python Slim (ветка develop)
```bash
docker build --platform linux/amd64 -f Dockerfile.python-slim -t yourusername/static-jinja-plus:develop --build-arg VERSION=heads/main --build-arg CHECKSUM=9adccb8fe17a40252df1a3acdea7edef4633b4ecaa8ba2dd5e0270f87ae43eab .
```

### 4. Публикация на Docker Hub
```bash
docker login
docker push yourusername/static-jinja-plus:0.1.0
```
Аналогично повторите для других тегов (`0.1.1`, `latest`, `develop`, `0.1.0-slim`, `0.1.1-slim`, `slim`, `develop-slim`).
### Примечания
- Замените `yourusername` на ваш логин на Docker Hub.
- Контрольные суммы можно обновлять при изменении репозитория, повторяя шаги из раздела "Получение контрольной суммы".
- Размеры образов: Ubuntu (~175 МБ) больше, чем Python Slim (~52 МБ), что важно для оптимизации.
