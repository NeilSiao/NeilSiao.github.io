---
title: 在 heroku 部屬 Laravel
date: 2021-01-25 14:27
tags: -heroku deployment
---
## 部屬流程
紀錄部屬 laravel app 到 heroku 上的流程, 雖然部屬過好幾次了, 可是每次都會忘記,因此留下這篇當作紀錄...

``` 
//heroku-cli

//指定Apache2 pubilc directory
echo "web: vendor/bin/heroku-php-apache2 public/" > Procfile

//建制 heroku app, 
heroku create [appName]

//有使用到 npm 所以需要啟用 heroku 編譯的套組
heroku buildpacks:add heroku/nodejs

// 設定 npm 不是 production mode 否則不會安裝 dev-dependencies 的套件
heroku config:set NPM_CONFIG_PRODUCTION=false

//重新產生 Laravel APP_KEY
heroku config:set APP_KEY=$(php artisan --no-ansi key:generate --show)

//關於 APP_KEY 以前一直有個迷思,如果 APP_KEY 換掉使用者是不是就無法登入了, 其實是不會的. 因為 
//bcrpt 會把演算的方式也一併加入在 Hash 後的字串當中. Laravel 好像只有用來加密 Session 這一部份...

//推送專案到 heroku 上面
git push heroku main 

```
## heroku command
```
// 設定 heroku 機器的環境變數, 相當於 laravel 的 env 檔案
heroku config:set VAR_NAME=VAR_VALUE

// 錯誤查找 
heroku logs

// 登入 heroku
heroku login 

// 列出你有哪些專案
heroku apps

// 詳系列出所有指令
heroku --help
```


## heroku 編譯腳本
由於有需要重新編譯前端資源,所以會在 package.json 新增資料.

 heroku-postbuild 是官方的 pipeline, 名稱正確就會幫你去跑指令了

```
    "scripts": {
        "dev": "npm run development",
        "heroku-postbuild": "npm run prod"
        ...
    },
```

> 如果有其他指令也可以透過 heroku run php artisan storage:link 或者是 heroku run npm install


## 免費的資料庫 heroku-postgre-sql
Heroku 有提供免費的資料庫系統使用. 只要修改 Laravel config/database.php 的setting 就可以抓取正確的資料了
但是！！ 必須先建立 pgsql
```
heroku addons:create heroku-postgresql:hobby-dev
```

調整 config/database.php
```php 
// 當你使用heroku addons 之後, 就會自動幫你增加 
//DATABASE_URL 變數, 這個變數包含的連線到資料庫的相關參數

// getenv from heroku
$DATABASE_URL = parse_url(getenv("DATABASE_URL"));

'pgsql' => [
    'driver' => 'pgsql',
    'url' => env('DATABASE_URL'),
    'host' => $DATABASE_URL["host"],
    'port' => $DATABASE_URL["port"],
    'database' => ltrim($DATABASE_URL["path"], "/"),
    'username' => $DATABASE_URL["user"],
    'password' => $DATABASE_URL["pass"],
    'charset' => 'utf8',
    'prefix' => '',
    'prefix_indexes' => true,
    'schema' => 'public',
    'sslmode' => 'prefer',
],
```
由於控制連線的方式是看 env 的 DB_CONNECTION, 所以我們使用 `heroku config:set DB_CONNECTION=pgsql`, 不喜歡打 command line 其實也可以直接在官網 Dashboard 進行設定

資料庫設定完畢後,就可以藉由 Laravel Migration Seeder 來進行資料表的開設了.
```
heroku run php artisan storage:link

heroku run php artisan migrate && php artisan db:seed
```

## db:seed Class 'Faker\Factory' not found
執行 php artisan db:seed 時, 如果發現這個錯誤則需要將 faker package 從 composer.json 的 require-dev 移動到 require 上面. 否則不會安裝..
[參考文章](https://stackoverflow.com/questions/32801183/deployment-on-laravel-forge-throwing-faker-not-found-exception)
```
//移動完後使用
composer install 
composer update
//add composer.lock change..
git add .
git commit -m "modify composer.lock"
git push heroku main
```


## Laravel asset http vs https 
如果在 Blade 使用 asset('js/index.js') 在部屬後可能會發現網站全面跑板, 此時就是因為 Heroku 預設是 https 可是資源卻是 http 請求, 瀏覽器的機制會自動讓請求失敗.

所以可以考慮在 Laravel AppServiceProvider 的 boot method 使用下方程式碼, 強制轉換所有請求為 https.
```
public function boot(){
    \Illuminate\Support\Facades\Url::forceScheme('https');
}
```

## Heroku Server Error
為了知道為什麼本地端沒問題, 一上去 Heroku 就出錯該怎麼辦？
可以照官方的作法在 config/logging.php 調整參數, env 預設的 LOG_CHANNLE 是 stack, 只需要使用 ```heroku config:set LOG_CHANNEL=single``` 即可. 這樣就可以使用 `heroku logs` 查看問題了
```php
  'channels' => [
        'stack' => [
            'driver' => 'stack',
            'channels' => ['single'],
            'ignore_exceptions' => false,
        ],

        'single' => [
            'driver' => 'errorlog', //log for heroku
            'path' => storage_path('logs/laravel.log'),
            'level' => 'debug',
        ],
```

參考來源：
1. [How to deploy a Laravel/Vue App to Heroku](https://dev.to/eduvin/how-to-deploy-a-laravel-vue-app-to-heroku-4kmg)
2. [Laravel/Vuejs app does not update after deploying to Heroku](https://stackoverflow.com/questions/59084954/laravel-vuejs-app-does-not-update-after-deploying-to-heroku)
3. [Deploy Laravel 7 to Heroku 2020](https://andyyou.medium.com/deploy-laravel-7-to-heroku-2020-bf8f420467e0)