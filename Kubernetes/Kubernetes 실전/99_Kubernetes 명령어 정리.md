![Kubernetes](https://github.com/user-attachments/assets/3ec2d35d-184a-480a-878f-1f89f9547880)

# Kubernets 자주사용하는 명령어 정리

## .Deployment 새로 재시작
```
$ kubectl rollout restart deployment <디플로이먼트 이름>
```
- 기존 파드 이미지는 삭제하고 새로 파드를 만들어 끼워넣는 방식
- 이 방식은 yaml 파일을 수정하지 않고도 최신 이미지를 땡겨와 파드를 재시작할 수 있는 방법