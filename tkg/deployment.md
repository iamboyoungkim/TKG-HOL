# Tanzu Kubernetes Grid 환경에서의 Deployment 생성 실습
본 가이드는 TKG 클러스터 생성 후, 테스트 nginx 웹 서버를 배포하는 방법에 대하여 설명합니다.

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

## 2. deployment.yaml 배포
k apply -f deployment.yaml 를 사용하여 nginx의 이미지로 Pod 3개를 생성합니다.

~~~
k apply -f deployment.yaml
~~~

이후, k get deploy 로 배포 과정을 확인하거나 k get po 로 각 pod의 배포 상태를 확인합니다.
~~~
k get deploy
k get po
~~~

<img width="1000" alt="Screenshot 2023-03-08 at 2 11 38 PM (2)" src="https://user-images.githubusercontent.com/30145956/223756631-c52550b5-007f-462b-b78e-26bc46da8729.png">
