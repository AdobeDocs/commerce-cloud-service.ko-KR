---
title: WAF(Web Application Firewall)
description: Fastly WAF 서비스가 Adobe Commerce 네트워크 또는 사이트를 손상시키기 전에 어떻게 악의적인 요청 트래픽을 탐지하고 로그하고 차단하는지 알아봅니다.
feature: Cloud, Configuration, Security
exl-id: 40bfe983-7f32-4155-ae77-7cd18866f6e2
source-git-commit: 6b01bf91c3bf63ba268d0f8e10e19db4cb355997
workflow-type: tm+mt
source-wordcount: '824'
ht-degree: 0%

---

# WAF(Web Application Firewall)

Fastly를 기반으로 하는 클라우드 인프라의 Adobe Commerce용 WAF(Web Application Firewall) 서비스는 사이트 또는 네트워크를 손상시키기 전에 악의적인 요청 트래픽을 감지하고, 기록하고, 차단합니다. WAF 서비스는 프로덕션 환경에서만 사용할 수 있습니다.

WAF 서비스는 다음과 같은 이점을 제공합니다.

- **PCI 규정 준수**—WAF 지원을 통해 프로덕션 환경의 Adobe Commerce 상점이 PCI DSS 6.6 보안 요구 사항을 충족하도록 합니다.
- **기본 WAF 정책** - Fastly에서 구성 및 유지 관리하는 기본 WAF 정책은 삽입 공격, 악의적인 입력, 교차 사이트 스크립팅, 데이터 내보내기, HTTP 프로토콜 위반 및 기타 [OWASP Top Ten](https://owasp.org/www-project-top-ten/) 보안 위협을 포함한 광범위한 공격으로부터 Adobe Commerce 웹 응용 프로그램을 보호하기 위해 맞춤화된 보안 규칙 모음을 제공합니다.
- **WAF 온보딩 및 활성화**—Adobe은 프로비저닝이 완료된 후 2~3주 내에 프로덕션 환경에 기본 WAF 정책을 배포하고 활성화합니다.
- **작업 및 유지 관리 지원**—
   - WAF 서비스에 대한 로그 및 경고를 Adobe 및 Fastly로 설정하고 관리합니다.
   - Adobe은 우선 순위 1 문제로서 합법적인 트래픽을 차단하는 WAF 서비스 문제와 관련된 고객 지원 티켓을 평가합니다.
   - WAF 서비스 버전으로의 자동 업그레이드는 새롭거나 진화하는 악용에 대한 즉각적인 보장을 보장합니다. [WAF 유지 관리 및 업그레이드](#waf-maintenance-and-updates)를 참조하세요.

>[!TIP]
>
>클라우드 인프라 스토어의 Adobe Commerce에 대한 PCI 규정 준수 유지 관리에 대한 자세한 내용은 [PCI 규정 준수](https://business.adobe.com/products/magento/pci-compliance.html)를 참조하십시오.

## WAF 활성화

Adobe은 프로비저닝이 완료된 후 2~3주 내에 새 계정에서 WAF 서비스를 활성화합니다. WAF는 Fastly CDN 서비스를 통해 구현됩니다. 하드웨어 또는 소프트웨어를 설치하거나 유지 관리할 필요가 없습니다.

>[!NOTE]
>
>WAF 서비스를 사용하려면 먼저 클라우드 인프라 프로젝트에서 Adobe Commerce에 대한 모든 외부 트래픽을 Fastly 서비스를 통해 라우팅해야 합니다. [빠르게 설정](fastly-configuration.md)을 참조하세요.

## 작동 방식

WAF 서비스는 Fastly와 통합되고 Fastly CDN 서비스 내의 캐시 로직을 사용하여 Fastly 전역 노드에서 트래픽을 필터링합니다. Trustwave SpiderLabs의 [ModSecurity 규칙](https://github.com/owasp-modsecurity/ModSecurity) 및 OWASP 10대 보안 위협을 기반으로 하는 기본 WAF 정책으로 프로덕션 환경에서 WAF 서비스를 사용하도록 설정합니다.

WAF 서비스는 WAF 규칙 세트에 대해 HTTP 및 HTTPS 트래픽(GET 및 POST 요청)을 필터링하고 악의적이거나 특정 규칙을 준수하지 않는 트래픽을 차단합니다. 이 서비스는 캐시를 새로 고침하는 원본 바인딩 트래픽만 필터링합니다. 그 결과 Fastly 캐시에서 대부분의 공격 트래픽을 중지하여 원본 트래픽을 악의적인 공격으로부터 보호합니다. 원본 트래픽만 처리하므로 WAF 서비스는 캐시 성능을 유지하여 캐싱되지 않는 모든 요청에 대해 약 1.5밀리초~20밀리초의 지연 시간만 제공합니다.

## 차단된 요청 문제 해결

WAF 서비스가 활성화되면 WAF 규칙에 대해 모든 웹 및 관리자 트래픽을 필터링하고 규칙을 트리거하는 모든 웹 요청을 차단합니다. 요청이 차단되면 요청자에게 차단 이벤트에 대한 참조 ID가 포함된 기본 `403 Forbidden` 오류 페이지가 표시됩니다.

![WAF 오류 페이지](../../assets/cdn/fastly-waf-403-error.png)

책임자로부터 이 오류 응답 페이지를 사용자 지정할 수 있습니다. [WAF 응답 페이지 사용자 지정](fastly-custom-response.md#customize-the-waf-error-page)을 참조하세요.

Adobe Commerce 관리 페이지 또는 상점 첫 화면에서 합법적인 URL 요청에 대한 응답으로 `403 Forbidden` 오류 페이지를 반환하는 경우 [Adobe Commerce 지원 티켓](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)을 제출하십시오. 오류 응답 페이지에서 참조 ID를 복사하여 티켓 설명에 붙여넣습니다.

## WAF 유지 관리 및 업데이트

Fastly는 상업용 제3자, Fastly 연구 및 오픈 소스의 규칙 업데이트를 기반으로 WAF 규칙 세트를 유지 관리하고 업데이트합니다. 필요한 경우 또는 해당 소스에서 규칙 변경 내용을 사용할 수 있는 경우 게시된 규칙을 정책으로 빠르게 업데이트합니다. 또한 Fastly는 WAF 서비스가 활성화된 후 게시된 규칙 클래스와 일치하는 규칙을 모든 서비스의 WAF 인스턴스에 추가할 수 있습니다. 이러한 업데이트는 새롭거나 진화하는 활동에 대한 즉각적인 보장을 보장합니다.

Adobe 및 Fastly는 업데이트 프로세스를 관리하여 업데이트가 차단 모드로 배포되기 전에 프로덕션 환경에서 새 규칙 또는 수정된 WAF 규칙이 효과적으로 작동하도록 합니다.

## 제한 사항

Fastly에서 제공하는 표준 WAF 서비스는 다음 기능을 지원하지 않습니다.

- 맬웨어 또는 봇 완화 방지 - [액세스 제어 목록](./fastly-vcl-allowlist.md) 또는 서드파티 서비스를 사용하는 것이 좋습니다.
- 속도 제한—Fastly 설명서에서 [속도 제한](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/RATE-LIMITING.md)을 참조하거나 _Commerce Web API_ 보안 섹션에서 [속도 제한](https://developer.adobe.com/commerce/webapi/get-started/rate-limiting/)을 참조하십시오.
- 고객에 대한 로깅 끝점 구성—대체 방법으로 [PrivateLink 서비스](../development/privatelink-service.md)를 참조하십시오.

WAF 서비스에서는 IP 주소를 기반으로 트래픽을 차단하거나 허용할 수 없지만 Fastly 서비스에 ACL(액세스 제어 목록) 및 사용자 지정 VCL 코드 조각을 추가하여 트래픽을 차단하거나 허용하는 IP 주소와 VCL 논리를 지정할 수 있습니다. [사용자 지정 Fastly VCL 코드 조각](fastly-vcl-custom-snippets.md)을 참조하세요.

WAF 서비스에서 TCP, UDP 또는 ICMP 요청에 대한 필터링을 지원하지 않습니다. 그러나 이 기능은 Fastly CDN 서비스에 포함된 내장 DDoS 보호에서 제공됩니다. [DDoS 보호](fastly.md#ddos-protection)를 참조하세요.
