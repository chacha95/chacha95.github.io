---
layout: post
title: 딥러닝을 위한 kubernetes2 - 튜토리얼
tags: [Backend]
use_math: true
---

## 1. Minikube를 사용한 클러스터 생성

Minikube는 로컬 머신에 VM(Virtual Machine)을 만들고, 하나의 노드로 구성된 간단한 클러스터를 배포하는 가벼운 쿠버네티스 구현체입니다.

**쿠버네티스는 서로 연결되어 단일 유닛처럼 동작하는 클러스터 오케스트레이션 매니지먼트 툴입니다.** 쿠버네티스의 추상화된 개념을 통해 개별 머신에 얽매이지 않고 도커로 컨테이너화된 애플리케이션을 클러스터에 배포할 수 있습니다. 모든 어플리케이션은 컨테이너화되고, 컨테이너화된 애플리케이션은 특정 머신에 직접 설치되는 배포 모델보다 유연하고 가용성이 높습니다. 쿠버네티스는 애플리케이션 컨테이너를 클러스터에 분산시키고 스케줄링하는 일을 자동화 해줍니다.

### 컨트롤 플레인

**컨트롤 플레인** 은 컨테이너 오케스트레이션 레이어입니다. 노드에게 명령을 내리는 API를 제공합니다. (원래는 마스터라 불렸지만 용어가 control plane으로 바뀜)

마스터는 애플리케이션을 스케줄링하거나, 애플리케이션의 항상성을 유지하거나, 애플리케이션을 스케일링하고, 새로운 변경사항을 순서대로 반영하는 일과 같은 클러스터 내 모든 활동을 조절합니다.

### 노드

**노드는 쿠버네티스 클러스터 내 worker machine으로써 동작하는 VM 또는 물리적인 컴퓨터입니다.** 각 노드들은 노드를 관리하고 마스터와 통신하는 **kubelet**이라는 에이전트를 갖습니다. 운영 트래픽을 처리하는 쿠버네티스 클러스터는 최소 세 대의 노드를 가져야합니다.

**노드는 마스터가 제공하는 쿠버네티스 API를 통해서 마스터와 통신합니다.** 최종 사용자도 쿠버네티스 API를 직접 사용해서 클러스터와 상호작용할 수 있습니다.

> 쿠버네티스 클러스터

![](https://user-images.githubusercontent.com/31475037/89846534-74e94280-dbbc-11ea-8dd7-e8c5cb82438e.png)

쿠버네티스 클러스터는 물리 및 VM 모두에 설치될 수 있습니다. 쿠버네티스 개발을 시작하려면 Minikube가 유용합니다. Minikube는 로컬 머신에 VM을 만들고 하나의 노드로 구성된 간단한 클러스터를 deployment하는 가벼운 쿠버네티스 구현체입니다.

```bash
# version 확인
minikube version

# start cluster 시작
# kubernetes cluster가 VM 위에서 작동됨 
minikube start

# kubectl version
# command line interface
kubectl version

# 클러스터 내 모든 node를 보여줌
kubectl get nodes
```

<br>

## 2. kubectl을 이용한 애플리케이션 deployment

쿠버네티스 클러스터를 구동시키면, 그 위에 컨테이너화된 애플리케이션을 deployment할 수 있습니다. 그러기 위해선, 쿠버네티스 deployment 설정을 만들어야 합니다. Deployment는 쿠버네티스가 애플리케이션의 인스턴스를 어떻게 생성하고 업데이트해야 하는지를 지시합니다. Deployment가 만들어지면, 마스터가 해당 deployment에 포함된 애플리케이션 인스턴스가 클러스터 개별 노드에서 실행되도록 스케줄링합니다.

애플리케이션 인스턴스가 생성되면, 쿠버네티스 deployment 컨트롤러는 지속적으로 인스턴스들을 모니터링합니다. 인스턴스를 구동 중인 노드가 다운되거나 삭제되면, deployment 컨트롤러가 인스턴스를 클러스터 내부의 다른 노드의 인스턴스로 교체합니다. 이런 기능을 self-healing 이라 불리며 쿠버네티스가 제공하는 기능 중 하나입니다.

> 쿠버네티스 클러스터 내부 노드

![](https://user-images.githubusercontent.com/31475037/89846533-73b81580-dbbc-11ea-80d0-a82b4593c50d.png)

그렇다면 어떻게 쿠버네티스 마스터에게 명령을 내려 클러스터 내 노드들에게 명령을 내릴 수 있을까요? 바로 **Kubectl**이라는 쿠버네티스 CLI를 통해 가능합니다. Kubectl은 클러스터와 상호 작용하기 위해 쿠버네티스 API를 사용합니다.

### kubectl

kubectl을 이용해 쿠버네티스 클러스터와 통신해봅시다.

```bash
# kubectl version 확인
kubectl version

# cluster의 node들을 보는 명령어
kubectl get nodes
```

### deploy app

```bash
# app을 deploy하는 명령어
kubectl create deployment
#exmple -> docker hub에서 이미지를 가져옴
kubectl create deployment kubernetes-bootcamp --image=gcr.io/google-samples/kubernetes-bootcamp:v1

# deployments들을 보여줌
kubectl get deployments
```

### view app

쿠버네티스 내에서 실행되는 pod은 격리 된 private 네트워크에서 실행됩니다. 기본적으로 동일한 쿠버네티스 클러스터 내 다른 pod 및 service에서 볼 수는 있지만, 외부에서는 볼 수 없습니다.

Kubectl 명령어를 통해 클러스터 내부로 연결되는 proxy를 설정 할 수 있습니다.

```bash
kubectl proxy
```

이제 host와 Kubernetes 클러스터가 연결되었습니다.

프록시를 사용하면 다른 터미널에서 쿠버네티스 API에 직접 액세스 할 수 있습니다.

```bash
# curl 명령어를 통해 version 확인
curl <http://localhost:8001/version>
```

API 서버는 프록시를 통해 액세스 할 수있는 pod name을 기반으로 각 pod에 대한 end-point를 자동으로 생성합니다.

```bash
# Pod name을 가져옴 
export POD_NAME=$(kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\\n"}}{{end}}')
# 환경 변수 POD_NAME에 저장
echo Name of the Pod: $POD_NAME
```

<br>

## 3. 앱 조사하기

### Pod

Pod은 하나 이상의 애플리케이션 컨테이너들의 그룹을 나타내는 쿠버네티스의 추상적 개념입니다.

pod끼린 다음 자원을 공유합니다.

- volume과 같은, 공유 스토리지
- 클러스터 IP 주소와 같은, network
- 컨테이너 이미지 버전 또는 사용할 특정 포트와 같이, 각 컨테이너가 동작하는 방식에 대한 정보

Pod 내 컨테이너는 IP 주소, port를 공유하고 항상 함께 위치, 스케쥴링 되고 동일 노드 상의 컨텍스트를 공유하면서 동작합니다.

> pod의 구조

![](https://user-images.githubusercontent.com/31475037/90489248-fd954f00-e177-11ea-9e01-098a4ac132c5.png)

Pod은 쿠버네티스 플랫폼 상에서 최소 단위가 됩니다. 우리가 쿠버네티스에서 **deployment**를 생성할 때, 그 deployment는 컨테이너 내부에서 컨테이너와 함께 pod을 생성합니다. 각 pod은 스케쥴 되어진 node에게 묶여지게 됩니다. 그리고 소멸되거나 삭제되기 전까지 해당 노드에 소속됩니다.

> pod의 개념

![](https://user-images.githubusercontent.com/31475037/90487998-33393880-e176-11ea-983c-cbf1dd4721c7.png)

### Node

Pod은 언제나 node 상에서 동작합니다. 하나의 node는 여러 개의 pod을 가질 수 있고, 마스터는 클러스터 내 node를 통해서 pod에 대한 스케쥴링을 자동으로 처리합니다.

모든 쿠버네티스 노드는 최소한 다음과 같이 동작합니다.

- Kubelet은, 쿠버네티스 마스터와 node간 통신을 책임지는 프로세스이며, 하나의 머신 상에서 동작하는 pod과 컨테이너를 관리함

> Node의 개념

![](https://user-images.githubusercontent.com/31475037/90488002-33d1cf00-e176-11ea-918e-8546dd1b7c86.png)

### app config 확인

```bash
# 현재 존재하는 pod 보여줌
kubectl get pods

# pod 안의 정보 확인 -> container 정보나 ip, port 정보 등등
kubectl describe pods
```

### show app

Pod은 격리된 네트워크에서 실행되고 있으므로, 다른 터미널에서 명령을 내릴 수 있도록 pod에 대한 액세스를 proxy 해야합니다.

```bash
kubectl proxy
export POD_NAME=$(kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\\n"}}{{end}}')
echo Name of the Pod: $POD_NAME
```

### 컨테이너 log

```bash
kubectl logs $POD_NAME
```

### 컨테이너에서 명령어 실행

Pod이 실행되면 컨테이너에서 직접 명령을 실행할 수 있습니다. 이를 위해 exec 명령을 사용하고 pod의 이름을 파라미터로 사용합니다.

```bash
kubectl exec $POD_NAME env
kubectl exec -ti $POD_NAME bash
```

<br>

## 4. 앱 외부로 노출하기

쿠버네티스 pod은 언젠가는 죽게됩니다.(life-cycle 존재) node가 죽으면, 노드 상에서 동작하는 pod들도 종료됩니다. 이런 문제를 막기위해 replicaset 설정을 합니다.

### ReplicaSet

**ReplicaSet**은 앱이 지속적으로 동작할 수 있도록 새로운 pod들의 생성을 통해, 동적으로 클러스터를 미리 지정해 둔 상태로 되돌립니다.

> ReplicaSet

![](https://user-images.githubusercontent.com/31475037/90489243-fbcb8b80-e177-11ea-9bba-b6c77065af51.png)

### Service

쿠버네티스에서 **service**는 논리적인 pod set과 그 pod들에 접근할 수 있는 policy를 정의하는 추상적 개념입니다. Service는 YAML 또는 JSON을 이용하여 정의됩니다. Service가 대상으로 하는 pod set은 보통 **Label selector**에 의해 식별됩니다.

비록 각 pod들이 고유 IP를 갖고 있기는 하지만, 그 IP들은 service의 도움없인 클러스터 외부로 노출 될 수 없습니다.

- **ClusterIP** - 클러스터 내에서 내부 IP 에 대해 서비스를 노출해줍니다. 이 방식은 오직 클러스터 내에서만 service가 접근될 수 있도록 해줍니다.
- **NodePort** - 노드들의 동일한 포트에 서비스를 노출시켜줍니다. 클러스터 외부로부터 service가 접근할 수 있도록 해줍니다. ClusterIP의 상위 집합입니다.
- **LoadBalancer** - 기존 클라우드에서 외부용 로드밸런서를 생성하고 서비스에 고정된 공인 IP를 할당해줍니다. NodePort의 상위 집합입니다.
- **ExternalName** - 이름으로 CNAME 레코드를 반환함으로써 임의의 이름을 이용하여 서비스를 노출시킵니다.

> Service의 개념

![](https://user-images.githubusercontent.com/31475037/90487996-32080b80-e176-11ea-941b-1a4a359583ce.png)

### Create service

minikube가 클러스터를 시작할 때 기본적으로 생성되는 kubernetes라는 서비스가 있습니다.

새 서비스를 만들고 외부 트래픽에 노출하기 위해 NodePort를 매개 변수로 사용하여 expose 명령을 사용합니다.

이제 kubernetes-bootcamp라는 실행 중 서비스가 있습니다.

```bash
# 현재 실행중인 service 확인
kubectl get service

# 외부 트래픽에 노출(NodePort 사용)
kubectl expose deployment/kubernetes-bootcamp --type="NodePort" --port 8080

# NodePort 옵션을 통해 외부에서 열린 포트를 확인
kubectl describe services / kubernetes-bootcamp
```

### Using labels

Deployment는 pod의 label을 자동으로 생성했습니다.

```bash
# label 이름 출력
kubectl describe deployment

# label을 이용해 pod 목록 출력
kubectl get pods -l run = kubernetes-bootcamp
```

### Deleting a service

```bash
# Service를 삭제하려면 delete service 명령을 사용 가능
kubectl delete service -l run = kubernetes-bootcamp

# 애플리케이션을 종료하려면 deployment도 삭제
kubectl exec -ti $POD_NAME curl localhost:8080
```

<br>

## 5. 앱 스케일링하기

트래픽이 증가하면, 사용자 요청에 맞추어 애플리케이션의 규모를 조정할 필요가 있습니다. 이 때, deployment의 replication 수를 변경하면 **스케일링**이 진행됩니다.

Deployment를 scale-out하면 신규 pod이 생성되어 가용할 자원이 있는 node에 스케줄됩니다. 스케일링 기능은 desired state까지 pod의 수를 늘립니다. 쿠버네티스는 pod의 오토스케일링도 지원합니다. (**0까지 스케일링**하는 것도 가능하며, 이 경우 해당 deployment의 모든 pod이 종료됨)

<br>

## 6. 앱 업데이트하기

사용자들은 애플리케이션이 항상 사용 가능 상태일 것이라 생각하고, 개발자들은 하루에 여러번씩 새로운 버전을 배포해야합니다.

쿠버네티스에서는 이것을 **rolling update**라 부릅니다. Rolling update는 pod 인스턴스를 점진적으로 업데이트하여 deployment 업데이트가 서비스 중단 없이 이뤄지게 해줍니다.

이런 rolling update는 deployment controller에 의해 관리됩니다.

애플리케이션 스케일링과 유사하게, deployment가 외부로 노출되면, 서비스는 업데이트가 이루어지는 동안 오직 가용한 pod에게만 트래픽을 로드밸런스합니다.

Rolling update는 다음 일을 합니다.

- 하나의 환경에서 또 다른 환경으로의 애플리케이션 프로모션(컨테이너 이미지 업데이트를 통해)
- 이전 버전으로의 **roll-back**
- 서비스 중단 없는 애플리케이션의 CI/CD

<br>

## 7. Storages

### **Volume(임시적인 데이터 저장)**

pod에 연결되어 디렉토리 형태로 데이터를 저장 할 수 있도록 제공합니다. Pod의 container들 끼리 공유가능하며, pod의 life-cycle과 일치하여 pod 삭제시 같이 삭제됩니다.

### **Persistence Volume(영구적 데이터 저장)**

k8s 클러스터 관리자에 의해 제공된 저장소의 일부입니다. volume과 유사하지만 pod과는 독립적인 life-cycle을 가집니다. 사용자가 용량, 모드 필요한 정보와 함께 PVC(PersistenceVolumeClaim)를 생성하면 이에 대응하는 Persistence Volume이 생성됩니다.

<br>

**출처**

[대화형 튜토리얼 - 클러스터 생성하기](https://kubernetes.io/ko/docs/tutorials/kubernetes-basics/create-cluster/cluster-interactive/)

[subicura 블로그](https://subicura.com/2019/05/19/kubernetes-basic-1.html)