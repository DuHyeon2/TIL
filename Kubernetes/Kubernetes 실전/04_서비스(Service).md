![Kubernetes](https://github.com/user-attachments/assets/3ec2d35d-184a-480a-878f-1f89f9547880)

# 서비스란
- 외부로부터 들어오는 트래픽을 받아 파드에 균등하게 분배해주는 로드밸런서 역할을 하는 기능
- 쿠버네티스 내부에 서비스란 명칭이 존재함
- 사용자로부터 요청을 받는 역할도 함
- 포트 포워딩으로 파드에 요청을 했는데 서비스를 통해 요청을 보내는게 일반적임

## 1. 서비스 생성
- yaml 파일 생성
    ```yaml
    # spring-service.yaml 
    apiVersion: v1
    kind: Service

    metadata:
        name: spring-service # 서비스 이름

    # 서비스 세부정보
    spec:
        type: NodePort # 서비스 종류
        selector: # 파드에서 설정한 레이블과 일치하는 파드를 선택
            app: backend-app # backend-app 레이블을 가진 파드와 서비스를 연결!(deployment 매니페스트 파일의 pod의 label과 일치시켜야함)
        ports:
          - protocol: TCP # 서비스에 접속하기 위한 프로토콜
            port: 8080 # 쿠버네티스 내부에서 Service에 접근하기 위한 포트 번호
            targetPort: 8080 # 매핑하기 위한 파드의 포트 번호
            nodePort: 30000 # 외부에서 사용자들이 접근하게 될 포트 번호
    ```

- 서비스 생성
    ```bash
    $ kubectl apply -f spring-service.yaml
    ```
