---
title: 클라우드에서 Commerce 프로비저닝
description: 클라우드 인프라 프로젝트에서 Adobe Commerce을 프로비저닝할 Adobe 고객 기술 관리자를 준비하는 방법에 대해 알아봅니다.
recommendations: noDisplay, catalog
role: Admin
exl-id: cfb354b0-c255-4b6e-94aa-c5a6bf7230d6
source-git-commit: 89c57a486545d6165e0407f913f4fa4cf6c95abe
workflow-type: tm+mt
source-wordcount: '728'
ht-degree: 0%

---

# Commerce on Cloud 프로비저닝 사전 요구 사항

클라우드 인프라에서 Commerce 프로젝트를 시작하고 초기화해 보겠습니다!

Adobe이 클라우드 프로젝트 환경에 Commerce을 프로비저닝하기 전에 다음 전략을 검토하고 Adobe 계정 팀과 첫 번째 협의할 답변을 준비하는 것이 좋습니다. 다음 섹션을 체크리스트로 사용하여 클라우드 프로젝트를 프로비저닝할 고객 기술 관리자와의 대화를 준비하는 데 도움이 됩니다.

## 도메인 정의

**질문 1**: _사이트 실행에 사용할 도메인 또는 도메인이 무엇입니까?_

Pro 스테이징 및 프로덕션 환경에 대한 최상위 수준 도메인 및 하위 도메인을 정의하여 Fastly 및 Ngix 서비스 통합을 준비합니다. 초기 설정 후에는 Adobe Commerce 지원 티켓을 제출해야만 도메인을 추가할 수 있으므로 도메인 정보를 준비하는 것이 좋습니다.

프로덕션 및 스테이징 도메인의 예:

- `www.your-store.com`
- `your-store.com`
- `mcprod.your-store.com`
- `mcstaging.your-store.com`

여러 도메인 또는 고유 도메인에 대한 자세한 지침은 _Cloud Infrastructure의 Commerce_ 안내서의 [여러 웹 사이트 또는 스토어 설정](../cloud-guide/store/multiple-sites.md)을 참조하십시오.

## 트랜잭션 이메일 도메인

**질문 2**: _트랜잭션 전자 메일에 사용할 도메인 또는 도메인이 무엇입니까?_

Adobe Commerce on Cloud는 아웃바운드 이메일 인증 및 신뢰도 모니터링 서비스를 제공하는 SendGrid SMTP(Simple Mail Transfer Protocol) 프록시 서비스를 사용합니다. SendGrid는 귀하를 대신하여 트랜잭션 이메일을 전송하므로 도메인 정보가 필요합니다.

SendGrid 도메인의 예: `example@your-store.com`

트랜잭션 전자 메일 및 도메인 설정에 대한 자세한 지침은 _Cloud Infrastructure의 Commerce_ 안내서의 [SendGrid 메일 서비스](../cloud-guide/project/sendgrid.md)를 참조하십시오.

## 스토리지 할당

**질문 3**: _파일 업로드 및 데이터베이스에 대해 계약된 저장소 중 얼마나 할당하시겠습니까?_

Adobe Commerce on cloud infrastructure는 파일 구조, 검색 색인화 및 데이터베이스를 위해 스토리지를 사용합니다. 각 파티션에 대해 10GB부터 시작하여 필요에 따라 저장소를 세분화할 수 있습니다.

클라우드 프로젝트에 계약한 스토리지의 양은 각 환경의 총 스토리지를 나타냅니다. 예를 들어 50GB의 스토리지 공간을 구입했다면 각 환경에 대해 50GB의 스토리지를 보유하고 있는 것입니다. 운영, 스테이징 및 각 통합 환경에 각각 50GB의 개별 스토리지가 있습니다.

언제든지 계약된 스토리지를 늘릴 수 있습니다. Pro 프로덕션 및 스테이징 환경의 경우 디스크 공간 할당을 변경하려면 Adobe Commerce 지원 티켓을 제출해야 합니다. 프로프로덕션 및 스테이징 환경의 크기 증가는 특정 간격으로만 이루어질 수 있습니다. 현재 디스크 공간 사용량에 따라 지원 팀은 최소 10GB의 디스크 공간 할당을 늘리는 것이 좋습니다. 할당되면 Pro Staging 및 Production에 대한 저장소 증가를 **되돌릴 수 없습니다**.

_Cloud Infrastructure의 Commerce_ 안내서에서 [디스크 공간 관리](../cloud-guide/storage/manage-disk-space.md)를 참조하십시오.

## 클라우드 서비스 지역

**질문 4**: _가까운 곳에서 가장 편리한 클라우드 서비스 지역이 어디입니까?_

Adobe Commerce on cloud infrastructure Pro 프로젝트를 위해 Amazon Web Services(AWS) 또는 Microsoft Azure를 IaaS(Infrastructure as a Service) 기반으로 선택합니다. 각 서비스 제공업체는 여러 지역에서 작동하며 여러 가용 영역을 제공합니다. 위치에 편리한 지역을 선택하고 지연 시간 및 높은 비용을 줄일 수 있습니다.

_구현 플레이북_&#x200B;에서 [Adobe Commerce 클라우드 지역 지도](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/infrastructure/cloud/regions.html)를 참조하십시오.

## 연결 서비스

**질문 5**: _PrivateLink 서비스가 필요하십니까? 그렇다면 PrivateLink 연결은 어느 지역에 있습니까?_

Adobe Commerce on cloud infrastructure는 AWS PrivateLink 또는 Azure Private Link 서비스와의 통합을 지원합니다. 이 서비스는 선택 사항이지만 PrivateLink를 사용하여 외부 시스템에 호스팅된 서비스 및 애플리케이션과 클라우드 인프라 환경 간의 안전한 개인 통신을 설정합니다.

동일한 지역 내에서 Adobe Commerce 인스턴스에 액세스할 수 있도록 클라우드 서비스 전략(AWS 또는 Azure)을 고려하는 것이 중요합니다. 지역 액세스 가능성에 대한 자세한 내용은 _Commerce on Cloud Infrastructure_ 안내서의 [PrivateLink 서비스](../cloud-guide/development/privatelink-service.md)를 참조하십시오.

## 타겟 사이트 시작

**질문 6**: _예상 목표 시작 날짜가 어떻게 됩니까?_

사이트를 실행하려면 반복적인 구성과 테스트가 필요하며 이를 통해 사이트 실행이 성공적으로 수행됩니다. 대상 날짜를 설정하면 사용자와 Adobe 계정 팀이 최종 단계를 조정하기 위한 호출을 포함하여 최종 실행 전 활동을 준비하는 데 도움이 됩니다.

전체 프로세스를 검토하고 Launch 검사 목록 복사본을 다운로드하려면 _Commerce on Cloud Infrastructure_ 안내서의 [Launch 사이트 개요](../cloud-guide/launch/overview.md)를 참조하십시오.

>[!TIP]
>
> Cloud Portal을 간단히 살펴보고 새 클라우드 프로젝트에 액세스합니다.
>
>**다음 단계**: [Commerce에 온보딩](onboarding.md)
