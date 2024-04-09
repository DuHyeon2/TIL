# Apache Tomcat 연동 이유?
- 정적 데이터는 웹서버인 Apache가 처리
- 동적 데이터는 WAS인 Tomcat이 처리
- 이렇게 서버의 부하를 분산하고 속도를 빠르게 하기 위해 연동하여 사용
- HTML, CSS, JavaScript같은 정적 콘텐츠들은 WAS까지 거칠 필요가 없으므로 웹서버인 Apache로 바로 응답 하는것이 빠르다.
- 연동 방법엔 mod_proxy, mod_proxy_ajp, mod_jk 3가지 방법이 있다.

## 1. mod_proxy
1. httpd.conf 수정
    - apahce가 설치된 곳으로 이동한다.
        ```
        # cd /etc/httpd/conf

        # vi httpd.conf
        ```

    - 이후 아래 주석을 제거 후 Include로 extra의 httpd-vhosts.conf를 로드하게 해준다.
    ![alt text](<img/Apache Tomcat 연동-1.png>)

2. httpd-vhosts.conf 작성

    - /etc/httpd/conf에 extra 디렉토리를 생성해준다.
        ```
        # mkdir extra
        ```

    - 이후 httpd-vhosts.conf를 작성한다.
![alt text](<img/Apache Tomcat 연동-2.png>)

3. CentOS SELinux 정책 수정

    - CentOS에선 SELinux 정책이 httpd가 네트워크에 연결되는 것을 막는데 SELinux 정책을 수정해야 한다.
        ```
        # sudo /usr/sbin/setsebool -P httpd_can_network_connect 1
        ```




## 2. mod_proxy_ajp
1. 

## 3. mod_jk
1. 
