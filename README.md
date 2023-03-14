# Outline wiki

# Введение 

Решил поискать для себя wiki, так как wikijs меня не совсем устраивала. И вот нашел я 

относительно новую wiki outline, еще совсем молодую. И очень понравился мне ее современный дизайн, и в этот момент я решил ее протестировать. И от сюда пошли танцы с бубном. Пришлось не одну статейку прочитать, чтобы у меня все заработало. Поэтому решил выложить мои конфиги, так как это может облегчить, кому то жизнь =)

 ![](/api/attachments.redirect?id=cb098f76-2cf7-40b7-b29a-edbcb22e24f5 " =333x290")

# Подготовка

В зависимости от ваших потребностей, вам может не понадобиться весь процесс установки.

## Веб-сервер

Для Outline за ранее нужно подготовить поддомены c сертификатами SSL/TLS, я использовал:

* wiki.your-domain.com >> сама wiki
* minio.your-domain.com >> для minio
* admin-minio.your-domain.com >> для minio админки
* kc.your-domain.com >> для keycloak


Необязательно использовать Nginx Proxy Manager, можете заменить. Я для своего удобства использовал его.

Решили пойти по моему пути и поставить NPM?

Да - Тогда читайте ниже =)


Создайте сеть в docker reverseproxy перед установкой!

`docker network create reverseproxy`

После этого можете поставить NPM. 

`docker compose up -d`


Заходите в админку, по умолчанию она localhost:81. Логин и пароль в env файле.

И создайте 3 proxy и выдайте им SSL сертификат: 


1. Для wiki.your-domain.com >> outline:3000 
2. Для minio.your-domain.com >> outline-minio:9000
3. Для admin-minio.your-domain.com >> outline-minio:9001
4. Для kc.your-domain.com >> keycloak:8080

P.S. Название хоста=название контейнера

## Авторизация

Как оказалось с завода нет нормальной авторизации, нужно привязать OIDC, ну или через другие сервисы.

Я использую keycloak, но вы можете сделать авторизацию через google, slack и тд. см. в документации Otline.


### Keycloak

После установки keycloak заходим на kc.your-domain.com:


1. Создаем realm. Это название будет использоваться в outline.env, переменные:
   * OIDC_AUTH_URI=<https://kc.your-domain.com/auth/realms/YOUR-NAME-REALMS/protocol/openid-connect/auth>


   * OIDC_TOKEN_URI=<https://kc.your-domain.com/auth/realms/YOUR-NAME-REALMS/protocol/openid-connect/token>
   * OIDC_USERINFO_URI=<https://kc.your-domain.com/auth/realms/YOUR-NAME-REALMS/protocol/openid-connect/userinfo>

     Замените YOUR-NAME-REALMS на название realm, которые вы создали выше и не забудьте указать ваш домен.

      
2. Создаем Clients, я назвал его outline_app, это название указываем в OIDC_CLIENT_ID >> outline.env

   settings выбираем в полях:  

   
   1. **Access Type >>** confidential
   2. **Direct Access Grants Enabled >> OFF**

      
3.  Во вкладке Credentials копируем ключ **Secret в** OIDC_CLIENT_SECRET >> outline.env


Сразу можем создать users, из под него мы будем заходить в Outline

# Установка Outline

Можем запускать контейнеры Outline. Основная настройка закончена.

После запуска, осталась настроить только Minio, чтобы нормально продгружались картинки. 

Заходим на admin-minio.your-domain.com


1. Создаем Bucket с именем outline
2. В его настройках Bucket outline > Anonymous > создаем два правила:
   * public >> readonly
   * avatars >> readonly


Готово. Заходим на wiki.your-domain.com


