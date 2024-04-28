# HTTP

## Содержание
- [Посмотреть IP-адрес](#посмотреть-ip-адрес)
- [Посмотреть открытые порты](#посмотреть-открытые-порты)
- [Определить IP-адрес по DNS-имени](#определить-ip-адрес-по-dns-имени)
- [Использование telnet](#использование-telnet)
- [Использование cURL](#использование-curl)
- [Gem 'httpclient'](#gem-httpclient)
  - [Установка](#установка)
  - [HTTP-клиент c помощью гема 'httpclient'](#http-клиент-c-помощью-гема-httpclient)
  - [Документация](#документация)
  - [Практика. Gem 'httpclient'](#практика-gem-httpclient)
- [Библиотека 'Socket'](#библиотека-socket)
  - [Веб-сервер с использованием библиотеки 'socket'](#веб-сервер-с-использованием-библиотеки-socket)
  - [Практика. Библиотека 'socket'](#практика-библиотека-socket)
- [Gem 'sinatra'](#gem-sinatra)
  - [Установка](#установка-1)
  - [Веб-сервер с использованием гема 'sinatra'](#веб-сервер-с-использованием-гема-sinatra)
  - [Практика. Gem 'sinatra'](#практика-gem-sinatra)
- [Docker-compose](#docker-compose)
  - [Пример `docker-compose.yml`](#пример-docker-composeyml)
  - [Практика. docker-compose](#практика-docker-compose)

## Посмотреть IP-адрес

```sh
ip a
# либо "ifconfig" - устаревший аналог из пакета "net-tools"

# Просмотр адресов IPv4
ip -4 a
```

## Посмотреть открытые порты

```sh
ss -tunlp
# sudo ss -tunlp

# либо "netstat -tunlp" - устаревший аналог из пакета "net-tools"
```

## Определить IP-адрес по DNS-имени

```sh
nslookup ya.ru
# либо
dig ya.ru

# Получение IP-адреса по доменному имени у указанного DNS-сервера

nslookup ya.ru 8.8.8.8
dig @8.8.8.8 ya.ru
```

## Использование telnet

```sh
telnet ifconfig.co 80

GET /ip HTTP/1.1
Host: ifconfig.co
```
## Использование cURL

```sh
curl -i -X PUT 'https://postman-echo.com/put' --data 'This is expected to be sent back as part of response body.'
```

## Gem 'httpclient'

### Установка

```sh
# Gemfile + bundle install
gem 'httpclient'

# или
gem install httpclient
```

### HTTP-клиент c помощью гема 'httpclient'

1. Создайте директорию для приложения `httpclient` и перейдите в неё.
2. Проинициализируйте `Gemfile`, добавьте в него `gem 'httpclient'` и выполните `bundle install` для создания `Gemfile.lock`:
    ```bash
    docker run --rm -it -u $(id -u):$(id -g) -v `pwd`:/app -w /app ruby:3.0-alpine ash

    bundle init
    echo "gem 'httpclient'" >> Gemfile
    bundle install
    ```

3. Создайте файл `httpclient.rb` и скопируйте в него код:
    ```ruby
    require 'httpclient'
    require 'json'

    client = HTTPClient.new
    response = client.request(:get, 'http://10.125.28.96/')
    result = JSON.parse(response.body)
    pp result
    ```

4. Создайте `Dockerfile`:
    ```dockerfile
    FROM ruby:3.0-alpine

    ENV SRC_PATH /app
    RUN mkdir -p $SRC_PATH
    WORKDIR $SRC_PATH

    COPY ./Gemfile* $SRC_PATH/
    RUN bundle install

    COPY . .

    CMD ["ruby", "httpclient.rb"]
    ```

5. Соберите образ:
    ```bash
    docker build -t httpclient .
    ```

6. Запустите приложение:
    ```bash
    docker run --rm httpclient ruby httpclient.rb
    ```

### Документация

* https://www.rubydoc.info/gems/httpclient/HTTPClient
* https://www.rubydoc.info/gems/httpclient/HTTP/Message
* https://github.com/nahi/httpclient
* https://github.com/nahi/httpclient/tree/master/sample

### Практика. Gem 'httpclient'
С помощью гема `httpclient` выполнить запрос:
```sh
curl -i -X PUT 'https://postman-echo.com/put' --data 'This is expected to be sent back as part of response body.'
```

## Библиотека 'Socket'

Документация: https://ruby-doc.org/stdlib-3.0.0/libdoc/socket/rdoc/Socket.html


### Веб-сервер с использованием библиотеки 'socket'

1. Создайте папку для проекта и перейдите в нее.
2. Создайте файл `web_server.rb` и скопируйте в него код:
    ```ruby
    require 'socket'

    server = TCPServer.new 5678

    while session = server.accept
      request = session.gets
      puts request

      session.print "HTTP/1.1 200\r\n"
      session.print "Content-Type: text/html\r\n"
      session.print "\r\n"
      session.print "Hello world! The time is #{Time.now}\r\n"

      session.close
    end
    ```

3. Запустите docker-контейнер с ruby и запустите в нем этот скрипт:

    ```sh
    docker run --rm -td -p 8080:5678 -v `pwd`:/app ruby:3.0-alpine ruby /app/web_server.rb
    ```

    Эта команда вернет идентификатор контейнера.

4. Отправьте запрос в запущенное приложение. Для этого выполните команду `curl 127.0.0.1:8080/test` или откройте страницу в браузере.

5. Посмотрите логи приложения с помощью команды:
    ```sh
    docker logs [identifier]
    ```

6. Остановите контейнер с помощью команды:
    ```sh
    docker stop [identifier]
    ```

### Практика. Библиотека 'socket'

Доработать веб-сервер так, чтобы
- `GET /time` возвращал текущее время
- `GET /hello` возвращал `"hello"`
- `GET /sum?a=1&b=2` возвращал сумму параметров `a` и `b`

## Gem 'sinatra'

### Установка

```sh
# Gemfile + bundle install
gem 'sinatra'
gem 'rackup'

# или
gem install sinatra
gem install rackup
```

### Веб-сервер с использованием гема 'sinatra'

1. Создайте папку для проекта и перейдите в неё.
2. Проинициализируйте `Gemfile`, добавьте в него `gem 'sinatra'` и `gem 'rackup'` и выполните `bundle install` для создания `Gemfile.lock`.
3. Создайте файл `sinatra_server.rb` и скопируйте в него код:
    ```ruby
    require 'sinatra'

    set :bind, '0.0.0.0'
    set :port, 5678
    get '/hello' do
      'Hello world!'
    end
    ```

4. Создайте `Dockerfile`.
5. Соберите образ с тэгом `sinatra_server`.
6. Запустите приложение:
    ```bash
    docker run --rm -t -p 8080:5678 -v `pwd`:/app sinatra_server ruby /app/sinatra_server.rb
    ```
7. Перейдите по адресу `http://127.0.0.1:8080/hello` для проверки.

### Практика. Gem 'sinatra'

Разработать веб-сервер с использованием `'sinatra'`, чтобы
- `GET /time` возвращал текущее время
- `GET /hello` возвращал "hello"
- `GET /sum?a=1&b=2` возвращал сумму параметров a и b

## Docker-compose

### Пример `docker-compose.yml`

```yml
version: '3'
services:
  app:
    build: .
    command: ruby /app/sinatra_server.rb
    volumes:
      - .:/app
    ports:
      - 8080:5678
```

### Практика. docker-compose
<br/>

Напишите `docker-compose.yml`, в котором будут два сервиса: клиент и сервер.

Клиент будет обращаться к серверу, который рассчитывает сумму двух переданных чисел, и выводить на экран результат (сумму) в формате:

`3 + 2 = 5`
