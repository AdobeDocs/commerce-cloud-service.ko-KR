---
title: 시작 체크리스트
description: 사이트 실행에 대한 체크리스트 항목을 검토합니다.
exl-id: 4525742e-18c5-40d1-975d-00ba3f3a51a0
source-git-commit: 6ac23cbcf7ab48d09b494ebe8c7136518d213c4e
workflow-type: tm+mt
source-wordcount: '1104'
ht-degree: 0%

---

# 시작 체크리스트

프로덕션 환경에 배포하기 전에 [Launch 검사 목록](../../assets/adobe-commerce-cloud-prelaunch-checklist.pdf)을 다운로드하고 이 지침과 함께 사용하여 필요한 모든 구성 및 테스트를 완료했는지 확인하십시오. [스토어 배포](../deploy/staging-production.md)에서 Starter 및 Pro의 전체 배포 프로세스 개요를 확인하세요.

## 프로덕션에서 완전히 테스트

사이트, 스토어 및 환경의 모든 측면을 테스트하려면 [배포 테스트](../test/staging-and-production.md)를 참조하십시오. 이러한 테스트에는 Fastly 확인, UAT(사용자 승인 테스트) 및 성능 테스트가 포함됩니다.

## TLS와 Fastly

Adobe은 각 환경에 대해 Let&#39;s Encrypt SSL/TLS 인증서를 제공합니다. 이 인증서는 Fastly가 HTTPS를 통해 보안 트래픽을 제공하는 데 필요합니다.

이 인증서를 사용하려면 Adobe이 도메인 유효성 검사를 완료하고 환경에 인증서를 적용할 수 있도록 DNS 구성을 업데이트해야 합니다. 각 환경에는 해당 환경에 배포된 클라우드 인프라 사이트의 Adobe Commerce 도메인을 다루는 고유한 인증서가 있습니다. [빠르게 설정 프로세스](../cdn/fastly-configuration.md) 중에 구성 업데이트를 완료하고 업데이트하는 것이 좋습니다.

## 프로덕션 설정으로 DNS 구성 업데이트

사이트를 시작할 준비가 되면 DNS 구성을 업데이트하여 프로덕션 환경에서 Fastly 서비스를 통해 트래픽을 라우팅해야 합니다.

**필수 구성 요소:**

- [개발 환경에서 Fastly 설정 및 테스트](../cdn/fastly-configuration.md#)

- 프로덕션 환경 구성이 모든 필수 도메인으로 업데이트되었습니다

  일반적으로 고객 기술 관리자와 협력하여 스토어에 필요한 모든 최상위 도메인 및 하위 도메인을 추가합니다. 프로덕션 환경의 도메인을 추가하거나 변경하려면 [Adobe Commerce 지원 티켓을 제출](https://support.magento.com/hc/en-us/articles/360019088251)하십시오. 프로젝트 구성이 업데이트되었는지 확인할 때까지 기다립니다.

  스타터 프로젝트에서 프로젝트에 도메인을 추가해야 합니다. [도메인 관리](../cdn/fastly-custom-cache-configuration.md#manage-domains)를 참조하십시오.

- 프로덕션 환경에 대해 프로비저닝된 SSL/TLS 인증서.

  Fastly 설정 프로세스 중에 프로덕션 도메인에 대한 ACME 챌린지 레코드를 추가한 경우, Adobe은 DNS 구성을 업데이트하여 Fastly 서비스로 트래픽을 라우팅할 때 프로덕션 환경에 SSL/TLS 인증서를 자동으로 업로드합니다. 인증서를 미리 프로비저닝하지 않았거나 도메인을 업데이트한 경우 Adobe이 도메인 유효성 검사를 완료하고 인증서를 프로비저닝해야 하며, 이 작업은 최대 12시간 정도 소요될 수 있습니다.

### 사이트 실행에 대한 DNS 구성을 업데이트하려면:

1. 프로덕션 사이트에 대한 다음 DNS 구성을 업데이트합니다.

   - 특히 기존 사이트에서 마이그레이션하는 경우 필요한 모든 리디렉션을 설정합니다

   - 호스트 이름을 지정하기 위해 영역의 루트 리소스 레코드 설정

   - TTL(Time-to-Live) 값을 낮추어 DNS 정보를 새로 고쳐 고객이 올바른 프로덕션 스토어를 찾도록 합니다.

     DNS 레코드를 전환할 때 TTL 값을 크게 낮추는 것이 좋습니다. 이 값은 DNS 레코드를 캐시하는 시간을 DNS에 알려줍니다. 단축하면 DNS가 더 빨리 새로 고쳐집니다. 예를 들어 사이트를 업데이트할 때 TTL 값을 3일에서 10분으로 변경할 수 있습니다. TTL 값을 줄이면 DNS 인프라에 로드가 증가합니다. 사이트 실행 후 이전 더 높은 값을 복원합니다.


1. 프로덕션 환경의 하위 도메인을 Fastly 서비스 `prod.magentocloud.map.fastly.net`에 가리키도록 CNAME 레코드를 추가합니다. 예:

   | 도메인 또는 하위 도메인 | CNAME |
   | ----------------------- | -------------------------------- |
   | `www.<domain-name>.com` | prod.magentocloud.map.fastly.net |
   | `mystore.<domain-name>.com` | prod.magentocloud.map.fastly.net |

1. 필요한 경우 A 레코드를 추가하여 Apex 도메인(`<domain-name>.com`)을 다음 Fastly IP 주소에 매핑합니다.

   | Apex 도메인 | ANAME |
   | --------------- | ----------------- |
   | `<domain-name>.com` | `151.101.1.124` |
   | `<domain-name>.com` | `151.101.65.124` |
   | `<domain-name>.com` | `151.101.129.124` |
   | `<domain-name>.com` | `151.101.193.124` |

>[!IMPORTANT]
>
>[RFC1034](https://www.rfc-editor.org/rfc/rfc1912)(**섹션 2.4**)의 DNS 지침 상태:
>_CNAME 레코드는 다른 데이터와 함께 사용할 수 없습니다. 즉, suzy.podunk.xx가 sue.podunk.xx에 대한 별칭인 경우 suzy.podunk.edu에 대한 MX 레코드나 A 레코드 또는 TXT 레코드도 가질 수 없습니다._
>
>따라서 DNS 레코드는 하위 도메인의 경우 유형 `CNAME`이고 Apex 도메인(루트 도메인)의 경우 유형 `A`이어야 합니다. 이 규칙을 무시하면 MX 또는 NS와 같은 다른 레코드를 추가할 수 없으므로 메일 서비스 또는 DNS 전파가 중단될 수 있습니다. 일부 DNS 공급자는 내부 사용자 지정을 사용하여 이를 우회할 수 있지만, 표준을 준수하면 안정성과 유연성(예: DNS 공급자 변경)이 보장됩니다.

1. 기본 URL을 업데이트합니다.

   - SSH를 사용하여 프로덕션 환경에 로그인합니다.

     ```bash
     magento-cloud ssh -e production
     ```

   - CLI를 사용하여 저장소의 기본 URL을 변경합니다.

     ```bash
     php bin/magento setup:store-config:set --base-url="https://www.<domain-name>.com/"
     ```

   **참고**: 관리자로부터 기본 URL을 업데이트할 수도 있습니다. _Adobe Commerce 스토어 및 구매 경험 가이드_&#x200B;에서 [스토어 URL](https://experienceleague.adobe.com/docs/commerce-admin/stores-sales/site-store/store-urls.html)을(를) 참조하십시오.

1. 사이트가 업데이트될 때까지 몇 분 정도 기다립니다.

1. 사이트를 테스트합니다.

## 프로덕션 구성 확인

최종 단계를 수행하여 하나 이상의 스토어에 대한 프로덕션 구성의 유효성을 검사합니다. 프로덕션 환경에서 구성을 업데이트할 수 있습니다. 설정이 읽기 전용인 경우 SSH 연결을 열고 CLI 명령을 사용하여 구성을 변경하거나 로컬 환경에서 구성을 변경해야 할 수 있습니다. 업데이트를 완료한 후 변경 사항을 스테이징 및 프로덕션 환경에 배포할 수 있습니다.

다음은 권장되는 변경 사항 및 확인 사항입니다.

- [발신 이메일 테스트 완료](../project/outgoing-emails.md)

- [관리자 자격 증명 및 기본 관리자 URL에 대한 보안 구성](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/security/security-admin)

- [웹에 대한 모든 이미지 최적화](../cdn/fastly-image-optimization.md)

- [HTML, JavaScript 및 CSS에 대한 축소 설정 확인](../deploy/static-content.md)

## Fastly 캐싱 확인

- 프로덕션 사이트에서 Fastly 캐싱이 올바르게 작동하는지 테스트하고 확인합니다. 자세한 테스트 및 검사는 [빠른 테스트](../test/staging-and-production.md#check-fastly-caching)를 참조하십시오.

- [Commerce용 Fastly CDN 모듈의 최신 버전이 프로덕션 환경에 설치되어 있는지 확인합니다](../cdn/fastly-configuration.md#upgrade-the-fastly-module)

- [최신 버전의 Fastly VCL 코드가 업로드되었는지 확인합니다.](../cdn/fastly-configuration.md#upload-vcl-to-fastly)

## 성능 테스트

출시 전 준비 프로세스의 일부로 [성능 도구 키트](https://github.com/magento/magento2/tree/2.4/setup/performance-toolkit) 옵션을 검토하는 것이 좋습니다.

다음 타사 옵션을 사용하여 테스트할 수도 있습니다.

- [Siege](https://www.joedog.org/siege-home/): 스토어를 한도로 늘리기 위한 트래픽 셰이핑 및 테스트 소프트웨어. 구성 가능한 수의 시뮬레이션된 클라이언트를 사용하여 사이트를 테스트합니다. Siege는 기본 인증, 쿠키, HTTP, HTTPS 및 FTP 프로토콜을 지원합니다.

- [Jmeter](https://jmeter.apache.org/): 플래시 판매와 같은 스파이크 트래픽의 성능을 측정하는 데 도움이 되는 우수한 부하 테스트입니다. 사이트에 대해 실행할 사용자 지정 테스트를 만듭니다.

- [New Relic](https://support.newrelic.com/s/)(제공됨): 데이터, 쿼리, Redis 전송 등과 같이 작업당 추적된 시간을 사용하여 느린 성능을 유발하는 사이트의 프로세스 및 영역을 찾는 데 도움이 됩니다.

- [WebPageTest](https://www.webpagetest.org/) 및 [Pingdom](https://www.pingdom.com/): 원본 위치가 다른 사이트 페이지 로드 시간을 실시간으로 분석합니다. 핑돔은 비용이 들 수 있습니다. WebPageTest는 무료 도구입니다.

## 보안 구성

- [보안 검사 설정](overview.md#set-up-the-security-scan-tool)

- [관리 사용자에 대한 보안 구성](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/security/security-admin)

- [관리 URL에 대한 보안 구성](https://experienceleague.adobe.com/en/docs/commerce-admin/stores-sales/site-store/store-urls#use-a-custom-admin-url)

- [Adobe Commerce on cloud infrastructure 프로젝트에서 더 이상 존재하지 않는 사용자 제거](../project/user-access.md)

- [이중 인증 구성](https://developer.adobe.com/commerce/testing/functional-testing-framework/two-factor-authentication/)

## 성능 모니터링

New Relic 서비스를 사용하여 Pro 및 Starter 환경의 성능을 모니터링할 수 있습니다. Pro 플랜 계정에서 Managed alerts for Adobe Commerce 경고 정책을 제공하여 New Relic APM 및 인프라 에이전트를 사용하여 애플리케이션 및 인프라 성능을 모니터링합니다. 이러한 서비스 사용에 대한 자세한 내용은 [관리 경고로 성능 모니터링](../monitor/investigate-performance.md#monitor-performance-with-managed-alerts)을 참조하십시오.

### 다음 단계

[시작 단계](steps.md)
