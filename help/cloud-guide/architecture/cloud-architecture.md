---
title: Commerce용 클라우드 아키텍처
description: 클라우드 인프라에서 Commerce를 위한 Starter 및 Pro 프로젝트 아키텍처와 어떻게 대비되는지 알아봅니다.
feature: Cloud, Iaas, Paas
topic: Architecture
recommendations: noDisplay
exl-id: 37cd6733-c10a-4d06-b784-171da576f9fc
source-git-commit: 1253d8357fd2554050d1775fefbc420a2097db5f
workflow-type: tm+mt
source-wordcount: '745'
ht-degree: 0%

---

# Commerce용 클라우드 아키텍처

클라우드 인프라의 Adobe Commerce에는 Starter 및 Pro 플랜이 있습니다. 각 계획에는 Adobe Commerce 개발 및 배포 프로세스를 구동하는 고유한 아키텍처가 있습니다. Starter 플랜과 Pro 플랜 아키텍처는 모두 지속적인 통합을 지원하면서 엔드 투 엔드 테스트를 위해 여러 환경에 데이터베이스, 웹 서버 및 캐싱 서버를 배포합니다.

비교를 위해 각 계획에는 다음 인프라 기능과 지원되는 제품이 포함되어 있습니다.

|          | 스타터 | Pro |
| -------- | --------------------| ------------------ |
| 핵심 기능 | <ul><li>[모든 Adobe Commerce 기능](https://experienceleague.adobe.com/docs/commerce-operations/release/features.html)</li><li>PayPal 온보딩 도구</li><li>[Commerce 보고](https://business.adobe.com/products/magento/business-intelligence.html?_ga=2.85288604.442698376.1665067470-1322106587.1655147209)</li></ul> | <ul><li>[모든 Adobe Commerce 기능](https://experienceleague.adobe.com/docs/commerce-operations/release/features.html)</li><li>PayPal 온보딩 도구</li><li>[Commerce 보고](https://business.adobe.com/products/magento/business-intelligence.html?_ga=2.85288604.442698376.1665067470-1322106587.1655147209)</li><li>[B2B 모듈](https://business.adobe.com/products/magento/b2b-ecommerce.html?_ga=2.105948422.442698376.1665067470-1322106587.1655147209)</li></ul> |
| 인프라 및 구축 | <ul><li>무제한 사용자가 포함된 지속적인 클라우드 통합 도구</li><li>Fastly CDN(Content Delivery Network), IO(이미지 최적화) 및 충분한 대역폭 허용으로 보안 강화 WAF(Web Application Firewall) 서비스는 프로덕션 환경에서만 사용할 수 있습니다.</li><li>[New Relic](../monitor/new-relic-service.md) 3개 분기의 APM(성능 모니터링): `master` 선택한 항목 2개<br>Adobe Commerce에 최적화된 PaaS(Platform as a Service) 프로덕션, 스테이징 및 개발 환경(총 4개의 활성 환경)</li><li>이그레스 필터링(아웃바운드 방화벽)</li></ul> | <ul><li>무제한 사용자가 포함된 지속적인 클라우드 통합 도구</li><li>Fastly CDN(Content Delivery Network), IO(이미지 최적화) 및 충분한 대역폭 허용으로 보안 강화 WAF(Web Application Firewall) 서비스는 프로덕션 환경에서만 사용할 수 있습니다.</li><li>[New Relic](../monitor/new-relic-service.md) 프로덕션 인프라 + 스테이징 및 프로덕션의 APM(성능 모니터링). 다음 [관리 경고 정책](../monitor/investigate-performance.md#monitor-performance-with-managed-alerts) for Adobe Commerce 정책은 사이트 성능에 영향을 주는 애플리케이션 및 인프라 문제에 대해 사전에 알리는 모니터링 모범 사례를 구현합니다.</li><li>PaaS(Platform as a Service) 기반 [통합 개발](pro-architecture.md#integration-environment) Adobe Commerce에 최적화된 환경(총 2개의 활성 환경)</li><li>IaaS(Infrastructure as a Service)—스테이징 및 프로덕션 환경을 위한 전용 가상 인프라</li></ul> |
| 고가용성 인프라 | | [고가용성 아키텍처](pro-architecture.md#redundant-hardware) 엔터프라이즈급 안정성과 가용성을 제공하기 위해 기본 IaaS(Infrastructure as a Service)에서 3대의 서버를 설정합니다. |
| 전용 하드웨어 | | 기본 IaaS(Infrastructure as a Service)에 있는 전용 하드웨어를 격리하여 더욱 높은 수준의 안정성과 가용성을 제공합니다. |
| 연중무휴 이메일 지원 | 핵심 애플리케이션 및 클라우드 인프라에 대한 24x7 모니터링 및 이메일 지원 | 핵심 애플리케이션 및 클라우드 인프라에 대한 24x7 모니터링 및 이메일 지원 |
| 전담 CTA(Customer Technical Advisor) | | 구독부터 최초 사이트 출시 전까지 최초 출시 기간 동안 전담 기술 계정 관리 |
| 추가 기능\* | <ul><li>[B2B 모듈](https://business.adobe.com/products/magento/b2b-ecommerce.html)</li></ul> |

\* _추가 요금으로 이용 가능_

## 초보자 프로젝트

다음 [스타터 플랜 아키텍처](starter-architecture.md) 에는 네 가지 환경이 있습니다.

- **통합**—통합 환경에서는 두 가지 테스트 가능한 환경을 제공합니다. 각 환경에는 활성 Git 분기, 데이터베이스, 웹 서버, 캐싱, 일부 서비스, 환경 변수 및 구성이 포함됩니다.

- **스테이징**—코드 및 확장이 테스트를 통과함에 따라 `integration` 스테이징 환경으로 분기합니다. 스테이징 환경은 프로덕션 이전 테스트 환경이 됩니다. 여기에는 다음이 포함됩니다. `staging` 활성 분기, 데이터베이스, 웹 서버, 캐싱, 타사 서비스, 환경 변수, 구성 및 서비스(예: Fastly 및 New Relic).

- **프로덕션**—코드가 준비되고 테스트되면 모든 코드가에 병합됩니다. `master` 프로덕션 라이브 사이트에 배포용. 이 환경에는 활성 상태가 포함됩니다. `master` 분기, 데이터베이스, 웹 서버, 캐싱, 타사 서비스, 환경 변수 및 구성

- **비활성**- 비활성 분기가 무제한으로 있습니다.

## 프로젝트 홍보

다음 [Pro 플랜 아키텍처](pro-architecture.md) 전역 포함 `master` 세 가지 환경:

- **통합**—통합 환경은 데이터베이스, 웹 서버, 캐싱, 일부 서비스, 환경 변수 및 구성을 포함하는 테스트 가능한 환경을 제공합니다. 스테이징 환경으로 병합하기 전에 코드를 개발, 배포 및 테스트할 수 있습니다.

   - _비활성_- 를 기반으로 하는 비활성 분기의 수를 제한 없이 가질 수 있습니다. `integration` 환경은 하나만 활성 분기(포함하지 않음) `integration` ).

- **스테이징**—스테이징 환경은 사전 프로덕션 테스트용이며 데이터베이스, 웹 서버, 캐싱, 타사 서비스, 환경 변수, 구성 및 서비스(예: Fastly)를 포함합니다.

- **프로덕션**—운영 환경에는 데이터, 서비스, 캐싱 및 스토어를 위한 고가용성 3노드 아키텍처가 포함되어 있습니다. 프로덕션은 환경 변수, 구성 및 서드파티 서비스가 있는 라이브 공용 스토어 환경입니다.

## 지원되는 소프트웨어 및 서비스

클라우드 인프라의 Adobe Commerce은 다음을 사용합니다.

- 운영 체제: Debian GNU/Linux
- 웹 서버: Nginx
- 데이터베이스: MySQL(MariaDB)
- CDN(Content Delivery Network): Fastly CDN

다음 서비스를 구성할 수 있습니다.

- [PHP](../application/php-settings.md)
- [MySQL](../services/mysql.md)
- [레디스](../services/redis.md)
- [RabbitMQ](../services/rabbitmq.md)
- [Elasticsearch](../services/elasticsearch.md)
- [OpenSearch](../services/opensearch.md)

{{elasticsearch-support}}

>[!NOTE]
>
>다음을 참조하십시오 [시스템 요구 사항](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html) 다음에서 _설치 안내서_ 권장 버전.

Fastly CDN 모듈은 스테이징 및 프로덕션 환경에서 CDN 및 캐싱 서비스에 사용됩니다. 다음을 참조하십시오 [Fastly 서비스 구성](../cdn/fastly.md).

구현에서 사용할 소프트웨어 버전을 구성하는 방법에 대한 자세한 내용은 클라우드 인프라 구성 파일에 대한 다음 Adobe Commerce을 참조하십시오.

- [애플리케이션 구성(.magento.app.yaml)](../application/configure-app-yaml.md)
- [환경 구성(.magento.env.yaml)](../environment/configure-env-yaml.md)
- [경로 구성(routes.yaml)](../routes/routes-yaml.md)
- [서비스 구성(services.yaml)](../services/services-yaml.md)
