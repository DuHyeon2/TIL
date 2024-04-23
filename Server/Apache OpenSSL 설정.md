![alt text](img/OpenSSL.png)
# OpenSSL이란?
- SSL인증서 설치 등 SSL 관련 작업을 수행할수 있게 해주는 암호화 라이브러리

## 1. OpenSSL 설치
- openssl 의 버전을 확인하고 없다면 설치를 한다.
    ```
    # openssl version

    # sudo yum install openssl
    ```

## 2. mod_ssl 설치
- mod_ssl도 설치해준다.
    ```
    # sudo yum install mod_ssl
    ```
- /etc/httpd/modules 디렉토리에 mod_ssl.so 가 생성된다.


## 3. 인증서 생성
- 개인키 생성 
    ```
    # openssl genrsa -des3 -out server.key 2048
    ```
    - 이후 비밀번호를 입력

        ![alt text](<img/개인키 생성.png>)

- 인증요청서 생성
    ```
    # openssl req -new -key server.key -out server.csr

    Enter pass phrase for server.key.origin: 개인 키 패스워드 입력

    Country Name (2 letter code) [XX]:​KR

    State or Province Name (full name) []:Daegu

    Locality Name (eg, city) [Default City]:SusungGu

    Organization Name (eg, company) [Default Company Ltd]:localhost

    Organizational Unit Name (eg, section) []:localhost

    Common Name (eg, your name or your server's hostname) []:localhost

    Email Address []:localhost@test.com

    Please enter the following 'extra' attributes

    to be sent with your certificate request

    A challenge password []: 그냥 패스(enter)

    An optional company name []: 그냥 패스(enter)
    ```
    - 개인 정보를 입력하면 된다(로컬로 진행할거라 아무거나 입력함)
    - 마지막에 비밀번호 입력란은 아무것도 입력하지 말고 엔터 두번 입력

        ![alt text](<img/인증요청서 생성.png>)

- 개인키 패스워드 제거
    ```
    # cp server.key server.key.origin

    # openssl rsa -in server.key.origin -out server.key

    Enter pass phrase for server.key.origin: 개인 키 패스워드 입력
    ```
    - 아파치 구동시마다 물어보므로 패스워드는 제거
    
        ![alt text](<img/개인키 패스워드 제거.png>)

- 인증서 생성
    ```
    # openssl x509 -req -days 3650 -in server.csr -signkey server.key -out server.crt
    ```
    - 위에서 만든 개인키와 요청서를 통해 3650일짜리 인증서 생성

        ![alt text](<img/개인키 패스워드 제거.png>)

## 4. ssl.conf 수정 혹은 httpd-ssl.conf 생성
- 아마 ssl.conf가 있을탠데 난 그냥 httpd-ssl을 새로 만들어 주었다.
    ```
    Listen 443 https

    <VirtualHost _default_:443>

        #DocumentRoot "/var/www/html"
        ServerName 127.0.0.1:443
        JkMount /* worker1

        ErrorLog logs/ssl_error_log
        TransferLog logs/ssl_access_log
        LogLevel warn

        SSLEngine on
        SSLCertificateFile /usr/local/ssl/server.crt
        SSLCertificateKeyFile /usr/local/ssl/server.key

        DocumentRoot /home/tomcat-test/webapps/ROOT
        <Directory "/home/tomcat-test/webapps/ROOT">
            Options Indexes FollowSymLinks
            AllowOverride None
            Order allow,deny
            Allow from all
            Require all granted
        </Directory>

    </VirtualHost>
    ```
    - SSLCertificateFile와 SSLCertificateKeyFile에 아까 만든 인증서 경로를 지정해준다.

## 5. httpd-vhost.conf 수정
- https로 rewrite 하게 수정해준다
    ```
    <VirtualHost *:80>
        ServerName 127.0.0.1
        
        DocumentRoot "/home/tomcat-test/webapps/ROOT"
        
        RewriteEngine On
        
        RewriteCond %{HTTPS} off
        
        RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [R=301,L]
        
        JkMount /* worker1
    </VirtualHost>
    ```

## 6. 실행결과
- 이후 아파치 재실행시 https로 접근되는걸 볼 수 있다.

    ![alt text](<img/OpenSSL 결과.png>)