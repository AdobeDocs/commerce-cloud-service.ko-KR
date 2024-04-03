---
title: 캐시 구성 사용자 정의
description: Fastly 서비스 설정이 완료된 후 캐시 구성 설정을 검토하고 사용자 지정하는 방법을 알아봅니다.
feature: Cloud, Configuration, Iaas, Cache
exl-id: f1fc85d4-7867-4bb5-9f11-bc8d2d80383b
source-git-commit: 8a0523f1714b6ea41887e99b5c31294cf5e5255e
workflow-type: tm+mt
source-wordcount: '1808'
ht-degree: 0%

---

# 캐시 구성 사용자 정의

스테이징 및 프로덕션 환경에서 Fastly 서비스를 설정하고 테스트한 후 캐시 구성 설정을 검토하고 사용자 지정합니다. 예를 들어 설정을 업데이트하여 TLS가 HTTP 요청을 Fastly로 리디렉션하도록 하고, 제거 설정을 업데이트하며, 기본 인증을 활성화하여 개발 중에 사이트를 암호로 보호할 수 있습니다.

다음 섹션에서는 일부 캐시 설정을 구성하는 방법에 대한 개요와 지침을 제공합니다. 에서 사용 가능한 구성 옵션에 대한 추가 정보를 확인하십시오. [Magento 2용 Fastly CDN 모듈](https://github.com/fastly/fastly-magento2/tree/master/Documentation) 설명서를 참조하십시오.

## TLS 강제 실행

Fastly는 _TLS 강제 실행_ 암호화되지 않은 요청(HTTP)을 Fastly로 리디렉션하는 옵션입니다. 스테이징 또는 프로덕션 환경이 [유효한 SSL/TLS 인증서](fastly-configuration.md#provision-ssltls-certificates), 저장소에 대한 Fastly 구성을 업데이트하여 TLS 강제 실행 옵션을 활성화할 수 있습니다. Fastly 보기 [TLS 강제 실행 안내서](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/FORCE-TLS.md) 다음에서 _Magento 2용 Fastly CDN 모듈_ 설명서를 참조하십시오.

>[!NOTE]
>
>TLS 강제 실행 옵션을 활성화하는 것은 클라우드 인프라 스토어의 Adobe Commerce에 권장되는 모범 사례입니다.

## 빠른 시간 제한 확장

Fastly 서비스 구성은 관리자에 대한 HTTPS 요청에 대한 기본 시간 제한 기간을 180초로 지정합니다. 시간 제한 기간을 초과하는 요청 처리는 503 오류를 반환합니다. 따라서 처리 시간이 오래 걸리는 요청에 응답하거나 대량 작업을 수행하려고 할 때 503 오류를 수신할 수 있습니다.

3분 이상 걸리는 일괄 작업을 완료하려면 _관리 경로 시간 제한_ value_를 입력하여 503 오류를 방지합니다.

>[!NOTE]
>
>Fastly UI에서 관리자 이외의 항목에 대한 Fastly 시간 초과 매개 변수를 확장하려면 다음을 참조하십시오. [긴 작업에 대한 시간 초과 증가](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/Edge-Modules/EDGE-MODULE-INCREASE-TIMEOUTS-LONG-JOBS.md).

**관리자의 Fastly 시간 제한을 확장하려면**:

{{admin-login-step}}

1. 클릭 **스토어** > 설정 > **구성** > **고급** > **시스템** 및 확장 **전체 페이지 캐시**.

1. 다음에서 _Fastly 구성_ 섹션, 확장 **고급 구성**.

1. 설정 **관리 경로 시간 제한** 값(초). 이 값은 10분(600초)을 초과할 수 없습니다.

1. 클릭 **구성 저장** 을 클릭합니다.

1. 페이지를 다시 로드한 후 을 선택합니다. **VCL을 Fastly에 업로드** 다음에서 _Fastly 구성_ 섹션.

에서 VCL 파일을 생성하기 위한 관리 경로를 빠르게 검색합니다. `app/etc/env.php` 구성 파일입니다.

## 제거 옵션 구성

Fastly는 제품 범주, 제품 에셋 및 콘텐츠를 제거하는 옵션을 포함하여 Magento 캐시 관리 페이지에 여러 유형의 제거 옵션을 제공합니다. 활성화되면 Fastly는 이벤트가 해당 캐시를 자동으로 제거하는지 감시합니다. 제거 옵션을 비활성화하면 [캐시 관리] 페이지를 통해 업데이트를 완료한 후 Fastly 캐시를 수동으로 제거할 수 있습니다.

지우기 옵션은 다음과 같습니다.

- **범주 제거**- 단일 제품을 추가하고 업데이트할 때 제품 카테고리 컨텐츠(제품 컨텐츠가 아님)를 삭제합니다. 제품 및 제품 범주를 제거하는 제품 제거를 비활성화하고 활성화할 수 있습니다.
- **제품 제거**- 제품에 대한 단일 수정 사항을 저장할 때 모든 제품 및 제품 범주 컨텐츠를 제거합니다. 제품 삭제를 활성화하면 가격을 변경하거나 제품 옵션을 추가하거나 제품 재고가 품절되었을 때 고객에게 즉시 업데이트를 제공하는 데 도움이 될 수 있습니다.
- **CMS 제거 페이지**- Adobe Commerce CMS에 페이지를 업데이트하고 추가할 때 페이지 콘텐츠를 삭제합니다. 예를 들어 약관 또는 반품 정책을 업데이트할 때 삭제할 수 있습니다. 이러한 변경 작업을 거의 수행하지 않는 경우 자동 제거를 비활성화할 수 있습니다.
- **소프트 삭제**-변경된 콘텐츠를 부실 시간으로 설정하고 부실 시기에 따라 삭제합니다. 부실 시간 외에도 고객은 부실 콘텐츠를 제공받는 반면 백그라운드에서 콘텐츠는 빠르게 업데이트됩니다.

![제거 옵션 구성](../../assets/cdn/fastly-purge-options.png)

**빠른 제거 옵션을 구성하려면**:

1. 다음에서 _Fastly 구성_ 섹션, 확장 **고급 구성** 삭제 옵션을 표시합니다.

1. 각 삭제 옵션에 대해 다음을 선택합니다. **예** 자동 삭제를 사용하려면 **아니요** 자동 삭제를 비활성화합니다.

   제거 옵션을 비활성화하는 경우, 다음에서 해당 범주에 대한 캐시를 수동으로 제거해야 합니다 _캐시 관리_ 페이지를 가리키도록 업데이트하는 중입니다.

1. 클릭 **구성 저장** 을 클릭합니다.

1. 페이지를 다시 로드한 후 을 선택합니다. **VCL을 Fastly에 업로드** 다음에서 _Fastly 구성_ 섹션.

자세한 내용은 [Fastly 구성 옵션](https://github.com/fastly/fastly-magento2/blob/21b61c8189971275589219d418332798efc7db41/Documentation/CONFIGURATION.md#further-configuration-options).

## GeoIP 처리 구성

Fastly 모듈에는 방문자를 자동으로 리디렉션하거나 획득한 국가 코드와 일치하는 스토어 목록을 제공하는 GeoIP 처리가 포함되어 있습니다. GeoIP 처리를 위해 이미 확장을 사용하는 경우 Fastly 옵션을 사용하여 기능을 확인해야 할 수 있습니다.

**GeoIp 처리를 설정하려면**:

{{admin-login-step}}

1. 클릭 **스토어** > 설정 > **구성** > **고급** > **시스템** 및 확장 **전체 페이지 캐시**.

1. 다음에서 _Fastly 구성_ 섹션, 확장 **고급 구성**.

1. 아래로 스크롤하여 선택 **예** 끝 **GeoIP 활성화**. 추가 구성 옵션이 표시됩니다.

1. GeoIP Action의 경우 방문자가 자동으로 로 리디렉션되는 경우 을 선택합니다. **리디렉션** 또는 선택할 저장소 목록을 제공했습니다. **대화 상자**.

1. 대상 **국가 매핑**, 선택 **추가** 목록에서 특정 Adobe Commerce 스토어와 매핑할 두 글자로 된 국가 코드를 입력합니다.

   ![GeoIP 국가 맵 추가](/help/assets/cdn/fastly-geo-code.png)

1. 클릭 **구성 저장** 을 클릭합니다.

1. 페이지를 다시 로드한 후 다음을 선택합니다. **VCL을 Fastly에 업로드** 다음에서 _Fastly 구성_ 섹션.

>[!NOTE]
>
>현재 Adobe Commerce Fastly GeoIP 모듈 구현은 여러 웹 사이트 간 리디렉션을 지원하지 않습니다.

Fastly는 또한 [지리적 위치 관련 VCL 기능](https://developer.fastly.com/reference/vcl/variables/geolocation/) 사용자 지정된 지리적 위치 코딩용.

## Fastly Edge 모듈 활성화

Fastly Edge Modules는 템플릿을 통해 UI 구성 요소 및 관련 VCL 코드를 정의할 수 있는 유연한 프레임워크입니다. 이러한 모듈을 사용하면 사용자 지정 VCL 코드 조각을 사용하는 대신 사용자 인터페이스를 통해 Fastly 서비스 구성을 손쉽게 맞춤화하고 확장할 수 있습니다.

Edge 모듈을 사용하면 CORS 헤더, 클라우드 사이트맵 재작성과 같은 특정 기능을 활성화하고 Adobe Commerce 스토어와 다른 CMS 또는 백엔드 간의 통합을 구성할 수 있습니다.

에지 모듈 메뉴에 액세스하여 사용 가능한 모듈을 보고, 구성하고, 관리하려면 _Fastly Edge 모듈 활성화_ 옵션을 선택합니다. 다음을 참조하십시오 [Fastly Edge 모듈](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/Edge-Modules/EDGE-MODULES.md) Fastly CDN 모듈 설명서.

## 백엔드 및 원본 차폐 구성

백엔드 설정은 원본 차폐 및 시간 초과와 함께 Fastly 성능을 위한 미세 조정을 제공합니다. A _백엔드_ 는 캐시된 콘텐츠를 확인 및 제공하기 위해 원본 실드 및 시간 제한 설정이 구성된 특정 위치(IP 또는 도메인)입니다.

_원점 차폐_ 저장소에 대한 모든 요청을 특정 POP(Point of Presence)로 보냅니다. 요청을 받으면 POP는 캐시된 콘텐츠를 확인하고 제공합니다. 캐시되지 않은 경우 Shield POP로 이동한 다음 콘텐츠를 캐시하는 Origin 서버로 이동합니다. 방패는 출발지까지 직접 교통량을 줄인다.

기본 Fastly VCL 코드는 클라우드 인프라 사이트에서 Adobe Commerce의 원본 차폐 및 시간 초과에 대한 기본값을 지정합니다. 경우에 따라 기본값을 수정해야 할 수도 있습니다. 예를 들어, 첫 번째 바이트(TTFB) 오류에 시간을 적용하는 경우 _첫 번째 바이트 시간 제한_ 값.

>[!NOTE]
>
>사이트에 다음과 같은 백엔드 통합을 통해 기능적으로 제공되어야 하는 경우 [워드프레스](fastly-vcl-wordpress.md)를 통해 Fastly 서비스 구성을 사용자 정의하여 백엔드를 추가하고 Adobe Commerce 스토어에서 Wordpress로의 리디렉션을 관리합니다. 자세한 내용은 [Fastly Edge 모듈 - 기타 CMS/백엔드 통합](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/Edge-Modules/EDGE-MODULE-OTHER-CMS-INTEGRATION.md) Fastly 모듈 설명서에서 확인할 수 있습니다.

**백엔드 설정 구성을 검토하려면**:

{{admin-login-step}}

1. 클릭 **스토어** > 설정 > **구성** > **고급** > **시스템** 및 확장 **전체 페이지 캐시**.

1. 확장 **Fastly 구성** 섹션.

1. 확장 **백엔드 설정** 기본 백 엔드를 확인할 기어를 선택합니다. 현재 설정을 변경할 수 있는 옵션이 있는 모달이 열립니다.

   ![백 엔드 수정](../../assets/cdn/fastly-backend.png)

1. 다음 항목 선택 **방패** 위치(또는 데이터 센터).

   프로젝트에 대한 기본 Fastly 구성은 클라우드 서비스 지역에 가장 가까운 위치를 설정합니다. 변경해야 하는 경우 기본 위치에 가까운 위치를 선택합니다.

1. 실드 연결에 대한 시간 제한 값(마이크로초), 바이트 간 시간 및 첫 번째 바이트에 대한 시간을 수정합니다. 기본 시간 초과 설정을 유지하는 것이 좋습니다.

1. 필요한 경우 다음을 선택합니다. **편집 또는 저장 후 백엔드 및 실드 활성화**.

1. 클릭 **업로드** 변경 사항을 저장하고 Fastly 서버에 업로드합니다.

1. 관리에서 을(를) 선택합니다. **구성 저장**.

자세한 내용은 [백엔드 설정 안내서](https://github.com/fastly/fastly-magento2/blob/21b61c8189971275589219d418332798efc7db41/Documentation/Guides/BACKEND-SETTINGS.md) Fastly 모듈 설명서에서 확인할 수 있습니다.

## 기본 인증

기본 인증은 사용자 이름과 암호로 사이트의 모든 페이지와 자산을 보호하는 기능입니다. We **추천하지 않음** 프로덕션 환경에서 기본 인증 활성화. 스테이징에서 구성 하여 개발 프로세스 중에 사이트를 보호할 수 있습니다. 다음을 참조하십시오. [기본 인증 안내서](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/BASIC-AUTH.md) Fastly CDN 모듈 설명서.

스테이징에서 사용자 액세스를 추가하고 기본 인증을 활성화해도 추가 자격 증명을 요구하지 않고 관리자에 액세스할 수 있습니다.

## 사용자 지정 VCL 코드 조각 만들기

Fastly는 사용자 지정된 버전의 VCL(Varnish Configuration Language)을 지원하여 Fastly 서비스 구성을 사용자 지정합니다. 예를 들어 Edge 및 ACL(액세스 제어 목록) 사전이 있는 VCL 코드 블록을 사용하여 특정 사용자 또는 IP 주소에 대한 액세스를 허용, 차단 또는 리디렉션할 수 있습니다.

사용자 지정 VCL 코드 조각, Edge 사전 및 ACL을 만드는 방법은 다음을 참조하십시오. [사용자 정의 Fastly VCL 스니펫](fastly-vcl-custom-snippets.md).

>[!NOTE]
>
>사용자 지정 VCL 코드, Edge 사전 및 ACL을 Fastly 모듈 구성에 추가하기 전에 Fastly 캐싱 서비스가 기본 구성에서 작동하는지 확인하십시오. 다음을 참조하십시오 [Fastly 설정](fastly-configuration.md).

## 도메인 관리

Starter 및 Pro 프로젝트 모두에 대해 [!UICONTROL Domains] 저장소에 대한 Fastly 도메인 구성을 추가하고 관리하는 옵션입니다.

- 시작 프로젝트의 경우 아래의 프로젝트 URL로 이동합니다. [!UICONTROL Domains] 의 탭 [!DNL Cloud Console] 을 클릭하여 프로젝트 URL을 추가합니다.

- Pro 프로젝트의 경우 [Adobe Commerce 지원 티켓](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) 을 클릭하여 도메인을 클라우드 프로젝트 구성에 추가합니다. 또한 지원 팀은 Adobe Commerce Fastly 계정 구성을 업데이트하여 도메인을 추가합니다.

**관리자로부터 Fastly 도메인 구성을 관리하려면**:

{{admin-login-step}}

1. 선택 **스토어** > 설정 > **구성** > **고급** > **시스템** 및 확장 **전체 페이지 캐시**.

1. 관리자 _Fastly 구성_ 섹션, 선택 **도메인**.

1. 클릭 **도메인 관리** 을 클릭하여 도메인 페이지를 엽니다.

1. 클라우드 환경의 저장소에 대한 최상위 수준 및 하위 도메인 이름을 추가합니다.

   클라우드 인프라 구성에 이미 추가된 도메인만 지정할 수 있습니다.

   ![Starter용 Fastly 도메인 구성 추가](../../assets/cdn/fastly-starter-activate-domain.png)

1. 클릭 **활성화** Fastly 도메인 구성을 업데이트합니다.

>[!NOTE]
>
>동일한 도메인이 다른 Fastly 계정에 구성된 경우 Adobe Commerce 지원 티켓을 제출하여 도메인 위임을 요청해야 Adobe Commerce에 도메인을 추가할 수 있습니다. 다음을 참조하십시오 [여러 Fastly 계정 및 할당된 도메인](fastly.md#multiple-fastly-accounts-and-assigned-domains).

## 유지 관리 모드 활성화

사용 _유지 관리 모드_ 다른 모든 요청에 대한 오류 페이지를 반환하면서 지정된 IP 주소에서 사이트에 대한 관리 액세스를 허용하는 옵션입니다.

**관리 액세스 권한으로 유지 관리 모드를 활성화하려면**:

1. 를 엽니다. _Fastly 구성_ 섹션에 있는 마지막 항목이 될 필요가 없습니다.

1. 다음에서 _Edge ACL_ 섹션, 업데이트 `maint_allow` 유지 관리 모드에 있는 동안 저장소에 액세스할 수 있는 관리 IP 주소가 있는 ACL(액세스 제어 목록)입니다.

   ![IP 유지 관리 모드 허용 목록 업데이트](../../assets/cdn/fastly-maint-allowlist.png)

1. 다음에서 _유지 관리 모드_ 섹션, 선택 **유지 관리 모드 활성화**.

   유지 관리 모드를 활성화하면 의 IP 주소에서 요청을 제외한 모든 트래픽이 차단됩니다. `maint_allowlist` ACL. 다음을 업데이트할 수 있습니다. `maint_allowlist` : ACL에서 IP 주소를 변경합니다.

   자세한 구성 지침은 [유지 관리 모드 안내서](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/MAINTENANCE-MODE.md) Fastly CDN for Magento 2 모듈 설명서.
