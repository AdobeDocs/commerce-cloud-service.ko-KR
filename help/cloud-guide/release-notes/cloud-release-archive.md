---
title: ece-tools용 릴리스 노트 아카이브
description: ece-tools의 아카이빙된 개선 사항에 대해 알아봅니다.
hide: true
hidefromtoc: true
recommendations: noDisplay, noCatalog
exl-id: 96663d2f-ef00-4490-ad2e-06e8041b228e
source-git-commit: 196efa316b9998c1980412ad96577d7ce42d4aec
workflow-type: tm+mt
source-wordcount: '7147'
ht-degree: 0%

---

# ece-tools용 릴리스 노트 아카이브

>[!NOTE]
>
>이 릴리스 정보는 `ece-tools` v2002.0.22 이상에 대한 정보와 업데이트를 제공합니다. `ece-tools` 및 기타 클라우드 패키지에 대한 최신 업데이트를 받으려면 [Cloud Tools 제품군의 릴리스 정보](cloud-tools-suite.md)를 참조하십시오.

## v2002.0.22

`ece-tools` 2002.0.22 릴리스는 `ece-tools` 패키지의 구조를 변경하여 ECE-Tools 릴리스에서 `Adobe Commerce on cloud infrastructure` 패치 릴리스를 분리합니다. 이 릴리스부터 패치 및 중요한 수정 사항이 [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches) 패키지를 사용하여 전달됩니다. 이 패키지는 `ece-tools` 패키지에 대한 새로운 종속성입니다. 릴리스 업데이트 예약과 커뮤니티 기여 작업에 대한 복잡성을 줄이기 위해 다음과 같이 변경되었습니다.

- ![새 아이콘](../../assets/new.svg) **ECE-Tools 패키지 변경**

   - ![새 아이콘](../../assets/new.svg) Adobe Commerce 패치를 `ece-tools` 패키지에서 새 [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches) 작성기 패키지로 이동했습니다.

   - ![새 아이콘](../../assets/new.svg) `magento/magento-cloud-patches` v1.0.0 패키지에 대한 종속성을 추가하도록 `ece-tools` 패키지의 `composer.json` 파일을 업데이트했습니다.

   - ![수정 아이콘](../../assets/fix.svg) 버전 2.3.2-p2 이상부터 보안 전용 릴리스 위에 패치 집합을 적용할 때 `ece-tools` 패치 적용 프로세스가 중단되는 문제를 해결했습니다. 이 문제는 [보안 전용 패치](https://experienceleague.adobe.com/en/docs/commerce-operations/release/notes/security-patches/overview).<!--MAGECLOUD-4661-->에 대해 채택된 새 버전 관리 체계에서 도입되었습니다.

- ![수정 아이콘](../../assets/fix.svg) **패치 및 중요 수정 사항**-다음 패치 및 중요 수정 사항을 적용하려면 클라우드 환경을 `ece-tools` 버전 2002.0.22로 업데이트하십시오. 이러한 패치는 `magento/magento-cloud-patches` v1.0.0 패키지에 포함되어 있습니다.

   - ![수정 아이콘](../../assets/fix.svg) **2.3.1.x 및 2.3.2.x 릴리스에 대한 Page Builder 보안 패치**-인증되지 않은 사용자가 네트워크(RCE)를 통해 임의 코드 실행을 트리거하는 데 사용할 수 있는 일부 템플릿 메서드에 액세스하여 전역 정보가 유출되는 Page Builder 미리 보기 문제를 해결합니다. 이 문제는 Adobe Commerce 버전 2.3.1 및 2.3.2에서 지원되지 않는 버전의 페이지 빌더를 사용할 때 발생할 수 있습니다.<!--MAGECLOUD-4649-->

   - ![수정 아이콘](../../assets/fix.svg) **MSI 패치** - 재고 관리에 기본 재고 설정을 사용할 때 인덱싱 오류 및 성능 문제가 발생하는 문제를 수정합니다.<!--MAGECLOUD-4428-->

   - ![수정 아이콘](../../assets/fix.svg) **새 메일 인터페이스의 이전 버전과의 호환성**-Adobe Commerce v2.3.3에 도입된 `Magento\Framework\Mail\EmailMessageInterface` PHP 인터페이스로 인한 이전 버전과의 비호환성 문제를 수정합니다. 이 패치의 범위에서 새 `EmailMessageInterface`은(는) 이전 `MessageInterface`에서 상속되며 Adobe Commerce 핵심 모듈은 `MessageInterface`에 종속된 것으로 되돌아갑니다.<!--MAGECLOUD-4422-->

   - ![수정 아이콘](../../assets/fix.svg) **카탈로그 페이지 매김이 Elasticsearch 6.x에서 작동하지 않습니다**-카탈로그 검색 엔진으로 Elasticsearch 6.x를 사용하는 고객에게 영향을 주는 검색 결과 페이지 매김과 관련된 중요한 문제를 해결합니다.<!--MAGECLOUD-4448-->

## v2002.0.21

- ![새 아이콘](../../assets/new.svg) **도커 업데이트**—

   - ![새 아이콘](../../assets/new.svg) **새 도커 이미지**—버전 2.3.3 이상에서 지원<!-- MAGECLOUD-3345 -->

      - PHP 버전 7.3.<!-- MAGECLOUD-4017 -->

      - Vannish 캐시 6.2.0<!-- MAGECLOUD-4017 -->

   - ![새 아이콘](../../assets/new.svg) 도커 환경의 `.magento.app.yaml`에 지정된 사용자 지정 후크 구성을 적용할 수 있는 지원이 추가되었습니다. 이전에는 도커 환경에서 기본 후크 구성만 지원했습니다.<!-- MAGECLOUD-3505-->

   - ![새 아이콘](../../assets/new.svg) Docker ENV 파일은 더 이상 Docker 빌드 중에 생성되지 않으며 `docker:config:convert` 명령은 더 이상 사용되지 않습니다. 해당 데이터가 이제 `docker-compose.yml` 파일에 저장됩니다.<!-- MAGECLOUD-3816-->

   - ![새 아이콘](../../assets/new.svg) **PHP 이미지를 업데이트했습니다** 노드, npm 및 grunt-cli 기능을 지원하기 위해 PHP Docker 이미지에 Node.js를 추가했습니다.<!-- MAGECLOUD-3953 -->

- ![새 아이콘](../../assets/new.svg) **환경 변수 업데이트**-

   - ![새 아이콘](../../assets/new.svg) **LOCK_PROVIDER** 배포 변수를 추가하여 중복된 cron 작업 및 cron 그룹이 시작되지 않도록 잠금 공급자를 구성했습니다. [변수 배포](../environment/variables-deploy.md#lock_provider) 항목에서 변수 설명을 참조하십시오.<!-- MAGECLOUD-4052 -->

   - ![새 아이콘](../../assets/new.svg) 소비자가 `CRON_CONSUMERS_RUNNER` 환경 변수를 사용하여 cron 작업을 관리할 때 메시지 큐에서 메시지를 처리하는 방법을 구성하기 위해 **CONSUMER_WAIT_FOR_MAX_MESSAGES** 환경 변수를 추가했습니다. [변수 배포](../environment/variables-deploy.md#consumers_wait_for_max_messages) 항목에서 변수 설명을 참조하십시오.<!-- MAGECLOUD-4071 -->

   - ![수정 아이콘](../../assets/fix.svg) `consumers_runner` cron 작업이 다른 노드에서 동일한 소비자의 여러 인스턴스를 시작할 때 데이터베이스 교착 상태 오류가 발생할 수 있는 문제를 해결했습니다. 이제 환경에서 [**CRON_CONSUMER_RUNNER**](../environment/variables-deploy.md#cron_consumers_runner) 배포 변수를 활성화한 경우 `consumers_runner` 작업은 `single-thread` 옵션을 사용하여 하나의 노드에서만 각 소비자의 인스턴스를 시작합니다.<!-- MAGECLOUD-3913 -->

   - ![수정 아이콘](../../assets/fix.svg) 기본 스토어 URL을 사용하는 [**WARM_UP_PAGES**](../environment/variables-post-deploy.md#warm_up_pages) 기능에 영향을 주는 문제를 해결했습니다. 이제 `config:show:default-url` 명령이 기본 URL을 가져올 수 없는 경우 MAGENTO_CLOUD_ROUTES 변수의 URL이 사용됩니다.<!-- MAGECLOUD-3866 -->

- ![새 아이콘](../../assets/new.svg) `module:refresh` 명령으로 반환된 로깅 정보가 업데이트되었습니다. 이제 `cloud.log` 파일에서 사용 가능한 모듈의 자세한 목록을 볼 수 있습니다.<!-- MAGECLOUD-2514 -->

- ![새 아이콘](../../assets/new.svg) Elasticsearch, [!DNL RabbitMQ], Redis, DB와 같은 Adobe Commerce 버전과 설치된 서비스 간의 호환성 문제에 대한 버전 호환성 유효성 검사와 경고 알림을 개선했습니다.<!-- MAGECLOUD-3535 -->

- ![새 아이콘](../../assets/new.svg) RabitMQ 버전 3.8에 대한 지원이 추가되었습니다.<!-- MAGECLOUD-4674-->

- ![새 아이콘](../../assets/new.svg) 새 Adobe Commerce 2.3.3 및 2.2.10 릴리스에 대해 지원되는 버전을 반영하도록 서비스 호환성에 대한 대화형 유효성 검사를 업데이트했습니다. 권장 버전은 _설치 안내서_&#x200B;의 [시스템 요구 사항](https://experienceleague.adobe.com/en/docs/commerce-operations/installation-guide/system-requirements)을 참조하십시오.<!-- MAGECLOUD-4018 -->

- ![수정 아이콘](../../assets/fix.svg) 배포 단계의 cron 작업 관리 프로세스에서 이미 완료된 cron 작업을 중지하려고 할 때 이 문제가 오류가 아님을 확인하는 로그 메시지가 반환되었습니다. 로그 수준을 `INFO`에서 `DEBUG`.<!-- MAGECLOUD-3653-->(으)로 변경했습니다.

- ![수정 아이콘](../../assets/fix.svg) `app:config:import` 작업 중에 오류가 발생했을 때 배포 프로세스를 중단하지 않는 `setup:upgrade` 명령을 실행할 때 발생하는 문제를 해결했습니다.<!-- MAGECLOUD-3806 -->

- ![새 아이콘](../../assets/new.svg) 디버깅을 위한 자세한 정보를 제공하는 동안 [!DNL Cloud Console]에 표시되는 로그의 세부 정보를 줄이기 위해 파일 처리기의 기본 로그 수준을 `debug`(으)로 변경했습니다.<!-- MAGECLOUD-3871 -->

- ![수정 아이콘](../../assets/fix.svg) 빌드하는 동안 정적 콘텐츠 배포에 오류가 발생하는 문제를 해결했습니다. 설치 및 `ece-tools` 구성 덤프 후 `config.php` 파일에 관리자 사용자에 대해 지정된 로케일이 없는 경우 오류가 발생했습니다. 이제 `config.php` 파일에 관리자 사용자의 기본 로케일이 있습니다.<!-- MAGECLOUD-3957 -->

- ![수정 아이콘](../../assets/fix.svg) 보안 URL(https)로 구성되지 않은 환경에서 `magento-cloud` CLI 명령이 실패할 때 발생하는 `Undefined index error`을(를) 수정했습니다. 이제 보안 URL을 사용할 수 없는 경우 ECE-Tools 패키지는 기본 URL(http)을 사용합니다.<!-- MAGECLOUD-4009 -->

## v2002.0.20

- ![새 아이콘](../../assets/new.svg) **도커 업데이트**—

   - ![새 아이콘](../../assets/new.svg) 이제 Docker 환경에서 `ece-tools` 패키지를 사용하여 기능 테스트를 수행할 수 있습니다. [응용 프로그램 테스트](https://developer.adobe.com/commerce/cloud-tools/docker/test/code-testing/).<!-- MAGECLOUD-3129/3684 -->를 참조하세요.

   - ![새 아이콘](../../assets/new.svg) `.magento.app.yaml` 파일을 사용하여 PHP 모듈을 구성할 수 있는 지원이 추가되었습니다. `.magento.app.yaml` 파일](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/configure/app/php-settings#enable-extensions)에 지정된 모든 [PHP 확장을 도커 PHP 컨테이너에서 사용할 수 있습니다.<!-- MAGECLOUD-3357 -->

   - ![새 아이콘](../../assets/new.svg) 도커 명령줄 환경을 개선하는 데 사용할 수 있는 새 명령이 있습니다. Docker 참조의 [`bin/magento-docker` 섹션을 참조하십시오](https://developer.adobe.com/commerce/cloud-tools/docker/quick-reference/#cloud-docker-cli).<!-- MAGECLOUD-3569 -->

   - ![새 아이콘](../../assets/new.svg) 로컬 호스트와 도커 간의 개발 중에 Mutagen.io를 사용하여 파일을 동기화하는 기능을 추가했습니다.<!-- MAGECLOUD-3559 -->

   - ![수정 아이콘](../../assets/fix.svg) 도커 환경을 사용할 때 기본 경로를 수정했습니다. 이제 SSH를 사용하여 Docker 컨테이너에 로그인하면 예상대로 `/app` 디렉터리의 프로젝트 루트에 있게 됩니다.<!-- MAGECLOUD-3582 -->

   - ![수정 아이콘](../../assets/fix.svg) 나트륨 라이브러리를 버전 1.0.11에서 버전 1.0.18로 업데이트하고 나트륨 PHP 확장을 업데이트했습니다.<!-- MAGECLOUD-3832 -->

     >[!WARNING]
     >
     >클라우드 인프라의 Adobe Commerce 고객은 Adobe Commerce 2.3.2로 업그레이드하기 전에 Pro 프로덕션 및 스테이징 환경에서 libsodium 패키지를 업그레이드하려면 [Adobe Commerce 지원 티켓을 제출](https://support.magento.com/hc/en-us/articles/360000913794#submit-ticket)해야 합니다. 현재 Starter 환경을 Adobe Commerce 2.3.2로 업그레이드할 수 없습니다.

   - ![수정 아이콘](../../assets/fix.svg) `analysis-icu` 및 `analysis-phonetic` Elasticsearch 플러그인을 모든 도커 이미지에 추가했습니다.<!-- MAGECLOUD-3446 -->

   - ![수정 아이콘](../../assets/fix.svg) 향상된 유효성 검사: `docker:build` 명령에 대한 옵션을 사용할 때는 옵션을 사용할 때 값을 제공해야 합니다. 또한 `docker:build run` 명령을 사용할 때 노드 버전에 대한 유효성 검사를 추가했습니다.<!-- MAGECLOUD-3486 & MAGECLOUD-3678 -->

- ![새 아이콘](../../assets/new.svg) **환경 변수 업데이트**—

   - ![새 아이콘](../../assets/new.svg) [DATABASE_CONFIGURATION 환경 변수](../environment/variables-deploy.md#database_configuration)을(를) 사용하여 데이터베이스 테이블 접두사에 대한 지원을 추가했습니다.<!-- MAGECLOUD-2901 -->

   - ![새 아이콘](../../assets/new.svg) Pro 및 Starter 프로덕션 및 스테이징 환경에 배포할 때 기본 URL을 업데이트하기 위해 **FORCE_UPDATE_URL** 배포 변수를 추가했습니다. [변수 배포](../environment/variables-deploy.md#force_update_urls) 콘텐츠에서 정의를 참조하십시오.<!-- MAGECLOUD-3602 -->

   - ![새 아이콘](../../assets/new.svg) 클라우드 인프라에 배포된 사이트에서 응용 프로그램 성능을 확인하기 위해 _첫 바이트에 대한 시간_ 페이지 테스트를 구성하기 위해 **TTFB_TESTED_PAGES** 배포 후 변수를 추가했습니다. [사후 배포 변수](../environment/variables-post-deploy.md)의 변수 설명을 참조하십시오.<!-- MAGECLOUD-3643 -->

   - ![수정 아이콘](../../assets/fix.svg) 정적 콘텐츠 배포에서 임의의 오류가 발생하는 다중 스레드 SCD 문제를 해결했습니다. 해결 방법으로 **SCD_THREADS** 변수를 `1`(으)로 설정했습니다. 이제 필요에 따라 개수를 늘릴 수 있습니다. [변수 배포](../environment/variables-deploy.md#scd_threads) 및 [변수 빌드](../environment/variables-build.md#scd_threads)의 정의를 참조하십시오.<!-- MAGECLOUD-3611 -->

   - ![수정 아이콘](../../assets/fix.svg) **WARM_UP_PAGES** 환경 변수를 구성하여 단일 페이지, 여러 도메인 및 여러 페이지를 캐시할 수 있습니다. [사후 배포 변수](../environment/variables-post-deploy.md#warm_up_pages) 콘텐츠에서 확장된 정의를 참조하십시오.<!-- MAGECLOUD-3258 -->

- ![수정 아이콘](../../assets/fix.svg) `pub/static/.htaccess` 파일을 제외 목록에 추가했습니다. [PHOENIX MEDIA GmbH의 Björn Kraus가 제출한 수정 내용](https://github.com/magento/ece-tools/pull/455).<!-- MAGECLOUD-3545/Github#455 -->

- ![수정 아이콘](../../assets/fix.svg) 하나 이상의 중요 수준 유효성 검사기에서 오류를 반환한 경우 모든 유효성 검사 메시지가 `Critical`(으)로 표시되는 오류를 해결했습니다.<!-- MAGECLOUD-3178 -->

- ![수정 아이콘](../../assets/fix.svg) 데이터베이스에 기본 URL이 없는 경우 배포 오류가 발생하는 문제를 해결했습니다.<!-- MAGECLOUD-3075 -->

- ![새 아이콘](../../assets/new.svg) 환경 서비스, 경로 또는 변수를 표시하는 `ece-tools` 패키지에 새 **`env:config:show`명령**&#x200B;을 추가했습니다. [서비스, 경로 및 변수](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/dev-tools/ece-tools/package-overview#services-routes-and-variables)를 참조하세요. [Vladimir Kerkhoff가 제출한 기능](https://github.com/magento/ece-tools/pull/486).<!-- MAGECLOUD-3451 -->

- ![수정 아이콘](../../assets/fix.svg) 셸 리팩터링 후 개발 `ece-tools`에서 Adobe Commerce 2.2.6 또는 이전 버전을 설치하려고 할 때 심각한 오류가 발생하는 문제를 해결했습니다.<!-- MAGECLOUD-3665 -->

- ![수정 아이콘](../../assets/fix.svg) 더 이상 사용되지 않는 버전의 Carbon 사용에 대한 경고와 함께 Adobe Commerce 2.1.x 및 2.2.x 설치가 실패하던 문제를 해결했습니다.<!-- MAGECLOUD-3704 -->

- ![수정 아이콘](../../assets/fix.svg) 셸 출력의 `cloud.log` 로그 수준을 `info`에서 `debug`(으)로 줄였습니다.<!-- MAGECLOUD-3277 -->

- ![수정 아이콘](../../assets/fix.svg) `ece-tools db-dump` 명령에 `--remove-definers (-d)` 옵션을 추가하여 덤프 파일에서 정의자를 제거했습니다.<!-- MAGECLOUD-3510 -->

## v2002.0.19

- ![수정 아이콘](../../assets/fix.svg) 배포 중에 `env.php` 파일을 덮어써서 사용자 지정 구성이 손실되는 문제를 해결했습니다. 이 업데이트를 통해 클라우드 인프라의 Adobe Commerce은 사용자 지정 구성을 유지하면서 모든 배포로 `env.php` 파일을 업데이트합니다.<!-- MAGECLOUD-3668 -->

## v2002.0.18

- ![새 아이콘](../../assets/new.svg) **도커 업데이트**—

   - ![새 아이콘](../../assets/new.svg) 이제 도커 환경에서는 .magento.app.yaml 파일의 [crons 속성에 정의된 cron 구성을 지원합니다](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/configure/app/properties/crons-property).<!-- MAGECLOUD-3150 -->

   - ![새 아이콘](../../assets/new.svg) **새 도커 컨테이너**—HTTPS에서 바니시 SSL 종료를 용이하게 하기 위해 [TLS 종료 프록시 컨테이너](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#varnish-container)를 추가했습니다.<!-- MAGECLOUD-2890 -->

   - ![새 아이콘](../../assets/new.svg) **새 도커 이미지**—Gulp 및 기타 기능(예: Jasmine JS 단위 테스트)을 지원하도록 Node.js 이미지를 추가했습니다.<!-- MAGECLOUD-3345 -->

   - ![새 아이콘](../../assets/new.svg) **도커 빌드 모드**—이제 [프로덕션 모드 또는 개발자 모드](https://developer.adobe.com/commerce/cloud-tools/docker/deploy/#launch-mode)에서 도커 환경을 시작하도록 선택할 수 있습니다. 개발자 모드는 전체 쓰기 가능한 파일 시스템 권한으로 활성 개발을 지원합니다.<!-- MAGECLOUD-3152/3511 -->

   - ![수정 아이콘](../../assets/fix.svg) 사용할 수 없는 서비스에 대해 캐시가 구성된 경우 `Name or service not known` 오류로 인해 도커 배포가 실패하는 문제를 해결했습니다. 이제 [`.magento/services.yaml` 파일](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/configure/service/services-yaml)에서 서비스를 제거할 수 있습니다. Docker 구성 생성기가 `docker/config.php.dist` 파일의 서비스를 자동으로 업데이트합니다.<!-- MAGECLOUD-3369 -->

   - ![새 아이콘](../../assets/new.svg) 서비스 호환성을 위해 대화형 유효성 검사를 추가했습니다. 이제 요청된 서비스가 Adobe Commerce 버전 또는 다른 서비스와 호환되지 않으면 _대화형 모드_&#x200B;에서 사용자에게 메시지와 계속할 수 있는 선택을 묻는 메시지가 표시됩니다. 도커에 사용할 수 있는 [서비스 버전](https://developer.adobe.com/commerce/cloud-tools/docker/containers/#service-containers)을 참조하세요. CICD를 위해 상호 작용을 건너뛰려면 `-n` 옵션을 사용하십시오.<!-- MAGECLOUD-3251 -->

   - ![수정 아이콘](../../assets/fix.svg) 기존 덤프를 지운 도커 구성 `db-dump` 명령의 문제를 해결했습니다.<!-- MAGECLOUD-3366 -->

   - ![수정 아이콘](../../assets/fix.svg) 동일한 데이터베이스 ID에 Redis `session`, `default` 및 `page_cache` 캐시 저장소를 할당한 문제를 해결했습니다.<!-- MAGECLOUD-3172 -->

- ![새 아이콘](../../assets/new.svg) **환경 변수 업데이트**—

   - ![새 아이콘](../../assets/new.svg) 새 **ELASTICSUITE\_CONFIGURATION** 환경 변수는 배포 간에 사용자 지정된 서비스 설정을 유지합니다. [변수 배포](../environment/variables-deploy.md#elasticsuite_configuration) 콘텐츠에서 정의를 참조하십시오.<!-- MAGECLOUD-3205 -->

   - ![새 아이콘](../../assets/new.svg) `.magento.env.yaml` 파일에서 정적 콘텐츠 배포를 완료하는 시간을 늘릴 수 있도록 **SCD_MAX_EXECUTION_TIMEOUT** 환경 변수를 추가했습니다. [변수 배포](../environment/variables-deploy.md#scd_max_execution_time), [변수 빌드](../environment/variables-build.md#scd_max_execution_time) 및 [전역 변수](../environment/variables-global.md#scd_max_execution_time)의 정의를 참조하십시오.<!-- MAGECLOUD-2822 -->

      - ![새 아이콘](../../assets/new.svg) 클라우드 인프라에서 잠금 공급자의 탑재 지점 경로를 구성하기 위해 **MAGENTO_CLOUD_LOCKS_DIR** 환경 변수를 추가했습니다. 잠금 공급자는 중복 크론 작업 및 크론 그룹의 시작을 방지합니다. 이 변수는 Adobe Commerce 버전 2.2.5 이상에서 지원되며 자동으로 구성됩니다. [클라우드 변수](../environment/variables-cloud.md)의 정의를 참조하십시오.<!-- MAGECLOUD-3135 -->

      - ![수정 아이콘](../../assets/fix.svg) 검색된 CPU 스레드 수를 기반으로 최적의 값을 자동으로 결정하도록 **SCD_THREADS** 환경 변수 기본값을 변경했습니다. [변수 배포](../environment/variables-deploy.md#scd_threads) 및 [변수 빌드](../environment/variables-build.md#scd_threads)에서 업데이트된 정의를 확인하십시오.<!-- MAGECLOUD-3382 -->

- ![수정 아이콘](../../assets/fix.svg) 클라우드 인프라 버전 2002.0.16에서 Adobe Commerce으로 업그레이드할 때 오류가 발생하는 DB 격리 메커니즘용 패치 문제를 해결했습니다.<!-- MAGECLOUD-3383 -->

- ![수정 아이콘](../../assets/fix.svg) _Google 이미지 차트_&#x200B;을(를) _이미지 차트_(으)로 바꾸는 패치를 추가했습니다. DevBlog 문서 [M1](https://community.magento.com/t5/Magento-DevBlog/Google-Image-Charts-deprecation-and-update-for-M1/ba-p/125006)에 대한 Google 이미지 차트 사용 중단 및 업데이트를 참조하십시오.<!-- MAGECLOUD-3456 -->

- ![수정 아이콘](../../assets/fix.svg) [SEARCH_CONFIGURATION 변수](../environment/variables-deploy.md#search_configuration)에 대한 유효성 검사를 추가했습니다. &#39;엔진&#39; 옵션이 설정되어 있지 않고 `_merge`이(가) 필요하지 않으면 배포가 실패합니다.<!-- MAGECLOUD-3470 -->

- ![수정 아이콘](../../assets/fix.svg) 예외가 발생한 후 중요한 데이터를 표시하는 문제를 해결했습니다. 이제 중요한 정보가 적절하게 마스킹됩니다.<!-- MAGECLOUD-3525 -->

- ![수정 아이콘](../../assets/fix.svg) Magento Open Source 패키지의 내결함성 설정을 개선했습니다. Adobe Commerce에서 Redis `slave` 인스턴스에서 데이터를 읽을 수 없는 경우 Redis `master` 인스턴스에서 읽습니다. [REDIS_USE_SLAVE_CONNECTION](../environment/variables-deploy.md#redis_use_slave_connection)을(를) 참조하십시오.<!-- MAGECLOUD-2899 -->

## v2002.0.17

>[!NOTE]
>
>`ece-tools` 버전 2002.0.17에는 중요한 보안 패치가 포함되어 있습니다. [기술 리소스: Magento Open Source 패치](https://magento.com/tech-resources/download#download2288)를 참조하십시오.

- ![새 아이콘](../../assets/new.svg) **서비스 업데이트**—다음 Adobe Commerce 버전에서 지원됩니다. 2.2.8 이상, 2.2.x, 2.3.1 이상, 2.3.x

   - Elasticsearch 버전 6.x.<!-- MAGECLOUD-3196 -->에 대한 지원을 추가했습니다.

   - Redis 버전 5.0에 대한 지원이 추가되었습니다.

- ![새 아이콘](../../assets/new.svg) **새 도커 이미지**—도커 빌드에 다음 서비스를 추가했습니다.

   - Elasticsearch 6.5<!-- MAGECLOUD-3196 -->

   - Redis 5.0<!-- MAGECLOUD-3223 -->

- ![새 아이콘](../../assets/new.svg) **새 환경 변수**—이전에는 SCD 압축에 대한 하드 코딩된 시간 제한이 있었습니다. 이제 **SCD_COMPRESSION_TIMEOUT** 환경 변수를 사용하여 SCD 압축 시간 제한을 구성할 수 있습니다. [빌드 변수](../environment/variables-build.md#scd_compression_timeout) 및 [배포 변수](../environment/variables-deploy.md#scd_compression_timeout) 컨텐츠의 정의를 참조하십시오.<!-- MAGECLOUD-2870 -->

- ![수정 아이콘](../../assets/fix.svg) 웹 서버가 보안 및 고객 환경을 개선하기 위해 상점 및 관리자 액세스에서 생성된 링크에 대한 웹 서버 재작성을 사용하도록 install 명령에 `--use-rewrites` 옵션을 추가했습니다.<!-- MAGECLOUD-3246 -->

- ![수정 아이콘](../../assets/fix.svg) 설치 및 업그레이드 이벤트에 대한 날짜가 표시되도록 `var/log/install_upgrade.log` 파일에 타임스탬프를 추가했습니다.<!-- MAGECLOUD-2895 -->

## v2002.0.16

- ![새 아이콘](../../assets/new.svg) **도커 업데이트**—

   - 이제 도커 환경에서 생성된 기본 서비스 구성은 클라우드 템플릿의 기본 구성과 동일합니다.<!-- MAGECLOUD-3025 -->

   - `sendmail` 서비스를 사용하여 도커 환경에서 메일을 보낼 수 있습니다.<!-- MAGECLOUD-2907 -->

   - Cloud Docker 환경에서 디버깅하도록 [Xdebug를 구성](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug/)하는 기능이 추가되었습니다.<!-- MAGECLOUD-2891 -->

   - `docker-compose.yml` 파일을 생성할 때 웹 서비스 권한 문제를 해결했습니다.<!-- MAGECLOUD-2883 -->

- ![새 아이콘](../../assets/new.svg) **업그레이드 개선 사항** - Adobe Commerce v2.3으로 업그레이드하기 전에 `composer.json` 파일의 `autoload` 속성에 필요한 구성 변경 사항이 포함되어 있는지 확인하는 유효성 검사가 추가되었습니다. [버전 업그레이드](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/develop/upgrade/commerce-version).<!-- MAGECLOUD-2392 -->를 참조하십시오.

- ![새 아이콘](../../assets/new.svg) 이제 정적 콘텐츠 배포의 압축 프로세스에 기본적으로 생성되거나 사용자 지정된 모든 자산이 포함되며 [`build:transfer` 섹션의 시작 부분에서 빌드 단계](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/configure/app/properties/hooks-property)에 발생합니다. 이전에는 압축 프로세스가 사용자 지정 축소와 정적 에셋의 번들링을 적용하기 전에 발생했습니다. [Tryzens Limited에서 Rafael Garcia Lepper가 제출한 수정 사항](https://github.com/magento/ece-tools/pull/413).<!-- MAGECLOUD-3104 -->

- ![수정 아이콘](../../assets/fix.svg) 추가 데이터베이스 및 서비스 관계를 구성한 후 바로 배포하는 동안 발생한 데이터베이스 연결 오류를 수정했습니다. 또한 이 수정 사항은 Starter용 Commerce 보고의 구성 프로세스 중에 발생한 문제를 해결합니다. Starter의 경우 이 업그레이드는 Commerce 보고를 사용하기 위한 &quot;필수 항목&quot;입니다.<!-- MAGECLOUD-3035 -->

- ![수정 아이콘](../../assets/fix.svg) 배포 프로세스가 실패하는 데이터베이스 구성의 유효성 검사 문제를 해결했습니다.<!-- MAGECLOUD-3003 -->

- ![수정 아이콘](../../assets/fix.svg) [PHP 상수](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/configure/env/configure-env-yaml#php-constants)와 함께 사용할 적절한 버전의 `symfony/yaml` 패키지로 제약 조건을 업데이트했습니다. 3.2 이전 버전의 `symfony/yaml` 패키지를 사용하는 경우 상수 구문 분석이 작동하지 않습니다. [Vladimir Kerkhoff가 제출한 수정 내용](https://github.com/magento/ece-tools/pull/404).<!-- MAGECLOUD-2956 -->

- ![새 아이콘](../../assets/new.svg) **환경 구성 확인**—PHP 버전을 확인하고 최신 권장 버전을 사용하지 않는 사용자에게 경고하는 유효성 검사가 추가되었습니다.<!--MAGECLOUD-2903-->

- ![수정 아이콘](../../assets/fix.svg) 잘못된 형식의 JSON 변수를 처리하는 문제를 해결했습니다. 이제 JSON 변수로 인해 구문 오류가 발생하면 `cloud.log` 파일에 경고가 나타나고 기본 변수를 사용하여 배포가 계속됩니다.<!-- MAGECLOUD-2851 -->

- ![수정 아이콘](../../assets/fix.svg) Redis 서비스를 사용하지 않도록 설정한 직후 배포 중에 발생한 연결 오류를 해결했습니다.<!-- MAGECLOUD-2747 -->

- ![새 아이콘](../../assets/new.svg) **변경 내용 로깅**—다음 빌드 및 배포 프로세스 이벤트에 대해 [로그 수준](../environment/log-handlers.md#log-levels)을(를) `Info`에서 `Notice`(으)로 업데이트했습니다.<!--MAGECLOUD-2925-->

   - `composer.json`의 설치된 모듈을 `app/etc/config.php` 파일의 공유 구성 설정과 일치시키기 위한 프로세스를 시작하고 종료합니다.

   - 구성 유효성 검사 프로세스 시작 및 종료

   - 클래스 생성을 위한 `setup:di:compile` 프로세스 시작 및 종료

- ![새 아이콘](../../assets/new.svg) **새 환경 변수**—

   - **[RESOURCE_CONFIGURATION 배포 변수](../environment/variables-deploy.md#resource_configuration)**—이 변수를 사용하여 리소스 이름을 데이터베이스 연결에 매핑합니다.<!-- MAGECLOUD-3026 & MAGECLOUD-2963-->

   - **[X_FRAME_CONFIGURATION 전역 변수](../environment/variables-global.md#x_frame_configuration)**—이 변수를 사용하여 `<frame>`, `<iframe>` 또는 `<object>`에서 Adobe Commerce 페이지를 렌더링하기 위한 `X-Frame-Options` 헤더 구성을 변경합니다.<!-- MAGECLOUD-3048 -->

- ![수정 아이콘](../../assets/fix.svg) **환경 변수 업데이트**—다음 환경 변수를 변경했습니다.

   - **[WARM_UP_PAGES](../environment/variables-post-deploy.md)**—Adobe Commerce 저장소에 대해 정의된 모든 도메인에서 지정된 페이지의 캐시를 미리 로드하는 기능이 추가되었습니다. 이전에는 사이트가 여러 도메인으로 구성된 경우 배포 후 프로세스가 기본값이 아닌 도메인에서 지정된 페이지에 대한 캐시를 미리 로드하지 못했으며 배포 후 로그에 다음 오류를 반환했습니다. `ERROR: Warming up failed: <uri>`<!-- MAGECLOUD-2466 -->

   - **SCD_COMPRESSION_LEVEL**—SCD 압축 수준에 대한 올바른 기본값으로 설명서와 샘플 `.magento.env.yaml` 파일을 업데이트했습니다. [빌드 변수](../environment/variables-build.md#scd_compression_level) 및 [배포 변수](../environment/variables-deploy.md#scd_compression_level) 컨텐츠의 정의를 참조하십시오.<!-- MAGECLOUD-2823 -->

   - **SCD_EXCLUDE_THEMES**—이 환경 변수는 더 이상 사용되지 않습니다. [SCD_MATRIX](../environment/variables-build.md#scd_matrix)을(를) 사용하여 테마 구성을 제어하십시오.<!--MAGECLOUD-2882-->

   - **SCD\_MATRIX**—SCD_MATRIX가 다른 문자 사례가 포함된 테마 값을 무시할 때 발생하는 문제를 방지하기 위해 유효성 검사 프로세스를 수정했습니다. [빌드 변수](../environment/variables-build.md#scd_matrix) 및 [배포 변수](../environment/variables-deploy.md#scd_matrix) 컨텐츠의 정의를 참조하십시오.<!-- MAGECLOUD-2904 -->

   - **관리자 변수**—<!-- MAGECLOUD-2573/MAGECLOUD-2848 -->

      - 환경 변수를 사용하여 관리자 사용자의 자격 증명을 관리할 때의 보안이 개선되었습니다. 업그레이드 중에 더 이상 ADMIN_EMAIL, ADMIN_USERNAME 및 ADMIN_PASSWORD 환경 변수를 사용하여 관리자 자격 증명을 재정의할 수 없습니다. 관리 패널에 액세스할 수 없는 경우 _암호 찾기_ 기능 또는 `admin:user:create` CLI 명령을 사용하여 새 관리 사용자를 만드십시오. [관리자 패널 액세스](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/start/onboarding#admin)를 참조하세요.

      - 패치를 업그레이드하거나 적용할 때 ADMIN_EMAIL이 더 이상 필요하지 않습니다.

## v2002.0.15

- ![새 아이콘](../../assets/new.svg) **도커 업데이트**—

   - 이제 [도커 환경을 빌드](https://developer.adobe.com/commerce/cloud-tools/docker/configure/)할 때 도커 생성기가 `.magento.app.yaml` 및 `.magento/services.yaml` 구성 파일에 지정된 서비스를 사용합니다. 빌드 매개 변수를 사용하여 다른 서비스 버전을 선택할 수 있습니다.<!-- MAGECLOUD-2888 -->

   - PHP 7.2 이미지 추가—클라우드 도커에서 PHP 7.2에 대한 지원을 추가했습니다. 사용 중인 Adobe Commerce 버전과 호환되는 PHP 버전을 지정하는 `docker:build --php` 옵션을 포함하도록 [Launch Docker 구성](https://developer.adobe.com/commerce/cloud-tools/docker/configure/)을 업데이트했습니다.<!-- MAGECLOUD-2799 -->

   - PHP-CLI 이미지를 기반으로 [Cron 컨테이너](https://developer.adobe.com/commerce/cloud-tools/docker/containers/cli/#cron-container)을 추가했습니다.<!-- MAGECLOUD-2565 -->

   - 도커 빌드에 다음 서비스를 추가했습니다.

      - [!DNL RabbitMQ] 3.5 및 3.7<!-- MAGECLOUD-2567 & 2889-->

      - Elasticsearch 1.7, 2.4 및 5.2<!-- MAGECLOUD-2569 & 2887 -->

      - Redis 3.2 및 4.0<!-- MAGECLOUD-2886 -->

- ![새 아이콘](../../assets/new.svg) **PHP 상수로 구성**—`.magento.env.yaml` 구성 파일에서 [PHP 상수](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/configure/env/configure-env-yaml#php-constants)에 대한 지원을 추가했습니다.<!-- MAGECLOUD- 2575 -->

- ![새 아이콘](../../assets/new.svg) **새 환경 변수**—기본적으로 프로덕션 환경에만 Google Analytics이 사용됩니다. [ENABLE_ANALYTICS_ANALYTICS 환경 변수](../environment/variables-deploy.md#enable_google_analytics)을(를) 사용하여 스테이징 및 통합 환경에서 Google AnalyticsGOOGLE 을 활성화할 수 있습니다.<!--MAGECLOUD-2879-->

- ![수정 아이콘](../../assets/fix.svg) 다시 배포 후 `env.php` 파일에서 사용자 지정된 cron 구성을 제거하는 문제를 해결했습니다. 이제 사용자 지정 cron 구성이 `env.php` 파일에 안전하게 유지됩니다.<!-- MAGECLOUD-2923 -->

- ![수정 아이콘](../../assets/fix.svg) 빌드, 배포 및 배포 후 단계에 대한 메시지와 [로그 수준](../environment/log-handlers.md#log-levels)의 불일치를 수정했습니다. 모든 단계 및 하위 단계에 대해 시작 및 종료 로그 메시지 수준을 **info**&#x200B;에서 **notice**(으)로 높였습니다. 시작 및 종료 로그 메시지를 추가했습니다(해당하는 경우).<!-- MAGECLOUD-2919 -->

- ![수정 아이콘](../../assets/fix.svg) 구성된 경우 배포 후 단계의 시작을 막는 cron 프로세스와 관련된 문제를 해결했습니다. 이제 배포 후 후크를 활성화하면 배포 후 단계의 시작 시 cron 프로세스가 다시 활성화됩니다.<!-- MAGECLOUD-2862 -->

- ![수정 아이콘](../../assets/fix.svg) 사용자 지정 데이터베이스 구성을 지정할 때 Adobe Commerce을 제대로 설치할 수 없는 문제를 해결했습니다. 이전에는 [DATABASE_CONFIGURATION 환경 변수](../environment/variables-deploy.md#database_configuration)에서 사용자 지정된 연결 정보를 지정하더라도 설치 프로세스에서 [MAGENTO_CLOUD_RELATIONSHIP 변수](../environment/variables-cloud.md)의 데이터베이스 구성을 사용했습니다.<!--MAGECLOUD-2736-->

- ![수정 아이콘](../../assets/fix.svg) `config.php` 파일의 `system` 섹션에 각 웹 사이트 로케일을 포함하도록 `config:dump` 명령을 수정했습니다.<!--MAGECLOUD-2740-->

- ![수정 아이콘](../../assets/fix.svg) 소스 기본 URL 참조를 수정하여 배포 후 단계에서 _준비_ 오류가 발생하는 문제를 해결했습니다.<!--MAGECLOUD-2797-->

- ![수정 아이콘](../../assets/fix.svg) `setup:di:compile` 프로세스 중에 파일을 잘못 생성하여 Amazon 결제 모듈에 영향을 주는 문제를 해결했습니다.<!--MAGECLOUD-2850-->

## v2002.0.14

- ![새 아이콘](../../assets/new.svg) **이상적인 상태 확인**—이제 `ideal-state` 마법사가 각 배포 중 현재 구성을 확인하고 구성을 업데이트하여 가동 중지 시간이 없는 빠른 배포를 수행하는 명확한 지침을 제공합니다.<!--MAGECLOUD-2372-->

- ![수정 아이콘](../../assets/fix.svg) **PCI 규정 준수**—서드파티 메시징 서비스에 연결할 때 TLS(전송 계층 보안) 버전 1.2가 필요하도록 클라우드 인프라의 Adobe Commerce에 대한 메시징 프로토콜을 업데이트했습니다. TLS 버전 1.2를 지원하지 않는 메시지 서비스를 사용하는 경우 서비스를 업그레이드해야 합니다. 그렇지 않으면 Adobe Commerce 응용 프로그램에서 메시지 서버에 연결하여 전자 메일을 보내려고 할 때 다음 오류 메시지가 표시됩니다. `Unable to connect via TLS`.<!--MAGECLOUD-2521-->

- ![새 아이콘](../../assets/new.svg) **배포 개선**—스테이징 또는 프로덕션 환경에서 과도한 로깅 활동으로 인한 성능 문제를 방지하기 위해 `dev`, `debug` 또는 `debug_logging` 옵션이 활성화된 경우 고객에게 경고하는 유효성 검사가 추가되었습니다.<!--MAGECLOUD-2517-->

- ![수정 아이콘](../../assets/fix.svg) **배포 수정**—

   - 이제 유지 관리 모드는 배포 단계가 시작될 때 활성화되고 끝날 때 비활성화됩니다. 배포가 실패하면 배포 문제가 해결될 때까지 사이트는 유지 관리 모드로 유지됩니다. 이전에는 배포가 실패하더라도 사이트가 프로덕션 모드로 돌아갔습니다.<!--MAGECLOUD-2603-->

      - 배포가 완료되도록 다음 배포 문제에 대한 오류 수준을 `CRITICAL`에서 `WARNING`(으)로 다운그레이드하도록 배포 단계 유효성 검사를 다시 작동했습니다. 이전에는 이러한 문제로 인해 배포가 실패했습니다.

      - 환경 구성에 배포 또는 클라우드 변수에 대한 잘못된 값이 포함되어 있습니다.

   - 클라우드 인프라의 Elasticsearch 버전이 클라우드 인프라의 Adobe Commerce에서 지원하는 elasticsearch/elasticsearch 모듈 버전과 호환되지 않습니다. Adobe Commerce 지원 기술 자료에서 [Elasticsearch 문제 해결 문서](https://support.magento.com/hc/en-us/articles/360015758471-Deployment-fails-or-interrupts-with-cloud-log-error-Elasticsearch-version-is-not-compatible-with-current-version-of-magento)을(를) 참조하십시오.<!--MAGECLOUD-2600-->

   - 배포 중 `recursion detected` 오류가 발생하는 `app/etc/config.php` 파일의 공유 구성 설정 문제를 해결했습니다.<!--MAGECLOUD-2173-->

- ![수정 아이콘](../../assets/fix.svg) **크론 관련 수정 사항**—

   - 기본(1분) 이외의 cron 빈도를 지정하면 작업이 실행되지 않는 cron 예약 문제를 해결했습니다.<!--MAGECLOUD-2602-->

   - 배포 중 크론 작업이 계속 실행될 수 있도록 하는 배포 단계의 문제를 해결했습니다. 이로 인해 데이터베이스 잠금 및 기타 중요한 문제가 발생할 수 있습니다. 이제 모든 cron 작업은 배포 단계가 시작되기 전에 중지되고 배포가 완료된 후에 다시 시작됩니다.&lt;!—MAGECLOUD—2537—>

   - 배포를 시작하기 전에 중단할 수 있도록 고정 cron 작업의 잠금을 해제하기 위한 버전 2.2.x의 cron 작업 워크플로우를 수정했습니다. 이전에는 고정 cron 작업으로 인해 배포가 중단되었습니다.<!--MAGECLOUD-2501-->

- ![수정 아이콘](../../assets/fix.svg) Adobe Commerce 코딩 표준을 준수하도록 짧은 배열 구문과 4공간 들여쓰기를 사용하도록 `vendor/bin/ece-tools config:dump` 명령에 의해 생성된 `config.php` 파일의 형식을 변경했습니다.<!--MAGECLOUD-2527-->

- ![수정 아이콘](../../assets/fix.svg) `.magento.env.yaml`에 클라우드 인프라 프로젝트의 Adobe Commerce에 대한 기본 URL 구성 대신 웹 구성에 대한 `{{ base_url }}` 및 `{{ unsecure_base_url }}` 자리 표시자가 포함되어 있을 때 발생하는 배포 오류를 수정했습니다./<!--MAGECLOUD-2607-->

## v2002.0.13

- ![새 아이콘](../../assets/new.svg) **가동 중지 시간 없이 배포 사용**—이제 Adobe Commerce on cloud infrastructure에서 배포 중 필요한 데이터베이스 변경 요청을 요청하고 배포가 완료되는 즉시 변경 사항을 적용합니다. 요청이 손실되지 않도록 최대 5분 동안 대기할 수 있습니다. 클라우드에서 배포 가동 중단을 줄이기 위한 [정적 콘텐츠 배포 옵션](https://support.magento.com/hc/en-us/articles/360004861194-Static-content-deployment-options-to-reduce-deployment-downtime-on-Cloud)을 참조하세요.<!--MAGECLOUD-2169-->

- ![새 아이콘](../../assets/new.svg) **클라우드용 Docker 작성**—[Docker 설정 및 구성](https://developer.adobe.com/commerce/cloud-tools/docker/configure/) 프로세스를 다음과 같이 개선했습니다.

   - 환경 구성을 단순화하기 위해 PHP 구성 파일을 Docker ENV 형식으로 변환하는 명령—`docker:config:convert`을(를) 추가했습니다. 이제 PHP 구성 파일을 Docker 디렉토리에 복사하고 Docker ENV 파일로 변환합니다. [Launch Docker](https://developer.adobe.com/commerce/cloud-tools/docker/configure/).<!--MAGECLOUD-2359--> 보기

   - 이제 Adobe Commerce on cloud infrastructure 설치 프로세스에서 읽기 전용 및 읽기-쓰기 파일 시스템에 모두 배포를 지원하여 클라우드 파일 시스템을 보다 면밀하게 에뮬레이션할 수 있습니다. [도커 구성](https://developer.adobe.com/commerce/cloud-tools/docker/configure/)을 참조하세요.&lt;!—MAGECLOUD—2357—>

   - **Redis 서비스 지원** - Docker 컨테이너에 배포되고 Docker 설치에서 작동하도록 자동으로 구성된 Redis 이미지가 추가되었습니다.&lt;!—MAGECLOUD—2442—>

   - 이제 Cloud Docker [데이터베이스 컨테이너](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#database-container)를 사용할 때 DB 덤프 기능을 사용할 수 있습니다. 또한 `docker/mnt` 디렉터리를 사용하여 호스트 컴퓨터와 컨테이너 간에 [파일을 공유](https://developer.adobe.com/commerce/cloud-tools/docker/containers/#sharing-data-between-host-machine-and-container)할 수 있습니다.<!-- MAGECLOUD-2577 -->

   - **바니시 서비스 지원**— 도커 컨테이너에 자동으로 배포되는 바니시 이미지가 추가되었습니다. 배포 후 Adobe Commerce 모범 사례에 따라 Vannish를 수동으로 구성할 수 있습니다. [바니시 구성 및 사용](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/cache/varnish/config-varnish)을 참조하세요.&lt;!—MAGECLOUD—2358—>

   - 보안 사이트 액세스 - Adobe Commerce 스토어 및 관리 패널에 액세스하기 위해 SSL 지원이 추가되었습니다.&lt;!—MAGECLOUD—2360—>

- ![수정 아이콘](../../assets/fix.svg) **클라우드 인프라 확장 기능 지원의 Adobe Commerce 개선**—클라우드 인프라 Adobe Commerce의 guzzlehttp/guzzle 패키지에 대한 최소 버전 요구 사항이 [composer.json 파일](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/develop/overview)에서 버전 6.2로 다운그레이드되어 `ece-tools` 패키지가 더 많은 확장과 호환됩니다.<!--MAGECLOUD-2205-->

- ![새 아이콘](../../assets/new.svg) **빌드 단계 동안 Adobe Commerce 응용 프로그램에 사용자 지정 변경 내용 적용** - 배포를 위해 응용 프로그램을 패키징하기 전에 후크를 사용하여 생성된 정적 콘텐츠에 사용자 지정 변경 내용을 적용할 수 있도록 빌드 단계를 두 개의 별도 프로세스로 분할합니다. _build:generate_ 프로세스는 코드를 생성하고 패치를 적용하며 정적 콘텐츠를 생성합니다. _build:transfer_ 프로세스에서 생성된 코드와 정적 콘텐츠를 최종 대상으로 전송합니다. [응용 프로그램 후크](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/configure/app/properties/hooks-property).<!--MAGECLOUD-2363-->를 참조하십시오.

- ![아이콘 수정](../../assets/fix.svg) **환경 구성 확인** - 클라우드 인프라에서 Adobe Commerce을 빌드하고 배포하기 전에 버전 비호환성 및 구성 오류에 대해 고객에게 경고하기 위해 환경 구성의 유효성 검사를 개선했습니다.

   - 지원되지 않거나 더 이상 사용되지 않는 환경 변수 및 값을 식별하기 위해 버전별 유효성 검사를 추가했습니다.<!--MAGECLOUD-2183-->

   - 사용자에게 Elasticsearch 구성 문제에 대해 경고하기 위한 Elasticsearch 호환성 검사를 추가했습니다. 이제 서버의 Elasticsearch 서비스 버전이 Adobe Commerce과 호환되지 않는 경우 배포가 실패합니다. 이전에는 Elasticsearch 버전이 호환되지 않더라도 배포가 성공하여 사이트 배포 후 제품 카탈로그 문제가 발생했습니다.<!--MAGECLOUD-2389-->

     [지원 티켓을 제출](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/develop/deploy/best-practices)하여 Elasticsearch을 호환 가능한 버전으로 업그레이드하거나 Adobe Commerce 구성을 변경하여 Elasticsearch PHP 클라이언트의 호환 가능한 버전을 지정하여 비호환성을 해결할 수 있습니다.

      - Adobe Commerce 버전 2.1.x에서 2.2.2로 업그레이드하는 경우 Elasticsearch을 버전 2.4로 업그레이드하십시오.

      - Adobe Commerce 버전 2.2.3 이상의 경우 Elasticsearch을 버전 5.2로 업그레이드하십시오.

      - Elasticsearch 1.x 또는 2.x가 있고 업그레이드하지 않으려면 composer.json의 Adobe Commerce Elasticsearch PHP 클라이언트 버전 요구 사항을 `"elasticsearch/elasticsearch": "~2.0"`(으)로 업데이트하십시오.

   - 빌드, 배포 및 배포 후 단계 중에 충돌을 야기할 수 있는 구성 설정을 식별하기 위해 환경 변수의 유효성 검사를 개선했습니다. 예를 들어, 정적 콘텐츠 배포에 대한 전역 설정이 빌드 또는 배포 단계의 설정과 충돌하는 경우 설치 및 업그레이드 프로세스 중에 경고 메시지가 표시됩니다.<!--MAGECLOUD-2156-->

- ![수정 아이콘](../../assets/fix.svg) **환경 변수 업데이트**—다음 환경 변수를 변경했습니다.

   - **[SKIP_VARIABLES_MINIFICATION 전역 HTML](../environment/variables-global.md#skip_html_minification)**—온디맨드 HTML 콘텐츠 축소를 활성화하기 위해 기본값을 `true`(으)로 변경했습니다. 스테이징 및 프로덕션 환경에 배포할 때 가동 중지 시간을 최소화합니다. 이 구성은 가동 중지 시간이 없는 배포에 필요합니다.<!--MAGECLOUD-2435-->

   - **[CLEAN_STATIC_FILES 배포 변수](../environment/variables-deploy.md#clean_static_files)** - CLEAN_STATIC_FILES 환경 변수 설정을 기반으로 빌드 단계 동안 생성된 정적 콘텐츠에 대한 정리 정적 파일 처리를 관리하는 기능이 추가되었습니다. 이전에는 빌드 단계에서 생성된 정적 콘텐츠 파일이 항상 정리되었습니다.<!--MAGECLOUD-1506-->

- ![수정 아이콘](../../assets/fix.svg) **로깅**—로그 메시지를 개선하고 로그 크기를 줄이기 위해 다음과 같이 변경했습니다.

   - 이제 배포 실패 로그 항목에는 환경 구성에서 디버그 수준 로깅을 지정하지 않더라도 오류를 발생시키는 작업의 명령 출력이 포함됩니다. [`MIN_LOGGING_LEVEL`](../environment/variables-global.md#min_logging_level).<!--MAGECLOUD-2489--> 보기

   - 파일 시스템이 읽기 전용 상태이므로 일부 확장에 필요한 팩토리를 생성할 수 없을 때 발생하는 배포 실패에 대한 로깅을 추가했습니다.<!--MAGECLOUD-2209-->

   - 대화형 진행률 표시줄을 사용하는 설치 명령으로 인해 발생하는 배포 로그 크기를 줄이고 서식 문제를 해결했습니다.<!--MAGECLOUD-2402-->

   - 불필요한 세부 정보를 제거하고 일부 로그 구문에 대한 우선 순위 수준을 업데이트했습니다.<!--MAGECLOUD-2227-->

- ![수정 아이콘](../../assets/fix.svg) **크론별 수정 사항**—

   - 기록 수명에 대한 기본 cron 작업 구성 설정을 3d(4320분)에서 1h(60분)로 변경하여 cron 큐가 너무 빨리 채워질 때 발생할 수 있는 성능 문제 및 배포 오류를 방지했습니다.<!--MAGECLOUD-2427-->

   - 배포 단계 동안 cron 작업 관리 프로세스를 개선하여 데이터베이스 잠금 및 기타 중요한 문제를 방지했습니다. 이제 모든 cron 작업이 배포 단계 동안 중지되고 배포가 완료된 후 다시 시작됩니다.<!--MAGECLOUD-2445-->

   - Adobe Commerce 버전 2.2.0 이상에서 cron job이 중복된 consumer를 실행하지 않도록 cron job이 시작한 소비자를 예약하기 위한 잠금 메커니즘의 문제를 해결했습니다.<!--MAGECLOUD-2464-->

- ![수정 아이콘](../../assets/fix.svg) 배포 프로세스 중에 압축된 파일을 참조할 때 `not overwritten` 및 `no such file or directory` 오류가 발생하는 [정적 콘텐츠 압축 프로세스](../environment/variables-intro.md)(`gzip`) 문제를 해결했습니다.<!-- MAGECLOUD-2182-->

- ![수정 아이콘](../../assets/fix.svg) 저장소 로캘이 지정되지 않은 경우 덤프 프로세스 중에 `php ./vendor/bin/ece-tools config:dump` 명령이 `config.php` 파일에서 중복 섹션을 제거하지 못하는 문제를 해결했습니다. 이제 구성 파일을 환경 간에 쉽게 이동할 수 있습니다. `ece-tools` v2002.0.13으로 업데이트한 후 개선된 `config:dump` 명령을 사용하여 이전 `config.php` 파일을 다시 생성합니다. 저장소 설정에 대한 [구성 관리](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/configure-store/store-settings)를 참조하십시오.<!--MAGECLOUD-2444-->

- ![수정 아이콘](../../assets/fix.svg) `.magento/routes.yaml` 파일의 경로 구성이 [apex](https://blog.cloudflare.com/zone-apex-naked-domain-root-domain-cname-supp/) 도메인에서 `www` 도메인으로 리디렉션되는 경우 배포 단계에서 오류가 발생하는 문제를 해결했습니다.<!--MAGECLOUD-2556-->

- ![수정 아이콘](../../assets/fix.svg) 업데이트된 `.magento.env.yaml` 구성 파일에 `engine` 매개 변수를 포함하지 않으면 잘못된 병합 결과를 초래하는 [`SEARCH_CONFIGURATION`](../environment/variables-deploy.md#search_configuration) 변수에 대한 `_merge` 옵션 문제를 해결했습니다. 이제 병합 작업은 `engine` 매개 변수를 설정하지 않고도 업데이트된 `.magento.env.yaml`에서 지정한 값만 올바르게 덮어씁니다.<!--MAGECLOUD-2520-->

- ![수정 아이콘](../../assets/fix.svg) 클라우드 인프라 버전 2.2.1 이상에서 Adobe Commerce에 대한 세션 잠금을 잘못 활성화하여 성능 저하 및 시간 초과를 초래할 수 있는 Redis 구성 문제를 해결했습니다. 이제 기본적으로 세션 잠금이 비활성화됩니다. 이 문제는 Redis 세션 처리기 패키지의 v1.3.4에 도입된 `disable_locking` 매개 변수의 기본 동작이 변경되어 발생했습니다. [colinmollenhour/php-redis-session-abstract package](https://github.com/colinmollenhour/php-redis-session-abstract)를 참조하십시오.<!-- MAGECLOUD-2515-->

## v2002.0.12

- ![새 아이콘](../../assets/new.svg) **클라우드용 Docker 구성**—Cloud `ece-tools` 저장소에서 [Docker 구성](https://developer.adobe.com/commerce/cloud-tools/docker/configure/) 구성을 생성하는 명령—`docker:build`이 추가되었습니다.<!-- MAGECLOUD-2250 -->

- ![새 아이콘](../../assets/new.svg) **로케일 변경**—이제 구성 내보내기 및 가져오기 프로세스를 수행하지 않고도 스토어 로케일을 변경할 수 있습니다. 응용 프로그램이 프로덕션에 있고 SCD_ON_DEMAND가 활성화되어 있는 동안 저장소 및 관리자 로케일 옵션을 사용할 수 있습니다.<!-- MAGECLOUD-2019 -->

- ![새 아이콘](../../assets/new.svg) <!-- MAGECLOU-1998 -->**사이트 맵 및 로봇**—인프라를 변경하지 않고도 `robots.txt` 파일을 추가하고 단일 도메인 구성에 대한 `sitemap.xml` 파일을 생성하는 [워크플로](../store/robots-sitemap.md)를 만들었습니다.

- ![새 아이콘](../../assets/new.svg) **마법사**—클라우드 구성에 도움이 되는 두 개의 [마법사](../deploy/smart-wizards.md)가 추가되었습니다.<!-- MAGECLOUD-1910 -->

   - `ideal-state`—배포 가동 중지 시간을 최소화하도록 이상적인 상태 구성

   - `master-slave`—데이터베이스 및 Redis에 대한 부하 분산 구성

- ![새 아이콘](../../assets/new.svg) **모듈 새로 고침**—빌드 중에 자동으로 수행되는 방식과 유사한 방식으로 비활성화되었거나 명시적으로 활성화되지 않은 모듈을 활성화하기 위한 클라우드 명령—`module:refresh`을(를) 추가했습니다.<!-- MAGECLOUD-1521 -->

- ![새 아이콘](../../assets/new.svg) [CACHE](../environment/variables-deploy.md#cache_configuration), [SESSION](../environment/variables-deploy.md#session_configuration), [QUEUE](../environment/variables-deploy.md#queue_configuration) 및 [SEARCH](../environment/variables-deploy.md#search_configuration) 구성에서 `_merge` 옵션을 사용하여 서비스에 대한 구성을 병합하거나 덮어쓰는 기능을 추가했습니다.<!-- MAGECLOUD-2105 -->

- ![새 아이콘](../../assets/new.svg) **환경 구성 샘플 파일**—자세한 설명과 각 환경 변수에 대해 가능한 값을 포함하는 `.magento.env.yaml` 샘플 파일을 ECE-Tools 패키지에 추가했습니다.<!-- MAGECLOUD-1908 -->

   - 예기치 않은 값으로 인해 배포 프로세스에 오류가 발생하지 않도록 하는 `.magento.env.yaml` 구성에 대한 유효성 검사도 추가했습니다. 오류가 발생하면 `Environment configuration is not valid. Please correct .magento.env.yaml file with next suggestions:`<!-- MAGECLOUD-1907 -->(으)로 시작하는 자세한 오류 메시지가 표시됩니다.

- ![새 아이콘](../../assets/new.svg) 다음 [**환경 변수**](../environment/variables-intro.md)&#x200B;을 추가했습니다.

   - 이제 새로운 [SCD_MATRIX](../environment/variables-deploy.md#scd_matrix) 환경 변수를 사용하여 각 테마에 대해 여러 로케일을 정의할 수 있습니다. 이렇게 하면 배포할 테마 파일의 양이 줄어듭니다.<!-- MAGECLOUD-1501 -->

   - 배포를 위해 데이터베이스 연결을 사용자 지정하기 위해 [DATABASE_CONFIGURATION](../environment/variables-deploy.md#database_configuration) 환경 변수를 추가했습니다.<!-- MAGECLOUD-2047 -->

   - 새 [MIN_LOGGING_LEVEL](../environment/variables-global.md#min_logging_level) 변수는 코드를 변경하지 않고 모든 출력 스트림에 대한 최소 로깅 수준을 무시합니다.<!-- MAGECLOUD-2129 -->

- ![수정 아이콘](../../assets/fix.svg) 배포 단계와 배포 후 단계 사이에 작동 중지 시간이 발생하는 문제를 해결했습니다. 이제 배포 후 단계가 배포가 끝난 후 _즉시_ 시작됩니다.

- ![수정 아이콘](../../assets/fix.svg) `status = success`이(가) 있는 작업을 일정에서 정리하지 못한 문제를 해결했습니다.<!-- MAGECLOUD-2268 -->

- ![수정 아이콘](../../assets/fix.svg) 프로젝트의 사후 배포 단계 대신 배포 단계에서 캐시를 지우는 `post_deploy` 후크 문제를 해결했습니다.<!-- MAGECLOUD-2113 -->

- ![수정 아이콘](../../assets/fix.svg) 각 로케일에 대해 동일한 `js-translation.json` 파일을 생성한 여러 로케일에서 SCD를 사용할 때 발생하는 문제를 해결했습니다.<!-- MAGECLOUD-2034 -->

- ![수정 아이콘](../../assets/fix.svg) 테이블을 잠그지 않고 속도를 높이기 위해 `ece-tools` 패키지에서 `db:dump` 명령을 최적화했습니다.<!-- MAGECLOUD-2033 -->

## v2002.0.11

>[!NOTE]
>
>2.2.4 호환성을 위해서는 ECE-Tools 버전 2002.0.11이 필요합니다.

- ![새 아이콘](../../assets/new.svg) **비마스터 노드에 대한 읽기 전용 연결 구성**—이 릴리스에서는 읽기 전용 트래픽을 수신하도록 비마스터 노드에 대한 읽기 전용 연결을 구성하는 기능이 추가되었습니다([MariaDB](../environment/variables-deploy.md#mysql_use_slave_connection)용).<!--MAGECLOUD-143 -->[레디스](../environment/variables-deploy.md#redis_use_slave_connection) 및 <!--MAGECLOUD-143 -->용

- ![새 아이콘](../../assets/new.svg) **구성 마법사** - 정적 콘텐츠 배포에 대한 구성을 확인하는 데 도움이 되는 마법사를 추가했습니다. [스마트 마법사](../deploy/smart-wizards.md).<!--MAGECLOUD-1910 --> 보기

- ![새 아이콘](../../assets/new.svg) **Symfony 콘솔 지원**—Adobe Commerce 2.3에서 Symfony 콘솔 4에 대한 지원이 추가되었습니다.<!-- MAGECLOUD-1966 -->

- ![수정 아이콘](../../assets/fix.svg) **Cron 예약 최적화**—cron 관련 문제를 디버깅하는 데 도움이 되도록 큐 관리 및 향상된 로깅을 개선했습니다.<!-- MAGECLOUD-1607 -->

- `ADMIN_EMAIL` 또는 `ADMIN_USERNAME` 값이 기존 관리자 계정과 동일한 경우 ![수정 아이콘](../../assets/fix.svg) 배포 유효성 검사가 실패합니다.<!-- MAGECLOUD-1221 -->

- ![수정 아이콘](../../assets/fix.svg) 2.2.x 버전에 대한 SOLR 지원이 제거되었습니다. 2.1.x 버전에서는 SOLR을 활성화하는 기능이 유지됩니다.<!-- MAGECLOUD-1282 -->

- ![수정 아이콘](../../assets/fix.svg) 이제 PRO 프로젝트의 스테이징 및 프로덕션 환경을 처음 설치하면 각 환경에 속한 레코드를 식별하는 동안 충돌을 방지하기 위해 Elasticsearch에 서로 다른 인덱스 접두사가 포함됩니다.<!-- MAGECLOUD-1489 -->

- ![수정 아이콘](../../assets/fix.svg) 정적 콘텐츠 배포 중에 레거시 아키텍처의 빌드 단계를 중단하는 문제를 해결했습니다.<!-- MAGECLOUD-2021 -->

- ![수정 아이콘](../../assets/fix.svg) **Cron 관련 개선 사항**—cron 구현을 다시 작업함:<!-- MAGECLOUD-1607 -->

   - cron 대기열이 빠르게 채워지는 문제를 해결했습니다. 이제 오래된 크론 작업을 보다 안정적인 방식으로 지웁니다.

   - 별도의 스레드의 모든 작업이 일반 그룹 전에 시작되도록 cron 작업 시퀀스를 다시 구성했습니다.

   - cron 문제 디버깅을 더 잘 지원하도록 로깅이 개선되었습니다.

   - **참고**—이 릴리스는 많은 cron 관련 문제를 해결합니다. 현재 _m2-핫픽스_&#x200B;에서 크론 관련 패치를 사용하는 경우 제거하십시오.

- ![수정 아이콘](../../assets/fix.svg) **SCD별 개선 사항**—

   - _build_ 및 de_ploy 단계 모두에서 `VERBOSE_COMMANDS` 및 `SCD_COMPRESSION_LEVEL` 환경 변수를 사용할 수 있습니다.<!-- MAGECLOUD-1819 -->

   - `SCD_COMPRESSION_LEVEL` 환경 변수에 대해 예기치 않은 값이 발생할 때 임의의 오류로 인해 배포가 실패하는 문제를 해결했습니다. 의미 있는 알림을 제공하도록 구성 유효성 검사를 개선했습니다. 허용되는 값은 [`SCD_COMPRESSION_LEVEL`](../environment/variables-build.md#scd_compression_level)을(를) 참조하십시오.<!-- MAGECLOUD-2043 -->

   - 재정의가 예상대로 작동하도록 `SCD_COMPRESSION_LEVEL` 환경 변수 구성 흐름의 동작을 수정했습니다.<!-- MAGECLOUD-2044 -->

   - `.magento.env.yaml` 파일 _배포_ 단계에서 `SCD_THREADS` 환경 변수를 구성할 수 없는 문제를 해결했습니다.<!-- MAGECLOUD-2046 -->

## v2002.0.10

- ![새 아이콘](../../assets/new.svg) **정적 콘텐츠 배포(SCD)**—요청 시(요청 시) 정적 콘텐츠를 생성하는 새로운 대체 배포 프로세스가 있습니다. 이렇게 하면 가동 중지 시간이 줄어들고 가장 중요한 자산을 생성하여 캐시 처리가 향상됩니다.<!-- MAGECLOUD-1285 -->

   - **새 환경 변수**—요청한 경우 정적 콘텐츠를 생성하기 위해 `SCD_ON_DEMAND` 전역 환경 변수를 추가했습니다.<!-- MAGECLOUD-1738 -->

   - **배포 후 후크**—캐시를 지우고 캐시를 미리 로드(warms)하는 `.magento.app.yaml` 파일에 대해 `post_deploy` 후크를 추가했습니다. _후_ 컨테이너가 연결을 수락하기 시작합니다. [!DNL Cloud Console]의 스테이징 및 프로덕션 환경이 포함된 Pro 프로젝트와 시작 프로젝트에만 사용할 수 있습니다. 필수는 아니지만 `SCD_ON_DEMAND` 환경 변수와 함께 작동합니다.<!-- MAGECLOUD-1788 -->

- ![새 아이콘](../../assets/new.svg) **최적화**—배포 속도를 개선하고 파일 시스템의 로드를 줄이기 위해 배포 중 파일 이동 또는 복사를 최적화했습니다.<!-- MAGECLOUD-1842 -->

- ![새 아이콘](../../assets/new.svg) **배포 로깅**—배포 프로세스 중에 로그를 출력하기 위해 Syslog 및 GELF(Graylog Extended Log Format) 처리기를 활성화하는 기능이 추가되었습니다. [처리기 로깅](../environment/log-handlers.md).<!-- MAGECLOUD-1751 -->을 참조하세요.

- ![새 아이콘](../../assets/new.svg) 다음 [**환경 변수**](../environment/variables-intro.md)&#x200B;을 추가했습니다.

   - `CRYPT_KEY`—데이터베이스를 이동할 때 다른 환경에 암호화 키를 제공합니다.<!-- MAGECLOUD-1556 -->

   - `var/view_preprocessed` 디렉터리의 정적 보기 파일 복사를 건너뛰고 요청 시 축소된 HTML을 생성하는 `SKIP_HTML_MINIFICATION`—_전역_ 환경 변수입니다.<!-- MAGECLOUD-1621 and MAGECLOUD-1736-->

   - 요청한 경우 정적 콘텐츠를 생성할 `SCD_ON_DEMAND`—_전역_ 환경 변수입니다.<!-- MAGECLOUD-1738 -->

   - `WARM_UP_PAGES` - 캐시를 미리 로드하는 데 사용할 페이지를 나열할 수 있습니다. 새 [사후 배포 변수](../environment/variables-post-deploy.md)에서 사용할 수 있습니다.

- ![수정 아이콘](../../assets/fix.svg) 로컬로 적용된 패치가 인스턴스에서 배포를 중단하는 문제를 해결했습니다. 이제 ECE-Tools는 패치가 적용되었음을 감지할 수 있습니다.<!-- MAGECLOUD-982 -->

- ![수정 아이콘](../../assets/fix.svg) JavaScript 번들 및 GZIP 기능 간의 충돌을 해결했습니다. 이제 이러한 기능이 올바르게 작동합니다.<!-- MAGECLOUD-1735 -->

- ![수정 아이콘](../../assets/fix.svg) 이전 PHP 7.0.x 버전을 사용할 때 ECE-Tools CLI 명령이 실패하는 문제를 해결했습니다.<!-- MAGECLOUD-1744 -->

- ![수정 아이콘](../../assets/fix.svg) 다중 스레드에서 소규모 전략으로 정적 콘텐츠 배포를 방해하는 문제를 해결했습니다.<!-- MAGECLOUD-1822 -->

- ![수정 아이콘](../../assets/fix.svg) 관리자 로그인이 지연되는 Redis 세션 잠금 문제를 해결했습니다. 또한 2.1.x.<!-- MAGECLOUD-1853 -->에 대한 수정 사항을 사용할 수 있습니다.

## v2002.0.9

>[!NOTE]
>
>이 업데이트와 향후 모든 업데이트를 가져오려면 [클라우드 인프라의 Adobe Commerce 메타패키지를 업그레이드](../dev-tools/install-package.md#update-the-metapackage)해야 합니다.

- ![새 아이콘](../../assets/new.svg) **ece-tools**—이제 `ece-tools` 패키지가 Adobe Commerce 2.1.x.<!-- MAGECLOUD-1086 -->을(를) 지원합니다.

- ![새 아이콘](../../assets/new.svg) **Redis 구성**—이제 환경 변수를 사용하여 [Redis 구성](../environment/variables-deploy.md#cache_configuration) 페이지와 기본 캐시 및 Redis 세션 저장소를 구성할 수 있습니다.<!-- MAGECLOUD-1552 -->

- ![새 아이콘](../../assets/new.svg) **검색, AMQP 및 Redis 서비스 개선 사항**—이제 모든 서비스에 대해 동일한 방식으로 작동하도록 서비스 구성 흐름을 통합했습니다. 서비스를 구성하기 위해 `env.php` 파일을 수동으로 편집하는 기능은 더 이상 지원되지 않습니다. 대신 환경 변수 또는 `.magento.env.yaml` 파일을 사용해야 합니다.<!-- MAGECLOUD-1437 -->

- ![수정 아이콘](../../assets/fix.svg) **환경 변수**—

   - `env:STATIC_CONTENT_THREADS`은(는) 더 이상 사용되지 않으며 향후 릴리스에서 제거됩니다. 대신 [SCD_THREADS](../environment/variables-deploy.md#scd_threads)을(를) 사용하십시오.<!-- MAGECLOUD-1507 -->

   - `STATIC_CONTENT_EXCLUDE_THEMES` 환경 변수는 더 이상 사용되지 않습니다. 대신 `SCD_EXCLUDE_THEMES` 환경 변수를 사용해야 합니다.<!-- MAGECLOUD-1640 -->

- ![수정 아이콘](../../assets/fix.svg) **로깅**—기본 제공 패치 작업에 대한 로깅을 간소화했습니다.<!-- MAGECLOUD-1674 -->

- ![수정 아이콘](../../assets/fix.svg) 예기치 않은 동작이 발생하여 `developer` 모드 지원 및 `APPLICATION_MODE` 환경 변수를 제거했습니다.<!-- MAGECLOUD-1615 -->

- ![수정 아이콘](../../assets/fix.svg) Redis와 관련된 정적 콘텐츠 배포 실패를 초래하던 문제를 수정했습니다. 이제 다중 스레드 정적 콘텐츠 배포가 설계된 대로 실행됩니다.<!-- MAGECLOUD-1630 -->

- ![수정 아이콘](../../assets/fix.svg) 사용자가 `app:config:dump` 명령을 실행한 후 중요한 것으로 표시된 관리자의 구성 필드에 수정 내용을 저장할 수 없는 문제를 해결했습니다.<!-- MAGECLOUD-1175 -->

- ![수정 아이콘](../../assets/fix.svg) 일부 패키지와 충돌을 해결하기 위해 이전 버전의 `symfony/yaml`에 대한 지원을 추가했습니다. 이 패키지는 아직 최신 버전과 호환되지 않습니다.<!-- MAGECLOUD-1674 -->

## v2002.0.8

>[!NOTE]
>
>이 릴리스에서 `vendor/magento/ece-patches`을(를) `vendor/magento/ece-tools`과(와) 병합했습니다. `vendor/magento/ece-patches` 패키지를 별도로 업데이트할 필요가 없습니다.

**새로운 기능:**

- **개선된 로깅**<!-- MAGECLOUD-1253 -MAGECLOUD-1495 -->
   - 빌드 또는 배포 프로세스가 환경 변수를 재정의할 때 더 나은 설명을 제공하기 위해 로그 메시지를 개선했습니다.
   - 이제 설치 및 업그레이드 진행 상황을 실시간으로 볼 수 있습니다. `install_update.log` 파일을 추적하여 진행률을 확인합니다. For example,

     ```bash
     tail -f var/log/install_upgrade.log
     ```

- **새 cron 명령**—이제 [`cron:unlock`](https://support.magento.com/hc/en-us/articles/360033099451) 명령을 사용하여 중단된 모든 cron 작업을 중지하고 다시 시작하는 대신 특정 cron 작업의 잠금을 해제할 수 있습니다. 2.1.<!-- MAGECLOUD-1367 -->에서 사용할 수 없음

- **통합 구성 파일**—이제 [`.magento.env.yaml`](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/configure/env/configure-env-yaml) 파일을 사용하여 빌드 및 배포 단계를 구성할 수 있습니다.<!-- MAGECLOUD-1369 -->

- **구성 파일 백업**—배포 프로세스가 이제 배포 후 `app/etc/env.php` 및 `app/etc/config.php` 구성 파일의 백업을 자동으로 만듭니다. 백업에서 이러한 구성 파일을 복원하기 위해 [새 CLI 명령](https://support.magento.com/hc/en-us/articles/360033182871)도 추가했습니다.<!-- MAGECLOUD-1372 -->

- **유효성 검사 오류 문제 해결** - `config.php`에 정적 콘텐츠 배포에 필요한 데이터가 충분하지 않은 경우 유효성 검사 오류를 해결하기 위해 사용해야 하는 명령을 변경했습니다. 이전에는 오류 메시지에서 `bin/magento app:config:dump`을(를) 실행하도록 지시했습니다. 이제 `php ./vendor/bin/ece-tools config:dump`.<!-- MAGECLOUD-1491 -->을(를) 실행해야 합니다.

- **새 환경 변수**—이제 환경 변수를 사용하여 사용자 지정 [검색](../environment/variables-deploy.md#search_configuration) 및 [AMQP 기반](../environment/variables-deploy.md#queue_configuration) 서비스를 사이트에 연결할 수 있습니다.<!-- MAGECLOUD-1410 -->

- 스마트 패치를 구현했습니다. 이제 패키지는 클라우드 인프라 버전의 Adobe Commerce이 아닌 패치된 패키지 버전을 기반으로 패치를 적용합니다.<!--MAGECLOUD-1090-->

**해결된 문제:**

- 빌드 오류를 일으키는 로깅 문제를 해결했습니다.<!-- MAGECLOUD-1162 -->

- 대화형 모드로 배포를 실행할 때 시간 초과 예외가 발생하는 문제를 해결했습니다.<!-- MAGECLOUD-1389 -->

- 정적 콘텐츠 생성을 위해 컴팩트 전략을 사용할 때 오류가 발생하는 문제를 수정했습니다. 2.1.<!-- MAGECLOUD-1446 MAGECLOUD-1485-->에서 사용할 수 없음

- 배포 스크립트가 스테이징 및 프로덕션 환경을 제대로 식별하지 못하는 문제를 해결했습니다.<!-- MAGECLOUD-1493 -->

- 네트워크 문제로 인해 데이터베이스 연결이 중단되고 설치 및 업그레이드 프로세스 중에 오류가 발생하는 문제를 해결했습니다.<!-- MAGECLOUD-1520 -->

- `app:config:dump`을(를) 사용하여 구성 파일을 두 번 이상 내보낼 수 없는 문제를 해결했습니다. 2.1.<!--  MAGECLOUD-1567  -->에서 사용할 수 없음

- _관리자_ 로그인이 지연되는 Redis 세션 _잠금_ 문제를 해결했습니다. 2.1.<!--  MAGECLOUD-1582  -->에서 사용할 수 없음

- 다른 Composer 기반 패치 모듈과 충돌하는 버전 관리와 관련된 구현 문제를 해결했습니다.<!-- MAGECLOUD-1450 -->

- 가져오는 동안 PHP 메모리 문제를 발생시키는 문제를 해결했습니다.<!-- MAGECLOUD-1310 -->

- 패치를 제거했습니다. 클라우드 인프라 2.2.1에서 Adobe Commerce을 지원하도록 `colinmollenhour/credis` v1.6에서 버그를 수정했습니다. 2.1.<!-- MAGECLOUD-1033 -->에서 사용할 수 없음

## v2002.0.7

**해결된 문제:**

- JavaScript 축소 충돌을 일으키는 문제를 해결하기 위해 `var/view_preprocessed` 심볼릭 링크를 제거했습니다.<!-- MAGECLOUD-1454 -->

## v2002.0.6

**해결된 문제:**

- 파일 또는 디렉터리 이름에 공백이 있을 때 `gzip` 오류가 발생하는 문제를 해결했습니다.<!-- MAGECLOUD-1413 -->

- 배포 스크립트가 모듈 종속성을 제대로 인식하고 활성화하지 못하는 문제를 해결했습니다.<!-- MAGECLOUD-1424 -->

## v2002.0.5

**새로운 기능:**

- **환경 변수를 사용하여 cron 소비자 구성** - 이제 새 `CRON_CONSUMERS_RUNNER` 환경 변수를 사용하여 cron 소비자를 구성할 수 있습니다.

- **구성 스캔**—이제 빌드/배포 프로세스 중에 중요한 구성 요소를 스캔하고 스캔이 실패할 경우 프로세스를 중지합니다. 이렇게 하면 사이트가 유지 관리 모드에 있기 때문에 불필요한 다운타임이 방지됩니다.

- **빌드/배포 알림**—모든 환경에서 빌드/배포 작업을 위해 [Slack 및/또는 전자 메일 알림을 설정](../environment/set-up-notifications.md)하는 데 사용할 수 있는 구성 파일을 추가했습니다.

- **정적 콘텐츠 압축** - 이제 빌드 및 배포 단계에서 [gzip](https://www.gnu.org/software/gzip/)을 사용하여 정적 콘텐츠를 압축합니다. 이 압축은 Fastly 압축과 함께 저장소 크기를 줄이고 배포 속도를 높이는 데 도움이 됩니다. 필요한 경우 [빌드 옵션](../environment/variables-build.md) 또는 [변수 배포](../environment/variables-deploy.md)를 사용하여 압축을 비활성화할 수 있습니다. 자세한 내용은 다음 항목을 참조하십시오.

   - [애플리케이션 환경 변수](../application/variables-property.md)

   - [정적 콘텐츠 배포 성능](../deploy/static-content.md)

   - [배포 프로세스](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/develop/deploy/best-practices)

- **구성 관리**—이제 빌드 단계 중에 `app/etc/config.php` 파일이 없는 경우 Git 저장소에 자동으로 생성됩니다. 자동 생성된 파일에는 모듈 및 확장 목록만 포함되어 있습니다. 파일이 이미 있으면 빌드 단계는 정상적으로 계속됩니다. 나중에 [구성 관리](../store/store-settings.md)를 수행하면 명령이 추가 단계를 수행하지 않고 파일을 업데이트합니다. 자세한 내용은 [배포 프로세스](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/develop/deploy/best-practices)를 참조하세요.

- **데이터베이스 덤프**—모든 환경에서 데이터베이스 덤프를 만들기 위한 `magento/ece-tools` CLI 명령을 추가했습니다. Pro 계획 프로덕션 환경의 경우 이 명령은 세 개의 고가용성 노드 중 하나에서만 덤프하므로 덤프 중에 다른 노드에 기록된 프로덕션 데이터는 복사되지 않을 수 있습니다. 프로덕션 환경에서 데이터베이스 덤프를 수행하기 전에 애플리케이션을 유지 관리 모드로 설정하는 것이 좋습니다. 자세한 내용은 [백업 관리](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/develop/storage/snapshots)를 참조하십시오.

- **크론 간격 제한 해제**—us-3, eu-3 및 ap-3 지역에 제공된 모든 환경의 기본 크론 간격은 1분입니다. 다른 모든 영역의 기본 크론 간격은 Pro 통합 환경의 경우 5분, Pro 스테이징 및 프로덕션 환경의 경우 1분입니다. 기존 cron 작업을 수정하려면 `.magento.app.yaml`에서 설정을 편집하거나 프로덕션/스테이징 환경에 대한 지원 티켓을 만드십시오. 자세한 내용은 [cron 작업 설정](../application/crons-property.md#set-up-cron-jobs)을 참조하세요.

**해결된 문제:**

- 배포 프로세스에서 정적 콘텐츠 배포 전에 `cache-clean` 작업을 호출하여 배포 시간이 길어지는 문제를 해결했습니다.<!-- MAGECLOUD-1327 -->

- 프로덕션 환경에 배포하는 정적 콘텐츠 생성 단계에서 오류가 발생하는 문제를 해결했습니다.<!-- MAGECLOUD-1322 -->

- 일부 `magento/ece-tools` 명령이 출력을 `stderr`.<!-- MAGECLOUD-1264 -->에 로깅하지 못하는 문제를 해결했습니다.

- `env.php`의 기본 URL 값이 포크된 분기에서 업데이트되지 않는 문제를 해결했습니다.<!-- MAGECLOUD-1242 -->

- `magento setup:install` 명령이 보안 기본 URL에 비보안 접두사(`http://`)를 추가하는 문제를 해결했습니다.<!-- MAGECLOUD-1171 -->

- 패치 오류로 인해 배포 오류가 발생하지 않는 문제를 해결했습니다.<!-- MAGECLOUD-1170 -->

- 패치를 적용할 수 없는 경우 `ece-tools`이(가) 실행을 중지하고 예외를 throw하지 못하는 문제를 해결했습니다.<!-- MAGECLOUD-1152 -->

- Admin에서 HTML 축소를 사용하도록 설정한 후 Storefront를 로드할 때 오류가 발생하는 문제를 해결했습니다.<!-- MAGECLOUD-1138 -->

## v2002.0.4

**해결된 문제:**

- 이제 SSH 액세스를 통해 모든 환경에서 CLI 명령을 사용하여 [중단된 cron 작업을 수동으로 재설정](https://support.magento.com/hc/en-us/articles/360033099451)할 수 있습니다. 배포 프로세스가 cron 작업을 자동으로 재설정합니다.<!-- MAGECLOUD-1355 -->

## v2002.0.3

**해결된 문제:**

- Redis가 읽기/쓰기에 너무 오래 걸려 페이지 시간이 초과되는 문제를 해결했습니다. 이제 Redis 구성에서 `disable_locking` 매개 변수를 사용하여 이 문제를 방지할 수 있습니다.<!-- MAGECLOUD-1311 -->

## v2002.0.2

**해결된 문제:**

- 이제 [!DNL RabbitMQ] 구성 프로세스가 모든 필수 매개 변수를 자동으로 가져옵니다.<!-- MAGECLOUD-1246 -->

## v2002.0.1

**새로운 기능:**

- 클라우드 인프라의 Adobe Commerce은 이제 범위 및 [정적 콘텐츠 배포 전략](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-strategy)을 지원합니다. 정적 콘텐츠 배포 전략에 대해 기본 설정이 `quick`인 `–s` 매개 변수를 추가했습니다. 환경 변수 [SCD_STRATEGY](../environment/variables-deploy.md)을(를) 사용하여 빌드 및 배포 작업에서 이러한 전략을 사용자 지정하고 사용할 수 있습니다. 이 변수는 `standard`, `quick` 또는 `compact` 옵션을 지원합니다. `compact`을(를) 선택하면 `STATIC_CONTENT_THREADS` 값을 `1`(으)로 재정의하므로 특히 프로덕션 환경에서 배포가 느려질 수 있습니다. 2.1.<!--- MAGECLOUD-1057 -->에서 사용할 수 없음

- 빌드 및 배포 작업을 캡처하고 컴파일하기 위해 환경에 로그 파일을 만들었습니다. `var/log/cloud.log` 파일이 루트 응용 프로그램 디렉터리에 있습니다.<!--- MAGECLOUD-1014 & MAGECLOUD-1023 -->

**해결된 문제:**

- `ece-tools` 패키지가 클라우드 인프라 2.2.0 이상에서 Adobe Commerce과 호환되도록 리팩터링되었습니다.<!-- MAGECLOUD-919 & MAGECLOUD-1030 -->

- 패치를 적용할 수 없는 경우 `ece-tools`이(가) 실행을 중단하고 예외를 throw하지 못하는 문제를 해결했습니다.<!-- MAGECLOUD-1186 -->

- 빌드 중에 DI(종속성 삽입) 컴파일을 건너뛸 때 예외가 발생하는 문제를 해결했습니다.<!-- MAGECLOUD-1047 & MAGECLOUD-1049 -->

- 배포 프로세스가 `env.php` 파일의 사용자 지정 Redis 구성을 덮어쓰는 문제를 해결했습니다.<!-- MAGECLOUD-1019 -->

- 기본 보안 관리자가 사용하지 않도록 설정했기 때문에 리디렉션 루프를 발생시키는 문제를 해결했습니다.<!-- MAGECLOUD-1020 -->

## v2002.0.0

>[!WARNING]
>
>이 패키지는 더 이상 클라우드 인프라의 다른 Adobe Commerce 버전과 호환되지 않으므로 **을(를) 사용하면 안 됩니다**.

### 초기 릴리스

클라우드 인프라 2.2.0에서 Adobe Commerce용 `ece-tools`의 초기 릴리스.
