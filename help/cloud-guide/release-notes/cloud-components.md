---
title: Commerce용 클라우드 구성 요소
description: 클라우드 구성 요소 패키지에 대한 최신 개선 사항 목록을 참조하십시오.
recommendations: noDisplay, catalog
exl-id: b4e2508a-3558-4fa8-bae0-3eb76c7b2775
source-git-commit: 196efa316b9998c1980412ad96577d7ce42d4aec
workflow-type: tm+mt
source-wordcount: '590'
ht-degree: 0%

---

# Commerce용 클라우드 구성 요소

[클라우드 구성 요소](https://github.com/magento/magento-cloud-components) 패키지는 클라우드 인프라에 배포된 사이트에 대해 확장된 Adobe Commerce 핵심 기능을 제공합니다. 이 패키지는 ECE-Tools 패키지에 종속됩니다. 이 릴리스 노트는 [Commerce용 Cloud Tools 제품군](cloud-tools-suite.md)의 구성 요소인 이 패키지에 대한 최신 개선 사항을 설명합니다.

`magento/magento-cloud-components` 패키지에서 다음 버전 시퀀스를 사용합니다. `<major>.<minor>.<patch>`

릴리스 노트는 다음과 같습니다.

- ![새 아이콘](../../assets/new.svg) 새 기능
- ![수정 아이콘](../../assets/fix.svg) 수정 사항 및 개선 사항

<!--Add release notes below-->

## v1.1.0 {#latest}

릴리스 날짜: 2024년 10월 7일

- ![수정 아이콘](../../assets/fix.svg) **리팩터링된 코드**—이전 PHP 버전 7.4, 7.3, 7.2 및 관련 라이브러리에 대한 지원이 제거되었습니다.<!-- MCLOUD-9278 - -->
- ![수정 아이콘](../../assets/fix.svg) **업그레이드된 Monolog 버전**—Monolog 3.6에 대한 지원이 추가되었습니다.<!-- MCLOUD-12855 - -->

## v1.0.14

릴리스 날짜: 2024년 4월 8일

- ![새 아이콘](../../assets/new.svg) **PHP**—PHP 8.3에 대한 지원을 추가했습니다.

## v1.0.13

릴리스 날짜: 2023년 3월 10일

- ![새 아이콘](../../assets/new.svg) **PHP 8.2.3에 대한 향상된 지원**—Commerce 2.4.6을 지원하도록 특정 PHP 8.2.x 버전의 호환성 문제를 해결했습니다.

## v1.0.12

릴리스 날짜: 2022년 9월 13일

- ![수정 아이콘](../../assets/fix.svg) **준비 시 오류**—페이지 가시성이 관리자에서 [**개별적으로 표시되지 않음**](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/data-transfer/data-attributes-product#simple-product-csv-file-structure)(으)로 설정되어 배포 로그에 `ERROR: Warming up failed: <link to page>` 오류가 발생하는 경우 [준비](../environment/variables-post-deploy.md#warm_up_pages)를 시도하는 문제를 해결했습니다.<!-- MCLOUD-9134 -->

## v1.0.11

릴리스 날짜: 2022년 8월 4일

- ![수정 아이콘](../../assets/fix.svg) **Symfony 5.4 호환성에 대한 지원 추가**—Symfony 5.4와의 호환성에 대한 수정 사항<!-- AC-3550 -->

## v1.0.10

릴리스 날짜: 2022년 3월 10일

- ![새 아이콘](../../assets/new.svg) **PHP 8.1 지원**—PHP 8.1에 대한 지원을 추가하고 PHP 7.1에 대한 지원을 삭제했습니다.

## v1.0.9

릴리스 날짜: 2021년 10월 25일

- ![수정 아이콘](../../assets/fix.svg) **모노로그 업데이트**—`monolog` 패키지에 필요한 최소 버전을 `^2.3`(으)로 업데이트했습니다.<!-- ACMP-1263 -->

## v1.0.8

릴리스 날짜: 2021년 7월 29일

- ![수정 아이콘](../../assets/fix.svg) **자동 생성 URL에서 후행 슬래시를 제거함**—캐시 준비 중 생성된 범주 페이지 URL에서 후행 슬래시를 제거했습니다.<!--MCLOUD-7192-->

## v1.0.7

릴리스 날짜: 2020년 9월 9일

- ![새 아이콘](../../assets/new.svg) **로깅 개선**—성능을 향상시키려면 `cache.log` 파일의 크기를 줄이십시오.<!--MCLOUD-6859-->

- ![수정 아이콘](../../assets/fix.svg) 캐시 구성 값에서 `php bin/magento cache:evict` CLI 명령이 실패하는 형식 오류를 해결했습니다.

## v1.0.6

릴리스 날짜: 2020년 8월 5일

- ![새 아이콘](../../assets/new.svg) **Redis 성능 개선**—만료된 Redis 키를 제거하기 위해 `./bin/magento cache:evict` 명령을 추가했습니다. 이 명령을 사용하면 Redis 메모리 사용이 감소하여 성능이 향상됩니다.<!--MCLOUD-6023-->

- ![수정 아이콘](../../assets/fix.svg) 성능 문제를 해결하기 위해 *컨텍스트의 New Relic 로그*&#x200B;에 대한 지원이 제거되었습니다.<!--MCLOUD-6422-->

## v1.0.5

릴리스 날짜: 2020년 6월 25일

- ![수정 아이콘](../../assets/fix.svg) magento/magento-cloud-components 버전 1.0.4에 도입된 배포 단계에서 플러시 캐시 작업이 실패하여 배포 프로세스가 중단되는 문제를 해결했습니다.

## v1.0.4

릴리스 날짜: 2020년 6월 25일

- ![새 아이콘](../../assets/new.svg) **컨텍스트에 구현된 New Relic 로그** - 이제 Adobe Commerce에서 생성한 응용 프로그램 로그가 문제 해결 기능을 개선하기 위해 New Relic 내의 추적에 표시됩니다.<!--MCLOUD-6029-->

- ![새 아이콘](../../assets/new.svg) **개선된 로깅**—캐시 무효화 및 전체 색인 재지정 이벤트를 추적하는 로깅을 추가했습니다.<!--MCLOUD-6157-->

## v1.0.3

릴리스 날짜: 2020년 2월 27일

- ![수정 아이콘](../../assets/fix.svg) 이전 PHP 버전을 사용하는 `ece-tools` 2002.0.x 릴리스를 지원하도록 호환성 문제를 해결했습니다.

## v1.0.2

릴리스 날짜: 2020년 2월 6일

- ![새 아이콘](../../assets/new.svg) 특정 제품 페이지에 대한 캐시 미리 로드를 지원하도록 `WARM_UP_PAGES` 환경 변수의 기능을 확장했습니다. 자세한 기능 설명은 [사후 배포 변수](../environment/variables-post-deploy.md#warm_up_pages) 항목을 참조하십시오.<!--MAGECLOUD-4444-->

- ![수정 아이콘](../../assets/fix.svg) `WARM_UP_PAGES` 기능을 사용하여 캐시를 채울 때 저장소 URL이 잘못되어 배포 후 후크가 실패하는 문제를 해결했습니다. 이 문제는 URL 재작성을 사용하지 않도록 설정한 경우에만 발생했습니다.<!-- MAGECLOUD-4094 -->

## v1.0.1

릴리스 날짜: 2019년 7월 23일

- ![수정 아이콘](../../assets/fix.svg) 기본 스토어 URL을 사용하는 [**WARM_UP_PAGES**](../environment/variables-post-deploy.md#warm_up_pages) 기능에 영향을 주는 문제를 해결했습니다. 이제 `config:show:default-url` 명령이 기본 URL을 가져올 수 없는 경우 MAGENTO_CLOUD_ROUTES 변수의 URL이 사용됩니다.<!-- MAGECLOUD-3866 -->

## v1.0.0

릴리스 날짜: 2019년 6월 12일

[`magento/magento-cloud-components`](https://github.com/magento/magento-cloud-components) 패키지의 첫 번째 릴리스이며, 이 릴리스는 `ece-tools` 패키지 버전 2002.0.20 이상에 대한 새 종속성입니다.

- ![새 아이콘](../../assets/new.svg) 정규 표현식 패턴을 사용하여 **WARM_UP_PAGES** 환경 변수를 구성하여 단일 페이지, 여러 도메인 및 여러 페이지를 캐시하는 기능이 추가되었습니다. [사후 배포 변수](../environment/variables-post-deploy.md#warm_up_pages)을(를) 참조하십시오.<!--MAGECLOUD-3258-->
