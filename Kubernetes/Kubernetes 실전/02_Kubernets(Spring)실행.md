![Kubernetes](https://github.com/user-attachments/assets/3ec2d35d-184a-480a-878f-1f89f9547880)

# Kubernets(Spring)실행

## 1. Dockerfile로 Docker image 생성
- Dockerfile 생성
    ```yaml
    # Dockerfile
    FROM: openjdk:17-jdk

    COPY build/lib/*SNAPSHOT.jar /app.jar

    EXPOSE 8080

    ENTRYPOINT ["java", "-jar", "/app.jar"]
    ```

- Docker image 빌드
    ```bash
    $ docker build -t spring-server(이미지이름) .
    ```
- Docker image 생성됐는지 확인
    ```bash
    $ docker image ls
    ```

## 2. Pod 생성
- yaml(매니페스트) 파일 생성
    ```yaml
    # spring-pad.yaml
    apiVersion: v1
    kind: pod

    metadata:
        name: spring-pod
    
    spec:
        containers:
            - name: spring-container
              image: spring-server
              ports:
                  - containerPort: 8080
              imagePullPolicy: IfNotPresent # 이미지 풀 정책 설정
    ```

- Pod 생성
    ```bash
    $ kubectl apply -f spring-pod.yaml  
    ```

- Pod 생성 확인
    ```bash
    $ kubectl get pods
    ```

- Pod 삭제
    ```bash
    $ kubectl delete pod spring-pod(파드이름)
    ```

- Pod 디버깅
    ```bash
    $ kubectl describe pods spring-pod(파드이름)

    $ kubectl logs spring-pod(파드이름)
    ```

## 3. 이미지 풀 정책
- 파드 조회 시 ImagePullBackOff 에러가 발생되어 있을것이다
- 이건 이미지 풀 정책(Image Pull Policy) 때문에 발생한 것이다.
- 이미지 풀 정책이란?
    - 쿠버네티스가 yaml파일을 읽어들여 파드를 생성할 때, 이미지를 어떻게 Pull을 받아올 건지에 대한 정책
    - Always : 로컬에서 이미지를 가져오지 않고, 무조건 레지스트리(=Dockerhub, ECR과 같은 원격 이미지 저장소)에서 가져온다.
    - IfNotPresent : 로컬에서 이미지를 먼저 가져온다. 만약 로컬에 이미지가 없으면 레지스트리에서 가져온다.
    - Never : 로컬에서만 가져온다.