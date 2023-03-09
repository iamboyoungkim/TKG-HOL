# Tanzu Kubernetes Grid 환경에서의 Deployment 생성 실습
본 가이드는 TKG 클러스터 생성 후, 테스트 nginx 웹 서버를 배포하는 방법에 대하여 설명합니다.

## 0. kubectl context 를 워크로드로 포인트 설정

워크로드 클러스터를 생성 후 nginx 웹 서버 pod를 배포하기 위해, kubectl context를 워크로드 클러스터로 포인트하도록 설정합니다.


<img width="1250" alt="Screenshot 2023-03-09 at 3 38 04 PM" src="https://user-images.githubusercontent.com/30145956/223940771-7c575056-b7e8-4328-828d-c59c34d2f2b3.png">

kubectl config get-contexts 로 현재 context 를 확인합니다. 현재 환경에서는 tap-build-cluster-admin@tap-build-cluster 로 context가 지정되어 있습니다. ('*'로 표시)
~~~
kubectl config get-contexts
~~~
<img width="1250" alt="Screenshot 2023-03-09 at 1 23 11 PM" src="https://user-images.githubusercontent.com/30145956/223924919-21b6e742-427b-477f-ad05-e5aa324fc409.png">
이후, kubectl config use-context <생성한클러스터context> 이름으로 kubectl의 context를 변경합니다. 현재 환경에서는 kubectl config use-context test-work-admin@test-work 로 context 를 설정했습니다. 이후 다시 kubectl config get-contexts 로 확인으 하면 워크로드 클러스터로 context 가 바뀐 것을 알 수가 있습니다.

~~~
kubectl config get-contexts <생성한클러스터context>
~~~

<img width="1250" alt="Screenshot 2023-03-09 at 1 23 11 PM copy" src="https://user-images.githubusercontent.com/30145956/223925854-f7f203ba-791b-4afd-bcff-dd5ae57abfaf.png">



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

nginx-deployment pod 3개가 생성된 것을 알 수가 있습니다.
