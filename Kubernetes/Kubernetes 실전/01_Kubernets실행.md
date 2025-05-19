![Kubernetes](https://github.com/user-attachments/assets/3ec2d35d-184a-480a-878f-1f89f9547880)

# Kubernets실행

## 1. pod생성
- yaml 파일 생성
    ```bash
    # nginx-pod.yaml
    apiVersion: v1 # Pod를 생성하기 위한 apiVersion(v1을 사용해야함)
    kind: Pod # Pod라는 리소스 생성

    metadata: # Pod에 대한 기본 정보
        name: nginx-pod # Pod의 이름

    spec: # Pod의 세부정보
    containers:
        - name: nginx-container
            image: nginx:latest
            ports:
            - containerPort: 80 # Pod의 컨테이너에서 사용되고 있는 포트(실행되는 port의 정보를 적는거지 port를 열어주는건 아님-문서상 사용)
    ```
    - yaml파일을 보고 Manifest File이라 부른다
    - 쿠버네티스에서 여러가지 리소스를 생성하고 관리하기위해 생성하는 파일
    - 도커에선 도커파일쯤이라 생각

- 실행
    ```bash
    $ kubectl apply -f nginx-pod.yaml 
    ```

- pod 접속이 안되는 이유?
    - 파드의 네트워크는 로컬 컴퓨터의 네트워크와는 독립적으로 분리되어있다.

- 접속방법
    1. 포트 포워딩
        - 파드와 로컬컴퓨터의 포트를 포워딩
        ```
        $ sudo kubectl port-forward pod/nginx-pod(파드이름) 80(로컬 포트):80(파드 내 포트)
        ```
- 종료
    ```
    $ kubectl delete pod nginx-pod(파드이름)
    ```
