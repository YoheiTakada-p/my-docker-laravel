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

修正  
・docker-compose.ymlのDB関係の環境変数をphpunitで別のDBを使用するため変更
・EC2でphp/DockerfileのENVのlocale関係の位置をlocale-genの後に移動しエラーが出ないように変更  
・Makefile追加  

a. ローカル環境の場合
```bash
$ git clone <http/ssh>
$ cd docker-laravel
$ make create-project # a. 最新版で始める場合
$ make create-project-6.x # b1. laravel6.xで始める場合
$ make create-project-react # b2. laravel6.xにreactを入れて始める場合
$ make init # c1. 既存のプロジェクトから始める場合
$ make init-yarn # c2. npm関係がある場合
```
b. AWS EC2の場合
```bash
$ sudo git clone <http/ssh>
$ cd docker-laravel
$ sudo git clone <http/ssh>
$ sudo mv <http/ssh> backend
$ sudo lsof -i :80 #確認
$ sudo apachectl stop　#apacheを止めよう
$ make init
$ make init-yarn
$ sudo chmod -R 777 backend/storage/ #権限付与
$ sudo chmod -R 777 backend/bootstrap/cache/ #権限付与
```
Tailwind  
https://tailwindcss.com/docs/guides/laravel
```bash
$ yarn add -D tailwindcss@npm:@tailwindcss/postcss7-compat @tailwindcss/postcss7-compat postcss@^7 autoprefixer@^9
$ yarn tailwindcss init

// webpack.mix.js
const mix = require('laravel-mix');
+ const tailwindcss = require('tailwindcss');

 mix.js('resources/js/app.js', 'public/js')
     .sass('resources/sass/app.scss', 'public/css')
+    .options({
+        processCssUrls: false,
+        postCss: [ tailwindcss('./tailwind.config.js') ],
+    });

//sass/app.sass
+ @tailwind base;
+ @tailwind components;
+ @tailwind utilities;

$ yarn dev
```
jetstream  
```bash
$ composer require laravel/jetstream
$ php artisan jetstream:install livewire
$ php artisan migrate
$ yarn install
$ yarn dev #すんごい時間かかる
```
phpunit
```
database.php
'connections' => [

    // ★ この設定を追加
    'testing' => [
        'driver' => 'sqlite',
        'database' => ':memory:',
        'prefix' => '',
    ],
]
phpunit.xmlのDB_CONNECTIONをtestingに変更
```bash
注意  
ローカルで $ docker-compose up でマウントできなかった時は再起動しよう  
devでModule build failedエラー出たらもう一度devしよう  
env設定を忘れずにしよう  
deploy.ymlは遅すぎるコンパイル対策  

以下参考  
Module build failedエラー  
https://github.com/babel/babel/issues/8599  

PHPUnitテスト用にSQLiteをDBに設定する場合  
https://qiita.com/sola-msr/items/80b0c0e0edb67a35d282  
