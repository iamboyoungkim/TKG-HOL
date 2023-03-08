# Tanzu Kubernetes Grid 환경에서의 Deployment 생성 실습
본 가이드는 TKG 클러스터 생성 후, 테스트 nginx 웹 서버르 배포하는 방법에 대하여 설명합니다.

## 1. deployment.yaml 생성
vi 등의 편집기로 deployment.yaml를 생성하여 다음과 같이 입력합니다.
~~~
vi deployment.yaml
~~~
~~~
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
~~~

## 1. deployment.yaml 배포
k apply -f deployment.yaml 를 사용하여 nginx의 이미지로 Pod 3개를 생성합니다.

~~~
k apply -f deployment.yaml
~~~

이후, k get deploy 로 배포 과정을 확인하거나 k get po를 통하여 
<img width="1000" alt="Screenshot 2023-03-08 at 2 11 38 PM (2)" src="https://user-images.githubusercontent.com/30145956/223756631-c52550b5-007f-462b-b78e-26bc46da8729.png">

  - [설치 방법](../tap/jumpbox-prepare.md)

> **_NOTE:_** 아래 가이드는 주어진 점프박스를 사용했을 때 기준으로 작성되었습니다. 일반적인 vSphere air-gap 환경에서의 설치는 다음 문서를 참고해 주시기 바랍니다.    [링크](https://github.com/tanzukorea/tanzu-install/blob/main/tap/airgapped/installation-on-vsphere.md)

### 2. TKG Workload Cluster scale
tanzu cluster list 명령어로 조회 시, 현재 워크로드 클러스터의 worker node 갯수가 1개로 설정되어 있습니다. TAP 실습을 위한 리소스 확보를 위해 worker node를 3개로 변경하는 작업을 먼저 시행합니다.

