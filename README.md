# RHACM-Overview-and-Operator-Install

### 1. Architecture

![01_architecture](C:\Works\01_자료\01_OCP\05_OCP_Demo_hyou\RHACM\01_architecture.png)

- `Hub Cluster` : Hub Cluster는 RHACM을 실행하는 한 가지만 수행합니다. 클러스터 수명 주기, 애플리케이션 수명 주기, 거버넌스, 위험 및 규정 준수(GRC) 기능을 제공하는 모든 RHACM 구성요소는 이 클러스터에서 실행됩니다. Hub Cluster를 관리 할 수는 없습니다. Hub Cluster를 관리되는 클러스터로 추가하면 오류 메시지가 표시되고 해당 작업이 금지됩니다.
- `Managed Cluster` : Managed Cluster는 워크로드를 실행하는 클러스터입니다. 고객 애플리케이션 워크로드는 분명 여기에서 실행되지만 Managed Cluster도 RHACM 엔드포인트 구성요소를 실행합니다. 이러한 엔드포인트 구성요소는 Pod 로 배포되고 다른 애플리케이션과 마찬가지로 클러스터의 노드에서 실행되도록 예약됩니다.

- Managed Cluster에 배포하는 엔드포인트는 Hub Cluster에서 실행 중인 RHACM과 통신합니다.
- Managed Cluster는 OpenShift 또는 Kubernetes의 다른 종류일 수 있지만 Hub Cluster는 OpenShift에만 배포할 수 있습니다. 



### 2. Cluster Lifecycle Functionality

RHACM의 클러스터 수명 주기 기능을 사용하면 다음 매개변수 내에서 Kubernetes 클러스터의 배포, 삭제, 가져오기 및 업그레이드를 관리할 수 있습니다.



- 지원되는 플랫폼에 새로운 OpenShift 클러스터 배포 :

  Amazon Web Services, Microsoft Azure 및 Google Cloud가 포함됩니다. 베어메탈은 TP 기능으로 포함됩니다.

- RHACM에 의해 배포된 OpenShift 클러스터 삭제 또는 파괴 :

  가져온 클러스터는 제거할 수 없지만 더 이상 관리하지 않으려면 RHACM에서 제거할 수 있습니다.

- 기존 Kubernetes 클러스터 가져오기 : 

  특정 플랫폼에 국한되지 않습니다.

- Managed Cluster 업그레이드 :

  새로운 클러스터를 배포하는 것은 RHACM의 클러스터 수명 주기의 핵심 기능입니다. 배포를 정의할 때 UI 마법사 또는 YAML 파일을 사용하거나 둘의 조합을 사용할 수 있습니다. 마법사에서 대부분의 배포를 정의한 다음 YAML 파일을 수정하여 UI 요소에 표시되지 않는 항목을 조정할 수 있습니다.



### 3. Application Lifecycle Functionality

RHACM의 애플리케이션 수명 주기 기능은 Managed Cluster에서 애플리케이션 리소스를 관리하는 데 사용되는 프로세스를 제공합니다. 이를 통해 Kubernetes 사양을 사용하여 단일 또는 다중 클러스터 애플리케이션을 정의할 수 있지만 개별 클러스터에 대한 리소스의 배포 및 수명 주기 관리를 추가로 자동화할 수 있습니다. 단일 클러스터에서 실행되도록 설계된 애플리케이션은 간단하며 OpenShift 기본 사항을 사용하여 작업할 때 익숙해져야 합니다.

다중 클러스터 애플리케이션을 사용하면 애플리케이션 구성 요소를 실행하는 클러스터에 대해 정의한 규칙 집합을 기반으로 이러한 동일한 리소스를 여러 클러스터에 배포할 수 있습니다.



- RHACM의 애플리케이션 수명 주기 모델이 구성되는 다양한 구성 요소

  | Resource      | Purpose                                                      |
  | ------------- | ------------------------------------------------------------ |
  | Channel       | 개체 저장소, Kubernetes 네임스페이스, Helm 리포지토리 또는 Github 리포지토리와 같은 배포 가능한 리소스가 저장되는 위치를 정의합니다. |
  | Subscription  | 대상 클러스터에 배포할 Resource 리소스에서 사용 가능하고 배포 가능한 리소스를 식별하는 정의입니다. |
  | PlacementRule | Subscription이 애플리케이션을 배포하고 유지 관리하는 대상 클러스터를 정의합니다. Subscription 리소스로 식별되고 Resource 리소스에 정의된 위치에서 가져온 Kubernetes 리소스로 구성됩니다. |
  | Application   | 여기에서 구성 요소를 더 쉽게 볼 수 있는 단일 리소스로 그룹화 하는 방법입니다. 애플리케이션 리소스는 일반적으로 Subscription 리소스를 참조합니다. |

  이는 RHACM이 설치될 때 생성되는 사용자 정의 리소스(CRD)에 의해 정의된 모든 Kubernetes 사용자 지정 리소스입니다. 이러한 객체를 Kubernetes 기본 객체로 생성하면 Pod와 동일한 방식으로 상호 작용할 수 있습니다. 예를 들어, `oc get pod`가 배포된 Pod  목록을 검색하는 것처럼 `oc get application`을 실행하면 배포된 RHACM 애플리케이션 목록이 검색됩니다.

### 3.1) RHACM 2.4 Operator Install

RHACM 2.4 Operator를 설치하기 위해서는 OpenShift Cluster 준비되어 있어야 합니다.

해당 Cluster를 RHACM의 Hub Cluster로 사용할 것이므로 **RHACM Operator**를 설치합니다.

- 설치 방법

  - OpenShift Console 접속 > Operator Hub > Red Hat Advanced Management for Kubernetes Operator 검색 > 설치

    ![04_rhacm_operator](C:\Works\01_자료\01_OCP\05_OCP_Demo_hyou\RHACM\04_rhacm_operator.png)
    
  - **Install** 버튼을 선택하여 설치를 계속 진행합니다.
  
    ![05_rhacm_operator_install](C:\Works\01_자료\01_OCP\05_OCP_Demo_hyou\RHACM\05_rhacm_operator_install.png)
  
  - 기본으로 선택된 정보를 그대로 두고 설치를 계속 진행합니다.
  
    ![06_rhacm_operator_install_processing](C:\Works\01_자료\01_OCP\05_OCP_Demo_hyou\RHACM\06_rhacm_operator_install_processing.png)
  
  - 설치가 완료되면 **open-cluster-management** 네임스페이스에 Operator가 설치된 것을 확인할 수 있습니다.
  
    ![07_rhacm_operator_install_success](C:\Works\01_자료\01_OCP\05_OCP_Demo_hyou\RHACM\07_rhacm_operator_install_success.png)
  
  - **RHACM Operator**가 설치된 OpenShift Cluster를 RHACM Hub Cluster로 사용하기 위해서 **MultiClusterHub** 인스턴스를 생성합니다.
  
  - **open-cluster-management** 프로젝트 선택 > **Installed Operators** > **RHACM Operator 선택** > **MultiClusterHub 인스턴스 생성**
  
    ![08_multiclusterhub](C:\Works\01_자료\01_OCP\05_OCP_Demo_hyou\RHACM\08_multiclusterhub.png)
  
  - Name 항목을 알맞게 입력하고 Create 버튼을 선택하여 인스턴스를 구성합니다.
  
    ![09_create_multiclusterhub](C:\Works\01_자료\01_OCP\05_OCP_Demo_hyou\RHACM\09_create_multiclusterhub.png)
  
  - OpenShift Cluster Console에서 **Workloads** > **Pods** 확인 시 새로운 Container들이 생성되는 것을 실시간으로 확인할 수 있으며, 전체적으로 리소스가 다 올라오는 데 시간이 조금 소요 될 수 있습니다.
  
    ![10_resource_pod_mon](C:\Works\01_자료\01_OCP\05_OCP_Demo_hyou\RHACM\10_resource_pod_mon.png)
  
  - 인스턴스 배포가 완료되면 다음과 같이 **MultiClusterHubs**의 상태가 **Running**으로 변경되게 됩니다.
  
    ![11_rhacm_multiclusterhub_success](C:\Works\01_자료\01_OCP\05_OCP_Demo_hyou\RHACM\11_rhacm_multiclusterhub_success.png)
  
  - 이제, OpenShift Console Main 화면의 왼쪽 맨위의 기어 모양을 선택하면 **Advanced Cluster Management(RHACM) Console**에 접속할 수 있게 됩니다. 그림에서 보이는 것처럼 **Advanced Cluster Management** 항목이 새로 추가된 것을 볼 수 있습니다.
  
    ![12_rhacm_console](C:\Works\01_자료\01_OCP\05_OCP_Demo_hyou\RHACM\12_rhacm_console.png)
  
  - Overview 화면
  
    RHACM의 콘솔에 접속한 화면입니다. 현재는 `local-cluster` 인 **Hub Cluster**의 정보만 보이게 되고, **Managed Cluster**를 추가하거나 기존 Cluster를 **import**하게 되면 **Hub Cluster**에서 관리되는 OpenShift Cluster를 확인 할 수 있습니다.
  
    ![13_rhacm_overview](C:\Works\01_자료\01_OCP\05_OCP_Demo_hyou\RHACM\13_rhacm_overview.png)
  
  - Cluster 정보
  
    ![14_rhacm_clusters](C:\Works\01_자료\01_OCP\05_OCP_Demo_hyou\RHACM\14_rhacm_clusters.png)

> 간단하게 정리하면, OpenShift 환경에서는 중앙에서 여러  Clsuter를 관리하는 솔루션인 RHACM을 Operator를 통해 쉽게 설치할 수 있습니다.  제공되는 기능으로는 Cluster를 생성, 삭제, 파괴 및 Application 배포, Governance를 중앙에서 일괄적으로 제어 할 수 있습니다.
