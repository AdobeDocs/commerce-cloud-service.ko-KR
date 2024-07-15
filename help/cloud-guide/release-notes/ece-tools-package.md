---
title: ECE-Tools 릴리스 노트
description: ECE-Tools 패키지에 대한 최신 개선 사항 목록을 참조하십시오.
recommendations: noDisplay, catalog
last-substantial-update: 2024-05-21T00:00:00Z
exl-id: a464b940-c56e-4a7c-9948-559539e25361
source-git-commit: 923e2114270df22e134e0676ac97f84d770bb226
workflow-type: tm+mt
source-wordcount: '2929'
ht-degree: 0%

---

# ECE-Tools 릴리스 노트

[ece-tools](https://github.com/magento/ece-tools) 패키지는 클라우드 프로젝트를 관리하고 배포하도록 설계된 스크립트 및 도구 세트입니다. 이 릴리스 노트는 [Commerce용 Cloud Tools 제품군](cloud-tools-suite.md)에 포함된 이 패키지에 대한 최신 개선 사항을 설명합니다.

>[!NOTE]
>
>`ece-tools` 패키지의 최신 릴리스로 업데이트하는 방법은 [ECE 도구 업그레이드](../dev-tools/update-package.md)를 참조하십시오.

`ece-tools` 패키지는 다음 릴리스 버전 관리 시퀀스를 사용합니다. `200<major>.<minor>.<patch>`

릴리스 노트는 다음과 같습니다.

- ![새 아이콘](../../assets/new.svg) 새 기능
- ![수정 아이콘](../../assets/fix.svg) 수정 사항 및 개선 사항

<!--Add release notes below-->

## v2002.1.19 {#latest}

릴리스 날짜: 2024년 5월 21일

- ![새 아이콘](../../assets/new.svg) **Lua**—CACHE_CONFIGURATION에 useLua 옵션이 추가되었습니다.
- ![수정 아이콘](../../assets/fix.svg) **유효성 검사기**—새 버전의 Redis 및 RabbitMQ에 대한 유효성 검사기를 업데이트했습니다.

## v2002.1.18

릴리스 날짜: 2024년 4월 8일

- ![새 아이콘](../../assets/new.svg) **PHP** — PHP 8.3에 대한 지원을 추가했습니다.
- ![수정 아이콘](../../assets/fix.svg) 유효성 검사기 - EOL 유효성 검사기를 업데이트했습니다.

## v2002.1.17

릴리스 날짜: 2024년 1월 16일

- ![수정 아이콘](../../assets/fix.svg) **Elasticsearch 및 OpenSearch용 유효성 검사기**—LiveSearch가 활성화된 경우 검색 서비스를 설치하는 잘못된 메시지를 생성한 유효성 검사기를 수정했습니다.<!-- MCLOUD-10167 -->
- ![수정 아이콘](../../assets/fix.svg) **배포 경고**—비어 있지 않은 폴더에 대한 배포 경고를 초래하는 문제가 해결되었습니다.<!-- MCLOUD-8958 -->

## v2002.1.16

릴리스 날짜: 2023년 10월 16일

- ![새 아이콘](../../assets/new.svg) **ENABLE_WEBHOOKS 전역 환경 변수**—App Builder 런타임 작업 또는 서드파티 인벤토리 관리 시스템과 같은 외부 엔드포인트에 연결하기 위해 Commerce 웹후크와 함께 사용할 [ENABLE_WEBHOOKS](../environment/variables-global.md#enable_webhooks) 전역 변수를 추가했습니다.

## v2002.1.15

릴리스 날짜: 2023년 7월 31일

- ![수정 아이콘](../../assets/fix.svg) **오류 코드**—오류 코드 스키마 및 오류 코드 문서 생성기가 업데이트되었습니다.
- ![수정 아이콘](../../assets/fix.svg) **사용자 지정 Redis 모델의 유효성 검사기**-사용자 지정 Redis 백엔드 모델의 유효성 검사기를 업데이트했습니다. [캐시 구성에 대한 예제를 참조하십시오](../environment/variables-deploy.md#cache_configuration).
- ![수정 아이콘](../../assets/fix.svg) **RabbitMQ 유효성 검사기** RabbitMQ 3.11에 대한 지원이 추가됨
- ![수정 아이콘](../../assets/fix.svg) **잘못된 링크를 수정했습니다**-시작 이메일 템플릿의 온보딩 설명서에 대한 잘못된 링크를 수정했습니다.

## v2002.1.14

릴리스 날짜: 2023년 3월 10일

- ![새 아이콘](../../assets/new.svg) **PHP**—PHP 8.2에 대한 지원을 추가했습니다.
- ![새 아이콘](../../assets/new.svg) **서비스에 대한 유효성 검사기**—Commerce 2.4.6 필수 서비스에 대한 유효성 검사기를 업데이트했습니다. MariaDB 10.6, Redis 7.0, PHP 8.2, OpenSearch 2.x 및 RabbitMQ 3.9.
- ![수정 아이콘](../../assets/fix.svg) **ece-tools db-dump** - `db-dump` 작업이 너무 빨리 중지되는 문제를 해결했습니다.

## v2002.1.13

릴리스 날짜: 2022년 10월 27일

- ![새 아이콘](../../assets/new.svg) **Adobe Commerce에 대한 Adobe I/O 이벤트에 대한 지원을 추가했습니다**. 이제 확장 개발자는 [Adobe I/O 이벤트](https://developer.adobe.com/events/docs/) 프레임워크를 사용하여 클라우드 인스턴스에서 [App Builder Adobe](https://developer.adobe.com/app-builder/docs/overview/)을 위해 작성된 응용 프로그램으로 Commerce 이벤트 정보를 보낼 수 있습니다. Adobe Commerce에 대한 Adobe I/O 이벤트가 파트너 미리 보기에 있습니다.<!-- CEXT-932 -->
- ![새 아이콘](../../assets/new.svg) **OPcache 구성에 대한 유효성 검사기** - 제외된 경로에 대해 OPcache 구성을 확인하는 유효성 검사기를 추가했습니다.<!-- MCLOUD-9485 -->
- ![수정 아이콘](../../assets/fix.svg) **GraphQL 캐시 구성 문제를 해결했습니다**—이제 ECE-Tools가 `app/etc/env.php` 파일의 `cache` 구성에서 GraphQL `id_salt` 값을 유지합니다.<!-- MCLOUD-9486 -->

## v2002.1.12

릴리스 날짜: 2022년 9월 13일

- ![새 아이콘](../../assets/new.svg) **활성화`synchronous_replication`**—ECE-Tools는 `MYSQL_USE_SLAVE_CONNECTION`이(가) 활성화되면 `app/etc/env.php` 파일에서 `synchronous_replication=>true`을(를) 설정합니다. 이 구성은 Commerce 2.4.6+에만 영향을 줍니다. [변수 배포](../environment/variables-deploy.md#mysql_use_slave_connection).<!-- MCLOUD-9142 -->에서 `MYSQL_USE_SLAVE_CONNECTION` 변수 설명을 참조하십시오.
- ![새 아이콘](../../assets/new.svg) **OpenSearch**—다음 Adobe Commerce 릴리스 2.4.6에 대해 `opensearch` 엔진을 구성하고 설정하는 기능이 추가되었습니다. [OpenSearch 서비스 설정](../services/opensearch.md).<!-- MCLOUD-9236 -->을 참조하십시오.

## v2002.1.11

릴리스 날짜: 2022년 8월 4일

- ![수정 아이콘](../../assets/fix.svg) **ElasticSuite Validator 및 OpenSearch**—OpenSearch가 설치될 때 ElasticSuite 무결성 검사 유효성 검사기 문제가 해결되었습니다.<!-- MCLOUD-8767 -->
- ![수정 아이콘](../../assets/fix.svg) **배포 명령에 대한 반환 형식**—배포 명령에 대한 반환 형식이 수정되었습니다.<!-- AC-3208 -->
- 새 Commerce 2.4.5 설치 시 ![아이콘 수정](../../assets/fix.svg) **[!DNL RabbitMQ]문제**—새 Commerce 2.4.5. 설치 시 [!DNL RabbitMQ] 충돌 문제가 해결되었습니다.<!-- MCLOUD-9059 -->

## v2002.1.10

릴리스 날짜: 2022년 3월 31일

- ![수정 아이콘](../../assets/fix.svg) **Elasticsearch 7.10**—7.10 버전의 Elasticsearch을 지원하도록 유효성 검사기를 업데이트했습니다.<!-- MCLOUD-8548 -->

## v2002.1.9

릴리스 날짜: 2022년 3월 10일

- ![새 아이콘](../../assets/new.svg) **OpenSearch**—Adobe Commerce 버전 2.4.4, 2.4.3-p2 및 2.3.7-p3에 대한 OpenSearch 지원이 추가되었습니다.<!-- MCLOUD-8296 -->
- ![새 아이콘](../../assets/new.svg) **PHP**—PHP 8.1에 대한 지원을 추가했습니다.
- ![수정 아이콘](../../assets/fix.svg) **symfony/process**—symfony/process ^5.3과의 호환성을 추가했습니다.<!-- MCLOUD-8283 -->

- ![새 아이콘](../../assets/new.svg) **소비자 다중 프로세스**—각 소비자에 대해 생성할 프로세스 수를 지정할 수 있도록 `multiple_processes` 옵션을 추가했습니다. [변수 배포](../environment/variables-deploy.md#cron_consumers_runner).<!-- MCLOUD-8295 -->에서 `CRON_CONSUMERS_RUNNER` 변수 설명을 참조하십시오.
- ![새 아이콘](../../assets/new.svg) **OpenSearch 체계 및 전체 호스트 경로**—Elasticsearch 체계 및 전체 호스트 경로를 구성하는 기능이 추가되었습니다.
- ![수정 아이콘](../../assets/fix.svg) **AWS S3**—AWS S3 사용 방법을 변경했습니다.
- ![수정 아이콘](../../assets/fix.svg) **드라이버 옵션 수정 판독기**—유효성 검사기에 대해 `ece-tools`이(가) `env.php` 파일에서 DB 연결에 대한 드라이버 옵션 읽기 구성을 추가했습니다.<!-- MCLOUD-8420 -->

## v2002.1.8

릴리스 날짜: 2021년 10월 25일

- ![새 아이콘](../../assets/new.svg) **대체 덤프 위치**—DB 덤프의 대상 디렉터리를 선택할 수 있도록 `--dump-directory` 옵션을 추가했습니다. 이제 `/app/var/dump-main`이(가) DB 덤프의 기본 대상 디렉터리입니다. [백업 관리: 데이터베이스 덤프](../storage/database-dump.md)<!-- MCLOUD-8063 -->를 참조하십시오.
- ![수정 아이콘](../../assets/fix.svg) **모노로그 업데이트**—`monolog` 패키지에 필요한 최소 버전을 `^2.3`(으)로 업데이트했습니다.<!-- ACMP-1263 -->
- ![수정 아이콘](../../assets/fix.svg) **Symfony 업데이트**—Adobe Commerce 2.4.4와 호환되도록 Symfony 종속성이 업데이트되었습니다.<!-- ACMP-1533 -->
- ![수정 아이콘](../../assets/fix.svg) **기능/자동 로드 해결**—통합 환경에 배포하고 `CRITICAL: [9] Required configuration is missed in autoload section of composer.json file.` 오류가 표시되는 문제를 해결했습니다.<!-- https://github.com/magento/ece-tools/pull/799 -->

## v2002.1.7

릴리스 날짜: 2021년 7월 29일

**구성 업데이트**—

- ![새 아이콘](../../assets/new.svg) 작성기 2.0에 대한 지원이 추가되었습니다.<!--MCLOUD-8003-->

- ![수정 아이콘](../../assets/fix.svg) **`symphony/console`**&#x200B;에 대한 작성기 요구 사항 업데이트됨—다음 오류로 인해 `di:compile` 명령이 실패하는 문제를 해결하기 위해 `symphony/console` 패키지에 대한 ECE-Tools `composer.json` 버전 요구 사항을 업데이트했습니다. `Incompatible argument type: Required type: int. Actual type: string`<!--MC-42919-->

- ![수정 아이콘](../../assets/fix.svg) Elasticsearch 7.9.x를 포함하도록 수명 종료 소프트웨어 검사(`eol.yaml`)를 업데이트했습니다.<!--MCLOUD-7938-->

## v2002.1.6

릴리스 날짜: 2021년 4월 20일

- ![새 아이콘](../../assets/new.svg) **Redis 인증 자격 증명**—배포 단계 동안 `relationships` 속성에서 Redis 인증 자격 증명을 읽는 기능이 추가되었습니다.<!--MCLOUD-7694-->

- ![새 아이콘](../../assets/new.svg) **Elasticsearch 권한 부여 자격 증명**—배포 단계 동안 `relationships` 속성에서 Elasticsearch 권한 부여 자격 증명을 읽는 기능이 추가되었습니다.<!--MCLOUD-7695-->

- ![새 아이콘](../../assets/new.svg) **전용 세션 저장소 서비스**—세션 저장소에 대한 두 번째 옵션으로 `redis-session`을(를) 추가했습니다. `redis-session` 서비스를 사용하여 세션 정보를 저장하고 캐시에 `redis` 서비스를 사용하여 성능을 개선할 수 있습니다.<!--MCLOUD-7698-->

- ![새 아이콘](../../assets/new.svg) **더 이상 사용되지 않는 SPLIT_DB 메시지**—Adobe Commerce 2.4.2의 더 이상 사용되지 않는 `SPLIT_DB` 옵션과 Adobe Commerce 2.5.0에서 해당 옵션에 대한 유효성 검사기 경고 및 중요 메시지가 추가되었습니다.<!--MCLOUD-7806-->

- ![수정 아이콘](../../assets/fix.svg) **관계의 Elasticsearch 버전**—Cloud Docker 및 통합 환경의 `relationships` 속성에서 올바른 Elasticsearch 버전을 검색하도록 서비스 유효성 검사기를 수정했습니다.<!--MCLOUD-7572-->

- ![고정 아이콘](../../assets/fix.svg) **유연한 Redis 포트 유효성 검사**—이제 Redis가 `server` URL에서 사용자 지정 캐시 연결에서 포트의 유효성을 확인할 수 있습니다. 예를 들어 다음과 같이 서버 URL에 포트 번호를 추가할 수 있습니다. `server: 'tcp://rfs-store-simple-page-cache:26379'`. 이렇게 하면 `port` 옵션이 없거나 잘못된 유효성 검사 오류를 방지할 수 있습니다.<!--MCLOUD-7722-->

- ![수정 아이콘](../../assets/fix.svg) **Adobe Commerce 2.4.2.3으로 업그레이드**—사용자가 Adobe Commerce 2.4.2로 업그레이드한 후 사이트를 작동시키기 위해 `bin/magento setup:upgrade`을(를) 수동으로 실행해야 하는 문제를 해결했습니다.<!--MCLOUD-7776-->

## v2002.1.5

릴리스 날짜: 2021년 2월 1일

- ![새 아이콘](../../assets/new.svg) **원격 저장소**—AWS S3와 같은 저장소 서비스를 사용하여 미디어 파일의 원격 저장을 위해 Cloud Projects를 사용하도록 설정하는 `REMOTE_STORAGE` 환경 변수를 추가했습니다. 이 구성 옵션은 ECE-Tools 패키지의 일부이지만 클라우드 인프라의 Adobe Commerce에서는 지원되지 않습니다.<!--MCLOUD-7153-->

- ![새 아이콘](../../assets/new.svg) **새 `cloud:config:validate` 명령**—변경 내용을 원격 클라우드 환경에 적용하기 전에 `.magento.env.yaml` 구성의 유효성을 검사하기 위해 명령 `php vendor/bin/ece-tools cloud:config:validate`을(를) 추가했습니다.<!--MCLOUD-7120-->

- ![새 아이콘](../../assets/new.svg) **opcache 초기화**—배포 후크를 실행하기 전에 OPcache를 플러시하는 `opcache.enable_cli` PHP 옵션에 대한 지원을 추가했습니다. 이 구성은 각 배포에 현재 구성 설정이 적용되도록 캐시 구성을 재설정합니다.<!--MCLOUD-7015-->

- ![새 아이콘](../../assets/new.svg) **Aurora DB 유효성 검사**—Aurora 데이터베이스와 호환되도록 데이터베이스 서비스 유효성 검사를 업데이트했습니다.<!--MCLOUD-7269-->

- ![새 아이콘](../../assets/new.svg) **새 SCD_NO_PARENT 환경 변수**—상위 테마에 대한 정적 콘텐츠 생성을 관리하기 위해 `SCD_NO_PARENT` 환경 변수(Adobe Commerce >=2.4.2용)를 추가했습니다.<!--MCLOUD-7284-->

- ![수정 아이콘](../../assets/fix.svg) **메모리 제한 및 명령**—`cloud.log` 파일의 크기가 PHP memory_limit를 초과할 경우 `php vendor/bin/ece-tools` 명령이 작동하지 않는 문제가 해결되었습니다. 이제 전체 `cloud.log` 파일을 메모리로 읽는 대신 로그 파일에서 더 작은 데이터 하위 집합만 읽습니다.<!--MCLOUD-7275--><!--MCLOUD-7400-->

- ![수정 아이콘](../../assets/fix.svg) **사용자 지정 데이터베이스 연결**—`DATABASE_CONFIGURATION`에 대해 정의된 사용자 지정 데이터베이스 연결이 사용되지 않는 `.magento.env.yaml` 구성 문제를 해결했습니다. 연결 설정을 `app/etc/env.php`에 추가하지 않았습니다.<!--MCLOUD-7426-->

- ![수정 아이콘](../../assets/fix.svg) **빈 오류 로그**—`cloud.error.log`이(가) 비어 있는 경우 배포가 실패하는 문제를 해결했습니다.<!--MCLOUD-7296-->

- ![수정 아이콘](../../assets/fix.svg) **MariaDB 10.3 유효성 검사**—Adobe Commerce 2.3.6-p1에 대한 MariaDB 10.3 유효성 검사가 수정되었습니다.<!--MCLOUD-7416-->

- ![수정 아이콘](../../assets/fix.svg) **캐시:플러시 로깅**—`cache:flush` 단계의 시작 및 완료를 나타내는 로그 항목이 개선되었습니다.<!--MCLOUD-7503-->

## v2002.1.4

릴리스 날짜: 2020년 11월 19일

- ![수정 아이콘](../../assets/fix.svg) `SEARCH_CONFIGURATION` 환경 변수에 지정된 검색 엔진이 `elasticsearch` 이외의 값인 경우 배포 오류가 발생하는 문제를 해결했습니다.<!--MCLOUD-7283-->

## v2002.1.3

릴리스 날짜: 2020년 11월 9일

**인프라 업데이트**—

- ![새 아이콘](../../assets/new.svg) 빌드 단계에서 정적 콘텐츠를 배포하도록 설정한 경우 읽기 전용 `pub/static` 디렉터리에 대한 ECE-Tools 지원이 추가되었습니다.<!--MC-37699-->

- ![새 아이콘](../../assets/new.svg) 예정된 Adobe Commerce 릴리스와의 호환성을 위해 Elasticsearch 7.9 및 Redis 6에 대한 지원이 추가되었습니다.<!--MCLOUD-7191-->

- ![수정 아이콘](../../assets/fix.svg) 품질 패치 도구에 필요한 종속성을 추가하도록 ECE 도구 `composer.json`을(를) 업데이트했습니다. 이렇게 하면 ECE-Tools 패키지와 magento-cloud-patches 패키지 사이에 있었던 순환 종속성이 수정됩니다.<!--MCLOUD-6910-->

**유효성 검사 및 로그 개선 사항**—

- ![새 아이콘](../../assets/new.svg) 검색 엔진 유효성 검사를 추가하여 `elasticsearch`이(가) cloud infrastructure 2.4 이상에서 Adobe Commerce에 대해 설정되었는지 확인했습니다. 유효성 검사에 실패하면 배포가 중지되고 문제에 대한 수정 사항을 제안하는 심각한 오류 메시지가 표시됩니다. [심각한 오류, 단계 배포](../dev-tools/error-reference.md#deploy-stage)를 참조하십시오.<!--MCLOUD-6937-->

- ![새 아이콘](../../assets/new.svg) Elasticsearch 서비스 버전과 Adobe Commerce 버전 간의 호환성을 확인하기 위해 Elasticsearch 유효성 검사를 추가했습니다.<!--MCLOUD-7193-->

- ![새 아이콘](../../assets/new.svg) Adobe Commerce Elasticsearch 모듈과 호환되는 Elasticsearch 버전을 표시하도록 Elasticsearch 호환성 오류 메시지를 업데이트했습니다. 이제 오류 메시지는 Adobe Commerce 버전에서 사용하는 Elasticsearch 모듈과 호환되도록 클라우드 인프라에 설치할 특정 Elasticsearch 버전을 제공합니다. [경고 오류, 단계 배포](../dev-tools/error-reference.md#deploy-stage-1)를 참조하십시오.<!--MCLOUD-6698-->

- ![새 아이콘](../../assets/new.svg)에 잘못된 `MAGE_MODE` 환경 변수 설정에 대한 경고 오류 `2026` 및 `2027`이(가) 추가되었습니다. 올바른 값은 `production`뿐입니다. 이 수정 전에 배포 오류 없이 `MAGE_MODE`을(를) `developer`(으)로 설정할 수 있습니다. 나중에 읽기 전용 파일에 쓰려고 할 때 오류가 발생합니다. [경고 오류](../dev-tools/error-reference.md#warning-errors).<!--MCLOUD-6708-->를 참조하십시오.

- ![수정 아이콘](../../assets/fix.svg) 이러한 버전이 Adobe Commerce 버전과 호환되는지 확인하기 위해 Redis, RabbitMQ 및 MySQL 서비스에 대한 유효성 검사를 수정했습니다. 이러한 서비스의 올바른 버전이 이제 `cloud.log`.<!--MCLOUD-7098-->에 작성되었습니다.

- ![수정 아이콘](../../assets/fix.svg) 캐시 준비 중 요청을 보내는 동시 요청 제한을 포함하도록 `cloud.log`을(를) 업데이트했습니다. 이 값은 [WARM_UP_CONCURRENCY](../environment/variables-post-deploy.md#warm_up_concurrency) 배포 후 변수에 구성됩니다.<!--MCLOUD-5563-->

**CLI 명령 업데이트**—

- ![새 아이콘](../../assets/new.svg) 하나 이상의 빌드, 배포 및 배포 후 변수를 포함할 수 있는 구성으로 `.magento.env.yaml` 파일을 만들고 업데이트하는 CLI 명령(`cloud:config:create` 및 `cloud:config:update`)을 추가했습니다. [CLI에서 구성 파일 만들기](../environment/configure-env-yaml.md#create-configuration-file-from-cli)를 참조하십시오.<!--MCLOUD-7072-->

**환경 변수 업데이트**—

- ![새 아이콘](../../assets/new.svg) [SKIP_COMPOSER_DUMP_AUTOLOAD](../environment/variables-build.md#skip_composer_dump_autoload) 빌드 변수를 추가했습니다. 변수를 `true`(으)로 설정하면 Commerce용 Cloud Docker 설치 중에 응용 프로그램에서 `composer dump-autoload` 명령을 실행할 수 없습니다. 변수는 쓰기 가능한 파일 시스템(`./vendor/bin/ece-docker build:compose --with-test`을 사용하여 테스트 및 개발용으로 생성됨)이 있는 Commerce 컨테이너용 Cloud Docker에만 관련이 있습니다. 이러한 설치를 통해 `composer dump-autoload` 명령을 건너뛰면 삭제된 `generated` 디렉터리의 파일에 액세스하려는 다른 명령을 실행할 때 오류가 발생하지 않습니다.<!--MCLOUD-6939-->

## v2002.1.2

릴리스 날짜: 2020년 8월 5일

**유효성 검사 및 로그 개선 사항**—

- ![새 아이콘](../../assets/new.svg) 오류를 해결하기 위한 제안 사항과 함께 빌드, 배포 및 배포 후 프로세스 중에 발생할 수 있는 모든 오류 및 경고 알림을 포함하는 `schema.error.yaml` 파일을 추가했습니다. 이 파일의 정보는 _Commerce용 클라우드 가이드_&#x200B;에서도 사용할 수 있습니다. [ece-tools에 대한 오류 메시지 참조](../dev-tools/error-reference.md)를 참조하십시오.<!--MCLOUD-5878-->

- ![새 아이콘](../../assets/new.svg) 클라우드 오류 로그(`/var/log/cloud.error.log`) 항목을 JSON 형식으로 변경했습니다.<!--MCLOUD-5879-->

- ![새 아이콘](../../assets/new.svg) 빌드, 배포 및 배포 후 처리에 대한 추가 오류 검사를 추가하고 기존 검사를 개선했습니다.

   - 오류 코드 2026 - 빌드 단계 중에 생성된 일부 데이터를 탑재된 디렉터리에 복원하지 못했습니다.

   - 오류 코드 3004 - 백업 파일을 만들 수 없음

   - 오류 코드 102 - `env.php` 파일에 쓸 수 없는 경우 발생하는 문제에 대한 추가 검사를 추가했습니다. <!--MCLOUD-6221-->

- ![새 아이콘](../../assets/new.svg) 배포 프로세스 중에 적용할 하나 이상의 품질 패치를 지정하기 위해 **QUALITY_PATCH** 환경 변수를 추가했습니다. [변수 빌드](../environment/variables-build.md#quality_patches).<!--MCLOUD-6375-->를 참조하십시오.

## v2002.1.1

릴리스 날짜: 2020년 6월 25일

- ![새 아이콘](../../assets/new.svg) **인프라 업데이트**—

   - ![새 아이콘](../../assets/new.svg) **로깅 개선 사항** - 종료 코드를 중요한 배포 오류에 할당하고 종료 코드를 오류 메시지 알림 및 로그 이벤트에 노출하여 로그 추적 기능이 개선되었습니다. [ece-tools에 대한 오류 메시지 참조](../dev-tools/error-reference.md)를 참조하십시오.<!-- MCLOUD-5637, 5531-->

   - ![새 아이콘](../../assets/new.svg) 데이터베이스 덤프에 대한 프로세스를 개선했습니다(`vendor/bin/ece-tools db-dump`). 데이터베이스 덤프 작업이 응용 프로그램을 유지 관리 모드로 전환하고 소비자 큐 프로세스를 중지하며 덤프가 시작되기 전에 cron 작업을 비활성화하도록 로그 메시지를 업데이트했습니다.<!--MCLOUD-5324, MCLOUD-2062-->

   - ![수정 아이콘](../../assets/fix.svg) 스테이징 및 프로덕션 환경에 배포할 때 프로젝트 URL이 올바르게 업데이트되는지 확인하는 문제를 해결했습니다. 이제 `ece-tools`은(는) 프로젝트 경로 구성에 설정된 `primary:true` 특성이 있는 경로의 URL을 사용합니다. [변수 배포](../environment/variables-deploy.md#update_urls).<!--MCLOUD-5883-->를 참조하세요.

   - ![수정 아이콘](../../assets/fix.svg) 패치를 적용하기 위한 `generate.xml` 빌드 시나리오 워크플로우를 업데이트했습니다. `di:compile` 및 `module:refresh` 단계가 실패할 수 있는 모든 문제를 해결하려면 Adobe Commerce을 업데이트하려면 패치를 먼저 적용해야 합니다.<!--MCLOUD-5941-->

   - ![수정 아이콘](../../assets/fix.svg) 설치 프로세스에서 `Crypt key missing` 오류를 잘못 반환하는 문제를 해결했습니다. 설치하는 동안 `crypt/key` 값이 자동으로 생성됩니다.<!--MCLOUD-6120-->

- ![새 아이콘](../../assets/new.svg) **서비스 업데이트**—

   - ![새 아이콘](../../assets/new.svg) PHP 7.4 및 MariaDB 10.4에 대한 지원이 추가되었습니다.<!--MAGECLOUD-2957, MCLOUD-4144-->

- ![새 아이콘](../../assets/new.svg) **환경 변수 업데이트**—

   - ![새 아이콘](../../assets/new.svg) 클라우드 인프라에서 Adobe Commerce 빌드 프로세스 중에 JavaScript 번들에 대한 Baler 모듈을 사용하도록 설정하기 위해 **SCD_USE_BALER** 변수를 추가했습니다. [빌드 변수](../environment/variables-build.md#scd_use_baler)에서 변수 설명을 참조하십시오.<!-- MCLOUD-3456, MCLOUD-3457-->

   - ![새 아이콘](../../assets/new.svg) **REDIS_BACKEND** 환경 변수를 추가하여 Adobe Commerce 2.3.5 이상의 Redis 캐시에 대한 Redis 백엔드 모델을 구성했습니다. [변수 배포](../environment/variables-deploy.md#redis_backend)에서 변수 설명을 참조하십시오.<!--MCLOUD-5721, MCLOUD-5865-->

- ![새 아이콘](../../assets/new.svg) **CLI 명령 업데이트**—

   - ![새 아이콘](../../assets/new.svg) 자세한 로깅을 위해 옵션으로 다음 CLI 명령을 업데이트했습니다.

      - `app:config:dump`
      - `app:config:import`
      - `module:enable`

     각 호출의 로깅 수준은 `.magento.env.yaml` 파일의 [`VERBOSE_COMMANDS`](../environment/variables-build.md#verbose_commands) 변수 구성에 의해 결정됩니다.<!--MCLOUD-3503-->

- ![새 아이콘](../../assets/new.svg) **유효성 검사 개선 사항**—

   - ![새 아이콘](../../assets/new.svg) **Elasticsearch 7.x 호환성 검사**—Elasticsearch 7.x 소프트웨어 호환성 검사에 대한 Elasticsearch 유효성 검사가 업데이트되었습니다.<!--MCLOUD-5542-->

   - ![새 아이콘](../../assets/new.svg) **서비스 버전 및 EOL 유효성 검사 업데이트**—Adobe Commerce 2.4에 대해 설치된 서비스 버전을 확인하는 유효성 검사가 업데이트되었습니다. 요구 사항.<!--MCLOUD-6144-->

   - ![수정 아이콘](../../assets/fix.svg) `.magento.app.yaml` 파일에서 `post-deploy` 후크 구성이 누락된 경우에만 다음 배포 후 경고 메시지가 표시되도록 유효성 검사 문제를 해결했습니다.

     ```text
     Your application does not have the "post_deploy" hook enabled.
     ```

     <!--MCLOUD-4077-->

   - ![새 아이콘](../../assets/new.svg) **Zend 프레임워크 종속 항목에 대한 유효성 검사 추가**—Laminas 프로젝트로 마이그레이션된 Zend 프레임워크에 대한 작성기 종속성 유효성 검사가 추가되었습니다. 필요한 종속성이 누락된 경우 빌드 프로세스 중에 다음 오류 메시지가 표시됩니다.

     ```text
     Required configuration is missing from the autoload section of the composer.json file.
     Add ("Laminas\Mvc\Controller\Zend\": "setupsrc/ Zend/Mvc/Controller/") to
     the `autoload -> psr-4` section. Then, re-run the "composer update" command locally, and
     commit the updated composer.json and composer.lock files.
     ```

     [Zend 프레임워크 종속성 확인](../development/commerce-version.md#verify-zend-framework-composer-dependencies).<!--MCLOUD-4094-->을 참조하십시오.

   - ![새 아이콘](../../assets/new.svg) **파일 및 데이터 `env.php`에 대한 유효성 검사가 추가됨**—설치 및 업그레이드 프로세스 중에 `env.php` 파일 및 데이터에 대한 검사가 추가되었습니다.<!--MCLOUD-5991-->

      - `env.php` 파일이 설치에서 누락되고 `crypt/key` 값이 `.magento.app.yaml` 파일에 지정되지 않은 경우 다음 알림과 함께 배포가 실패합니다.

        ```text
        The crypt/key key value does not exist in the ./app/etc/env.php file or the CRYPT_KEY cloud environment variable``Missing crypt key for upgrading Magento`.
        ```

      - 설치에 `env.php` 파일이 포함되어 있지 않거나 구성에 하나의 캐시 유형만 포함되어 있는 경우 업그레이드 프로세스 중에 `cron:enable` 명령이 실행되어 모든 `cache_types`(으)로 파일을 복원합니다. 다음 알림이 로그에 추가됩니다.

        ```text
        Magento state indicated as installed but configuration file app/etc/env.php was empty or did not exist.
        Required data will be restored from environment configurations and from the .magento.env.yaml file.
        ```

## v2002.1.0

릴리스 날짜: 2020년 2월 6일

- ![새 아이콘](../../assets/new.svg) **인프라 업데이트**—

   - ![새 아이콘](../../assets/new.svg) **Commerce용 Cloud Docker에 대한 개별 패키지 추가**—코드 품질을 유지하고 독립적인 릴리스를 제공하기 위해 `ece-tools` 패키지에서 Docker 패키지를 분리했습니다. `ece-tools`과(와) 관련된 업데이트 및 수정 사항은 [magento-cloud-docker](https://github.com/magento/magento-cloud-docker) GitHub 리포지토리에서 관리합니다.<!--MAGECLOUD-2927-->

   - ![새 아이콘](../../assets/new.svg) **패치 기능 업데이트됨**—ECE-Tools 패키지에서 별도의 [magento-cloud-patches](https://github.com/magento/magento-cloud-patches) 패키지로 패치 기능을 이동했습니다. 배포 중에 `ece-tools`은(는) 새 패키지를 사용하여 패치를 적용합니다. [클라우드 패치 릴리스 정보](cloud-patches.md).<!--MAGECLOUD-4567-->를 참조하세요.

   - ![새 아이콘](../../assets/new.svg) **Updated Composer 종속성**—클라우드 인프라의 Adobe Commerce에 대한 `composer.json` 파일을 `magento/magento-cloud-docker` 패키지에 대한 종속으로 업데이트했습니다. 이제 `ece-tools`에 [`Cloud Tools Suite for Commerce`](cloud-tools-suite.md)의 모든 패키지에 대한 종속성이 포함됩니다. 이러한 패키지는 `ece-tools`을(를) 설치하거나 업데이트할 때 자동으로 설치 및 업데이트됩니다.

- ![새 아이콘](../../assets/new.svg) **시나리오 기반 배포 지원**—<!--MAGECLOUD-4101-->

   - ![새 아이콘](../../assets/new.svg) 이제 XML 구성 파일을 사용하여 빌드, 배포 및 배포 후 프로세스를 사용자 지정하여 기본 구성을 재정의하거나 사용자 지정할 수 있습니다.

   - ![새 아이콘](../../assets/new.svg) **`.magento.app.yaml`**&#x200B;에서 `hooks` 구성을 변경했습니다. 시나리오 기반 배포를 지원하도록 `hooks` 구성 형식을 업데이트했습니다. 이전 ECE-Tools 2002.0.x 릴리스의 레거시 형식은 여전히 지원됩니다. 그러나 시나리오 기반 배포 기능을 사용하려면 새 형식으로 업데이트해야 합니다. [시나리오 기반 배포](../deploy/scenario-based.md#add-scenarios-using-build-and-deploy-hooks)를 참조하세요.

>[!NOTE]
>
>ECE-Tools 버전 2002.1.0으로 업데이트하기 전에 [뒤로 검토   호환되지 않는 변경 내용](backward-incompatible-changes.md)을(를) 입력하여 다음을 수행해야 할 수 있는 변경 내용을 알아보세요.   클라우드 인프라 프로젝트 구성 또는 프로세스에서 Adobe Commerce을 업데이트합니다.

- ![새 아이콘](../../assets/new.svg) **서비스 업데이트**—

   - ![새 아이콘](../../assets/new.svg) PHP 7.3에 대한 지원이 추가되었습니다.<!--MAGECLOUD-4022-->

   - ![새 아이콘](../../assets/new.svg) RabbitMQ 3.8에 대한 지원이 추가되었습니다.<!--MAGECLOUD-4674-->

   - ![새 아이콘](../../assets/new.svg) 각 서비스의 EOL 날짜에 대해 설치된 서비스 버전을 확인하는 유효성 검사가 추가되었습니다. 이제 고객은 서비스 버전이 EOL 날짜의 3개월 이내인 경우 알림을 받고, EOL 날짜가 과거인 경우 경고를 받습니다.<!--MAGECLOUD-4076-->

   - ![수정 아이콘](../../assets/fix.svg) 모든 환경에서 올바른 Elasticsearch 설정이 구성되었는지 확인하기 위해 Elasticsearch 구성 문제를 해결했습니다.<!--MAGECLOUD-4474-->

>[!NOTE]
>
>클라우드 인프라에서 Adobe Commerce에 사용되는 서비스 목록과 클라우드 템플릿과의 버전 호환성은 [서비스 버전](../services/services-yaml.md#service-versions)을 참조하십시오.

- ![새 아이콘](../../assets/new.svg) **환경 변수 업데이트**—

   - ![새 아이콘](../../assets/new.svg) 특정 제품 페이지에 대한 캐시 미리 로드를 지원하도록 `WARM_UP_PAGES` 환경 변수의 기능을 확장했습니다. [사후 배포 변수](../environment/variables-post-deploy.md#warm_up_pages) 항목에서 확장된 정의를 참조하십시오.<!--MAGECLOUD-4444-->

   - ![새 아이콘](../../assets/new.svg) `<magento_root>/var/report/` 디렉터리에서 오류 보고서 데이터 관리를 단순화하기 위해 `ERROR_REPORT_DIR_NESTING_LEVEL` 환경 변수를 추가했습니다. [변수 빌드](../environment/variables-build.md#error_report_dir_nesting_level) 항목에서 변수 설명을 참조하십시오.

   - ![수정 아이콘](../../assets/fix.svg)에서 `SCD_EXCLUDE_THEMES`, `STATIC_CONTENT_THREADS`,`DO_DEPLOY_STATIC_CONTENT` 및 `STATIC_CONTENT_SYMLINK` 환경 변수를 제거했습니다. [이전 버전과 호환되지 않는 변경 내용](backward-incompatible-changes.md#environment-configuration-changes)을 참조하십시오.<!--MAGECLOUD-4407, MAGECLOUD-3873-->

   - ![수정 아이콘](../../assets/fix.svg) `_merge` 옵션 없이 `ELASTICSUITE_CONFIGURATION` 배포 변수를 구성할 때 기본 구성이 예상대로 덮어쓰도록 Elastic Suite 구성 프로세스의 문제를 해결했습니다.<!--MAGECLOUD-4388-->

- ![새 아이콘](../../assets/new.svg) **CLI 명령 업데이트**—

   - ![새 아이콘](../../assets/new.svg) **새 cron 명령** - 이제 `cron:disable` 및 `cron:enable` 명령을 사용하여 클라우드 인프라 환경의 Adobe Commerce에서 cron 처리를 수동으로 관리할 수 있습니다. disable 명령을 사용하여 모든 활성 cron 프로세스를 중지하고 모든 cron 작업을 비활성화합니다. 준비가 되면 cron 작업을 다시 활성화하려면 enable 명령을 사용합니다. [cron 작업 사용 안 함](../application/crons-property.md#disable-cron-jobs)을 참조하세요.

   - ![새 아이콘](../../assets/new.svg) **향상된 오류 보고**—ECE-Tools 처리 중에 발생하는 CLI 명령 오류에 대한 로깅이 향상되었습니다.<!--MAGECLOUD-4849-->

   - ![새 아이콘](../../assets/new.svg) **더 이상 사용되지 않는 빌드 명령 제거**— `m2-ece-build`, `m2-ece-deploy`, `m2-ece-scd-dump` 빌드 명령을 제거하고 `ece-tools docker` 명령을 `ece-docker`(으)로 이름을 변경했습니다. [이전 버전과 호환되지 않는 변경 내용](backward-incompatible-changes.md)<!--MAGECLOUD-4392-->을 참조하십시오.

- ![새 아이콘](../../assets/new.svg) 더 이상 사용되지 않는 `build_options.ini` 파일을 제거하고 파일이 있는 경우 빌드에 실패하도록 유효성 검사를 추가했습니다. [.magento.env.yaml](../environment/configure-env-yaml.md) 파일을 사용하여 빌드 옵션을 구성합니다.

- ![수정 아이콘](../../assets/fix.svg) `config.php` 파일이 비어 있을 때 빌드 프로세스가 실패하는 문제를 해결했습니다.<!--MAGECLOUD-4127-->

## 2002.0.23

릴리스 날짜: 2020년 2월 27일

- ![수정 아이콘](../../assets/fix.svg) 프로덕션 모드에서 온디맨드 정적 콘텐츠 생성을 완료할 수 없는 `ece-tools` 2002.0.x 릴리스의 호환성 문제를 해결했습니다.

## 이전 릴리스

버전 2002.0.22 및 이전 버전에 대한 [릴리스 정보 보관](cloud-release-archive.md)을 참조하세요.
