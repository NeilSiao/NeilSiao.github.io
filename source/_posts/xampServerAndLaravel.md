---
title: 利用 xampp 架設 Laravel 和 SSL、Let's encrypted
date: 2021-03-15 15:46
tags: -learning xampp laravel ssl
---

[TOC]

# xampp 安裝教學

xampp 安裝教學的部分參考 [挨踢路人甲
](https://walker-a.com/archives/6255)

# Laravel 安裝

選擇專案資料夾

```console
//安裝
composer create-project laravel/laravel chatbot
//產生金鑰
php artisan key:generate
```

# DNS 申請

使用免費的 DNS 服務 [Freenom](https://www.freenom.com/zu/index.html?lang=zu)， 參考[電腦王阿達 - 架站的前哨站，免費的 freenom 網域申請](https://www.kocpc.com.tw/archives/180195)

主要就是輸入自己喜歡的 domainName 註冊完畢，然後設定 A (IPv4) 紀錄，指向哪一個 IP，再讓子彈飛一會，DNS 擴散完畢，就可以對應到你的主機了。

> [網站可以檢查 DNS 散播情況](https://appuals.com/how-to-fix-server-dns-address-could-not-be-found-error-on-google-chrome/)

> AAAA 是指 IPv6

> CNAME 表示 domainName 的其他別名, 例如 www.neilsiao.tk 指向 neilsiao.tk

# Let's encrypted 申請

由於環境不是 Unix like 的環境，所以使用 sslforfree.com 取得憑證，下載後有三個檔案 ca_bundle.crt、certificate.crt、private.key，將檔案放置到 apache/conf/ssl 相關位置，修改 httpd-vhosts.conf 設定如下，接著為了驗證網站所有者是妳本人，到 Laravel 專案的 public 資料夾建立 /well-known/pki-validation/{sslforfree 的 txt 檔案}，搭配 sslforfree 驗證按鈕，就會去驗證你確實是網站的主人，完成 SSL 設定。

```console
SSLEngine on
SSLCertificateFile "D:/xampp/apache/conf/ssl.crt/certificate.crt"
SSLCertificateKeyFile "D:/xampp/apache/conf/ssl.key/private.key"
SSLCertificateChainFile "D:/xampp/apache/conf/ssl.crt/ca_bundle.crt"
```

> sslforfree.com 本身也是使用 let's encrypted 的服務，

參考教學 [XAMPP 使用 Let’s Encrypt SSL 開啟加密連線—完整教學](https://medium.com/@45EMC521/xampp-%E4%BD%BF%E7%94%A8-lets-encrypt-ssl-%E9%96%8B%E5%95%9F%E5%8A%A0%E5%AF%86%E9%80%A3%E7%B7%9A-%E5%AE%8C%E6%95%B4%E6%95%99%E5%AD%B8-c05b5f97195a)

# Apache 設定檔

參考教學 [Laravel 5.4 On Apache：在 Apache 架 Laravel 網站](https://medium.com/%E6%B5%A6%E5%B3%B6%E5%A4%AA%E9%83%8E%E7%9A%84%E6%B0%B4%E6%97%8F%E7%BC%B8/laravel-5-4-on-apache-%E5%9C%A8-apache-%E6%9E%B6-laravel-%E7%B6%B2%E7%AB%99-9b7d1ad938af)

透過 xampp 的控制面板，可以找到 Apache config 按鈕，可以找到相關路徑，主要的設定檔有 httpd.conf、httpd-vhosts

```console
// httpd.conf 確保有 Include vhost
# Virtual hosts
Include conf/extra/httpd-vhosts.conf

// httpd-vhosts.conf
<VirtualHost *:80>
    #ServerAdmin webmaster@dummy-host.example.com
    ServerName neilsiao.tk
    ServerAlias www.neilsiao.tk
    # 80 port 的流量導到 443 https
    Redirect 301 / https://neilsiao.tk
</VirtualHost>

<VirtualHost *:443>
    #ServerAdmin webmaster@dummy-host.example.com
    DocumentRoot "D:/Interview/chatbot/public"
    ServerName neilsiao.tk
    ServerAlias www.neilsiao.tk
    ErrorLog "logs/dummy-host.example.com-error.log"
    CustomLog "logs/dummy-host.example.com-access.log" common

    #Laravel 專案位置
	<Directory D:/Interview/chatbot/public>
		AllowOverride all
		Require all granted # 不限制 IP 連入網站
	</Directory>

    # SSL 設定
	SSLEngine on
	SSLCertificateFile "D:/xampp/apache/conf/ssl.crt/certificate.crt"
	SSLCertificateKeyFile "D:/xampp/apache/conf/ssl.key/private.key"
	SSLCertificateChainFile "D:/xampp/apache/conf/ssl.crt/ca_bundle.crt"
</VirtualHost>

```

> [Apache 從 2.2 換至 2.4 httpd.conf 的調整筆記 (windows 環境)](https://dotblogs.com.tw/maplenote/2012/07/20/apache24_httpd_conf)

# 總結

1. xampp 安裝 Apache, MariaDB, PHP
2. Freenom 購買 Domain, 設定 A、CNAME 參照 IP 與 www 的名稱
3. 安裝 Laravel 專案，php artisan key:generate
4. 申請 SSL 憑證，至 xampp 的 httpd-vhosts 設定憑證的三個檔案，在 Laravel public 新增 /well-known/pki-validation/{sslforfree.txt}檔案
5. 訪問 domain name ， OK
