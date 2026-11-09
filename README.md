# Data Fabric 배포 구성

> ## 저장소 상태
>
> 본 저장소는 아래 과제의 **1단계(2023~2024)** Kubernetes(Helm/Argo) 기반
> 배포 구성입니다. 각 구성요소의 빌드·실행 방법은 해당 저장소 README를
> 참고하십시오.
>
> - 과제명: 분산된 데이터에 대한 논리적 데이터 통합과 복합분석을 지원하는 데이터 패브릭 기술개발
> - 과제번호: RS-2023-00222191
> - 개발 기간: 2024년 12월

## 개요

지식그래프 분석 작업 실행에 필요한 기반 구성요소를 Kubernetes에 설치하기 위한 Helm 차트입니다.
분석 작업을 워크플로로 실행하는 Argo Workflows와, 분석 메타데이터를 보관하는 MySQL로 구성됩니다.

## 저장소 구성

| 경로 | 차트 | 설명 |
|---|---|---|
| `install/argo/argo` | `argo-workflows` 0.22.8 (appVersion v3.4.4) | 분석 작업을 Kubernetes 워크플로로 실행합니다. Argo Workflows 공식 차트를 과제 환경에 맞게 설정한 것입니다. |
| `install/mysql/mysql` | `datafabric-mysql` 0.1.0 | 분석 메타데이터를 보관하는 MySQL입니다. |

## 설치

### 요구 사항

- Kubernetes 1.24 이상
- Helm 3

### 설치 절차

네임스페이스를 준비합니다.

```bash
kubectl create namespace datafabric
```

MySQL을 설치합니다.

```bash
helm install datafabric-mysql install/mysql/mysql -n datafabric \
  --set mysql.image=<컨테이너 이미지> \
  --set mysql.username=<계정> \
  --set mysql.password=<비밀번호> \
  --set mysql.database=datafabric
```

Argo Workflows를 설치합니다.

```bash
helm install argo-workflows install/argo/argo -n datafabric
```

`values.yaml`의 계정 정보와 이미지 경로는 **설치 환경에 맞게 반드시 지정하십시오.**
차트에 포함된 값은 구조를 보이기 위한 예시입니다.

### 설치 확인

```bash
kubectl get pods -n datafabric
```

## 관련 저장소

| 저장소 | 설명 |
|---|---|
| [KnowledgeGraphBE](https://github.com/DataFabricTech/KnowledgeGraphBE) | 분석 작업을 생성·실행·조회하는 백엔드 서버입니다. |
| [KnowledgeGraphFE](https://github.com/DataFabricTech/KnowledgeGraphFE) | 분석 결과를 그래프로 탐색하는 프론트엔드입니다. |
| [KnowledgeGraphSampleData](https://github.com/DataFabricTech/KnowledgeGraphSampleData) | 분석 에이전트와 샘플 데이터 처리 코드입니다. |

## 포함된 오픈소스

`install/argo/argo`는 [argo-helm](https://github.com/argoproj/argo-helm)의 `argo-workflows` 차트를
포함하고 있으며, 해당 차트는 Apache License 2.0을 따릅니다.

## 라이선스

Apache License 2.0을 따릅니다. 자세한 내용은 [LICENSE](LICENSE)를 참고하십시오.
