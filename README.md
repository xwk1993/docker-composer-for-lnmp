### docker-compose for lnmp

- 准备工作

    - 创建目录并给予读写权限，用于统一存放mysql数据文件，redis数据文件，nginx日志文件

- Nginx

    - version

        ​   _1.12.2_

    - image

       ​    _nginx:1.12.2-alpine_

- mysql

    - version

        ​   _5.6.40_
  
    - image

        ​   _mysql:5.6.40_

- redis

    - version

        ​   _3.2.11_

    - image

        ​   _redis:3.2.11-alpine_
    
- php

    - version

        ​   _7.1.18_
    
    - image
  
        ​   _php:7.1.18-fpm-alpine_

    - extension

            apcu (v5.1.11)
            bcmath
            Core
            ctype
            curl
            date
            dom
            fileinfo
            filter
            ftp
            gd
            hash
            iconv
            json
            libxml
            mbstring
            mcrypt
            mysqli
            mysqlnd
            openssl
            pcre
            PDO
            pdo_mysql
            pdo_sqlite
            phalcon (v3.4.0)
            Phar
            posix
            readline
            redis (v4.0.2)
            Reflection
            session
            SimpleXML
            SPL
            sqlite3
            standard
            swoole (v2.2.0)
            tokenizer
            xml
            xmlreader
            xmlwriter
            zlib
