![Kubernetes](https://github.com/user-attachments/assets/3ec2d35d-184a-480a-878f-1f89f9547880)

# Kubernets 자주사용하는 명령어 정리

## .새로 재시작
```
$ kubectl rollout restart deployment <디플로이먼트 이름>
```
- 기존 파드는 삭제하고 새로 파드를 만들어 끼워넣는 방식