cli 사용해서 클려스터 provisioning/scale

# CLI를 사용하여 클러스터 배포, Scaling

본 과정에서는 Tanzu CLI를 사용하여 TKG 클러스터를 배포하고 워크로드 및 컨트롤 플레인 노드를 Scailing 하는 법에 대하여 설명합니다.

## 1. 클러스터 목록 확인
### 1. Tanzu 세션 로그인

실습 한경에서 PuTTY 를 실행하여 Jumpbox 세션에 접속합니다. 그 뒤 로그인을 하지 않았으면, tanzu login 으로 'tap-mgmt-01' 과 같은 매니지먼트 클러스터로 로그인을 합니다.
```cmd
$ tanzu login
? Select a server
> tap-mgmt-01
```
### 2.Tanzu Cluster List 확인
Tanzu cluster list 를 수행해 현재 실행되고 있는 클러스터가 있는지 조회합니다.

> 클러스터 목록 조회
```cmd
tanzu cluster list
```
만일 클러스터가 이전에 배포되었을 경우, 다음과 같은 화면이 출력됩니다.

<img width="1062" alt="Screenshot 2023-03-08 at 1 54 01 PM" src="https://user-images.githubusercontent.com/30145956/223631937-79a3f4c0-1f31-42a9-9ce9-f4881b1fe627.png">

> 컨트롤 플레인을 포함한 클러스터 목록 조회
```cmd
tanzu cluster list --include-management-cluster
```
컨트롤 플레인을 포함한 클러스터가 이전에 배포되었을 경우, 다음과 같은 화면이 출력됩니다.

<img width="1490" alt="Screenshot 2023-03-08 at 1 54 10 PM" src="https://user-images.githubusercontent.com/30145956/223641030-b30ad5ab-9b91-4db5-ac86-c847da9a4a02.png">

## 2. mgmt cluster config 파일을 사용하여 클러스터 생성

### 1. mgmt cluster config 파일 복제

클러스터를 생성하려면 Tanzu CLI 에서 생성된 Management Cluster Config 파일을 복제하고, 편집하여 생성합니다.
Management Cluster Config 파일은 ~/.config/tanzu/tkg/clusterconfig 위치에 있습니다. 


```cmd
cd  ~/.config/tanzu/tkg/clusterconfig
ls -al
```

그 중 UI installer 설치 시, 생성되었던 무작위 이름의 YAML 파일을 복제하여 새로운 이름으로 파일을 생성합니다. <자동생성파일명> 를 생성된 mgmt cluster config 파일 이름으로 대체합니다. <파일명> 에 지정하고자 하는 파일 이름을 적습니다. 

```cmd
cp  <자동생성파일명>.yaml <파일명>.yaml
```

<img width="690" alt="Screenshot 2023-03-08 at 1 57 34 PM (2) copy" src="https://user-images.githubusercontent.com/30145956/223652351-3f06fca6-4a7d-44b9-8cf0-8bc1a42ce6d8.png">
  
### 2. mgmt cluster config 복제 파일 편집

vi 등의 편집기로 복제하 파일을 편집합니다. 다음과 같이 변수명을 설정합니다.
  
|변수명|지정값|
|------|---|
|CLUSTER_NAME|지정하고자 하는 클러스터 이름|
|VSPHERE_CONTROL_PLANE_DISK_GIB|20|
|VSPHERE_CONTROL_PLANE_MEM_GIB|4|
|VSPHERE_CONTROL_PLANE_NUM_CPUS|2|
|VSPHERE_WORKER_DISK_GIB|20|
|VSPHERE_WORKER_MEM_GIB|4|
|VSPHERE_WORKER_NUM_CPUS|2|

<img width="1493" alt="Screenshot 2023-03-08 at 1 55 09 PM (2)" src="https://user-images.githubusercontent.com/30145956/223660055-f497ca46-f2ae-410c-9633-c04e2acffc32.png">

<img width="1493" alt="Screenshot 2023-03-08 at 2 05 36 PM (2)" src="https://user-images.githubusercontent.com/30145956/223660131-64805d7c-4ea4-4827-a197-bcb91262bc29.png">

### 3. 클러스터 생성

이후 tanzu cluster create -f <파일명> -v 6 으로 클러스터를 생성해 줍니다. -v 는 생성 시 로그의 상세도 (0~9)를 나타냅니다.

```cmd
tanzu cluster create -f <파일명> -v 6 
```
<img width="1489" alt="Screenshot 2023-03-08 at 1 57 34 PM (2) copy 2" src="https://user-images.githubusercontent.com/30145956/223667436-8a00c580-c83a-4ea5-92e4-f1b828c2bef4.png">
<img width="1491" alt="Screenshot 2023-03-08 at 2 05 21 PM (2)" src="https://user-images.githubusercontent.com/30145956/223667750-1c4576d2-cc70-43eb-9699-6d1df85c587d.png">


  
------

클러스터 생성 하려면 기존 mgmt cluster configfile 복제해서 써야한다.

~/.config/tanzu/tkg/clusterconfigs 하에 있음
cp 로 복사해가지고 파일만들고

CLUSTER_NAME 이랑 리소스들 작게 변경해서

tanzu cluster create -f <파일명> -v 6 으로 생성



### 2. TKG Workload Cluster scale
워크로드 클러스터를 수평으로 확장하려면 'tanzu cluster scale' 명령을 사용합니다.
-c 옵션으로 컨트롤 플레인 노드 수를 변경하며, -w 옵션을 지정하여 워크로드 노드 수를 변경합니다.
~~~
tanzu cluster scale cluster_name -c 3 -w 5
~~~

이후 다시 tanzu cluster list 를 수행해 클러스터가 성공적으로 scale 되어 running 상태임을 확인 후 다음 단계로 이동합니다.


