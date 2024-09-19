---
title: Fastly 서비스 개요
description: 클라우드 인프라의 Adobe Commerce에 포함된 Fastly 서비스를 통해 Adobe Commerce 사이트의 컨텐츠 전달 작업을 최적화하고 보호하는 방법에 대해 알아봅니다.
feature: Cloud, Configuration, Iaas, Paas, Cache, Security, Services
exl-id: dc4500bf-f037-47f0-b7ec-5cd1291f73a1
source-git-commit: dc331df378074af8a8776a33784b73082a39cf10
workflow-type: tm+mt
source-wordcount: '1426'
ht-degree: 0%

---

# Fastly 서비스 개요

>[!WARNING]
>
>클라우드 플랫폼에 배포된 Adobe Commerce 사이트에 대한 PCI 규정 준수를 유지 관리하려면 스타터 주 분기, Pro 프로덕션 및 Pro 스테이징 환경에서 Fastly를 설정하십시오. Headless 배포에서 Adobe Commerce을 사용하는 경우 Fastly를 사용하여 GraphQL 응답을 캐시하는 것이 좋습니다. *GraphQL 개발자 안내서*&#x200B;에서 [Fastly로 캐싱](https://developer.adobe.com/commerce/webapi/graphql/usage/caching/#caching-with-fastly)을 참조하십시오.

Fastly는 클라우드 인프라 프로젝트에서 Adobe Commerce에 대한 콘텐츠 전달 작업을 최적화하고 보호하기 위해 다음 서비스를 제공합니다. 이러한 서비스는 추가 비용 없이 클라우드 인프라의 Adobe Commerce에 포함됩니다.

- **CDN(Content Delivery Network)** - 사용자가 설정한 백엔드 데이터 센터에서 사이트 페이지, 자산, CSS 등을 캐시하는 바니시 기반 서비스. 고객이 사이트에 액세스하고 저장할 때 요청이 Fastly에 도달하여 캐시된 페이지를 더 빨리 로드합니다. CDN 서비스는 다음과 같은 기능을 제공합니다.

- **캐시 관리** - 대역폭 부하 및 비용을 줄이기 위해 설정한 백엔드 데이터 센터에 사이트 페이지, 자산, CSS 등을 캐시합니다.

   - [Fastly 사용자 지정 VCL 코드 조각](fastly-vcl-custom-snippets.md)(Varnish 2.1 호환)을 사용하여 캐싱이 요청에 응답하는 방식을 수정합니다

   - [GeoIP 서비스 지원 설정](fastly-custom-cache-configuration.md#configure-geoip-handling)

   - [암호화되지 않은 요청을 TLS로 강제 전송](fastly-custom-cache-configuration.md#force-tls)

   - 대량 작업 요청에 대해 503 응답을 방지하도록 [Fastly 시간 초과 사용자 지정](fastly-custom-cache-configuration.md#extend-fastly-timeout) 설정

   - [사용자 지정 오류 응답 페이지 만들기](fastly-custom-response.md)

- **보안**—Adobe Commerce 사이트에 대해 Fastly 서비스를 사용하도록 설정하면 사이트 및 네트워크를 보호하는 추가 보안 기능을 사용할 수 있습니다.

   - [웹 응용 프로그램 방화벽](fastly-waf-service.md)(WAF) - 클라우드 인프라 사이트 및 네트워크에서 프로덕션 Adobe Commerce을 손상시킬 수 있기 전에 악성 트래픽을 차단하는 PCI 호환 보호 기능을 제공하는 관리되는 웹 응용 프로그램 방화벽 서비스입니다. WAF 서비스는 Pro 및 Starter 프로덕션 환경에서만 사용할 수 있습니다.

   - [DDoS(Distributed Denial of Service) 보호](#ddos-protection)—Ping of Death, Smurf 공격 및 기타 ICMP 기반 플러드 공격과 같은 일반적인 공격에 대한 기본 제공 DDoS 보호 기능.

   - [SSL/TLS 인증서](fastly-configuration.md#provision-ssltls-certificates)—Fastly 서비스를 사용하려면 HTTPS에서 보안 트래픽을 제공하는 SSL/TLS 인증서가 필요합니다.

     Adobe Commerce은 각 스테이징 및 프로덕션 환경에 대해 도메인에 의해 검증된 Let&#39;s Encrypt SSL/TLS 인증서를 제공합니다. Adobe Commerce은 Fastly 설정 프로세스 중에 도메인 유효성 검사 및 인증서 프로비저닝을 완료합니다.

- **원본 차단**—트래픽이 Fastly WAF을 우회하지 못하도록 하고 원본 서버의 IP 주소를 숨겨 직접 액세스 및 DDoS 공격으로부터 보호합니다.

  원본 클로킹은 클라우드 인프라 Pro 프로덕션 프로젝트의 Adobe Commerce에서 기본적으로 활성화됩니다. 클라우드 인프라 스타터 프로덕션 프로젝트에서 Adobe Commerce의 원본 클로킹을 사용하려면 [Adobe Commerce 지원 티켓](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)을 제출하십시오. 캐싱이 필요하지 않은 트래픽이 있는 경우 Fastly 서비스 구성을 사용자 지정하여 [Fastly 캐시 우회](fastly-vcl-bypass-to-origin.md)를 요청할 수 있습니다.

- **[이미지 최적화](fastly-image-optimization.md)** - 서버가 주문 및 전환을 보다 효율적으로 처리할 수 있도록 이미지 처리 및 크기 조정 로드를 Fastly 서비스로 오프로드합니다.

- **[Fastly CDN 및 WAF 로그](../monitor/new-relic-service.md#new-relic-log-management)** - Cloud Infrastructure Pro 프로젝트의 Adobe Commerce에 대해 New Relic 로그 서비스를 사용하여 Fastly CDN 및 WAF 로그 데이터를 검토하고 분석할 수 있습니다.

## Magento 2용 Fastly CDN 모듈

클라우드 인프라의 Adobe Commerce용 Fastly 서비스는 Pro Staging 및 Production, Starter Production(`master` 분기)에 설치된 Magento 2]용 [Fastly CDN 모듈을 사용합니다.

Adobe Commerce 프로젝트의 초기 프로비저닝 또는 업그레이드 시 Adobe은 스테이징 및 프로덕션 환경에 Fastly CDN 모듈의 최신 버전을 설치합니다. Fastly에서 모듈 업데이트를 릴리스하면 환경에 대한 알림을 관리자의 알림을 받게 됩니다. Adobe은 최신 릴리스를 사용하도록 환경을 업데이트할 것을 권장합니다. [빠르게 업그레이드](fastly-configuration.md#upgrade-the-fastly-module)를 참조하세요.

## 빠른 서비스 계정 및 자격 증명

클라우드 인프라 프로젝트에 대한 Adobe Commerce에는 전용 Fastly 계정이 제공되지 않습니다. Fastly 서비스는 Adobe에 등록된 중앙 집중식 계정에서 관리되며 관리 대시보드는 클라우드 지원 팀에서만 액세스할 수 있습니다.

대신, 각 스테이징 및 프로덕션 환경에는 Commerce 관리자의 Fastly 서비스를 구성하고 관리할 수 있는 고유한 Fastly 자격 증명(API 토큰 및 서비스 ID)이 있습니다. Fastly API는 Fastly 서비스의 고급 관리를 수행하는 데 사용할 수 있으며, 이러한 요청을 제출하려면 자격 증명이 필요합니다.

프로젝트 프로비저닝 중에 Adobe은 클라우드 인프라의 Adobe Commerce에 대한 Fastly 서비스 계정에 프로젝트를 추가하고 스테이징 및 프로덕션 환경에 대한 구성에 Fastly 자격 증명을 추가합니다. [Fastly 자격 증명 가져오기](fastly-configuration.md#get-fastly-credentials)를 참조하십시오.

### Fastly API 토큰 변경

Fastly API 토큰 자격 증명을 변경하려면 Adobe Commerce 지원 티켓을 제출하십시오. 새 토큰을 받으면 스테이징 또는 프로덕션 환경을 업데이트하여 새 토큰을 사용합니다.

**Fastly API 토큰 자격 증명을 변경하려면**:

1. 새 Fastly API 자격 증명을 요청하는 [Adobe Commerce 지원 티켓을 제출](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)합니다.

   새 자격 증명이 필요한 환경 및 클라우드 인프라 프로젝트 ID에 Adobe Commerce을 포함하십시오.

1. 새 API 토큰을 받은 후 관리자의 [Fastly 자격 증명 구성](fastly-configuration.md#test-the-fastly-credentials) 또는 [[!DNL Cloud Console] 환경 변수](../project/overview.md#configure-environment)에서 API 토큰 값을 업데이트하십시오.

1. [새 자격 증명을 테스트합니다](fastly-configuration.md#test-the-fastly-credentials).

1. 자격 증명을 업데이트한 후 Adobe Commerce 지원 티켓을 제출하여 이전 API 토큰을 삭제합니다.

### 여러 Fastly 계정 및 할당된 도메인

Fastly를 사용하면 하나의 Fastly 서비스 및 계정에 Apex 도메인 및 관련 하위 도메인만 할당할 수 있습니다. Adobe Commerce 사이트에 사용되는 동일한 apex 및 하위 도메인을 연결하는 기존 Fastly 계정이 있는 경우 다음 옵션이 제공됩니다.

- 클라우드 인프라 프로젝트 환경에서 Adobe Commerce에 대한 Fastly 서비스 자격 증명을 요청하기 전에 기존 계정에서 apex 및 하위 도메인을 제거합니다. Fastly 설명서에서 [도메인 작업]을 참조하십시오.

  이 옵션을 사용하여 Apex 도메인과 모든 하위 도메인을 클라우드 인프라의 Adobe Commerce에 대한 Fastly 서비스 계정에 연결합니다.

- Apex 및 하위 도메인이 다른 계정에 연결될 수 있도록 도메인 위임을 요청하려면 Adobe Commerce 지원 티켓을 제출하십시오.

  Adobe Commerce 및 Adobe Commerce이 아닌 사이트에 대한 여러 하위 도메인이 있는 Apex 도메인이 있고 이러한 하위 도메인을 다른 Fastly 계정에 연결하려는 경우 이 옵션을 사용합니다.

#### 도메인 위임 요청

*시나리오 1:*

Apex 도메인(`testweb.com` 및 `www.testweb.com`)이 기존 Fastly 계정에 연결되어 있습니다. `mcstaging.testweb.com` 및 `mcprod.testweb.com` 하위 도메인으로 구성된 클라우드 인프라의 Adobe Commerce 프로젝트가 있습니다. Apex 도메인을 클라우드 인프라의 Adobe Commerce에 대한 Fastly 서비스 계정으로 이동하지 않으려는 경우

하위 도메인을 기존 Fastly 계정에서 클라우드 인프라의 Adobe Commerce에 대한 Fastly 계정으로 위임하도록 요청하는 [Fastly 지원 티켓을 제출]합니다. 티켓에 Adobe Commerce 프로젝트 ID를 포함하십시오.

위임이 완료되면 프로젝트 하위 도메인을 클라우드 인프라의 Adobe Commerce에 대한 Fastly 서비스 계정에 추가할 수 있습니다. [Fastly 자격 증명 가져오기](fastly-configuration.md#get-fastly-credentials)를 참조하십시오.

*시나리오 2:*

Apex 도메인(`testweb.com` 및 `www.testweb.com`)이 클라우드 인프라 Fastly 서비스 계정의 Adobe Commerce에 연결되어 있습니다. 다른 Fastly 계정의 `service.testweb.com` 및 `product-updates.testweb.com` 하위 도메인에 대한 Fastly 서비스를 관리하려는 경우

하위 도메인을 클라우드 인프라 Fastly 서비스 계정의 Adobe Commerce에서 Fastly 계정으로 위임하도록 요청하는 Adobe Commerce 지원 티켓을 제출합니다. 티켓에 Fastly 계정의 서비스 ID를 포함하십시오.

## DOS 보호

DDOS 보호는 Fastly CDN 서비스에 내장되어 있습니다. Adobe Commerce 사이트에 대해 Fastly 서비스를 활성화하면 Fastly는 모든 웹 및 관리자 트래픽을 필터링하여 잠재적인 공격을 감지하고 차단합니다.

- 계층 3 또는 4를 대상으로 하는 공격의 경우 Fastly 서비스는 포트 및 프로토콜을 기반으로 트래픽을 필터링하여 HTTP 또는 HTTPS 요청만 검사합니다. ICMP, UDP 및 기타 네트워크 시작 공격은 네트워크 에지에서 삭제됩니다. 여기에는 SSDP 또는 NTP와 같은 UDP 서비스를 사용하는 반사 및 증폭 공격이 포함됩니다. 이러한 수준의 보호를 제공함으로써 Ping of Death, Smurf 공격 및 기타 ICMP 기반 홍수와 같은 여러 가지 일반적인 공격을 효과적으로 차단합니다.

  캐시 계층에서 TCP 레벨 공격을 빠르게 관리합니다. 이 전략은 클라이언트별로 SYN 플러드 공격과 Fastly 시스템 내의 TCP 스택, 리소스 공격 및 TLS 공격을 포함한 다양한 변형을 처리하는 데 필요한 규모와 컨텍스트를 제공합니다.

- Fastly는 Layer 7 공격으로부터 보호합니다. 스토어에서 성능 문제가 발생하고 Layer 7 DDoS 공격이 의심되는 경우 Adobe Commerce 지원 티켓을 제출하십시오. Adobe은 사용자 정의 규칙을 만들어 Fastly 서비스에 적용하여 헤더, 페이로드 또는 공격 트래픽을 식별하는 속성의 조합을 기반으로 악의적인 요청을 검사하고 필터링할 수 있습니다. *Adobe Commerce 도움말 센터*&#x200B;에서 [DDoS 공격 확인] 및 [악성 트래픽을 차단하는 방법]을 참조하세요.

<!--Link definitions-->

[Caching with Fastly]: https://developer.adobe.com/commerce/webapi/graphql/usage/caching/#caching-with-fastly

[DDoS 공격 확인]: https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/checking-for-ddos-attack-from-cli.html

[Magento 2용 Fastly CDN 모듈]: https://github.com/fastly/fastly-magento2

[Fastly 지원 티켓]: https://docs.fastly.com/products/support-description-and-sla#support-requests

[악성 트래픽을 차단하는 방법]: https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/block-malicious-traffic-for-magento-commerce-on-fastly-level.html

[도메인 작업]: https://docs.fastly.com/en/guides/working-with-domains
