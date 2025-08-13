# Helm이란?
- 쿠버네티스의 패키지라 보면 된다
- 맥북에선 Homebrew느낌?

## 1. Chart란?
- 쿠버네티스 매니페스트 파일들과 메타정보 파일들을 묶어놓은것을 차트라 함
- Chart만을 가지고 쿠버네티스에 배포 가능
- artifactHUB에 차트들이 많음
    - docker hub 같은 느낌

## 2. value.yaml
- template에 있는 값을 동적으로 변경하고 주입 할 때 사용

## 3. Chart.yaml
- helm 차트의 버전, 이름 등 메타정보를 저장
- apiVersion, name, version은 꼭 기입해야한다
    - apiVersion : 
    - name : 
    - version : 