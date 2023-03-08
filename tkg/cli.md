cli 사용해서 클려스터 provisioning/scale

tanzu cluster list

tanzu cluster list --include-management-cluster
리스트 조회하고


클러스터 생성 하려면 기존 mgmt cluster configfile 복제해서 써야한다.

~/.config/tanzu/tkg/clusterconfigs 하에 있음
cp 로 복사해가지고 파일만들고

CLUSTER_NAME 이랑 리소스들 작게 변경해서

tanzu cluster create -f <파일명> -v 6 으로 생성



### 2. TKG Workload Cluster scale
워크로드 클러스터를 수평으로 확장하려면 'tanzu cluster scale' 명령을 사용합니다.
-c 옵션으로 컨트로 플레인 노드 수를 변경하며, -w 옵션을 지정하여 워크로드 노드 수를 변경합니다.
~~~
tanzu cluster scale cluster_name -c 3 -w 5
~~~

이후 다시 tanzu cluster list 를 수행해 클러스터가 성공적으로 scale 되어 running 상태임을 확인 후 다음 단계로 이동합니다.


