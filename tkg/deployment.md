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

## 3. nodeport.yaml 생성, 배포
deployment.yaml 로 생성된 nginx pod를 외부 ip 및 포트로 노출시키는 nodeport.yaml 서비스를 생성 및 배포합니다. (설명 맞나요...?)

vi 등의 편집기로 deployment.yaml를 생성하여 다음과 같이 입력합니다.
~~~
vi nodeport.yaml
~~~
~~~
apiVersion: v1
kind: Service
metadata:
  name: my-nginx
  labels:
    run: my-nginx
spec:
  type: NodePort
  ports:
  - port: 8080
    targetPort: 80
    protocol: TCP
    name: http
  selector:
    app: nginx
~~~

k apply -f nodeport.yaml 를 사용하여 NodePort 서비스를 생성합니다.

~~~
k apply -f nodeport.yaml
~~~

k get svc 로 실행되고 있는 서비스를 확인합니다. 또한 접속할 서비스 포트를 확인합니다. 해당 예시의 경우에는 포트 30929입니다. k get nodes -o wide 로 노드의 INTERNAL-IP 정보를 확인합니다.

<img width="1120" alt="Screenshot 2023-03-08 at 2 17 19 PM (2) copy" src="https://user-images.githubusercontent.com/30145956/223763893-4b7e4b72-5554-4158-bbb8-c772339fa7b8.png">

브라우저를 사용하여 <nginx node의 INTERNAL-IP>:<nodeport 포트 번호> 로 접속을 진행하면 nginx 서버를 확인할 수가 있습니다.

<img width="1105" alt="Screenshot 2023-03-08 at 2 17 11 PM (2) copy" src="https://user-images.githubusercontent.com/30145956/223770646-487d1e06-f626-428c-a3be-616a045973e3.png">


