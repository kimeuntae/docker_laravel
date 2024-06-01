## 🙌 Laravel & PHP Docker
- **프로젝트구성**

## 🛠  **프로젝트구성**
- Nginx
- Mysql
-  PHP

##   **0. docker-compose.xml 생성**

```
version: '3.8'
services:
  server:
    image: 'nginx:stable-alpine'
    ports:
      - '8000:80'
    volumes:
      - ./src:/var/www/html
      - ./nginx/nginx.conf:/etc/nginx/conf.d/default.conf:ro
    depends_on:
      - php
      - mysql
  php:
    build:
      context: ./dockerfiles
      dockerfile: php.dockerfile
    volumes:
      - ./src:/var/www/html:delegated
    ports:
      - '3000:9000'

  mysql:
    image: mysql:5.7
    env_file:
      - ./env/mysql.env
  composer:
    build:
      context: ./dockerfiles
      dockerfile: composer.dockerfile
    volumes:
      - ./src:/var/www/html
  artisan:
      build:
        context: ./dockerfiles
        dockerfile: php.dockerfile
      volumes:
        - ./src:/var/www/html
      entrypoint: ["php","/var/www/html/artisan"]
  npm:
    image: node:14
    working_dir: /var/www/html
    entrypoint: ["npm"]
    volumes:
      - ./src:/var/www/html
```
##   **1. Laravel 앱 생성**

```
docker-compose run --rm --build composer create-project --prefer-dist laravel/laravel .
```
##   **2. docker 실행 및 중지**

- 도커 실행 (컨테이너 생성 및 실행)
```
docker-compose up -d server php mysql 또는 docker-compose  up -d --build server
```
- 도커 중지 (생성된 컨테이너 자동삭제)
```
docker-compose down
```
- 테스트
```
docker-compose run --rm  artisan migrate
```

##   **📢 이슈**

- 버전이슈문제로 인덱스 페이지 오류
