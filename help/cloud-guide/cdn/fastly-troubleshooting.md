---
title: Fastly 문제 해결
description: Adobe Commerce용 Fastly CDN 모듈 및 서비스의 문제를 해결하고 관리하는 방법에 대해 알아봅니다.
feature: Cloud, Configuration, Cache, Services
exl-id: e4c47035-cbad-4838-8d44-fa5eaaac42d1
source-git-commit: 1253d8357fd2554050d1775fefbc420a2097db5f
workflow-type: tm+mt
source-wordcount: '1834'
ht-degree: 0%

---

# Fastly 문제 해결

다음 정보를 사용하여 클라우드 인프라 프로젝트 환경의 Adobe Commerce에서 Magento 2에 대한 Fastly CDN 모듈의 문제를 해결하고 관리하십시오. 예를 들어 응답 헤더 값과 캐싱 동작을 조사하여 Fastly 서비스 및 성능 문제를 해결할 수 있습니다.

Pro 프로덕션 및 스테이징 환경에서 다음을 사용할 수 있습니다 [New Relic 로그](../monitor/log-management.md) Fastly CDN 및 WAF 로그 데이터를 보고 분석하여 오류 및 성능 문제를 해결합니다.

>[!NOTE]
>
>Fastly 설정 및 구성에 대한 자세한 내용은 [Fastly 설정](fastly.md).

## Fastly 서비스 ID 찾기

관리자에서 Fastly를 구성하거나 고급 Fastly 구성 및 문제 해결을 위해 Fastly API 요청을 제출하려면 Fastly 서비스 ID가 필요합니다.

프로젝트 환경에서 Fastly가 활성화된 경우 관리자로부터 서비스 ID를 가져올 수 있습니다. 다음을 참조하십시오 [Fastly 자격 증명 가져오기](fastly-configuration.md#get-fastly-credentials).

개발자와 고급 VCL 사용자는 사용자 지정 VCL을 사용하여 Fastly 변수를 사용하여 서비스 ID를 검색할 수 있습니다 `req.service_id`. 예를 들어 `req.service_id` 를 VCL의 사용자 지정 logging 지시문에 추가하여 서비스 ID 값을 캡처합니다.

```json
log {"syslog"} req.service_id {" my_logging_endpoint_name :: "}
```

프로덕션 및 스테이징 환경에 동일한 VCL을 사용할 수 있습니다. 다음을 참조하십시오 [vcl_log 구성 방법](https://support.fastly.com/hc/en-us/community/posts/360040447172-How-to-configure-vcl-log).

## 사이트 성능, 제거 및 캐시 문제

다음 목록을 사용하여 클라우드 인프라 환경의 Adobe Commerce에 대한 Fastly 서비스 구성과 관련된 문제를 식별하고 해결하십시오.

- **스토어 메뉴가 표시되지 않거나 작동하지 않음**- 라이브 사이트 URL을 사용하는 대신 원본 서버에 직접 연결되는 링크 또는 임시 링크를 사용하거나 `-H "host:URL"` 다음 기간: [cURL 명령](#check-live-site-through-fastly). 원본 서버로 Fastly를 무시하면 기본 메뉴가 작동하지 않고 브라우저측에서 캐싱을 허용하는 잘못된 헤더가 표시됩니다.

- **위쪽 탐색이 작동하지 않음**- 위쪽 탐색은 기본 Magento Fastly VCL 코드 조각을 업로드할 때 활성화되는 ESI(Edge Side Includes) 처리를 사용합니다. 내비게이션이 작동하지 않으면, [Fastly VCL 업로드](fastly-configuration.md#upload-vcl-to-fastly) 사이트를 다시 확인하십시오.

- **지리적 위치/지리적 IP가 작동하지 않음**— 기본 Magento Fastly VCL 코드 조각은 URL에 국가 코드를 추가합니다. 국가 코드가 작동하지 않으면, [Fastly VCL 업로드](fastly-configuration.md#upload-vcl-to-fastly) 사이트를 다시 확인하십시오.

- **페이지가 캐싱하지 않음**- 기본적으로 Fastly는 `Set-Cookies` 머리글입니다. Adobe Commerce은 캐시 가능한 페이지에서도 쿠키를 설정합니다(TTL > 0). 기본 Magento Fastly VCL은 캐시 가능한 페이지에서 이러한 쿠키를 제거합니다. 페이지가 캐싱하지 않는 경우 [Fastly VCL 업로드](fastly-configuration.md#upload-vcl-to-fastly) 사이트를 다시 확인하십시오.

  이 문제는 템플릿의 페이지 블록이 캐시 불가로 표시된 경우에도 발생할 수 있습니다. 이 경우 문제는 서드파티 모듈 또는 확장이 Adobe Commerce 헤더를 차단하거나 제거하여 발생할 수 있습니다. 문제를 해결하려면 다음을 참조하십시오. [X-Cache에 MISS만 포함되고, 히트는 없습니다.](#x-cache-contains-only-miss-no-hit).

- **제거 요청이 실패했습니다.**- 삭제 요청을 실행할 때 다음 오류를 즉시 반환합니다.

  ```text
  The purge request was not processed successfully.
  ```

  이 문제는 다음 문제 중 하나로 인해 발생할 수 있습니다.

   - 클라우드 인프라 프로젝트 환경의 Adobe Commerce에 대한 Fastly 서비스 구성에서 잘못된 Fastly 자격 증명
   - 사용자 지정 VCL 코드 조각의 잘못된 코드

  문제를 해결하려면 다음을 참조하십시오. [클라우드에서 Fastly 캐시를 제거하는 도중 오류 발생](https://support.magento.com/hc/en-us/articles/115001853194-Error-purging-Fastly-cache-on-Cloud-The-purge-request-was-not-processed-successfully-) Adobe Commerce 도움말 센터

## Fastly에서 503 오류 발생

Fastly가 503 시간 초과 오류를 반환하는 경우 오류 로그와 503 오류 페이지를 확인하여 근본 원인을 파악하십시오.

>[!NOTE]
>
>대량 작업을 실행할 때 시간 초과가 발생하는 경우 [관리자의 Fastly 시간 제한 확장](fastly-custom-cache-configuration.md#extend-fastly-timeout).

503 오류가 발생하면 프로덕션 또는 스테이징 환경 오류 로그와 php 액세스 로그를 확인하여 문제를 해결하십시오.

**오류 로그를 확인하려면**:

- [오류 로그](../test/log-locations.md#application-logs)

  ```text
  /var/log/platform/<project-ID>/error.log
  ```

  이 로그에는 예를 들어 응용 프로그램 또는 PHP 엔진에서 발생하는 모든 오류가 포함됩니다. `memory_limit` 또는 `max_execution_time exceeded` 오류. Fastly와 관련된 오류를 찾지 못한 경우 PHP 액세스 로그를 확인하십시오.

- PHP 액세스 로그

  ```text
  /var/log/platform/<project-ID>/php.access.log
  ```

  로그에서 503 오류를 반환한 URL에 대한 HTTP 200 응답을 검색합니다. 200 응답을 찾으면 Adobe Commerce이 오류 없이 페이지를 반환했음을 의미합니다. 즉, 다음 범위를 초과하는 간격 후에 문제가 발생했을 수 있습니다. `first_byte_timeout` fastly 서비스 구성에 설정된 값입니다.

503 오류가 발생하면 Fastly는 오류 및 유지 관리 페이지에서 이유를 반환합니다. 에 대한 코드를 추가한 경우 이유를 보지 못할 수 있습니다. [사용자 지정 응답 페이지](fastly-custom-response.md). 기본 오류 페이지에서 원인 코드를 보려면 사용자 지정 오류 페이지의 HTML 코드를 제거할 수 있습니다.

**Fastly 503 오류 페이지를 확인하려면**:

{{admin-login-step}}

1. 클릭 **스토어** > **설정** > **구성** > **고급** > **시스템**.

1. 오른쪽 창에서 를 확장합니다. **전체 페이지 캐시**.

1. 다음에서 **Fastly 구성** 섹션, 확장 **사용자 지정 합성 페이지** 다음 그림과 같이.

   ![사용자 지정 503 오류 페이지](../../assets/cdn/fastly-custom-synthetic-pages-edit-html.png)

1. 클릭 **HTML 설정**.

1. 사용자 지정 코드를 제거합니다. 텍스트 프로그램에 저장하여 나중에 다시 추가할 수 있습니다.

1. 클릭 **업로드** Fastly에 업데이트를 보냅니다.

1. 클릭 **구성 저장** 을 클릭합니다.

1. 503 오류의 원인이 된 URL을 다시 엽니다. Fastly는 다음 예제와 같이 이유가 있는 오류 페이지를 반환합니다.

   ![fastly 오류](../../assets/cdn/fastly-503-example.png)

## Fastly 계정과 이미 연결된 Apex 및 하위 도메인

클라우드 인프라 프로젝트의 Adobe Commerce에 대한 Apex 도메인 및 하위 도메인이 할당된 서비스 ID를 가진 기존 Fastly 계정과 이미 연결되어 있는 경우 Fastly 구성을 업데이트할 때까지 를 시작할 수 없습니다.

- 기존 Fastly 계정에서 Apex 및 하위 도메인 구성을 업데이트합니다. 다음을 참조하십시오 [여러 Fastly 계정 및 할당된 도메인](fastly.md#domain).

- [Fastly 활성화 및 구성](fastly-configuration.md#enable-fastly-caching) 다음을 완료합니다. [DNS 구성](../launch/checklist.md#update-dns-configuration-with-production-settings)

## Fastly 서비스 확인 또는 디버그

사이트 URL을 테스트하고 응답에서 반환된 헤더 값을 검사하여 클라우드 인프라 사이트의 Adobe Commerce에 대한 성능 또는 캐싱 문제를 해결할 수 있습니다.

### Fastly를 통해 라이브 사이트 확인

Fastly API를 사용하여  `Fastly-Magento-VCL-Uploaded` 및 `X-Cache` 라이브 사이트에서 반환된 응답 헤더입니다.

Fastly API 요청은 Fastly 확장을 통해 전달되어 원본 서버에서 응답을 가져옵니다. 응답이 잘못된 헤더를 반환하는 경우 [원본 서버 직접](#bypass-fastly-cache-to-check-adobe-commerce-sites).

**응답 헤더를 확인하려면**:

1. 터미널에서 다음을 사용합니다 `curl` 라이브 사이트 URL을 테스트하는 명령:

   ```bash
   curl https://<live URL> -vo /dev/null -H Fastly-Debug:1
   ```

   정적 경로를 설정하지 않았거나 라이브 사이트의 도메인에 대한 DNS 구성을 완료한 경우 `--resolve` DNS 이름 확인을 우회하는 플래그입니다.

   ```bash
   curl -svo /dev/null --resolve '<your_hostname>:443:<IP-address-of-cache-node>' <https-URL>
   ```

   >[!NOTE]
   >
   >이 명령을 `--resolve` 옵션은 SSL/TLS 인증서를 통해 Fastly로 TLS를 활성화하고 캐시 노드의 IP 주소를 찾아야 합니다.

1. 응답에서 다음을 확인합니다 [헤더](#check-cache-hit-and-miss-response-headers) Fastly가 작동하는지 확인합니다. 응답에 다음의 고유한 헤더가 표시되어야 합니다.

   ```http
   < Fastly-Magento-VCL-Uploaded: yes
   < X-Cache: HIT, MISS
   ```

헤더에 올바른 값이 없으면 다음 정보를 참조하십시오.

- [VCL 업로드 확인](#fastly-vcl-has-not-been-uploaded)

- [X-Cache에 MISS만 포함되고, 히트는 없습니다.](#x-cache-contains-only-miss-no-hit)

### Adobe Commerce 사이트를 확인하기 위해 Fastly 캐시 우회

Fastly 서비스가 잘못된 헤더를 반환하는 경우 Fastly 캐시를 우회하는 요청을 제출할 수 있도록 해 주는 VCL 코드 조각을 만들 수 있습니다. 다음을 참조하십시오 [Fastly 캐시 우회](fastly-vcl-bypass-to-origin.md).

VCL 코드 조각을 추가한 후 cURL 명령을 사용하여 지정된 IP 주소에서 원천 서버에 요청을 제출합니다. 그런 다음 응답에 오류가 있는지 확인합니다.

### 캐시 적중 및 실패 응답 헤더 확인

반환된 응답에 다음 정보가 포함되어 있는지 확인합니다.

- 다음을 포함합니다. `X-Magento-Tags` 머리글

- 값 `Fastly-Module-Enabled` 머리글은 다음 중 하나입니다. `Yes` 또는 프로젝트 환경에 설치된 CDN Magento 2 모듈용 Fastly의 버전 번호입니다

- [Cache-Control: max-age](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) 이(가) 0보다 큼

- [Pragma](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.32) 설정: `cache`

cURL 명령 출력에서 발췌한 다음 내용은 `Pragma`, `X-Magento-Tags`, 및 `Fastly-Module-Enabled` 헤더:

```terminal
* STATE: INIT => CONNECT handle 0x600057800; line 1402 (connection #-5000)
* Rebuilt URL to: https://www.mymagento.biz.c.sv7gVom4qrpek.ent.magento.cloud/
* Added connection 0. The cache now contains 1 members
* Trying 192.0.2.31...
* STATE: CONNECT => WAITCONNECT handle 0x600057800; line 1455 (connection #0)

% Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0* Connected to www.mymagento.biz.c.sv7gVom4qrpek.ent.magento.cloud (54.229.163.31) port 443 (#0)

* STATE: WAITCONNECT => SENDPROTOCONNECT handle 0x600057800; line 1562 (connection #0)
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0* ALPN, offering h2

... portion omitted for brevity ...

< Set-Cookie: mage-messages=%5B%5D; expires=Wed, 22-Nov-2017 17:39:58 GMT; Max-Age=31536000; path=/
< Pragma: cache
< Expires: Wed, 23 Nov 2016 17:39:56 GMT
< Cache-Control: max-age=86400, public, s-maxage=86400, stale-if-error=5, stale-while-revalidate=5
< X-Magento-Tags: cb_welcome_popup store cb cb_store_info_mobile cb_header_promotional_bar cb_store_info cb_discount-promo-bar cpg_2 cb_83 cb_81 cb_84 cb_85 cb_86 cb_87 cb_88 cb_89 p5646 catalog_product p5915 p6040 p6197 p6227 p7095 p6109 p6122 p6331 p7592 p7651 p7690
< Fastly-Module-Enabled: yes
< Strict-Transport-Security: max-age=31536000
    < Content-Security-Policy: upgrade-insecure-requests
    < X-Content-Type-Options: nosniff
    < X-XSS-Protection: 1; mode=block
    < X-Frame-Options: SAMEORIGIN
    < X-Platform-Server: i-dff64b52
    <
    * STATE: PERFORM => DONE handle 0x600057800; line 1955 (connection #0)
    * multi_done
      0     0    0     0    0     0      0      0 --:--:--  0:00:02 --:--:--     0
    * Connection #0 to host www.mymagento.biz.c.sv7gVom4qrpek.ent.magento.cloud left intact
```

>[!NOTE]
>
>히트 및 누락에 대한 자세한 내용은 다음을 참조하십시오. [보호 서비스를 통한 캐시 히트 및 MISS 헤더 이해](https://docs.fastly.com/guides/performance-tuning/understanding-cache-hit-and-miss-headers-with-shielded-services) Fastly 설명서

### 응답 헤더에서 발견된 오류 해결

이 섹션에서는 Fastly API를 사용하여 응답 헤더를 확인할 때 반환되는 오류를 해결하기 위한 제안 사항을 제공합니다.

#### Fastly 모듈이 활성화되지 않음

Fastly 모듈이 활성화되지 않은 경우 (`Fastly-Module-Enabled: no`) 또는 헤더가 누락된 경우 [ssh를 사용하여 로그인](../development/secure-connections.md#connect-to-a-remote-environment) 프로젝트에. 그런 다음 다음 다음 명령을 실행하여 모듈 상태를 확인합니다.

```bash
php bin/magento module:status Fastly_Cdn
```

반환된 상태에 따라 다음 지침에 따라 Fastly 구성을 업데이트합니다.

- `Module does not exist`- 모듈이 없는 경우 [설치 및 구성](https://github.com/fastly/fastly-magento2/blob/master/Documentation/INSTALLATION.md) 통합 분기의 Magento 2에 대한 Fastly CDN 모듈. 설치가 완료되면 모듈을 활성화하고 구성합니다. 다음을 참조하십시오 [Fastly 설정](fastly-configuration.md).

- `Module is disabled`- Fastly 모듈이 비활성화된 경우 `integration` 로컬 환경에서 분기하여 활성화합니다. 그런 다음 변경 사항을 스테이징 및 프로덕션에 푸시합니다. 다음을 참조하십시오 [확장 관리](../store/extensions.md#install-an-extension).

  를 사용하는 경우 [구성 관리](../store/store-settings.md#configure-store)에서 Fastly CDN 모듈 상태를 확인합니다. `app/etc/config.php` 프로덕션 또는 스테이징 환경에 변경 사항을 푸시하기 전에 구성 파일입니다.

  모듈이 활성화되지 않은 경우(`Fastly_CDN => 0`)에 있는 `config.php` 파일을 삭제하고 다음 명령을 실행하여 업데이트합니다 `config.php` 최신 구성 설정 사용.

  ```bash
  bin/magento magento-cloud:scd-dump
  ```

#### Fastly VCL이 업로드되지 않았습니다.

Fastly VCL이 업로드되지 않은 경우(`Fastly-Magento-VCL-Uploaded`: `false`), *VCL 업로드* 옵션을 사용하여 업로드하십시오. 다음을 참조하십시오 [Fastly VCL 코드 조각 업로드](fastly-configuration.md#upload-vcl-to-fastly).

#### X-Cache에 MISS만 포함되고, 히트는 없습니다.

다음과 같은 경우 `X-Cache` 헤더 포함 항목 `HIT` (`HIT, HIT` 또는 `HIT, MISS`)를 입력하면 Fastly가 캐시된 콘텐츠를 성공적으로 반환함을 나타냅니다.

다음과 같은 경우 `X-Cache` 헤더: `MISS, MISS` 및 다음을 포함하지 않음 `HIT`를 실행하고 `curl` 다시 명령을 실행하여 캐시에서 페이지가 최근에 삭제되지 않았는지 확인합니다.

동일한 결과를 얻으면 다음을 사용합니다. [`curl` 명령](#check-live-site-through-fastly) 및 확인 [응답 헤더](#check-cache-hit-and-miss-response-headers):

- `Pragma` 은(는) `cache`
- `X-Magento-Tags` 존재함
- `Cache-Control: max-age` 이(가) 0보다 큼

문제가 지속되면 다른 확장이 이러한 헤더를 재설정할 수 있습니다. 모든 확장을 비활성화한 후 다시 활성화하여 스테이징 환경에서 다음 절차를 반복하여 헤더를 재설정하는 확장을 확인합니다. 문제를 일으키는 확장을 식별한 후 프로덕션 환경에서 비활성화해야 합니다.

**응답 헤더를 재설정하는 확장을 식별하려면 다음을 수행합니다.**

{{admin-login-step}}

1. 다음으로 이동 **스토어** > **설정** > **구성** > **고급** > **고급**.

1. 다음에서 *모듈 출력 비활성화* 오른쪽 창에서 모든 확장을 찾아 비활성화합니다.

1. 클릭 **구성 저장**.

1. 클릭 **시스템** > **도구** > **캐시 관리**.

1. 클릭 **Magento 캐시 초기화**.

1. Fastly 헤더에 문제를 일으킬 수 있는 각 확장에 대해 다음 단계를 완료합니다.

   - 한 번에 하나의 확장을 활성화하고 구성을 저장하고 Adobe Commerce 캐시를 플러시합니다.

   - 실행 [`curl` 명령](#check-live-site-through-fastly) 을(를) 확인하려면 [응답 헤더](#check-cache-hit-and-miss-response-headers).

   각 확장에 대해 이 프로세스를 반복합니다. Fastly 응답 헤더가 더 이상 표시되지 않으면 Fastly에 문제를 일으키는 확장을 식별한 것입니다.

Fastly 헤더를 재설정하는 확장을 식별한 후 추가 지원이 필요하면 확장 개발자에게 문의하십시오. 타사 확장이 Fastly 캐싱에서 작동하도록 하는 수정 사항 또는 업데이트를 제공할 수 없습니다.

## Fastly 구성 롤백

사용자 지정 VCL 코드 조각 업데이트 또는 기타 Fastly 구성 변경으로 인해 클라우드 인프라 사이트의 Adobe Commerce에서 오류가 발생하거나 반환되는 경우 Fastly API를 사용하십시오 [활성화](https://docs.fastly.com/api/config#version_0b79ae1ba6aee61d64cc4d43fed1e0d5) 이전 VCL 버전으로 롤백하는 명령입니다. 관리에서 VCL 버전을 롤백할 수 없습니다.

**VCL 버전을 롤백하려면**:

1. 서비스에 사용할 수 있는 VCL 버전 목록을 가져오려면 다음 명령을 실행합니다

   ```bash
   curl -H "Fastly-Key: <FASTLY_API_TOKEN>" -H "Accept: application/json" https://api.fastly.com/service/<FASTLY_SERVICE_ID>/version
   ```

1. 다음 명령을 실행하여 활성 VCL 버전을 지정된 버전으로 변경합니다.

   ```bash
   curl -H "Fastly-Key: <FASTLY_API_TOKEN>" -H "Content-Type: application/x-www-form-urlencoded" -H "Accept: application/json" -X PUT https://api.fastly.com/service/<FASTLY_SERVICE_ID>/version/<VERSION_ID>/activate
   ```

Fastly API를 사용하여 VCL을 검토하고 관리하는 방법에 대한 자세한 내용은 [API를 사용하여 VCL 관리](fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api).
