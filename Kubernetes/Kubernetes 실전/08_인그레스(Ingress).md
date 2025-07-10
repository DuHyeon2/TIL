![Kubernetes](https://github.com/user-attachments/assets/3ec2d35d-184a-480a-878f-1f89f9547880)

# Ingress
- 외부에서 쿠버네티스 클러스터 내부의 서비스로 접근할 수 있도록 해주는 역할
- 접두어 단위로 service마다 라우팅
- 도메인을 기반으로 하는 서비스
- Ingress-Controller가 Ingress 리소스를 감시하고, 해당 리소스에 정의된 규칙에 따라 트래픽을 라우팅
- Ingress-Controller 설치 시 자동으로 AWS LoadBalancer가 생성됨
- 정리하자면 LoadBalancer가 Ingress-Controller로 요청을 하면 url 패턴을 분석하여 해당하는 서비스로 요청을 전달하는 구조

## 1. Ingress Controller 설치