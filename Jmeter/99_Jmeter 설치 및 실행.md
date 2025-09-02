# 1. jmeter설치
- https://jmeter.apache.org/download_jmeter.cgi 
![alt text](img/jmeter설치.png)


# 2. 간단 테스트
- Test Plan -> Add -> Threads -> Thread Group

    ![alt text](img/image.png)
    
    ![alt text](img/image-7.png)
    
    - 동시접속자, 사용자 등을 테스트하기 위한거임

- Thread Group -> Add -> Sampler -> HTTP Request

    ![alt text](img/image-2.png)

    ![alt text](img/image-4.png)

    - 테스트 요청 API 설정


- Test Plan -> Add -> Config Element -> HTTP () Manager

    ![alt text](img/image-1.png)

    ![alt text](img/image-5.png)

    ![alt text](img/image-6.png)

    - 위 테스트 요청 API에 필요한 헤더, 쿠키 값 등을 설정

- Thread Group -> Add -> Listener -> 각종 결과값

    ![alt text](img/image-3.png)
    - 테스트 결과를 보여주는 화면
    - 주로 Summary Report를 자주 봄

# 3. 시나리오 기반 테스트
- Chrome 확장자 BlazeMeter 추가

    ![alt text](<img/blaze meter 추가.png>)
    - 시나리오를 녹화 해주는 역할

- BlazeMeter 설정

    ![alt text](<img/blaze meter 설정1.png>)

    ![alt text](<img/blaze meter 설정2.png>)
    - 잘 몰라서 이것저것 눌러보면서 함
    - 사진대로 하면 될듯


- 녹화 시작

    ![alt text](<img/blaze meter 저장.png>)


- 녹화 불러오기

    ![alt text](<img/jmeter blaze meter 불러오기.png>)
    - 저장된 시나리오를 Jmeter에서 불러오기


- 쿠키 설정

    ![alt text](<img/jmeter 쿠키 추가.png>)
    - 쿠키나 헤더등은 녹화가 되지 않아 직접 설정해야함

- 동시접속자 세팅
    
    ![alt text](<img/jmeter 동시접속자 세팅.png>)

- Jmeter 시작

    ![alt text](<img/jmeter 시작.png>)

- 결과

    ![alt text](img/결과.png)
    - grafana 결과로 CPU가 튀는 모습을 볼 수 있다