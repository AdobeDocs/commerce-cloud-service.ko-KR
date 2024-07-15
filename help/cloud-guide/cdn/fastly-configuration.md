---
title: Fastly 서비스 구성
description: Adobe Commerce 프로젝트에 대한 Fastly 서비스를 설정하고 구성하는 방법에 대해 알아봅니다.
feature: Cloud, Configuration, Iaas, Cache, Security
exl-id: c53ff3bd-3df2-45fb-933e-d3b29f7edf4e
source-git-commit: 8a0523f1714b6ea41887e99b5c31294cf5e5255e
workflow-type: tm+mt
source-wordcount: '1961'
ht-degree: 0%

---

# Fastly 서비스 구성

클라우드 인프라 스테이징 및 프로덕션 환경의 Adobe Commerce에는 Fastly가 필요합니다.

Fastly는 Vannish와 함께 작동하여 빠른 캐싱 기능과 정적 자산에 대한 [CDN(Content Delivery Network](https://glossary.magento.com/content-delivery-network))을 제공합니다. 또한 Fastly는 사이트 및 클라우드 인프라를 보호하기 위한 WAF(Web Application Firewall)를 제공합니다. 악성 트래픽 및 공격으로부터 사이트 및 클라우드 인프라를 보호하려면 들어오는 모든 사이트 트래픽을 Fastly를 통해 라우팅합니다.

>[!NOTE]
>
>Fastly는 통합 환경에서 사용할 수 없습니다.

다음 단계를 완료하여 사이트 개발 프로세스 초기에 Fastly를 활성화, 구성 및 테스트하여 사이트에 대한 보안 액세스를 활성화합니다.

- 스테이징 및 프로덕션 환경에 대한 Fastly 자격 증명 가져오기
- Fastly CDN 캐싱 활성화
- Fastly VCL 코드 조각 업로드
- Fastly 서비스로 트래픽을 라우팅하도록 DNS 구성 업데이트
- Fastly 캐싱 테스트

>[!NOTE]
>
>초기 Fastly 구성을 활성화하고 확인한 후 구성을 사용자 지정할 수 있습니다. 예를 들어 이미지 최적화, Edge 모듈 및 사용자 지정 VCL 코드와 같은 추가 옵션을 활성화할 수 있습니다. [캐시 구성 사용자 지정](fastly-custom-cache-configuration.md)을 참조하십시오.

## Fastly 자격 증명 가져오기

프로젝트를 프로비저닝하는 동안 Adobe은 클라우드 인프라의 Adobe Commerce에 대한 [Fastly 서비스 계정](fastly.md#fastly-service-account-and-credentials)에 프로젝트를 추가하고 스타터 `master` 및 Pro 스테이징 및 프로덕션 환경에 대한 Fastly 계정 자격 증명을 만듭니다. 각 환경에는 고유한 자격 증명이 있습니다.

관리자로부터 Fastly CDN 서비스를 구성하고 Fastly API 요청을 제출하려면 Fastly 자격 증명이 필요합니다.

>[!NOTE]
>
>클라우드 인프라의 Adobe Commerce을 사용하면 Fastly 관리자에 직접 액세스할 수 없습니다. 관리자를 사용하여 환경에 대한 Fastly 구성을 검토하고 업데이트합니다. 관리자의 Fastly 기능을 사용하여 문제를 해결할 수 없는 경우 [Adobe Commerce 지원 티켓](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)을 제출하세요.

다음 메서드를 사용하여 환경에 대한 Fastly 서비스 ID 및 API 토큰을 찾아 저장합니다.

**Fastly 자격 증명을 보려면**:

자격 증명을 보는 방법은 Pro 및 Starter 프로젝트에서 다릅니다.

- IaaS 탑재 공유 디렉터리 - Pro 프로젝트에서 SSH를 사용하여 서버에 연결하고 `/mnt/shared/fastly_tokens.txt` 파일에서 Fastly 자격 증명을 가져옵니다. 스테이징 및 프로덕션 환경에는 고유한 자격 증명이 있습니다. 각 환경에 대한 자격 증명을 가져와야 합니다.

- 로컬 작업 영역—명령줄에서 `magento-cloud` CLI를 사용하여 Fastly 환경 변수를 [나열하고 검토](../environment/variables-cloud.md#viewing-environment-variables)합니다.

  ```bash
  magento-cloud variable:get -e <environment-ID>
  ```

- [!DNL Cloud Console] - [환경 구성](../project/overview.md#configure-environment)에서 다음 환경 변수를 확인하십시오.

   - `CONFIG__DEFAULT__SYSTEM__FULL_PAGE_CACHE__FASTLY__FASTLY_API_KEY`

   - `CONFIG__DEFAULT__SYSTEM__FULL_PAGE_CACHE__FASTLY__FASTLY_SERVICE_ID`

>[!NOTE]
>
>스테이징 또는 프로덕션 환경에 대한 Fastly 자격 증명을 찾을 수 없는 경우 Adobe CTA(Customer Technical Advisor)에 문의하십시오.

## Fastly 캐싱 활성화

Fastly 서비스를 활성화하고 구성하려면 다음 구성 요소가 필요합니다.

- 스테이징 및 프로덕션 환경에 설치된 [Magento 2 모듈](fastly.md#fastly-cdn-module-for-magento-2)용 Fastly CDN의 최신 버전입니다. [빠르게 업그레이드](#upgrade-the-fastly-module)를 참조하세요.

- 클라우드 인프라 스테이징 및 프로덕션 환경의 Adobe Commerce에 대한 [Fastly 자격 증명](#get-fastly-credentials)

**스테이징 및 프로덕션에서 Fastly CDN 캐싱을 사용하려면**:

{{admin-login-step}}

1. **저장** > 설정 > **구성** > **고급** > **시스템**&#x200B;을 클릭하고 **전체 페이지 캐시**&#x200B;를 확장합니다.

   ![빠르게 선택하려면 확장](../../assets/cdn/fastly-menu.png)

1. _응용 프로그램 캐싱_ 섹션의 **시스템 값 사용**&#x200B;에서 선택 항목을 제거한 다음 드롭다운 목록에서 **Fastly CDN**&#x200B;을 선택합니다.

   ![빠르게 선택](../../assets/cdn/fastly-enable-admin.png)

1. **빠른 구성**&#x200B;을 확장하고 [캐싱 옵션을 선택](https://github.com/fastly/fastly-magento2/blob/master/Documentation/CONFIGURATION.md#configure-the-module)합니다.

1. 캐싱 옵션을 구성한 후 페이지 상단에서 **구성 저장**&#x200B;을 클릭합니다.

1. 알림에 따라 캐시를 지웁니다.

1. **스토어** > **설정** > **구성** > **고급** > **시스템** > **Fastly 구성**(으)로 다시 이동하여 Fastly 구성을 계속합니다.

### Fastly 자격 증명 테스트

1. 관리자의 경우 **스토어** > 설정 > **구성** > **고급** > **시스템** > **빠른 구성**&#x200B;으로 이동합니다.

1. 필요한 경우 프로젝트 환경에 대해 **Fastly 서비스 ID** 및 **API 토큰** 값을 추가하십시오.

   ![Fastly 자격 증명 관리자](../../assets/cdn/fastly-credentials-admin-ui.png)

   >[!NOTE]
   >
   >Fastly API 토큰을 만들 링크를 선택하지 마십시오. 대신 Adobe에서 제공한 ](#get-fastly-credentials) Adobe에서 제공한 [Fastly 자격 증명(서비스 ID 및 API 토큰)을 사용하십시오.

1. **자격 증명 테스트**&#x200B;를 클릭합니다.

1. 테스트가 성공하면 **구성 저장**&#x200B;을 클릭한 다음 캐시를 지웁니다.

   테스트가 실패하면 올바른 서비스 ID 및 API 토큰 값이 현재 환경의 자격 증명과 일치하는지 확인하십시오.

   테스트가 다시 실패하면 Adobe Commerce 지원 티켓을 제출하거나 Adobe 계정 담당자에게 문의하십시오. Pro 프로젝트의 경우 프로덕션 및 스테이징 사이트의 URL을 포함합니다. 시작 프로젝트의 경우 `Master` 및 스테이징 사이트의 URL을 포함하십시오.

>[!NOTE]
>
>스테이징 또는 프로덕션 환경에 대한 Fastly API 토큰 자격 증명을 변경하는 방법은 [Fastly 자격 증명 변경](fastly.md#change-fastly-api-token)을 참조하십시오.

### VCL을 Fastly에 업로드

Fastly 모듈을 사용하도록 설정한 후 기본 [VCL 코드](https://github.com/fastly/fastly-magento2/tree/master/etc/vcl_snippets)를 Fastly 서버에 업로드합니다. 이 코드는 클라우드 인프라에서 Adobe Commerce에 대한 캐싱 및 기타 Fastly CDN 서비스를 사용하도록 설정하는 구성 설정을 지정하는 일련의 VCL 코드 조각을 제공합니다.

>[!NOTE]
>
>Fastly 캐싱 서비스는 Adobe Commerce 스테이징 및 프로덕션 사이트에 Fastly VCL 코드의 초기 업로드를 완료할 때까지 작동하지 않습니다.

**가장 빠른 VCL을 업로드하려면**:

1. 다음 그림과 같이 _Fastly 구성_ 섹션에서 **Fastly에 VCL 업로드**&#x200B;를 클릭합니다.

   ![Magento VCL을 Fastly에 업로드](../../assets/cdn/fastly-upload-vcl-admin.png)

1. 업로드가 완료되면 페이지 상단의 알림에 따라 캐시를 새로 고칩니다.

## SSL/TLS 인증서 프로비저닝

Adobe은 Fastly에서 보안 HTTPS 트래픽을 제공하기 위해 도메인에 의해 검증된 Let&#39;s Encrypt SSL/TLS 인증서를 제공합니다. Adobe은 각 Pro 프로덕션, 스테이징 및 스타터 프로덕션 환경에 대해 하나의 인증서를 제공하여 해당 환경의 모든 도메인을 보호합니다. 제공된 인증서에 대한 자세한 내용은 [클라우드 인프라의 Adobe Commerce에 대한 SSL(TLS) 인증서 Adobe](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/ssl-tls-certificates-for-magento-commerce-cloud-faq.html)을 참조하십시오.

>[!NOTE]
>
>Adobe에서 제공하는 Let&#39;s Encrypt 인증서를 사용하는 대신 자체 TLS 또는 SSL 인증서를 제공할 수 있습니다. 그러나 이 프로세스를 설정하고 유지 관리하려면 추가 작업이 필요합니다. 이 옵션을 선택하려면 Adobe Commerce 지원 티켓을 제출하거나 Adobe을 사용하여 사용자 지정 호스팅 인증서를 클라우드 인프라 환경의 Adobe Commerce에 추가하십시오.

Adobe Commerce 환경에 대한 SSL/TLS 인증서를 활성화하려면 Adobe 자동화는 다음 단계를 완료합니다.

- 도메인 소유권 확인
- 스토어에 대해 지정된 최상위 수준 및 하위 도메인을 포함하는 Let&#39;s Encrypt SSL/TLS 인증서 프로비저닝
- 사이트가 라이브 상태일 때 클라우드 환경에 인증서를 업로드합니다

이 자동화를 사용하려면 사이트에 대한 DNS 구성을 업데이트하여 도메인 유효성 검사 정보를 제공해야 합니다. 다음 메서드 중 **one**&#x200B;을(를) 사용합니다.

- **DNS 유효성 검사**-라이브 사이트의 경우 Fastly 서비스를 가리키는 CNAME 레코드로 DNS 구성을 업데이트하십시오
- **ACME 챌린지 CNAME 레코드**-환경의 각 도메인에 대해 Adobe이 제공한 ACME 챌린지 CNAME 레코드로 DNS 구성을 업데이트합니다.

>[!TIP]
>
>활성화되지 않은 프로덕션 도메인이 있는 경우 도메인 유효성 검사에 ACME 챌린지 CNAME 레코드를 사용합니다. 조기에 DNS 구성에 레코드를 추가하면 Adobe이 사이트를 시작하기 전에 올바른 도메인에 SSL/TLS 인증서를 프로비저닝할 수 있습니다. 프로덕션에 시작하기 전에 이러한 자리 표시자 레코드를 Adobe에서 제공하는 CNAME 레코드로 바꿔야 합니다.

도메인 유효성 검사가 완료되면 Adobe은 Let&#39;s Encrypt TLS/SSL 인증서를 프로비저닝하고 라이브 스테이징 또는 프로덕션 환경에 업로드합니다. 이 프로세스는 최대 12시간 정도 소요될 수 있습니다. 사이트 개발 및 사이트 실행이 지연되지 않도록 며칠 전에 DNS 구성 업데이트를 완료하는 것이 좋습니다.

## 개발 설정으로 DNS 구성 업데이트

초기 Fastly 설정 프로세스 중에 다음 URL을 사용하여 스테이징 및 프로덕션 환경에서 Fastly 캐싱을 구성하고 테스트할 수 있습니다.

- Pro Staging 및 프로덕션의 경우

   - `mcprod.<your-domain>.com`
   - `mcstaging.<your-domain>.com`

- 스타터 프로덕션의 경우:

   - `mcprod.<your-domain>.com`

이러한 기본 사전 프로덕션 URL은 프로젝트가 프로비저닝된 후에 사용할 수 있습니다. `"your-domain"`의 값은 온보딩 프로세스 중에 지정한 도메인 이름입니다.

>[!NOTE]
>
>스타터 프로젝트에서 비프로덕션 환경에 대한 사용자 정의 도메인을 지정할 수 없습니다.

스토어 URL에서 Fastly 서비스로 트래픽을 라우팅하려면 DNS 구성을 업데이트합니다. 구성을 업데이트하면 Adobe에서 필요한 SSL/TLS 인증서를 자동으로 프로비저닝하고 클라우드 환경에 업로드합니다. 이 프로비저닝은 최대 12시간 정도 소요될 수 있습니다.

>[!NOTE]
>
>프로덕션 사이트를 시작할 준비가 되면 DNS 구성을 다시 업데이트하여 프로덕션 도메인을 Fastly 서비스로 유도하고 추가 구성 작업을 완료해야 합니다. [시작 검사 목록](../launch/checklist.md)을 참조하세요.

**필수 구성 요소:**

- Fastly 모듈을 활성화합니다.
- 기본 Fastly VCL 코드를 업로드합니다.
- 각 환경에 대한 최상위 및 하위 도메인 목록을 Adobe에 제공하거나 Adobe Commerce 지원 티켓을 제출합니다.
- 지정된 도메인이 클라우드 환경에 추가되었는지 확인될 때까지 기다립니다.
- 스타터 프로젝트에서 도메인을 Fastly 서비스 구성에 추가합니다. [도메인 관리](fastly-custom-cache-configuration.md#manage-domains)를 참조하십시오.
- DNS 구성 업데이트에 대한 자세한 내용은 [DNS 등록자](https://lookup.icann.org/)에서 도메인 서비스에 대한 올바른 방법을 확인하십시오.

**개발을 위해 DNS 구성을 업데이트하려면**:

1. CNAME 레코드 `prod.magentocloud.map.fastly.net`을(를) 추가하여 Fastly 서비스로 사전 프로덕션 URL을 지정합니다. 예:

   | 도메인 또는 하위 도메인 | CNAME |
   |---------------------------|----------------------------------|
   | mcprod.your-domain.com | prod.magentocloud.map.fastly.net |
   | mcstaging.your-domain.com | prod.magentocloud.map.fastly.net |

   CNAME 레코드가 활성화되면 Adobe은 인증서를 프로비저닝하고 SSL/TLS 인증서를 업로드합니다.

   >[!NOTE]
   >
   >프로덕션 사이트에 apex 도메인(`your-domain.com`)을 사용할 계획이라면 Fastly 서버 IP 주소를 가리키도록 DNS 주소 레코드(A 레코드)를 구성해야 합니다. [프로덕션 설정으로 DNS 구성 업데이트](../launch/checklist.md#to-update-dns-configuration-for-site-launch)를 참조하십시오.


1. 다음과 같이 도메인 유효성 검사 및 프로덕션 SSL/TLS 인증서의 사전 프로비저닝에 대한 ACME 과제 CNAME 레코드를 추가합니다.

   | 도메인 또는 하위 도메인 | CNAME |
   |-------------------------------------------|-------------------------------------------|
   | _acme-challenge.your-domain.com | 0123456789abcdef.validation.magento.cloud |
   | _acme-challenge.www.your-domain.com | 9573186429stuvwx.validation.magento.com |
   | _acme-challenge.mystore.your-domain.com | 1234567898zxywvu.validation.magento.cloud |
   | _acme-challenge.subdomain.your-domain.com | 1098765743lmnopq.validation.magento.cloud |

   >[!NOTE]
   >
   >이 예제의 ACME 챌린지 레코드는 Adobe Commerce 스테이징 및 프로덕션 사이트를 프로비저닝하기 위한 것이 아닌 자리 표시자입니다. Adobe에 연락하여 프로젝트에 대한 올바른 ACME 챌린지 기록 정보를 얻으십시오.

   CNAME 레코드를 추가한 후, Adobe은 도메인의 유효성을 검사하고 환경에 대한 SSL/TLS 인증서를 프로비저닝합니다. 이러한 도메인에서 Fastly 서비스로 트래픽을 라우팅하도록 DNS 구성을 업데이트하면 Adobe이 환경에 인증서를 업로드합니다.

1. Adobe Commerce 기본 URL을 업데이트합니다.

   - SSH를 사용하여 프로덕션 환경에 로그인합니다.

     ```bash
     magento-cloud ssh
     ```

   - Cloud CLI를 사용하여 스토어의 기본 URL을 변경합니다.

     ```bash
     php bin/magento setup:store-config:set --base-url="https://mcstaging.your-domain.com/"
     ```

   >[!NOTE]
   >
   >Cloud CLI를 사용하는 대신 [관리자](https://experienceleague.adobe.com/docs/commerce-admin/stores-sales/site-store/store-urls.html)에서 기본 URL을 업데이트할 수 있습니다

1. 웹 브라우저를 다시 시작합니다.

1. 웹 사이트를 테스트합니다.

## Fastly 캐싱 테스트

DNS 구성 변경을 완료한 후 [cURL](https://curl.se/) 명령줄 도구를 사용하여 Fastly 캐시가 작동하는지 확인하십시오.

**응답 헤더를 확인하려면**:

1. 터미널에서 다음 `curl` 명령을 사용하여 라이브 사이트 URL을 테스트합니다.

   ```bash
   curl -vo /dev/null -H Fastly-Debug:1 https://<live-URL>
   ```

   정적 경로를 설정하지 않았거나 라이브 사이트의 도메인에 대한 DNS 구성을 완료한 경우 DNS 이름 확인을 무시하는 `--resolve` 플래그를 사용합니다.

   ```bash
   curl -vo /dev/null -H Fastly-Debug:1 --resolve <live-URL-hostname>:443:<live-IP-address>
   ```

1. 응답에서 [headers](fastly-troubleshooting.md#check-cache-hit-and-miss-response-headers)을(를) 확인하여 Fastly가 작동하는지 확인하십시오. 응답에 다음의 고유한 헤더가 표시되어야 합니다.

   ```http
   < Fastly-Magento-VCL-Uploaded: yes
   < X-Cache: HIT, MISS
   ```

헤더에 올바른 값이 없으면 [응답 헤더에서 발견된 오류 해결](fastly-troubleshooting.md#curl)에서 문제 해결 도움말을 참조하십시오.

## Fastly 모듈 업그레이드

Fastly는 Magento 2 모듈용 Fastly CDN을 업데이트하여 문제를 해결하고, 성능을 향상시키며, 새로운 기능을 제공합니다.
스테이징 및 프로덕션 환경의 Fastly 모듈을 [최신 버전](https://github.com/fastly/fastly-magento2/blob/master/VERSION)(으)로 업데이트하는 것이 좋습니다.

모듈을 업데이트한 후 VCL 코드를 업로드하여 변경 사항을 Fastly 서비스 구성에 적용해야 합니다.

>[!WARNING]
>
> 사용자 지정 버전으로 기본 Fastly VCL 코드를 사용자 지정한 경우 Fastly 모듈을 업그레이드하면 변경 사항이 덮어쓰여집니다. 고유한 이름의 사용자 지정 VCL 코드 조각을 추가한 경우 이러한 변경 사항은 업그레이드 프로세스 동안 유지됩니다. 프로덕션 환경에 변경 사항을 적용하기 전에 스테이징 환경을 업그레이드하고 변경 사항을 확인하는 것이 좋습니다.

**Magento 2에 대한 Fastly CDN 모듈의 버전을 확인하려면**:

1. 클라우드 환경의 루트 디렉터리로 변경합니다.

1. 작성기를 사용하여 설치된 버전을 확인합니다.

   ```bash
   composer show *fastly*
   ```

1. [최신 릴리스](https://github.com/fastly/fastly-magento2/releases)이 설치되지 않은 경우 Fastly 모듈을 업그레이드하는 단계를 완료하십시오.

**Fastly 모듈을 업그레이드하려면**:

1. 로컬 통합 환경에서 다음 모듈 정보를 사용하여 [Fastly 모듈을 업그레이드](../store/extensions.md#upgrade-an-extension)합니다.

   ```text
   module name: fastly/magento2
   repository: https://github.com/fastly/fastly-magento2.git
   ```

1. 스테이징 환경에 업데이트를 푸시합니다.

1. 스테이징 환경의 Admin에 로그인하여 [VCL 코드를 업로드](#upload-vcl-to-fastly)합니다.

1. Adobe Commerce 스테이징 사이트에서 [Fastly 서비스를 확인](fastly-troubleshooting.md#verify-or-debug-fastly-services)합니다.

스테이징 사이트에서 Fastly 서비스를 확인한 후 프로덕션 환경에서 업그레이드 프로세스를 반복합니다.

>[!TIP]
>
> Adobe Commerce 환경에서 Fastly 서비스에 문제가 있는 경우 [Adobe Commerce Fastly 문제 해결사](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/magento-fastly-troubleshooter.html)를 참조하세요.
