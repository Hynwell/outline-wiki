# Подготовка

### Поддомены

Нужно создать 4 поддомена на своем сервисе домменов (например: [Reg](https://www.reg.ru/domain/shop/), [Nic](https://www.nic.ru/catalog/domains/)):

* wiki.your-domain.com -> для захода на Outline
* minio.your-domain.com -> для захода на Minio
* admin-minio.your-domain.com -> для захода на Minio, в админку
* kc.your-domain.com -> для захода на Keycloak  

Это нужно чтобы можно было зайти на свои сервисы.

### Скачивание файлов

Создаем директорию для контейнеров docker: 
`mkdir -p /opt/docker`

Скачивание файлы в эту эту директорию: 
`git clone https://github.com/Hynwell/outline-wiki.git /opt/docker`

# Reverse proxy

Я буду использовать Nginx Proxy Manager. Он является обратным прокси, основанный на Nginx. Имеет приятный и интуитивно понятный веб интерфейс.
Используется для того чтобы проксировать Ваше приложение на необходимый домен либо поддомен.

### Установка Nginx Proxy Manager
Заходим в директорию Nginx Proxy Manager:
`cd /opt/docker/nginx-proxy-manager/`

Копируем файл ".env.example" и меняем его название на ".env":
`cp .env.example .env`

Открываем ".env" редактором:
`nano .env`

Придумываем пароль и заменяем им текс "your-password" в переменных:
```
MYSQL_ROOT_PASSWORD=your-password
MYSQL_PASSWORD=your-password
```

Запускаем контейнеры командой: 
`docker compose up -d`

### Настройка reverse proxy и выдача SSL-сертификатов

Заходим в админку Nginx Proxy Manager, по умолчанию она находиться по адресу:  `ip-вашего-сервера:81`

Логин и пароль по умолчанию:
Login: `admin@example.com`  
Password: `changeme`


# Скоро будет продолжение...


