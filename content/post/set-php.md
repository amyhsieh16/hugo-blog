# 在Linux(Centos/Ubuntu)上，安裝LAMP/LEMP(LNMP)

[TOC]
# PHP

## Centos 
1. 從官網取得 Remi和 EPEL rpm的連結
```bash
centos 7:
$sudo rpm -Uvh https://rpms.remirepo.net/enterprise/remi-release-7.rpm;

$sudo rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm;

centos 8:
$sudo rpm -Uvh https://rpms.remirepo.net/enterprise/remi-release-8.rpm;

$sudo rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm;
```
2. 安装 yum-utils工具，並設定 php套件為 7.2 版本(可省略)
```bash
$sudo yum install yum-utils
```
設定 php套件為 7.2 版本
```bash
$sudo  yum-config-manager --enable remi-php72
```

3. 安裝 php和相關套件

**如果跳過第2步驟，則指令如下：**
```bash
$sudo yum --enablerepo=remi-php72 install ...(下列套件)
```

```bash
$sudo yum install php php-ctype php-json php-openssl php-nette-tokenizer php-pecl-zip php-pdo php-mbstring php-xml php-mcrypt php-cli php-gd php-curl php-mysql php-ldap php-zip php-fileinfo
zip unzip php-bcmath(laravel5.7↑)
```

## Ubuntu

Ubuntu 18.04 預設安裝 php7.2，若想安裝最新版本，需新增 PPA

Ubuntu之前有安裝php，但因更新Ubuntu版本後，PPA需再新增一次

1. Add PPA

We’ll add ppa:ondrej/php PPA repository which has the latest build packages of PHP.
```bash
$sudo add-apt-repository ppa:ondrej/
```
* [Ubuntu-PPA](https://blog.gtwang.org/linux/ubuntu-linux-add-and-remove-ppa-command-tutorial/)

2. 安裝 php 7.4和相關套件

```bash
$sudo apt install php7.4
```
確認安裝版本
```bash
$php -v

PHP 7.4.6 (cli) (built: May 14 2020 10:02:44) ( NTS )
Copyright (c) The PHP Group
Zend Engine v3.4.0, Copyright (c) Zend Technologies
    with Zend OPcache v7.4.6, Copyright (c), by Zend Technologies
```
相關套件
```bash
$sudo apt install php7.4-{bcmath,bz2,intl,gd,mbstring,mysql,zip}
```

# 伺服器(Apache)

## Centos

1. 安裝
```bash
$sudo yum install httpd
```
2. 設定狀態 
```bash
$sudo systemctl start httpd (啟動apache)
$sudo systemctl enable httpd (開機時啟動)
$sudo systemctl status httpd(查看狀態)
$sudo systemctl restart httpd(重新啟動)
$sudo systemctl stop httpd(停止apache)
```

## Ubuntu

1. 安裝
```bash
$sudo yum install apache2
```
2. 設定狀態 
```bash
$sudo systemctl start apache2 (啟動apache)
$sudo systemctl enable apache2 (開機時啟動)
$sudo systemctl status apache2(查看狀態)
$sudo systemctl restart apache2(重新啟動)
$sudo systemctl stop apache2(停止apache)
```


# 伺服器(Nginx)

## Centos
1. 安裝
```bash
$sudo yum install nginx
```

2. 啟動

``` bash
$sudo systemctl start nginx (啟動nginx)
$sudo systemctl enable nginx (開機時啟動)
$sudo systemctl status nginx(查看狀態)
$sudo systemctl restart nginx(重新啟動)
```

3. 設定
在設定檔中，新增設定
```bash
$sudo vim /etc/nginx/nginx.conf
```
```vim
server {
    .
    index index.php index.html
    .
    
    location ~ \.php$ {
            root           /usr/share/nginx/html;
            fastcgi_pass   unix:/var/run/php-fpm.sock;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME $document_root$fastcgi_script_name;
            include        fastcgi_params;
    }
}
```
3. 預設目錄
:::info
/usr/share/nginx/html/
:::
## Ubuntu

1. Install

```bash
$sudo apt install nginx
```

2. Setting

```bash
$sudo vim /etc/nginx/sites-available/default
```

```vim
server {
	
	index index.php index.html index.htm index.nginx-debian.html;

	server_name _;

	#Add Down
    
	location ~ \.php$ {
        	include snippets/fastcgi-php.conf;
        	fastcgi_pass unix:/run/php/php7.4-fpm.sock;
	}
 
    location ~ /\.ht {
            deny all;
    }
	
}

```


# MySQL

## Centos
## Ubuntu

# PhpMyAdmin

## Centos
## Ubuntu


# 防火牆(Firewall)
