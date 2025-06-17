![Kubernetes](https://github.com/user-attachments/assets/3ec2d35d-184a-480a-878f-1f89f9547880)

# 볼륨
- 데이터를 영속적으로 저장하기 위한 방법
- 쿠버네티스에선 보륨은 크게 2가지 종류로 나뉨

## 1. 로컬 볼륨
- 파드 내부의 공간 일부를 볼륨으로 활용하는 방식
- 이 방식은 파드가 삭제되는 즉시 데이터도 함께 삭제
- 그래서 잘 사용안함

## 2. 퍼시스던트 볼륨(Persistent Volume, PV)
- 파드 외부의 공간 일부를 볼륨(Volume)으로 활용하는 방식
- 이 방식은 파드가 삭제되는 것과 상관없이 데이터를 영구적으로 사용할 수 있다는 장점이 있다.
- 현업에서 주로 사용
- 작성법
    ```yaml
    # mysql-pv.yaml
    apiVersion: v1
    kind: PersistentVolume

    metadata:
        name: mysql-pv

    spec:
        storageClassName: my-storage #pv이름과 pvc이름을 매칭시켜 연결하기 위한 이름
        capacity:
            storage: 1Gi # 용량
        accessModes:
            - ReadWriteOnce # 읽기와 쓰기가 가능 
        hostPath: # 쿠버네티스 내부의 공간을 볼륨으로 사용할 때 hostPath를 사용
            path: "/Users/gimduhyeon/Documents/dev/study/kube/mysql-project/data" # 쿠버네티스가 사용할 경로(내 실제 경로를 사용해야함)
    ```

## 3. 퍼시스던트 볼륨 클레임(Persistent Volume Claim, PVC)
- 파드와 퍼시스던트 볼륨을 연결해주는 역할
- 파드와 퍼시스던트 볼륨은 실제로 직접 연결할 수 없어서 퍼시스던트 볼륨 클레임이 필요함
    ```yaml
    # mysql-pvc.yaml
    apiVersion: v1
    kind: PersistentVolumeClaim

    metadata:
        name: mysql-pvc

    spec: 
        storageClassName: my-storage # pv이름과 pvc이름을 매칭시켜 연결하기 위한 이름
        accessModes:
            - ReadWriteOnce # 읽기와 쓰기가 가능
        resources: # pvc가 pv에 요청하는 용량
            requests:
            storage: 1Gi # 용량
    ```

## 4. 디플로이먼트 파일 작성
    ```yaml
    # mysql-deployment.yaml
    apiVersion: apps/v1
    kind: Deployment

    metadata:
    name: mysql-deployment

    spec:
    replicas: 1
    selector:
        matchLabels:
        app: mysql-db
        
    template:
        metadata:
        labels:
            app: mysql-db
        spec:
        containers:
            - name: mysql-container
                image: mysql
                ports:
                    - containerPort: 3306
                env:
                    - name: MYSQL_ROOT_PASSWORD
                      valueFrom: 
                        secretKeyRef:
                            name: mysql-secret
                            key: mysql-root-password
                    - name: MYSQL_DATABASE
                      valueFrom:
                        configMapKeyRef:
                            name: mysql-config
                            key: mysql-database
                volumeMounts:
                - name: mysql-persistent-storage
                  mountPath: /var/lib/mysql # mysql의 데이터가 저장되는 경로
        volumes:
            - name: mysql-persistent-storage
              persistentVolumeClaim:
              claimName: mysql-pvc # pvc이름
    ```
