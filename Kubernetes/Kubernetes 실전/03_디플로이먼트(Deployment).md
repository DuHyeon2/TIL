![Kubernetes](https://github.com/user-attachments/assets/3ec2d35d-184a-480a-878f-1f89f9547880)

# 디플로이먼트
- 파트를 묶음으로 쉽게 관리할 수 있는 기능

## 1. 디플로이먼트 생성
- yaml 파일 생성
    ```
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
    ```