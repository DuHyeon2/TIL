![Kubernetes](https://github.com/user-attachments/assets/3ec2d35d-184a-480a-878f-1f89f9547880)


## 작성중................


# Kubernets 설치

## 1. Rocky linux 기본설치
 - 패키지 업데이트, 타임존 설정

## 2. Ubuntu 24 + k8s 1.27 설치
```bash
$ curl -O https://raw.githubusercontent.com/projectcalico/calico/v3.27.3/manifests/calico.yaml

$ kubectl apply -f calico.yaml
```