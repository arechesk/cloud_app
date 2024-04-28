# Введение в Docker

## Установка docker

```bash
# Скачиваем скрипт установки docker
curl -fsSL https://get.docker.com -o get-docker.sh

# Запускаем скрипт
sudo sh get-docker.sh
```

### Добавление текущего пользователя в группу docker (для использования команд без sudo)
```bash
sudo usermod -aG docker $USER
````

### Проверка версии установленного docker
```bash
docker –v
```

## Основные команды

### `docker ps` - просмотр списка запущенных контейнеров

```bash
docker ps

# Просмотр всех контейнеров (запущенных и остановленных)
docker ps -a
```

## `docker run` - запуск контейнера

```bash
docker run ruby:3.0-alpine
```

### Параметр `--rm` - удаление контейнера после запуска

```bash
docker ps -a
# запомнить количество контейнеров

docker run --rm ruby:3.0-alpine
docker ps -a
# количество контейнеров не изменилось

docker run ruby:3.0-alpine
# появился остановленный контейнер со статусом «Exited (0) X seconds ago»
```

### Параметры `-it` - интерактивный терминал

```bash
docker run --rm -it ruby:2.7-alpine ash
docker run --rm ruby:2.7-alpine ash
```

### Параметр `-v` - монтирование директории

```bash
docker run --rm -it -v `pwd`:/app ruby:2.7-alpine ash
```

### Параметр `-p` - проброс порта

```bash
docker run --rm -p 8888:80 nginx:alpine

docker run --rm nginx:alpine
# открыть в браузере localhost

docker run --rm -p 8888:80 nginx:alpine
# открыть в браузере localhost
# открыть в браузере localhost:8888
```

### Параметр `-d` - запуск в фоновом режиме

```bash
docker run --rm -d -p 8888:80 nginx:alpine
```

## `docker stop` - остановка контейнера

```bash
docker stop <CONTAINER ID>
docker ps
```

## `docker images` - просмотр списка образов

```bash
docker images
```

## Создание файлов внутри docker

```bash
docker run --rm -it -v $(pwd):/app -w /app ruby:3.0-alpine ash
touch file.txt
exit # или Ctrl + D
nano file.txt # или другой редактор
# что-нибудь написать, попытаться сохранить
```

### Решение проблемы

```bash
ls -l

# 1. Смена владельца:
# указанного файла
sudo chown user file.txt
# директории рекурсивно
sudo chown user -R dir

# 2. Либо при запуске контейнера добавить параметр -u $(id -u):$(id -g)
# для прокидывания текущего пользователя в контейнер.
```

## Пример Dockerfile

```dockerfile
FROM ruby:3.0-alpine

ENV SRC_PATH /app
RUN mkdir -p $SRC_PATH
WORKDIR $SRC_PATH

COPY . .

CMD ["ruby", "test.rb"]
```

## Сборка образа из Dockerfile

```bash
docker build . -t hello_world

docker run -rm hello_world
```

## Документация

https://docs.docker.com/reference/ - Описание Dockerfile, docker-compose.yml, синтаксиса команд docker и docker-compose
