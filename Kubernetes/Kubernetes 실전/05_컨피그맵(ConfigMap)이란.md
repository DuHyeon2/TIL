![Kubernetes](https://github.com/user-attachments/assets/3ec2d35d-184a-480a-878f-1f89f9547880)

# 컨피그맵이란
- 쿠버네티스 내 환경 변수를 관리하는 역할
- 디플로이먼트 내에서 환경 변수를 작성할 수 있지만 다른 환경에서 서버 실행 시 유연하게 설정 값을 변경하기 난해해짐

## 1. 컨피그맵 생성
- yaml 파일 생성
    ```yaml
    # spring-config.yaml
    apiVersion: v1
    kind: ConfigMap

    metadata:
        name: spring-config

    data:
        my-account: 'duhyeon'
    ```

- 컨피그맵 생성
    ```
    $ kubectl apply -f spring-config.yaml
    ```

## 2. 디플로이먼트 내에 환경변수 사용
- 디플로이먼트 yaml 파일 수정
    ```yaml
    # spring-deployment.yaml
    apiVersion: apps/v1 # 디플로이먼트를 생성하기 위한 apiVersion(apps/v1을 사용해야함)
    kind: Deployment

    metadata: # 디플로이먼트에 대한 기본 정보
        name: spring-deployment

    spec: 
        replicas: 3 # 파드생성 개수
        selector: # 파드에 대한 정보를 
            matchLabels: # 아래 카테고리와 매치시킨다
            app: backend-app # 'backend-app' 에 매칭!

        template: # 파드에 대한 정보
            metadata: # 파드에 대한 기본 정보
            labels: # 파드의 카테고리
                app: backend-app # 'backend-app'이라는 이름
            spec:
            containers: 
                - name: spring-container
                  image: spring-server # spring-server(기존에 dockerfile로 만든 이미지) 이미지를 풀 받음
                  imagePullPolicy: IfNotPresent # 이미지 풀 정책
                  ports:
                    - containerPort: 8080
                  # 아래에부터 환경변수 추가
                  env :
                      - name : MY_ACCOUNT
                        valueFrom : 
                            configMapKeyRef:
                                name : spring-config # configMap의 이름
                                key : my-account # configMap에 설정되어 있는 Key값 
    ```

- 디플로이먼트 수정 반영
    ```bash
    $ kubectl apply -f spring-deployment.yaml
    ```

- 디플로이먼트 재시작
    ```bash
    $ kubectl rollout restart deployment spring-deployment
    ```