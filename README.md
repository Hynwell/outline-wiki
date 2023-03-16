# Подготовка

### Поддомены

Нужно создать 4 поддомена на своем сервисе домменов (например: [Reg](https://www.reg.ru/domain/shop/), [Nic](https://www.nic.ru/catalog/domains/)) - > создайте dns-записи для каждого поддомена:

* wiki.your-domain.com -> для захода на Outline
* minio.your-domain.com -> для захода на Minio
* admin-minio.your-domain.com -> для захода на Minio, в админку
* kc.your-domain.com -> для захода на Keycloak  


### Скачивание файлов

Создаем директорию для контейнеров docker:  
`mkdir -p /opt/docker`

Скачивание файлы в эту эту директорию:  
`git clone https://github.com/Hynwell/outline-wiki.git /opt/docker`

# Reverse proxy

Я буду использовать [Nginx Proxy Manager](https://nginxproxymanager.com/). Он является обратным прокси, основанным на Nginx. Имеет приятный и интуитивно понятный веб интерфейс.

### Установка Nginx Proxy Manager
Заходим в директорию Nginx Proxy Manager:  
`cd /opt/docker/nginx-proxy-manager/`

Копируем файл ".env.example" и меняем его название на ".env":  
`cp .env.example .env`

Открываем ".env" редактором:  
`nano .env`

Придумываем пароль и заменяем им значения переменных:
```
MYSQL_ROOT_PASSWORD=your-mysqlroot-password
MYSQL_PASSWORD=your-mysql-password
```

Запускаем контейнеры командой:  
`docker compose up -d`

### Настройка Nginx Proxy Manager и выдача SSL-сертификатов

Заходим в админку Nginx Proxy Manager, по умолчанию она находится по адресу:  
`http://your.server.ip.address:81`

Логин и пароль по умолчанию:  
Login: `admin@example.com`  
Password: `changeme`

# Скоро будет продолжение...


