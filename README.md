## environment
windows10
docker:19.03.13
### app container
- Base image
  - php:7.4-fpm-buster
  - composer:2.0
### web container
- Base image
  - nginx:1.18-alpine
  - node:14.2-alpine
### db container
- Base image
  - mysql:8.0

https://github.com/ucan-lab/docker-laravel 参考
