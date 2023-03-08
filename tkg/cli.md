cli 사용해서 클려스터 provisioning/scale

## CLI를 사용하여 클러스터 배포, Scaling

본 과정에서는 Tanzu CLI를 사용하여 TKG 클러스터를 배포하고 워크로드 및 컨트롤 플레인 노드를 Scailing 하는 법에 대하여 설명합니다.

### 1. 클러스터 목록 확인

실습 한경에서 PuTTY 를 실행하여 Jumpbox 세션에 접속합니다. 그 뒤 로그인을 하지 않았으면 tanzu login 으로 'tap-mgmt-01' 과 같은 매니지먼트 클러스터로 로그인을 합니다.
```cmd
$ tanzu login
? Select a server
> tap-mgmt-01
```
그 다음, tanzu cluster list 를 수행해 현재 실행되고 있는 클러스터가 있는지 조회합니다.

> 클러스터 목록 조회
```cmd
tanzu cluster list
```

> 컨트롤 플레인을 포함한 클러스터 목록 조회
```cmd
tanzu cluster list --include-management-cluster
```
ㄷ
다
다으
다음
다음ㄱ
다음고
다음과


<img width="1062" alt="Screenshot 2023-03-08 at 1 54 01 PM" src="https://user-images.githubusercontent.com/30145956/223631937-79a3f4c0-1f31-42a9-9ce9-f4881b1fe627.png">


tanzu cluster list --include-management-cluster
리스트 조회하고


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


