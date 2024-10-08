# Примеры н # Примеры на Python

## Пример 1: Клиент и сервер с использованием сокетов

### Серверная часть:

```python
import socket

# Создаем сокет
server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# Привязываем сокет к адресу и порту
server_socket.bind(('localhost', 8080))

# Начинаем слушать входящие подключения (ожидание клиентов)
server_socket.listen(1)
print("Сервер запущен на порту 8080...")

while True:
    # Принимаем соединение от клиента
    client_connection, client_address = server_socket.accept()
    print(f'Подключение от {client_address}')

    # Получаем сообщение от клиента
    request = client_connection.recv(1024).decode()
    print(f'Запрос от клиента: {request}')

    # Отправляем ответ клиенту
    response = 'Привет от сервера!'
    client_connection.sendall(response.encode())

    # Закрываем соединение
    client_connection.close()
```

#### Объяснение серверной части:

1. **Создание сокета:**  
   `socket.socket(socket.AF_INET, socket.SOCK_STREAM)` — создаем новый сокет. Мы используем `AF_INET`, чтобы работать с IP-адресами (IPv4), и `SOCK_STREAM` для использования протокола TCP.

2. **Привязка сокета к адресу и порту:**  
   `server_socket.bind(('localhost', 8080))` — привязываем сокет к адресу `localhost` (127.0.0.1) и порту 8080. Это означает, что сервер будет слушать подключения только с этого порта.

3. **Ожидание подключений:**  
   `server_socket.listen(1)` — переводим сервер в режим ожидания подключения. Число в скобках указывает на максимальное количество ожидающих подключений (в данном случае 1).

4. **Прием подключения от клиента:**  
   `client_connection, client_address = server_socket.accept()` — сервер принимает соединение от клиента. Функция `accept()` блокирует выполнение программы до тех пор, пока не придет запрос на подключение. После этого возвращается кортеж, содержащий объект для общения с клиентом (`client_connection`) и адрес клиента (`client_address`).

5. **Получение данных от клиента:**  
   `request = client_connection.recv(1024).decode()` — получаем данные от клиента, используя метод `recv()`. Мы указываем максимальный размер данных в 1024 байта. После этого данные декодируются из байтов в строку.

6. **Ответ клиенту:**  
   `client_connection.sendall(response.encode())` — отправляем клиенту ответ. Мы кодируем строку в байтовый формат перед отправкой.

7. **Закрытие соединения:**  
   `client_connection.close()` — закрываем соединение с клиентом.

### Клиентская часть:

```python
import socket

# Создаем сокет
client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# Подключаемся к серверу
client_socket.connect(('localhost', 8080))

# Отправляем сообщение серверу
client_socket.sendall(b'Привет, сервер!')

# Получаем ответ от сервера
response = client_socket.recv(1024)
print(f'Ответ от сервера: {response.decode()}')

# Закрываем соединение
client_socket.close()
```

#### Объяснение клиентской части:

1. **Создание сокета:**  
   `client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)` — создаем клиентский сокет, который работает по протоколу TCP и использует IPv4.

2. **Подключение к серверу:**  
   `client_socket.connect(('localhost', 8080))` — подключаемся к серверу, который работает на `localhost` (127.0.0.1) и порту 8080.

3. **Отправка сообщения:**  
   `client_socket.sendall(b'Привет, сервер!')` — отправляем сообщение серверу. Оно должно быть в виде байтов, поэтому добавляем префикс `b` перед строкой.

4. **Получение ответа от сервера:**  
   `response = client_socket.recv(1024)` — получаем ответ от сервера. Максимальный размер получаемых данных — 1024 байта.

5. **Вывод ответа:**  
   `print(f'Ответ от сервера: {response.decode()}')` — декодируем байты в строку и выводим полученный ответ от сервера.

6. **Закрытие соединения:**  
   `client_socket.close()` — закрываем соединение с сервером.

### Общее описание:

- **Серверная часть** всегда остается активной, ожидая подключения клиентов.
- **Клиентская часть** подключается к серверу, отправляет сообщение, получает ответ и закрывает соединение.
- Используемый протокол TCP гарантирует надежную передачу данных между клиентом и сервером.

Вместе этот пример демонстрирует базовые принципы работы с сокетами в Python, а также клиент-серверное взаимодействие через TCP/IP.

## Пример 2: HTTP-сервер

Вот пример простого HTTP-сервера на Python, который можно запустить, чтобы пользователь мог подключиться к нему через браузер и увидеть HTML-страницу:

### Серверная часть:

```python
import socket

# Параметры сервера
HOST = 'localhost'  # Адрес хоста (localhost для локальных соединений)
PORT = 8080         # Порт, на котором будет работать сервер

# Создаем сокет
server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# Привязываем сокет к адресу и порту
server_socket.bind((HOST, PORT))

# Начинаем слушать входящие соединения
server_socket.listen(5)
print(f"HTTP сервер запущен на {HOST}:{PORT}...")

# HTML-страница, которая будет отображаться в браузере
html_content = """
<!DOCTYPE html>
<html>
<head>
    <title>Простой сервер на Python</title>
</head>
<body>
    <h1>Привет! Это простая HTML-страница.</h1>
    <p>Этот сервер написан на Python и работает через сокеты.</p>
</body>
</html>
"""

while True:
    # Принимаем соединение от клиента
    client_connection, client_address = server_socket.accept()
    print(f'Подключение от {client_address}')

    # Получаем запрос от клиента (например, из браузера)
    request = client_connection.recv(1024).decode()
    print(f'Запрос клиента:\n{request}')

    # Формируем HTTP-ответ с заголовками и HTML-контентом
    http_response = (
        "HTTP/1.1 200 OK\r\n"
        "Content-Type: text/html; charset=UTF-8\r\n"
        f"Content-Length: {len(html_content)}\r\n"
        "Connection: close\r\n"
        "\r\n"
        + html_content
    )

    # Отправляем HTTP-ответ клиенту
    client_connection.sendall(http_response.encode())

    # Закрываем соединение
    client_connection.close()
```

#### Как запустить сервер:
1. Сохраните код в файл, например, `simple_server.py`.
2. Откройте терминал (или командную строку) и запустите сервер командой:

```bash
python simple_server.py
```

3. В браузере откройте URL: `http://localhost:8080`

#### Объяснение:

- **Сокет** создается на порту 8080 (или на другом порту, если вы измените значение `PORT`).
- **HTML-страница** сохраняется в переменной `html_content` и отправляется клиенту (например, браузеру).
- Сервер принимает HTTP-запросы и отправляет обратно HTTP-ответ с HTML-страницей.

После запуска, при обращении к `http://localhost:8080` в браузере, вы увидите HTML-страницу с сообщением.

