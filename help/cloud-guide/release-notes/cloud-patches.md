---
title: Commerce용 클라우드 패치
description: Cloud Patches 패키지에 대한 최신 개선 사항 목록을 참조하십시오.
recommendations: noDisplay, catalog
last-substantial-update: 2024-04-08T00:00:00Z
exl-id: ae6b511b-a37d-4776-9a5e-ad7d9f9f6611
source-git-commit: d5ab7c4f1d2edbd85eab5a4ca098b3d156e562e5
workflow-type: tm+mt
source-wordcount: '2187'
ht-degree: 0%

---

# Commerce용 클라우드 패치

다음 [클라우드 패치](https://github.com/magento/magento-cloud-patches) 패키지는 모든 Adobe Commerce 버전과 클라우드 환경의 통합을 향상하고 중요한 수정 사항의 빠른 전달을 지원하는 필수 패치 집합을 제공합니다.

Cloud Patches for Commerce 패키지는 ECE-Tools 패키지에 종속되며 ECE-Tools 패키지를 설치하거나 업데이트할 때 설치 및 업데이트됩니다. 또한 Commerce용 클라우드 패치를 독립형 패키지로 사용 및 관리하여 클라우드 플랫폼에 없는 Adobe Commerce 프로젝트에 패치를 적용할 수도 있습니다. 이 릴리스 노트는 이 패키지에 대한 최신 개선 사항을 설명합니다.

>[!TIP]
>
>프로젝트에 필요한 모든 패치가 있는지 확인하려면 [최신 버전의 ece-tools](../dev-tools/update-package.md).

>[!NOTE]
>
>다음을 참조하십시오 [패치 적용](../development/apply-patches.md) 를 참조하십시오.

다음 `magento/magento-cloud-patches` 패키지는 다음 버전 시퀀스를 사용합니다. `<major>.<minor>.<patch>`

<!--Add release notes below-->

## v1.0.26 {#latest}

릴리스 날짜: 2024년 4월 8일

- ![새 아이콘](../../assets/new.svg) **PHP** — PHP 8.3에 대한 지원이 추가되었습니다.

## v1.0.25

릴리스 날짜: 2024년 1월 16일

- **캐시 개선**-이 패치는 레이아웃 캐시 효율성을 향상시켜 Adobe Commerce 버전 2.4.4 이상의 경우 메모리 사용량을 크게 줄입니다.<!-- MCLOUD-11514 -->
- **CRON 작업 개선**-이 패치를 사용하면 누락된 작업이 Adobe Commerce 버전 2.4.4 이상의 cron 작업 잠금을 불필요하게 대기하는 문제가 해결됩니다.<!-- MCLOUD-11329 -->

## v1.0.24

릴리스 날짜: 2023년 9월 15일

- **성능 향상**-이 패치는 Adobe Commerce 2.4.6에 대해 동일한 배포 구성이 로드되는 횟수를 2.4.6-p1로 줄여 성능에 영향을 주는 문제를 해결합니다.<!-- MCLOUD-10604 -->

## v1.0.23

릴리스 날짜: 2023년 7월 31일

- **MCLOUD-10604 패치를 제거했습니다.**-이 패치는 QPT로 이동되었습니다.<!-- MCLOUD-10736 -->

## v1.0.22

릴리스 날짜: 2023년 6월 19일

- **향상된 QPT CLI 마법사/출력**—QPT CLI 마법사/출력에 종속성이 있는 경우 패치 세부 정보 및 요구 사항을 확인하라는 경고가 추가되었습니다.<!-- ACP2E-1963 -->
- **Commerce 2.4.6용 패치가 추가되었습니다.**
   - 을(를) 수정했습니다. `regexp cache tag` 유효성 검사.<!-- MCLOUD-10226 -->
   - 동일한 배포 구성이 로드되는 횟수를 줄여 성능이 향상되었습니다.<!-- MCLOUD-10604 -->
- **2.4.6에 Commerce 2.3.7용 패치 추가**—에 대해 1씩 증가하는 대신 임의의 값으로 증가하는 문제를 해결했습니다. `catalog_product_entity_*` 테이블.<!-- MCLOUD-10032 -->
- **2.4.6에 Commerce 2.4.0용 패치 추가**- 다음을 명시하는 오류를 수정했습니다. `The file can't be deleted. Warning!unlink: No such file or directory`: 관리자에서 JS/CSS 캐시를 플러시할 때 발생합니다.<!-- MCLOUD-10279 -->

## v1.0.21

릴리스 날짜: 2023년 3월 10일

- **PHP 8.2에 대한 향상된 지원**—Commerce 2.4.6을 지원하도록 특정 PHP 8.2.x 버전과 관련된 호환성 문제를 해결했습니다.

## v1.0.20

릴리스 날짜: 2022년 10월 27일

- **L2 캐시 개선 패치 추가**—이 패치는 Commerce 버전 2.4.0 및 2.4.1용 로컬 L2 캐시를 플러시하는 문제를 해결합니다.<!-- MCLOUD-7845 -->

## v1.0.19

릴리스 날짜: 2022년 9월 13일

- **PHP 8.1에 대한 향상된 지원**- 특정 PHP 8.1.x 버전의 호환성 문제가 해결되었습니다.

## v1.0.18

릴리스 날짜: 2022년 8월 11일

Adobe Commerce 2.4.5용 중요 패치:

- **Braintree 결제를 사용한 주문 문제**—이 패치는 관리자가 새 주문 또는 재주문을 할 수 없는 중요한 문제를 해결합니다.<!-- MCLOUD-9137 -->

다음을 참조하십시오 [Braintree 결제가 활성화된 경우 책임자가 주문/재주문을 만들 수 없음](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/admin-cant-create-order-reorder-when-braintree-payment-enabled.html).

## v1.0.17

릴리스 날짜: 2022년 5월 24일

의 보안 패치에 대한 제약 조건 해결 `patches.json` 파일.

## v1.0.16

릴리스 날짜: 2022년 3월 31일

Adobe Commerce 2.3.3-p1 이상 버전용 주요 패치:

패치를 업데이트하여 해결 **중요** 취약성으로 인해 인증되지 않은 원격 코드가 실행됩니다.<!-- MCLOUD-8479 -->

다음을 참조하십시오 [Adobe 보안 게시판 APSB22-12](https://helpx.adobe.com/security/products/magento/apsb22-12.html).

## v1.0.15

릴리스 날짜: 2022년 3월 10일

- **PHP 8.1 지원**- PHP 8.1에 대한 지원을 추가하고 PHP 7.0 및 7.1에 대한 지원을 삭제했습니다.
- **Adobe Commerce 2.3.3용 패치 추가됨**- 제품 페이지에 표시되는 고정 통화.

## v1.0.14

릴리스 날짜: 2022년 2월 13일

Adobe Commerce 2.3.3-p1 이상 버전용 주요 패치:

문제를 해결하기 위해 패치를 추가했습니다. **중요** 취약성으로 인해 인증되지 않은 원격 코드가 실행됩니다.<!-- MCLOUD-8461 -->

다음을 참조하십시오 [Adobe 보안 게시판 APSB22-12](https://helpx.adobe.com/security/products/magento/apsb22-12.html).

## v1.0.13

릴리스 날짜: 2021년 10월 25일

- **모노로그 업데이트**- 필요한 최소 버전을 업데이트했습니다. `monolog` 패키지 대상 `^2.3`.<!-- ACMP-1263 -->
- **호환되지 않는 PHP 메서드**- Adobe Commerce 버전 2.4.3 및 2.3.7-p1에서 호환되지 않는 PHP 방법이 수정되었습니다.<!-- AC-384 -->
- **PHP 오류**- 고정 `PHP error 'Undefined variable: errorMessage' ...` 패치를 적용하는 동안 오류가 발생했습니다.<!-- ACP2E-138 -->

## v1.0.12

릴리스 날짜: 2021년 8월 12일

Adobe Commerce 2.4.3 및 2.3.7-p1용 중요 패치:

- **API 속도 제한 문제**—이 패치는 배열에 있는 항목이 20개가 넘는 요청을 Web API에서 처리하지 못하는 기본 속도 제한을 수정합니다. 이 패치는 속도 제한의 기본값을 높입니다. Adobe Commerce 보기 [2.4.3 릴리스 노트](https://devdocs.magento.com/guides/v2.4/release-notes/commerce-2-4-3.html#apply-mc-43048__set_rate_limits__243patch-to-address-issue-with-api-rate-limiting) 및 [2.3.7 릴리스 노트](https://devdocs.magento.com/guides/v2.3/release-notes/2-3-7-p1.html#apply-mc-43048__set_rate_limits__237-p1patch-to-address-issue-with-api-rate-limiting).<!-- MC-43048 -->

## v1.0.11

릴리스 날짜: 2021년 7월 29일

- **B2B 계층화된 탐색 패치를 적용할 때 발생하는 문제를 해결했습니다**—B2B 계층 탐색 패치를 적용한 고객의 경우 이 수정 사항이 다음을 해결합니다. `Undefined offset` 스토어 보기를 전환한 후 검색 페이지에 표시되는 오류입니다.<!--MCLOUD-5287-->

- **Paypal 체크아웃 패치**—PayPal Express에서 이전에 주문한 가격이 표시되는 Adobe Commerce 2.3.7 문제를 수정합니다.<!--MC-42674-->

- **패치 범주 지원**- 품질 패치에 지정된 패치 범주 및 원본 소스 처리에 대한 지원을 추가했습니다. 범주를 사용하면 필터 및 정렬을 사용하여 다음을 사용할 때 패치를 보다 신속하게 찾을 수 있습니다. [품질 패치 도구](https://github.com/magento/quality-patches) 사이트 전체 분석 도구(SWAT) <!--MC-38577-->

## v1.0.10

릴리스 날짜: 2021년 5월 10일

- **Adobe Commerce 2.3.7과의 호환성**—Adobe Commerce 2.3.7에서 설치에 대한 작성기 종속성 충돌이 해결되었습니다.<!--MC-42131-->
- **번들 패치를 여러 번 적용하여 발생하는 문제를 해결했습니다**—번들 패치(더 이상 사용되지 않는 다른 패치가 포함된 패치)를 두 번 이상 적용하면 포함된 더 이상 사용되지 않는 패키지를 되돌릴 수 있습니다. 이제 모든 패치가 한 번만 적용됩니다. 동일한 패키지를 다시 적용하려고 하면 패치가 이미 적용되었다는 메시지가 표시됩니다.<!--MC-41912-->
- **B2B 계층화된 탐색 패치**—사용자가 B2B 공유 카탈로그를 활성화할 때 계층 탐색 기능이 모든 제품 옵션을 표시하지 못하는 다른 문제를 해결했습니다.<!--MCLOUD-7742-->

## v1.0.9

릴리스 날짜: 2021년 2월 1일

- **B2B 계층화된 탐색 패치**—B2B 공유 카탈로그가 활성화되면 계층 탐색 시 모든 제품 옵션이 표시되지 않는 문제를 해결했습니다.<!--MCLOUD-6923-->
- **PHP 7.4와의 호환성**—PHP 7.4의 클라우드 패치 호환성 문제를 해결했습니다.<!--MCLOUD-7367-->
- **더 이상 사용되지 않는 패치가 표시됨**- 더 이상 사용되지 않는 패치의 전체 내용이 포함된 대체 패치를 적용한 후 더 이상 사용되지 않는 패치가 패치 테이블에 표시되는 클라우드 패치 문제를 해결했습니다. 여러 다른 패치를 결합한 패치를 적용하면 이러한 문제가 발생할 수 있습니다.<!--MC-40626-->
- **패치를 적용할 때 자동 오류 발생**—에서 발생하는 클라우드 패치 문제를 해결했습니다. `git apply` 일부 환경에서 명령이 패치를 자동으로 적용하지 못했습니다.<!--MC-40529-->

## v1.0.8

릴리스 날짜: 2020년 10월 14일

- **magento/magento-cloud-patches 호환성 업데이트**- 를 업데이트했습니다. `symfony` 및 `semver` 의 버전 제한 `composer.json` Adobe Commerce 2.4.1 이상 릴리스와의 호환성을 위한 파일입니다.<!--MCLOUD-7111-->

## v1.0.7

릴리스 날짜: 2020년 10월 14일

- **Adobe Commerce 2.3.0 - 2.3.5, 2.4.0용 Redis 패치**—레벨 2 캐시를 구현할 때 범주에 제품을 추가할 수 있도록 Redis 패치를 업데이트했습니다. <!--MCLOUD-6659-->

- **Braintree VBE 패치**—관리자가 Braintree 결제 보고서를 보려고 할 때 오류가 발생하는 문제를 수정합니다. <!--MCLOUD-6684-->

- 이제 `ece-patches apply` 이 명령은 Unix를 사용합니다. `patch` 호스트 시스템에서 Git을 사용할 수 없는 경우 패치를 적용하는 명령입니다. <!--MCLOUD-7069-->

## v1.0.6

릴리스 날짜:

- **Adobe Commerce 2.3.0 - 2.3.4용 Redis 패치**—통신 최적화 및 성능 향상
   - Redis와 Adobe Commerce 간의 네트워크 전송 크기 감소
   - Redis 로드 및 쓰기 작업에 대한 경합 조건 수정
   - 저장 시 오류를 처리하기 위해 기본 캐시 어댑터를 다시 작성합니다.
   - Redis CPU 사용량 감소<!--MCLOUD-6139-->

- **Adobe Commerce 2.3.0 - 2.3.5용 Redis 패치**—성능 향상 및 오류 수정
   - 무한 잠금을 방지하기 위해 캐시 잠금 구현을 수정합니다
   - 현재 잠금 메커니즘 개선
   - 병렬 요청에서 잠금 해제를 방지하기 위해 서명된 잠금 구현
   - Redis 쓰기 작업 시 발생하는 다음 오류를 수정합니다. `OOM command not allowed when used memory > maxmemory`
   - 다음 방법으로 클린 캐시에 대한 처리 수정 `cat_p` 제품 업데이트 중에 실행되는 태그<!--MCLOUD-6110-->

- 필요한 을 적용할 때 오류가 발생하던 문제를 수정했습니다 `amzn/amazon-pay-module` 이 모듈을 포함하지 않는 Adobe Commerce v2.2.6 또는 2.3.5를 사용하여 클라우드 인프라 프로젝트에서 Adobe Commerce에 패치합니다. 이제 패치 작업 프로세스에서 `amzn/amazon-pay-module` 모듈이 설치되지 않은 경우 패치합니다.<!--MCLOUD-6588-->

## v1.0.5

릴리스 날짜: 2020년 6월 26일

- **Redis 성능 향상**- Adobe Commerce 버전 2.3.3 및 2.3.4에 Redis 최적화 기능을 추가합니다. 이러한 수정 사항은 Adobe Commerce 버전 2.3.5 릴리스에 포함되어 있습니다. 다음을 참조하십시오 [성능 향상](https://devdocs.magento.com/guides/v2.3/release-notes/release-notes-2-3-5-commerce.html#performance-boosts) 다음에서 _Adobe Commerce 2.3.5 릴리스 노트_.<!--MCLOUD-5771-->

- **New Relic 로그 보강**—Commerce 버전 1.0.4의 Cloud Components에 도입된 New Relic 로깅 기능에 대한 개선 사항을 지원하는 데 필요한 Monolog ProcessorInterface를 추가합니다. 이 패치는 Adobe Commerce 2.1.x를 배포하는 데 필요합니다. 패치가 적용되지 않으면 다음 작업 중에 빌드가 실패합니다. `di:compile` 프로세스.<!--MCLOUD-6029-->

## v1.0.4

릴리스 날짜: 2020년 5월 12일

- **Amazon 결제 체크아웃**—고객이 의 결제 방법을 변경할 수 없도록 하는 Amazon Pay 결제 위젯 문제를 수정했습니다. _검토 및 결제_ 체크아웃 프로세스 도중 단계.<!--MCLOUD-5930-->

- **범주 페이지의 제품 표시**—의 카테고리 페이지에 제품이 표시되지 않는 문제를 해결합니다. _모든 페이지 표시_ 보기.<!--MCLOUD-5684-->

- **페이지 빌더 이미지 업로드**—이미지를 이미지 갤러리에 업로드할 때 다음 오류가 발생하는 Page Builder 인터페이스 문제를 수정합니다. `Destination folder is not writable or does not exist`<!--MCLOUD-5837-->

- **불필요한 사이트 맵 생성 경고 표시 안 함**- 사이트 맵 생성 중에 오류가 발생하면 재시도 시도를 추가하고, 오류가 자동으로 복구될 수 있는 경우 고객 이메일 알림을 건너뜁니다.<!--MCLOUD-3025-->

- **사이트 성능 개선**—의 성능 문제를 수정합니다. `Magento\Framework\App\DeploymentConfig\Reader::load` 사이트 성능에 영향을 주는 긴 로드 시간을 주기적으로 경험하는 함수. <!--MCLOUD-5650-->

- 결제 모듈이 있는 경우에만 결제 패치가 적용되도록 결제 방법 패치에 대한 패치 할당이 Magento 기반 패키지(magento/magento2-base) 대신 결제 모듈을 대상으로 업데이트되었습니다.<!--MCLOUD-5666-->

- Magento Open Source과의 호환성을 위해 패치를 업데이트했습니다.<!--MCLOUD-5701-->

## v1.0.3

릴리스 날짜: 2020년 4월 28일

- Adobe Commerce 2.3.5를 지원하는 &quot;FPC가 배포 중 비활성화됩니다&quot; 패치에 대한 수정 사항이 추가되었습니다.

## v1.0.2

릴리스 날짜: 2020년 2월 27일

이번 릴리스에는 다음과 같은 패치 및 주요 수정 사항이 포함되어 있습니다.

- **magento/magento-cloud-patches 호환성 업데이트**

   - 을(를) 업데이트함 `symfony` 및 `semver` 의 버전 제한 `composer.json` Adobe Commerce 2.4 이상 릴리스와의 호환성을 위한 파일입니다.<!--MAGECLOUD-5127-->

   - 의 업데이트된 제한 `composer.json` 과의 호환성을 위해 `ece-tools` 2002.0.22 이상 2002.0.x 릴리스.

- **PayPal Express 체크아웃**—2020년 2월 12일에 게시된 이 패치는 주문에 대한 배송 주소가 배송 페이지의 드롭다운 메뉴에서 선택되지 않고 텍스트 필드에 수동으로 입력된 국가 지역을 지정하는 PayPal Express Checkout을 통해 주문하는 데 영향을 주는 문제를 해결합니다. 패치 다운로드 페이지에서 전체 패치 설명을 참조하십시오.

- **응용 프로그램 배포 수정**—배포 프로세스 중에 전체 페이지 캐시를 비활성화한 문제를 해결하기 위해 패치를 추가했습니다. 이 패치는 Adobe Commerce 2.3.2 이상 릴리스에 적용됩니다.

- **비동기/벌크 API에 대한 범위 매개 변수**- 의 구문 오류를 수정하기 위해 이 패치를 업데이트했습니다. `composer.json` 파일. 이 패치는 Magento Open Source 2.3.1 및 2.3.2에 적용됩니다. 패치 다운로드 페이지에서 전체 패치 설명을 참조하십시오.

## v1.0.1

릴리스 날짜: 2020년 2월 6일

magento/magento-cloud-patches v1.0.1 릴리스의 소프트웨어 다운로드 페이지에서 모든 Magento Open Source 2.x 패치를 포함했습니다. 이전에 프로젝트에 패치를 복사한 경우에는 패치를 제거하여 충돌을 방지하십시오.

이번 릴리스에는 다음과 같은 패치 및 주요 수정 사항이 포함되어 있습니다.

- **크론 교착 상태를 수정하고 크론 잠금을 개선합니다.**—

   - 의 잘못된 상태 값으로 인해 일부 cron 작업이 실행되지 않는 문제를 해결했습니다. `cron_schedule` 테이블. 이제 Adobe Commerce 잠금 프레임워크를 사용하여 를 사용하는 대신 cron 작업 상태를 확인하고 업데이트합니다 `cron_schedule` 테이블. 오류 상태로 끝난 Cron 작업은 24시간 대기하는 대신 다음 Cron 실행 중에 다시 시도됩니다.

   - 를 추가합니다. _다시 시도_ 의 데이터를 업데이트하는 동안 교착 상태를 방지하기 위한 작업 `cron_schedule` 테이블.

- **업데이트됨 `magento/magento-cloud-patches` Magento Open Source 2.x에 사용 가능한 모든 패치를 포함하려면**—소프트웨어 다운로드 페이지에서 사용할 수 있는 모든 Magento Open Source 2.x 패치를 포함하도록 magento/magento-cloud-patches 패키지를 업데이트했습니다. 이전에 Magento Open Source 패치를 Adobe Commerce on cloud infrastructure 프로젝트에 복사한 경우 충돌을 방지하려면 패치를 제거하십시오.<!--MAGECLOUD-4606-->

- **Elasticsearch 카탈로그 페이지 매김 수정** —magento/magento-cloud-patches v1.0에 제공된 Elasticsearch 카탈로그 페이지 매김 패치를 보다 효과적인 수정 사항으로 교체했습니다.<!--MAGECLOUD-4847-->

- **페이지 빌더 패치**—Commerce 1.0.0용 Cloud Patches에서 Page Builder 패치를 번들로 제공하여 알려진 Page Builder RCE(원격 코드 실행) 취약점을 해결하기 위해 Adobe Commerce 2.3.3을 기반으로 초기 수정 작업을 수행했습니다. Adobe Commerce 2.3.4.를 기반으로 하는 보다 안정적인 구현으로 이러한 패치를 업데이트했습니다. 이 패치에는 문제 해결을 위한 여러 최적화가 포함됩니다.<!--MAGECLOUD-4884-->

  magento/magento-cloud-patches 1.0.0 패키지가 있는 경우 여전히 Page Builder RCE 취약성 문제로부터 보호됩니다. 1.0.1 이상으로 업데이트하면 동일한 수정 사항을 더 잘 구현할 수 있습니다.

## v1.0.0

릴리스 날짜: 2019년 11월 14일

의 첫 번째 릴리스입니다. [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches) 패키지: 다음에 대한 새 종속성 `ece-tools` 패키지 버전 2002.0.22 이상 릴리스.

이번 릴리스에는 다음과 같은 패치 및 주요 수정 사항이 포함되어 있습니다.

- **2.3.1.x 및 2.3.2.x 릴리스용 Page Builder 보안 패치**—인증되지 않은 사용자가 네트워크(RCE)를 통해 임의 코드 실행을 트리거하는 데 사용할 수 있는 일부 템플릿 메서드에 액세스하여 전역 정보 누출을 초래하는 Page Builder 미리보기 문제를 해결합니다. 이 문제는 Adobe Commerce 버전 2.3.1 및 2.3.2에서 지원되지 않는 버전의 Page Builder를 사용할 때 발생할 수 있습니다.<!--MAGECLOUD-4649-->

- **MSI 패치**- 재고 관리에 기본 재고 설정을 사용할 때 색인 지정 오류 및 성능 문제가 발생하는 문제를 수정합니다.<!--MAGECLOUD-4428-->

- **새 메일 인터페이스의 이전 버전과의 호환성**-으로 인해 발생하는 이전 버전과의 비호환성 문제를 해결합니다. `Magento\Framework\Mail\EmailMessageInterface` Adobe Commerce v2.3.3에 도입된 PHP 인터페이스. 이 패치의 범위 내에서 `EmailMessageInterface` 이전 상속인 `MessageInterface`, 및 Adobe Commerce 핵심 모듈은 다음에 종속된 상태로 되돌아갑니다. `MessageInterface`.<!--MAGECLOUD-4422-->

- **카탈로그 페이지 매김이 Elasticsearch 6.x에서 작동하지 않습니다**—Elasticsearch 6.x를 카탈로그 검색 엔진으로 사용하는 고객에게 영향을 주는 검색 결과 페이지 매김 의 중요한 문제를 수정합니다.<!--MAGECLOUD-4448-->
