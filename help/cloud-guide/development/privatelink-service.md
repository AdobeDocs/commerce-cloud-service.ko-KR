---
title: PrivateLink 서비스
description: PrivateLink 서비스를 사용하여 동일한 지역에서 사설 클라우드와 Adobe Commerce 클라우드 플랫폼 간의 보안 연결을 설정하는 방법에 대해 알아봅니다.
feature: Cloud, Iaas, Security
exl-id: b25548b8-312b-4a74-b242-f4e2ac6cf945
source-git-commit: 5b52157c6c623bb4c765a2d545919df79d81f1da
workflow-type: tm+mt
source-wordcount: '1609'
ht-degree: 0%

---

# PrivateLink 서비스

Adobe Commerce on cloud infrastructure는 [AWS PrivateLink](https://aws.amazon.com/privatelink/) 또는 [Azure 개인 링크](https://learn.microsoft.com/en-us/azure/private-link/) 서비스. PrivateLink를 사용하여 외부 시스템에 호스팅된 서비스 및 애플리케이션을 사용하는 클라우드 인프라 환경의 Adobe Commerce 간에 안전한 개인 통신을 설정할 수 있습니다. Adobe Commerce 애플리케이션과 외부 시스템은 모두 동일한 클라우드 영역 내의 동일한 클라우드 플랫폼(AWS 또는 Azure)에 구성된 Virtual Private Cloud(VPC) 엔드포인트를 통해 액세스할 수 있어야 합니다.

>[!TIP]
>
>PrivateLink는 데이터베이스 또는 파일 전송과 같은 비 HTTP(S) 통합에 대한 연결을 보호하는 데 가장 적합합니다. 애플리케이션을 Adobe Commerce API와 통합하려는 경우 다음을 만드는 방법을 참조하십시오. [Adobe API 메쉬](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/) 위치: _Adobe Developer 앱 빌더용 API Mesh_.

## 기능 및 지원

클라우드 인프라 프로젝트에서 Adobe Commerce을 위한 PrivateLink 서비스 통합에는 다음 기능 및 지원이 포함됩니다.

- 고객 Virtual Private Cloud(VPC)와 동일한 클라우드 영역 내의 동일한 클라우드 플랫폼(AWS 또는 Azure)에 있는 Adobe VPC 간의 보안 연결입니다.
- Adobe 및 고객 VPC에서 사용할 수 있는 엔드포인트 서비스 간의 단방향 또는 양방향 통신을 지원합니다.
- 서비스 지원:

   - Adobe Commerce on cloud infrastructure 환경에서 필요한 포트 열기
   - 고객과 Adobe VPC 간의 초기 연결 설정
   - 활성화 중 연결 문제 해결

## 제한 사항

- PrivateLink는 Pro 프로덕션 및 스테이징 환경에서만 지원됩니다. 로컬 또는 통합 환경이나 스타터 프로젝트에서는 사용할 수 없습니다.
- PrivateLink를 사용하여 SSH 연결을 설정할 수 없습니다. 다음을 참조하십시오 [SSH 키 사용](secure-connections.md).
- Adobe Commerce 지원에서는 초기 지원 외에 AWS PrivateLink 문제 해결을 다루지 않습니다.
- 고객은 자신의 VPC 관리와 관련된 비용을 부담해야 합니다.
- 다음 이유로 인해 HTTPS 프로토콜(포트 443)을 사용하여 Azure 개인 링크를 통해 클라우드 인프라의 Adobe Commerce에 연결할 수 없습니다. [기원 차단](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/faq/fastly-origin-cloaking-enablement-faq.html). 이 제한은 AWS PrivateLink에는 적용되지 않습니다.
- PrivateDNS를 사용할 수 없습니다.

## PrivateLink 연결 유형

다음 네트워크 다이어그램에 표시된 두 가지 PrivateLink 연결 유형을 사용하여 클라우드 환경 외부에서 호스팅되는 외부 시스템과 스토어 간 보안 통신을 설정할 수 있습니다.

![PrivateLink 네트워크 다이어그램](../../assets/privatelink-architecture-diagram.png)

클라우드 인프라 환경에서 Adobe Commerce에 가장 적합한 PrivateLink 연결 유형 중 하나를 선택합니다.

- **단방향 개인 링크**-Adobe Commerce on cloud infrastructure store에서 데이터를 안전하게 검색하려면 이 구성을 선택하십시오.
- **양방향 개인 링크**-이 구성을 선택하여 Adobe Commerce on cloud infrastructure 환경 외부의 시스템에 대한 보안 연결 및 보안 연결을 설정합니다. 양방향 옵션을 사용하려면 다음 두 개의 연결이 필요합니다.

   - 고객 VPC와 Adobe VPC 간의 연결
   - Adobe VPC와 고객 VPC 간의 연결

>[!TIP]
>
>네트워크 관리자 또는 클라우드 플랫폼 공급자와 협력하여 PrivateLink 연결 유형을 선택하거나 VPC 설정 및 관리에 대한 도움을 받으십시오. Cloud platform PrivateLink 설명서 를 참조하십시오. [AWS PrivateLink](https://aws.amazon.com/privatelink/) 또는 [Azure 개인 링크](https://learn.microsoft.com/en-us/azure/private-link/).

## PrivateLink 지원 요청

>[!WARNING]
>
>PrivateLink 활성화 소요 시간: _5_ 영업일. 불완전하거나 부정확한 정보를 제공하면 처리가 지연될 수 있다.

### 전제 조건

![check](../../assets/fix.svg) 클라우드 인프라 인스턴스의 Adobe Commerce과 동일한 지역의 클라우드 계정(AWS 또는 Azure).

![check](../../assets/fix.svg) PrivateLink를 통해 연결할 서비스를 호스팅하는 고객 환경의 VPC입니다. VPC 설정에 대한 도움말은 AWS 또는 Azure 설명서를 참조하거나 네트워크 관리자에게 문의하십시오.

![check](../../assets/fix.svg) 양방향 PrivateLink 연결의 경우 PrivateLink 활성화를 요청하기 전에 애플리케이션이나 서비스에 대한 엔드포인트 서비스 구성을 만들고 VPC 환경에 엔드포인트를 만들어야 합니다. 다음을 참조하십시오 [양방향 PrivateLink 연결 설정](#set-up-for-bidirectional-privatelink-connections).

PrivateLink 활성화에 필요한 다음 데이터 수집:

- **Customer Cloud 계정 번호** (AWS 또는 Azure) - Adobe Commerce on cloud infrastructure 인스턴스와 동일한 지역에 있어야 합니다.
- **클라우드 영역**—확인을 위해 계정이 호스팅되는 클라우드 영역을 제공합니다.
- **서비스 및 통신 포트**—Adobe이 VPC 간 서비스 통신을 위해 포트를 열어야 합니다(예: SQL 포트 3306, SFTP 포트 2222).
- **프로젝트 ID**—Adobe Commerce on cloud infrastructure Pro 프로젝트 ID를 제공합니다. 다음을 사용하여 프로젝트 ID 및 기타 프로젝트 정보를 가져올 수 있습니다 [Cloud CLI](../dev-tools/cloud-cli-overview.md) 명령: `magento-cloud project:info`
- **연결 유형**- 연결 유형에 단방향 또는 양방향을 지정합니다.
- **엔드포인트 서비스**—양방향 PrivateLink 연결의 경우, Adobe이 연결해야 하는 VPC 엔드포인트 서비스에 대한 DNS URL을 제공합니다. 예를 들면 다음과 같습니다. `com.amazonaws.vpce.<cloud-region>.vpce-svc-<service-id>`
- **끝점 서비스 액세스 권한 부여됨**—외부 서비스에 연결하려면 끝점 서비스에서 다음 AWS 계정 사용자에 액세스할 수 있도록 허용합니다. `arn:aws:iam::402592597372:root`

  >[!WARNING]
  >
  >엔드포인트 서비스에 대한 액세스가 제공되지 않으면 VPC의 서비스에 대한 양방향 PrivateLink 연결이 **아님** 가 추가되어 설정이 지연됩니다.

#### Azure 개인 링크 지원에 대한 추가 사전 요구 사항

- 클러스터 ID를 제공합니다. SSH를 사용하여 원격으로 로그인하고 다음 명령을 사용합니다. `cat /etc/platform_cluster`
- 외부 서비스가 Adobe Commerce Pro 클러스터에 연결하려면 다음이 필요합니다.

   - 새 외부 개인 끝점에 표시할 Pro 클러스터의 포트 목록
   - 비공개 끝점 연결에 대한 Azure 구독 ID 목록

- Adobe Commerce Pro 클러스터를 외부 서비스에 연결하려면 다음이 필요합니다.

   - 대상 서비스의 리소스 ID 목록입니다. 외부 개인 링크 서비스 ID는 다음과 비슷합니다.

  ```text
  /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/privateLinkServices/{svcNameID}
  ```

### 지원 워크플로

다음 워크플로에서는 클라우드 인프라에서 Adobe Commerce과의 PrivateLink 통합을 위한 활성화 프로세스에 대해 간략히 설명합니다.

1. **고객** 제목란으로 PrivateLink 활성화를 요청하는 지원 티켓을 제출합니다. `PrivateLink support for <company>`. 포함 [활성화를 위해 필요한 데이터](#prerequisites) 티켓에 있습니다. Adobe은 지원 티켓을 사용하여 활성화 프로세스 동안 통신을 조정합니다.

1. **Adobe** Adobe VPC에서 엔드포인트 서비스에 대한 고객 계정 액세스를 활성화합니다.

   - 고객 AWS 또는 Azure 계정에서 시작된 요청을 수락하도록 Adobe 끝점 서비스 구성을 업데이트합니다.
   - 예를 들어 연결할 Adobe VPC 끝점의 서비스 이름을 제공하도록 지원 티켓을 업데이트합니다. `com.amazonaws.vpce.<cloud-region>.vpce-svc-<service-id>`.

1. **고객** Adobe 끝점 서비스를 해당 Cloud 계정(AWS 또는 Azure)에 추가하여 Adobe에 대한 연결 요청을 트리거합니다. 지침이 필요하면 Cloud Platform 설명서 를 참조하십시오.

   - AWS의 경우 [인터페이스 끝점 연결 요청 수락 및 거부].
   - Azure의 경우 [연결 요청 관리].

1. **Adobe** 는 연결 요청을 승인합니다.

1. 연결 요청 승인 후, **고객** [연결 확인](#test-vpc-endpoint-service-connection) VPC와 Adobe VPC 간.

1. 양방향 연결을 활성화하는 추가 단계:

   - **Adobe** 은 Adobe 계정 주체(AWS 또는 Azure 계정의 루트 사용자)를 제공하고 고객 VPC 엔드포인트 서비스에 대한 액세스를 요청합니다.
   - **고객** 고객 VPC의 엔드포인트 서비스에 대한 Adobe 액세스를 활성화합니다. 이는 Adobe 계정 주체가 다음에 대한 액세스 권한을 가지고 있다고 가정합니다. `arn:aws:iam::402592597372:root`에 이전에 설명된 대로 **끝점 서비스 액세스 권한 부여됨** 전제 조건.

      - Adobe 계정에서 시작된 요청을 수락하도록 고객 엔드포인트 서비스 구성을 업데이트합니다. 지침이 필요하면 Cloud Platform 설명서 를 참조하십시오.

         - AWS의 경우 [Endpoint Service에 대한 권한 추가 및 제거].
         - Azure의 경우 [개인 끝점 연결 관리]

      - 고객 VPC에 대한 엔드포인트 서비스 이름을 Adobe에게 제공합니다.

   - **Adobe** customer endpoint service를 Adobe 플랫폼 계정(AWS 또는 Azure)에 추가하여 고객 VPC에 대한 연결 요청을 트리거합니다.
   - **고객** 설치를 완료하기 위해 Adobe의 연결 요청을 승인합니다.
   - **고객** [연결 확인](#test-vpc-endpoint-service-connection) Adobe VPC에서.

## VPC 엔드포인트 서비스 연결 테스트

Telnet 애플리케이션을 사용하여 VPC 엔드포인트 서비스에 대한 연결을 테스트할 수 있습니다.

**VPC 엔드포인트 서비스에 대한 연결을 테스트하려면**:

1. 프로젝트 루트 디렉토리에서 **체크아웃** PrivateLink 끝점 서비스에 액세스하도록 구성된 스테이징 또는 프로덕션 환경입니다.

   ```bash
   magento-cloud environment:checkout <environment-id>
   ```

1. 다음 CURL 명령을 실행합니다.

   ```bash
   curl -v telnet://<endpoint-service-dns-url>:<port>/
   ```

   예:

   ```terminal
   $ curl -v telnet://vpce-007ffnb9qkcnjgult-yfhmywqh.vpce-svc-083cqvm2ta3rxqat5v.us-east-1.vpce.amazonaws.com:80 -vvv
   ```

   샘플 성공 응답:

   ```terminal
   * Rebuilt URL to: telnet://vpce-007ffnb9qkcnjgult-yfhmywqh.vpce-svc-083cqvm2ta3rxqat5v.us-east-1.vpce.amazonaws.com:80
   * Connected to vpce-0088d56482571241d-yfhmywqh.vpce-svc-083cqvm2ta3rxqat5v.us-east-1.vpce. amazonaws.com (191.210.82.246) port 80 (#0)
   ```

   샘플 실패 응답:

   ```terminal
   Failed to connect to vpce-007ffnb9qkcnjgult-yfhmywqh.vpce-svc-083cqvm2ta3rxqat5v.ap-southeast-1.vpce.amazonaws.com port 80: Connection timed out
   * Closing connection 0
   ```

1. 서비스가 VM에서 수신 대기하는지 확인합니다.

   ```bash
   netstat -na | grep <port>
   ```

1. 패키지 흐름을 확인하십시오.

   ```bash
   tcpdump -i <ethernet-interface> -tt -nn port <destination-port> and host <source-host>
   ```

   다음 내부 설정을 확인하여 구성이 유효한지 확인합니다.

   - 끝점 및 끝점 서비스 설정
   - 네트워크 부하 분산 장치(NLB) 설정
   - NLB의 대상 그룹을 확인하고 정상인지 확인합니다
   - 각 VM의 netcat/curl 끝점 URL(위에 나열됨)

   연결 문제 해결에 대한 도움말은 다음 문서를 참조하십시오.

   - [AWS: 엔드포인트 서비스 연결 문제 해결]
   - [Amazon: Azure Private Link 연결 문제 해결]

   오류를 해결할 수 없는 경우 Adobe Commerce 지원 티켓을 업데이트하여 연결 설정에 대한 도움을 요청합니다.

## PrivateLink 구성 변경

[Adobe Commerce 지원 티켓 제출](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) 기존 PrivateLink 구성을 변경합니다. 예를 들어 다음과 같은 변경 사항을 요청할 수 있습니다.

- Adobe Commerce on cloud infrastructure Pro 프로덕션 또는 스테이징 환경에서 PrivateLink 연결을 제거합니다.
- Adobe 끝점 서비스에 액세스하기 위한 고객 클라우드 플랫폼 계정 번호를 변경합니다.
- Adobe VPC에서 고객 VPC 환경에서 사용할 수 있는 다른 엔드포인트 서비스로의 PrivateLink 연결을 추가하거나 제거합니다.

## 양방향 PrivateLink 연결 설정

고객 VPC는 양방향 PrivateLink 연결을 지원할 수 있는 다음 리소스를 보유하고 있어야 합니다.

- 네트워크 로드 밸런서(NLB)
- 고객 VPC에서 애플리케이션이나 서비스에 액세스할 수 있도록 하는 엔드포인트 서비스 구성
- An [인터페이스 엔드포인트] (AWS) 또는 [비공개 엔드포인트] (Azure) Adobe이 VPC에서 호스팅되는 엔드포인트 서비스에 연결할 수 있도록 해줍니다

이러한 리소스를 고객 VPC에서 사용할 수 없는 경우 Cloud Platform 계정에 로그인하여 구성을 추가해야 합니다.

- Amazon VPC 콘솔- `https://console.aws.amazon.com/vpc/`
- Azure 포털- `https://portal.azure.com`

PrivateLink 설정 지침은 클라우드 플랫폼 설명서를 참조하십시오.

- **AWS PrivateLink 설명서**
   - [네트워크 로드 밸런서 만들기]
   - [끝점 서비스 구성 만들기]
   - [인터페이스 엔드포인트 만들기]
   - [인터페이스 엔드포인트 라이프사이클]

- **Azure PrivateLink 설명서**
   - [로드 밸런서 만들기]
   - [Azure 개인 링크 워크플로]

<!--Link definitions-->

[인터페이스 끝점 연결 요청 수락 및 거부]: https://docs.aws.amazon.com/vpc/latest/userguide/accept-reject-endpoint-requests.html
[Endpoint Service에 대한 권한 추가 및 제거]: https://docs.aws.amazon.com/vpc/latest/userguide/add-endpoint-service-permissions.html
[Amazon: Azure Private Link 연결 문제 해결]: https://docs.microsoft.com/en-us/azure/private-link/troubleshoot-private-link-connectivity
[AWS: 엔드포인트 서비스 연결 문제 해결]: https://aws.amazon.com/premiumsupport/knowledge-center/connect-endpoint-service-vpc/
[Azure 개인 링크 워크플로]: https://docs.microsoft.com/en-us/azure/private-link/private-link-service-overview#workflow
[로드 밸런서 만들기]: https://docs.microsoft.com/en-us/azure/load-balancer/quickstart-load-balancer-standard-public-portal
[네트워크 로드 밸런서 만들기]: https://docs.aws.amazon.com/elasticloadbalancing/latest/network/create-network-load-balancer.html
[끝점 서비스 구성 만들기]: https://docs.aws.amazon.com/vpc/latest/userguide/create-endpoint-service.html
[인터페이스 엔드포인트 만들기]: https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#create-interface-endpoint
[interface endpoint lifecycle]: https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#vpce-interface-lifecycle
[인터페이스 엔드포인트]: https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html
[개인 끝점 연결 관리]: https://docs.microsoft.com/en-us/azure/private-link/manage-private-endpoint
[연결 요청 관리]: https://docs.microsoft.com/en-us/azure/private-link/private-link-service-overview#manage-your-connection-requests
[비공개 엔드포인트]: https://docs.microsoft.com/en-us/azure/private-link/private-endpoint-overview
