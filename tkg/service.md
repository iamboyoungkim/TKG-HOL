# 서비스 생성 작업 (Nodeport / LoadBalancer)

Pod의 애플리케이션을 네트워크에 노출시키는 서비스를 생성을 한 뒤 접속하는 방법에 대해 설명합니다.

## 1. nodeport.yaml 생성, 배포
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

~~~
k get svc
k get nodes -o wide
~~~

<img width="1120" alt="Screenshot 2023-03-08 at 2 17 19 PM (2) copy" src="https://user-images.githubusercontent.com/30145956/223763893-4b7e4b72-5554-4158-bbb8-c772339fa7b8.png">

브라우저를 사용하여 <nginx node의 INTERNAL-IP>:<nodeport 포트 번호> 로 접속을 진행하면 nginx 서버를 확인할 수가 있습니다.

<img width="1105" alt="Screenshot 2023-03-08 at 2 17 11 PM (2) copy" src="https://user-images.githubusercontent.com/30145956/223770646-487d1e06-f626-428c-a3be-616a045973e3.png">

## 2. nginx-lbsvc.yaml 생성, 배포

Nginx 컨테이너를 external loadbalancer로 사용하는 서비스를 배포합니다

vi 등의 편집기로 nginx-lbsvc.yaml를 생성하고 다음과 같이 입력합니다.
~~~
vi nginx-lbsvc.yaml
~~~

~~~
kind: Service
apiVersion: v1
metadata:
  name: srvclb-ngnx
spec:
  selector:
    app: hello
    tier: frontend
  ports:
  - protocol: "TCP"
    port: 80
    targetPort: 80
  type: LoadBalancer
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: loadbalancer
spec:
  replicas: 2
  selector:
    matchLabels:
      app: hello
  template:
    metadata:
      labels:
        app: hello
        tier: frontend
    spec:
      containers:
      - name: nginx
        image: "nginxdemos/hello"
~~~

YAML 파일을 적용합니다.

~~~
k apply -f nginx-lbsvc.yaml
~~~

svclb-nginx 서비스가 실행되고 있는지 확인하기 위하여 k get svc 를 입력합니다. 

~~~
k get svc
~~~

svclb-nginx 의 EXTERNAL-IP 주소를 확인 후, 브라우저에 이를 입력하여 Nginx Loadbalancer Service에 접속합니다.

<img width="1084" alt="Screenshot 2023-03-08 at 2 22 26 PM (2)" src="https://user-images.githubusercontent.com/30145956/223774708-32d1e131-55b3-4668-9d94-6966d5d07f1e.png">
