---
title: Commerce용 클라우드 구성 요소
description: 클라우드 구성 요소 패키지에 대한 최신 개선 사항 목록을 참조하십시오.
recommendations: noDisplay, catalog
exl-id: b4e2508a-3558-4fa8-bae0-3eb76c7b2775
source-git-commit: f9edcc85c14354a2eddacbc5219557e107a367cb
workflow-type: tm+mt
source-wordcount: '548'
ht-degree: 0%

---

# Commerce용 클라우드 구성 요소

다음 [클라우드 구성 요소](https://github.com/magento/magento-cloud-components) 패키지는 클라우드 인프라에 배포된 사이트에 대해 확장된 Adobe Commerce 핵심 기능을 제공합니다. 이 패키지는 ECE-Tools 패키지에 종속됩니다. 이 릴리스 노트는 의 구성 요소인 이 패키지의 최신 개선 사항을 설명합니다. [Commerce용 Cloud Tools 제품군](cloud-tools-suite.md).

다음 `magento/magento-cloud-components` 패키지는 다음 버전 시퀀스를 사용합니다. `<major>.<minor>.<patch>`

릴리스 노트는 다음과 같습니다.

- ![새 아이콘](../../assets/new.svg) 새로운 기능
- ![고정 아이콘](../../assets/fix.svg) 수정 사항 및 향상된 기능

<!--Add release notes below-->

## v1.0.13 {#latest}

릴리스 날짜: 2023년 3월 10일

- ![새 아이콘](../../assets/new.svg) **PHP 8.2에 대한 향상된 지원**—Commerce 2.4.6을 지원하도록 특정 PHP 8.2.x 버전과 관련된 호환성 문제를 해결했습니다.

## v1.0.12

릴리스 날짜: 2022년 9월 13일

- ![고정 아이콘](../../assets/fix.svg) **준비 시 오류**—에서 시도한 문제가 해결되었습니다. [온열](../environment/variables-post-deploy.md#warm_up_pages) 페이지 가시성이 로 설정된 경우 [**개별적으로 보이지 않음**](https://docs.magento.com/user-guide/system/data-attributes-product.html#simple-product-csv-file-structure) 관리자 권한으로, 결과 `ERROR: Warming up failed: <link to page>` 배포 로그에 오류가 표시됩니다.<!-- MCLOUD-9134 -->

## v1.0.11

릴리스 날짜: 2022년 8월 4일

- ![고정 아이콘](../../assets/fix.svg) **Symfony 5.4 호환성 지원 추가**—Symfony 5.4와의 호환성 수정 사항.<!-- AC-3550 -->

## v1.0.10

릴리스 날짜: 2022년 3월 10일

- ![새 아이콘](../../assets/new.svg) **PHP 8.1 지원**- PHP 8.1에 대한 지원을 추가하고 PHP 7.1에 대한 지원을 삭제했습니다.

## v1.0.9

릴리스 날짜: 2021년 10월 25일

- ![고정 아이콘](../../assets/fix.svg) **모노로그 업데이트**- 필요한 최소 버전을 업데이트했습니다. `monolog` 패키지 대상 `^2.3`.<!-- ACMP-1263 -->

## v1.0.8

릴리스 날짜: 2021년 7월 29일

- ![고정 아이콘](../../assets/fix.svg) **자동 생성된 URL에서 후행 슬래시를 제거했습니다.**- 캐시 준비 중 생성된 카테고리 페이지 URL에서 후행 슬래시를 제거했습니다.<!--MCLOUD-7192-->

## v1.0.7

릴리스 날짜: 2020년 9월 9일

- ![새 아이콘](../../assets/new.svg) **로깅 개선 사항**- 의 크기를 줄입니다 `cache.log` 파일을 만들어 성능을 향상시킵니다.<!--MCLOUD-6859-->

- ![고정 아이콘](../../assets/fix.svg) 캐시 구성 값의 유형 오류가 발생하여 `php bin/magento cache:evict` CLI 명령이 실패합니다.

## v1.0.6

릴리스 날짜: 2020년 8월 5일

- ![새 아이콘](../../assets/new.svg) **Redis 성능 향상**- 를 추가했습니다. `./bin/magento cache:evict` 만료된 Redis 키를 제거하는 명령으로 Redis 메모리 사용을 줄여 성능을 개선합니다.<!--MCLOUD-6023-->

- ![고정 아이콘](../../assets/fix.svg) 에 대한 지원이 제거됨 *컨텍스트의 New Relic 로그* 성능 문제를 해결했습니다.<!--MCLOUD-6422-->

## v1.0.5

릴리스 날짜: 2020년 6월 25일

- ![고정 아이콘](../../assets/fix.svg) 배포 단계에서 플러시 캐시 작업이 실패하여 배포 프로세스가 중단되는 magento/magento-cloud-components 버전 1.0.4에 도입된 문제를 해결했습니다.

## v1.0.4

릴리스 날짜: 2020년 6월 25일

- ![새 아이콘](../../assets/new.svg) **컨텍스트에서 New Relic 로그 구현됨**—Adobe Commerce에서 생성한 애플리케이션 로그가 이제 New Relic 내의 추적에 표시되므로 문제 해결 기능을 개선할 수 있습니다.<!--MCLOUD-6029-->

- ![새 아이콘](../../assets/new.svg) **향상된 로깅**- 캐시 무효화 및 전체 색인 재지정 이벤트를 추적하는 로깅이 추가되었습니다.<!--MCLOUD-6157-->

## v1.0.3

릴리스 날짜: 2020년 2월 27일

- ![고정 아이콘](../../assets/fix.svg) 를 지원하도록 호환성 문제를 해결했습니다. `ece-tools` 이전 PHP 버전을 사용하는 2002.0.x 릴리스입니다.

## v1.0.2

릴리스 날짜: 2020년 2월 6일

- ![새 아이콘](../../assets/new.svg) 의 기능을 확장했습니다 `WARM_UP_PAGES` 특정 제품 페이지에 대한 캐시 미리 로드를 지원하는 환경 변수입니다. 다음을 참조하십시오. [배포 후 변수](../environment/variables-post-deploy.md#warm_up_pages) 항목 을 참조하십시오.<!--MAGECLOUD-4444-->

- ![고정 아이콘](../../assets/fix.svg) 잘못된 스토어 URL이 을 사용할 때 사후 배포 후크가 실패하는 문제를 해결했습니다. `WARM_UP_PAGES` 캐시를 채우는 기능입니다. 이 문제는 URL 재작성을 비활성화한 경우에만 발생했습니다.<!-- MAGECLOUD-4094 -->

## v1.0.1

릴리스 날짜: 2019년 7월 23일

- ![고정 아이콘](../../assets/fix.svg) 다음 항목에 영향을 주는 문제가 해결되었습니다. [**WARM_UP_PAGES**](../environment/variables-post-deploy.md#warm_up_pages) 기본 스토어 URL을 사용하는 기능입니다. 자, 만약 `config:show:default-url` 명령이 기본 URL을 가져올 수 없는 경우 MAGENTO_CLOUD_ROUTES 변수의 URL이 사용됩니다.<!-- MAGECLOUD-3866 -->

## v1.0.0

릴리스 날짜: 2019년 6월 12일

의 첫 번째 릴리스입니다. [`magento/magento-cloud-components`](https://github.com/magento/magento-cloud-components) 패키지: 다음에 대한 새 종속성 `ece-tools` 패키지 버전 2002.0.20 이상.

- ![새 아이콘](../../assets/new.svg) 정규 표현식 패턴을 사용하여 다음을 구성하는 기능이 추가되었습니다. **WARM_UP_PAGES** 단일 페이지, 여러 도메인 및 여러 페이지를 캐시하기 위한 환경 변수입니다. 다음을 참조하십시오 [사후 배포 변수](../environment/variables-post-deploy.md#warm_up_pages).<!--MAGECLOUD-3258-->
