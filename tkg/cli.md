cli 사용해서 클려스터 provisioning/scale





### 2. TKG Workload Cluster scale
워크로드 클러스터를 수평으로 확장하려면 'tanzu cluster scale' 명령을 사용합니다.
--controlplane-machine-count 옵션으로 컨트로 플레인 노드 수를 변경하며, --worker-machine-count 옵션을 지정하여 워크로드 노드 수를 변경합니다.
~~~
tanzu cluster scale cluster_name --controlplane-machine-count 5 --worker-machine-count 10
~~~

이후 다시 tanzu cluster list 를 수행해 클러스터가 성공적으로 scale 되어 running 상태임을 확인 후 다음 단계로 이동합니다.

