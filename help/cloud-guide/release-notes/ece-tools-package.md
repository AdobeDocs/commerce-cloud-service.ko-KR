---
title: ECE-Tools 릴리스 노트
description: ECE-Tools 패키지에 대한 최신 개선 사항 목록을 참조하십시오.
recommendations: noDisplay, catalog
last-substantial-update: 2024-01-16T00:00:00Z
exl-id: a464b940-c56e-4a7c-9948-559539e25361
source-git-commit: f2aa4aa183298d829d27c4641f21acef1514d312
workflow-type: tm+mt
source-wordcount: '2887'
ht-degree: 0%

---

# ECE-Tools 릴리스 노트

다음 [ece-tools](https://github.com/magento/ece-tools) package는 Cloud 프로젝트를 관리 및 배포하도록 설계된 스크립트 및 도구 세트입니다. 이 릴리스 노트는 의 일부인 이 패키지에 대한 최신 개선 사항을 설명합니다. [Commerce용 Cloud Tools 제품군](cloud-tools-suite.md).

>[!NOTE]
>
>다음을 참조하십시오 [ECE-Tools 업그레이드](../dev-tools/update-package.md) 의 최신 릴리스로 업데이트하는 방법에 대한 자세한 내용은 `ece-tools` 패키지.

다음 `ece-tools` 패키지는 다음 릴리스 버전 관리 시퀀스를 사용합니다. `200<major>.<minor>.<patch>`

릴리스 노트는 다음과 같습니다.

- ![새 아이콘](../../assets/new.svg) 새로운 기능
- ![고정 아이콘](../../assets/fix.svg) 수정 사항 및 향상된 기능

<!--Add release notes below-->

## v2002.1.17 {#latest}

릴리스 날짜: 2024년 1월 16일

- ![고정 아이콘](../../assets/fix.svg) **Elasticsearch 및 OpenSearch용 검사기**- LiveSearch가 활성화된 경우 검색 서비스를 설치하라는 잘못된 메시지를 표시하는 유효성 검사기를 수정했습니다.<!-- MCLOUD-10167 -->
- ![고정 아이콘](../../assets/fix.svg) **배포 경고**—비어 있지 않은 폴더에 대한 배포 경고가 발생하는 문제를 해결했습니다.<!-- MCLOUD-8958 -->

## v2002.1.16

릴리스 날짜: 2023년 10월 16일

- ![새 아이콘](../../assets/new.svg) **ENABLE_WEBHOOKS 전역 환경 변수**- 를 추가했습니다. [ENABLE_WEBHOOKS](../environment/variables-global.md#enable_webhooks) 외부 엔드포인트(예: App Builder 런타임 작업 또는 서드파티 인벤토리 관리 시스템)에 연결하기 위해 Commerce 웹후크와 함께 사용할 전역 변수입니다.

## v2002.1.15

릴리스 날짜: 2023년 7월 31일

- ![고정 아이콘](../../assets/fix.svg) **오류 코드**—오류 코드 스키마 및 오류 코드 문서 생성기가 업데이트되었습니다.
- ![고정 아이콘](../../assets/fix.svg) **사용자 지정 Redis 모델의 유효성 검사기**-사용자 지정 Redis 백엔드 모델에 대한 유효성 검사기를 업데이트했습니다. [캐시 구성에 대한 예를 참조하십시오](../environment/variables-deploy.md#cache_configuration).
- ![고정 아이콘](../../assets/fix.svg) **RabbitMQ의 유효성 검사기**-RabbitMQ 3.11에 대한 지원이 추가됨
- ![고정 아이콘](../../assets/fix.svg) **잘못된 링크를 수정했습니다.**- 시작 이메일 템플릿에서 온보딩 설명서에 대한 잘못된 링크를 수정했습니다.

## v2002.1.14

릴리스 날짜: 2023년 3월 10일

- ![새 아이콘](../../assets/new.svg) **PHP**- PHP 8.2에 대한 지원을 추가했습니다.
- ![새 아이콘](../../assets/new.svg) **서비스용 유효성 검사기**—MariaDB 10.6, Redis 7.0, PHP 8.2, OpenSearch 2.x 및 RabbitMQ 3.9와 같은 Commerce 2.4.6 필수 서비스의 유효성 검사기를 업데이트했습니다.
- ![고정 아이콘](../../assets/fix.svg) **ece-tools db-dump**—다음을 발생시킨 문제가 해결되었습니다. `db-dump` 작업이 너무 빨리 중지되었습니다.

## v2002.1.13

릴리스 날짜: 2022년 10월 27일

- ![새 아이콘](../../assets/new.svg) **Adobe Commerce에 대한 이벤트 Adobe I/O 지원이 추가되었습니다**. 이제 확장 개발자가 [Adobe I/O 이벤트](https://developer.adobe.com/events/docs/) 클라우드 인스턴스의 상거래 이벤트 정보를 용으로 작성된 해당 애플리케이션으로 전송하는 프레임워크 [Adobe 앱 빌더](https://developer.adobe.com/app-builder/docs/overview/). Adobe Commerce에 대한 Adobe I/O 이벤트는 Partner Preview에 있습니다.<!-- CEXT-932 -->
- ![새 아이콘](../../assets/new.svg) **OPcache 구성 유효성 검사기**- 제외된 경로에 대한 OPcache 구성을 확인하는 유효성 검사기를 추가했습니다.<!-- MCLOUD-9485 -->
- ![고정 아이콘](../../assets/fix.svg) **GraphQL 캐시 구성 문제를 해결했습니다**—이제 ECE-Tools가 GraphQL을 유지 `id_salt` 값: `cache` 에서 구성 `app/etc/env.php` 파일.<!-- MCLOUD-9486 -->

## v2002.1.12

릴리스 날짜: 2022년 9월 13일

- ![새 아이콘](../../assets/new.svg) **사용`synchronous_replication`**- ECE-Tools 세트 `synchronous_replication=>true` 다음에서 `app/etc/env.php` 다음과 같은 경우 파일 `MYSQL_USE_SLAVE_CONNECTION` 이(가) 활성화되었습니다. 이 구성은 Commerce 2.4.6+에만 영향을 줍니다. 다음을 참조하십시오. `MYSQL_USE_SLAVE_CONNECTION` 의 변수 설명 [변수 배포](../environment/variables-deploy.md#mysql_use_slave_connection).<!-- MCLOUD-9142 -->
- ![새 아이콘](../../assets/new.svg) **OpenSearch**- 를 구성하고 설정하는 기능이 추가되었습니다. `opensearch` 다음 Adobe Commerce 릴리스 2.4.6용 엔진 다음을 참조하십시오 [OpenSearch 서비스 설정](../services/opensearch.md).<!-- MCLOUD-9236 -->

## v2002.1.11

릴리스 날짜: 2022년 8월 4일

- ![고정 아이콘](../../assets/fix.svg) **ElasticSuite Validator 및 OpenSearch**- OpenSearch 설치 시 ElasticSuite 무결성 검사 유효성 검사기 문제를 해결했습니다.<!-- MCLOUD-8767 -->
- ![고정 아이콘](../../assets/fix.svg) **배포 명령 반환 유형**- 배포 명령의 반환 유형이 수정되었습니다.<!-- AC-3208 -->
- ![고정 아이콘](../../assets/fix.svg) **[!DNL RabbitMQ]새 Commerce 2.4.5 설치 문제**- 고정 [!DNL RabbitMQ] 새 Commerce 2.4.5 설치 충돌 문제.<!-- MCLOUD-9059 -->

## v2002.1.10

릴리스 날짜: 2022년 3월 31일

- ![고정 아이콘](../../assets/fix.svg) **Elasticsearch 7.10**- 7.10 버전의 Elasticsearch을 지원하도록 유효성 검사기를 업데이트했습니다.<!-- MCLOUD-8548 -->

## v2002.1.9

릴리스 날짜: 2022년 3월 10일

- ![새 아이콘](../../assets/new.svg) **OpenSearch**—Adobe Commerce 버전 2.4.4, 2.4.3-p2 및 2.3.7-p3에 대한 OpenSearch 지원이 추가되었습니다.<!-- MCLOUD-8296 -->
- ![새 아이콘](../../assets/new.svg) **PHP**- PHP 8.1에 대한 지원을 추가했습니다.
- ![고정 아이콘](../../assets/fix.svg) **교감/과정**—symfony/process ^5.3과의 호환성을 추가했습니다.<!-- MCLOUD-8283 -->

- ![새 아이콘](../../assets/new.svg) **소비자 다중 프로세스**- 을(를) 추가했습니다. `multiple_processes` 옵션을 사용하여 각 소비자에 대해 생성할 프로세스 수를 지정할 수 있습니다. 다음을 참조하십시오. `CRON_CONSUMERS_RUNNER` 의 변수 설명 [변수 배포](../environment/variables-deploy.md#cron_consumers_runner).<!-- MCLOUD-8295 -->
- ![새 아이콘](../../assets/new.svg) **OpenSearch 체계 및 전체 호스트 경로**- Elasticsearch 체계 및 전체 호스트 경로를 구성하는 기능이 추가되었습니다.
- ![고정 아이콘](../../assets/fix.svg) **AWS**—AWS S3 지원 방법을 변경했습니다.
- ![고정 아이콘](../../assets/fix.svg) **드라이버 옵션 판독기 수정**—에서 DB 연결에 대한 읽기 driver_options 구성을 추가했습니다. `env.php` 파일 기준 `ece-tools` 유효성 검사기용<!-- MCLOUD-8420 -->

## v2002.1.8

릴리스 날짜: 2021년 10월 25일

- ![새 아이콘](../../assets/new.svg) **대체 덤프 위치**- 를 추가했습니다. `--dump-directory` 옵션을 사용하여 DB 덤프의 대상 디렉토리를 선택할 수 있습니다. 지금 `/app/var/dump-main` 는 DB 덤프의 기본 대상 디렉토리입니다. 다음을 참조하십시오 [백업 관리: 데이터베이스 덤프](../storage/database-dump.md)<!-- MCLOUD-8063 -->
- ![고정 아이콘](../../assets/fix.svg) **모노로그 업데이트**- 필요한 최소 버전을 업데이트했습니다. `monolog` 패키지 대상 `^2.3`.<!-- ACMP-1263 -->
- ![고정 아이콘](../../assets/fix.svg) **Symfony 업데이트**—Adobe Commerce 2.4.4와 호환되도록 Symfony 종속성을 업데이트했습니다.<!-- ACMP-1533 -->
- ![고정 아이콘](../../assets/fix.svg) **기능/자동 로드 해결**—통합 환경에 배포하고 를 볼 때 발생하는 문제를 해결했습니다. `CRITICAL: [9] Required configuration is missed in autoload section of composer.json file.` 오류.<!-- https://github.com/magento/ece-tools/pull/799 -->

## v2002.1.7

릴리스 날짜: 2021년 7월 29일

**구성 업데이트**—

- ![새 아이콘](../../assets/new.svg) Composer 2.0에 대한 지원이 추가되었습니다.<!--MCLOUD-8003-->

- ![고정 아이콘](../../assets/fix.svg) **에 대한 작성기 요구 사항이 업데이트되었습니다.`symphony/console`**- ECE-Tools 업데이트 `composer.json` 버전 요구 사항 `symphony/console` 패키지: 다음을 발생시킨 문제를 해결하는 패키지 `di:compile` 다음 오류로 인해 실패하는 명령: `Incompatible argument type: Required type: int. Actual type: string`<!--MC-42919-->

- ![고정 아이콘](../../assets/fix.svg) 서비스 종료 소프트웨어 검사 업데이트(`eol.yaml`) Elasticsearch 7.9.x를 포함합니다.<!--MCLOUD-7938-->

## v2002.1.6

릴리스 날짜: 2021년 4월 20일

- ![새 아이콘](../../assets/new.svg) **인증 자격 증명 편집**—에서 Redis 인증 자격 증명을 읽는 기능이 추가되었습니다. `relationships` 배포 단계 중 속성.<!--MCLOUD-7694-->

- ![새 아이콘](../../assets/new.svg) **Elasticsearch 인증 자격 증명**—에서 Elasticsearch 인증 자격 증명을 읽는 기능이 추가되었습니다. `relationships` 배포 단계 중 속성.<!--MCLOUD-7695-->

- ![새 아이콘](../../assets/new.svg) **전용 세션 스토리지 서비스**- 추가됨 `redis-session` 세션 저장을 위한 두 번째 옵션으로 다음을 사용할 수 있습니다. `redis-session` 세션 정보를 저장하고 `redis` 더 나은 성능을 제공하기 위한 캐시 서비스입니다.<!--MCLOUD-7698-->

- ![새 아이콘](../../assets/new.svg) **더 이상 사용되지 않는 SPLIT_DB 메시지**- 더 이상 사용되지 않는 항목에 대한 유효성 검사기 경고 및 중요 메시지 추가 `SPLIT_DB` Adobe Commerce 2.4.2 및 Adobe Commerce 2.5.0에서 해당 제거 옵션.<!--MCLOUD-7806-->

- ![고정 아이콘](../../assets/fix.svg) **관계의 Elasticsearch 버전**—에서 올바른 버전의 Elasticsearch을 검색하도록 서비스 유효성 검사기를 수정했습니다. `relationships` Cloud Docker 및 통합 환경의 속성입니다.<!--MCLOUD-7572-->

- ![고정 아이콘](../../assets/fix.svg) **유연한 Redis 포트 유효성 검사**—이제 Redis에서 사용자 지정 캐시 연결의 포트를 확인할 수 있습니다. `server` URL. 예를 들어 다음과 같이 서버 URL에 포트 번호를 추가할 수 있습니다. `server: 'tcp://rfs-store-simple-page-cache:26379'`. 이렇게 하면 유효성 검사 오류가 `port` 옵션이 누락되었거나 잘못되었습니다.<!--MCLOUD-7722-->

- ![고정 아이콘](../../assets/fix.svg) **Adobe Commerce 2.4.2로 업그레이드**—사용자가 수동으로 실행해야 하는 문제가 해결되었습니다. `bin/magento setup:upgrade` Adobe Commerce 2.4.2로 업그레이드한 후 사이트를 운영 상태로 만들 수 있습니다.<!--MCLOUD-7776-->

## v2002.1.5

릴리스 날짜: 2021년 2월 1일

- ![새 아이콘](../../assets/new.svg) **원격 스토리지**- 를 추가했습니다. `REMOTE_STORAGE` AWS S3와 같은 스토리지 서비스를 사용하여 미디어 파일을 원격으로 저장하기 위해 클라우드 프로젝트를 활성화하는 환경 변수입니다. 이 구성 옵션은 ECE-Tools 패키지의 일부이지만 클라우드 인프라의 Adobe Commerce에서는 지원되지 않습니다.<!--MCLOUD-7153-->

- ![새 아이콘](../../assets/new.svg) **신규 `cloud:config:validate` 명령**- 명령이 추가되었습니다. `php vendor/bin/ece-tools cloud:config:validate` 을(를) 확인하려면 `.magento.env.yaml` 원격 클라우드 환경에 변경 사항을 푸시하기 전에 구성합니다.<!--MCLOUD-7120-->

- ![새 아이콘](../../assets/new.svg) **opcache 초기화**- 을(를) 위한 지원이 추가되었습니다. `opcache.enable_cli` 배포 후크를 실행하기 전에 OPcache를 플러시하는 PHP 옵션 이 구성은 캐시 구성을 재설정하여 각 배포에 현재 구성 설정이 적용되도록 합니다.<!--MCLOUD-7015-->

- ![새 아이콘](../../assets/new.svg) **Aurora DB 유효성 검사**- Aurora 데이터베이스와 호환되도록 데이터베이스 서비스 유효성 검사를 업데이트했습니다.<!--MCLOUD-7269-->

- ![새 아이콘](../../assets/new.svg) **새 SCD_NO_PARENT 환경 변수**- 를 추가했습니다. `SCD_NO_PARENT` 환경 변수(Adobe Commerce >=2.4.2용)를 사용하여 상위 테마에 대한 정적 콘텐츠 생성을 관리할 수 있습니다.<!--MCLOUD-7284-->

- ![고정 아이콘](../../assets/fix.svg) **메모리 제한 및 명령**—다음의 문제가 해결되었습니다. `php vendor/bin/ece-tools` 의 크기가 `cloud.log` 파일이 PHP memory_limit를 초과했습니다. 전체를 읽는 대신 `cloud.log` 파일을 메모리에 저장하면 이제 로그 파일에서 더 작은 데이터 부분만 읽습니다.<!--MCLOUD-7275--><!--MCLOUD-7400-->

- ![고정 아이콘](../../assets/fix.svg) **사용자 지정 데이터베이스 연결**- 고정 `.magento.env.yaml` 사용자 정의 데이터베이스 연결이 정의된 구성 문제 `DATABASE_CONFIGURATION` 이(가) 사용되지 않았습니다. 연결 설정이에 추가되지 않았습니다. `app/etc/env.php`.<!--MCLOUD-7426-->

- ![고정 아이콘](../../assets/fix.svg) **빈 오류 로그**—다음과 같은 경우 배포가 실패하는 문제가 해결되었습니다. `cloud.error.log` 이(가) 비어 있습니다.<!--MCLOUD-7296-->

- ![고정 아이콘](../../assets/fix.svg) **MariaDB 10.3 유효성 검사**- Adobe Commerce 2.3.6-p1에 대한 MariaDB 10.3의 유효성 검사가 수정되었습니다.<!--MCLOUD-7416-->

- ![고정 아이콘](../../assets/fix.svg) **캐시:플러시 로깅**—의 시작 및 마침을 나타내는 로그 항목이 개선되었습니다. `cache:flush` 단계.<!--MCLOUD-7503-->

## v2002.1.4

릴리스 날짜: 2020년 11월 19일

- ![고정 아이콘](../../assets/fix.svg) 에 검색 엔진이 지정되었을 때 배포 실패를 초래하던 문제를 수정했습니다. `SEARCH_CONFIGURATION` 환경 변수가 다음 이외의 값입니다. `elasticsearch`.<!--MCLOUD-7283-->

## v2002.1.3

릴리스 날짜: 2020년 11월 9일

**인프라 업데이트**—

- ![새 아이콘](../../assets/new.svg) 읽기 전용에 대한 ECE-Tools 지원 추가 `pub/static` 정적 컨텐츠가 빌드 단계에서 배포되도록 설정된 경우 디렉터리입니다.<!--MC-37699-->

- ![새 아이콘](../../assets/new.svg) 예정된 Adobe Commerce 릴리스와의 호환성을 위해 Elasticsearch 7.9 및 Redis 6에 대한 지원이 추가되었습니다.<!--MCLOUD-7191-->

- ![고정 아이콘](../../assets/fix.svg) ECE-Tools 업데이트 `composer.json` 품질 패치 도구에 필요한 종속성을 추가합니다. 이렇게 하면 ECE-Tools 패키지와 magento-cloud-patches 패키지 사이에 있었던 순환 종속성이 수정됩니다.<!--MCLOUD-6910-->

**유효성 검사 및 로그 개선 사항**—

- ![새 아이콘](../../assets/new.svg) 다음을 확인하기 위해 검색 엔진 유효성 검사 추가 `elasticsearch` 클라우드 인프라 2.4 이상의 Adobe Commerce에 대해 설정됩니다. 유효성 검사에 실패하면 배포가 중지되고 문제에 대한 수정 사항을 제안하는 심각한 오류 메시지가 표시됩니다. 다음을 참조하십시오 [심각한 오류, 배포 단계](../dev-tools/error-reference.md#deploy-stage).<!--MCLOUD-6937-->

- ![새 아이콘](../../assets/new.svg) Elasticsearch 서비스 버전과 Adobe Commerce 버전 간의 호환성을 확인하는 Elasticsearch 유효성 검사가 추가되었습니다.<!--MCLOUD-7193-->

- ![새 아이콘](../../assets/new.svg) Adobe Commerce Elasticsearch 모듈과 호환되는 Elasticsearch 버전을 표시하도록 Elasticsearch 호환성 오류 메시지가 업데이트되었습니다. 이제 오류 메시지는 Adobe Commerce 버전에서 사용하는 Elasticsearch 모듈과 호환되도록 클라우드 인프라에 설치할 특정 Elasticsearch 버전을 제공합니다. 다음을 참조하십시오 [경고 오류, 배포 단계](../dev-tools/error-reference.md#deploy-stage-1).<!--MCLOUD-6698-->

- ![새 아이콘](../../assets/new.svg) 경고 오류 추가됨 `2026` 및 `2027` 의 경우 잘못됨 `MAGE_MODE` 환경 변수 설정입니다. 유일하게 유효한 값은 다음과 같습니다. `production`. 이 수정 전, `MAGE_MODE` 을(를) (으)로 설정할 수 있습니다 `developer` 배포 오류가 없으면 나중에 읽기 전용 파일에 쓰려고 할 때만 오류가 발생합니다. 다음을 참조하십시오 [경고 오류](../dev-tools/error-reference.md#warning-errors).<!--MCLOUD-6708-->

- ![고정 아이콘](../../assets/fix.svg) 이러한 버전이 Adobe Commerce 버전과 호환되는지 확인하기 위해 Redis, RabbitMQ 및 MySQL 서비스에 대한 유효성 검사를 수정했습니다. 이러한 서비스의 유효한 버전은 이제 `cloud.log`.<!--MCLOUD-7098-->

- ![고정 아이콘](../../assets/fix.svg) 을(를) 업데이트함 `cloud.log` 캐시 준비 중 요청 전송에 대한 동시 요청 제한을 포함합니다. 이 값은 [WARM_UP_CONCURRENCY](../environment/variables-post-deploy.md#warm_up_concurrency) 배포 후 변수.<!--MCLOUD-5563-->

**CLI 명령 업데이트**—

- ![새 아이콘](../../assets/new.svg) 추가된 CLI 명령(`cloud:config:create` 및 `cloud:config:update`)을 클릭하여 새로 만들고 업데이트합니다 `.magento.env.yaml` 하나 이상의 빌드, 배포 및 배포 후 변수를 포함할 수 있는 구성이 있는 파일입니다. 다음을 참조하십시오 [CLI에서 구성 파일 생성](../environment/configure-env-yaml.md#create-configuration-file-from-cli).<!--MCLOUD-7072-->

**환경 변수 업데이트**—

- ![새 아이콘](../../assets/new.svg) 을(를) 추가함 [SKIP_COMPOSER_DUMP_AUTOLOAD](../environment/variables-build.md#skip_composer_dump_autoload) 변수를 빌드합니다. 변수 설정 `true` 응용 프로그램 실행 중지 `composer dump-autoload` commerce용 Cloud Docker 설치 시 명령 변수는 쓰기 가능한 파일 시스템(을 사용하여 테스트 및 개발을 위해 생성됨)이 있는 Commerce용 Cloud Docker 컨테이너에만 관련이 있습니다. `./vendor/bin/ece-docker build:compose --with-test`). 이러한 설치를 사용하면 `composer dump-autoload` 명령은 삭제된 파일의 파일에 액세스하려는 다른 명령을 실행할 때 오류를 방지합니다 `generated` 디렉토리.<!--MCLOUD-6939-->

## v2002.1.2

릴리스 날짜: 2020년 8월 5일

**유효성 검사 및 로그 개선 사항**—

- ![새 아이콘](../../assets/new.svg) 을(를) 추가함 `schema.error.yaml` 빌드, 배포 및 배포 후 프로세스 중에 발생할 수 있는 모든 오류 및 경고 알림과 오류 해결을 위한 제안 사항이 포함된 파일입니다. 이 파일의 정보는 _Commerce용 Cloud 안내서_. 다음을 참조하십시오 [ece-tools에 대한 오류 메시지 참조](../dev-tools/error-reference.md).<!--MCLOUD-5878-->

- ![새 아이콘](../../assets/new.svg) 클라우드 오류 로그를 변경했습니다(`/var/log/cloud.error.log`) 로그를 프로그래밍 방식으로 더 쉽게 구문 분석할 수 있도록 JSON 형식에 대한 항목을 추가했습니다.<!--MCLOUD-5879-->

- ![새 아이콘](../../assets/new.svg) 빌드, 배포 및 배포 후 처리에 대한 추가 오류 검사와 개선된 기존 검사를 추가했습니다.

   - 오류 코드 2026 - 빌드 단계 중에 생성된 일부 데이터를 탑재된 디렉터리에 복원하지 못했습니다.

   - 오류 코드 3004 - 백업 파일을 만들 수 없음

   - 오류 코드 102 - 다음과 같은 경우에 발생하는 문제에 대한 추가 검사가 추가되었습니다. `env.php` 파일에 쓸 수 없음 <!--MCLOUD-6221-->

- ![새 아이콘](../../assets/new.svg) 을(를) 추가함 **QUALITY_PATCH** 배포 프로세스 중에 적용할 하나 이상의 품질 패치를 지정하는 환경 변수입니다. 다음을 참조하십시오 [변수 작성](../environment/variables-build.md#quality_patches).<!--MCLOUD-6375-->

## v2002.1.1

릴리스 날짜: 2020년 6월 25일

- ![새 아이콘](../../assets/new.svg) **인프라 업데이트**—

   - ![새 아이콘](../../assets/new.svg) **로깅 개선 사항**—심각한 배포 오류에 종료 코드를 할당하고 오류 메시지 알림 및 로그 이벤트에 종료 코드를 노출하여 로그 추적 기능을 개선했습니다. 다음을 참조하십시오 [ece-tools에 대한 오류 메시지 참조](../dev-tools/error-reference.md).<!-- MCLOUD-5637, 5531-->

   - ![새 아이콘](../../assets/new.svg) 데이터베이스 덤프에 대한 프로세스가 개선되었습니다(`vendor/bin/ece-tools db-dump`)와 업데이트된 로그 메시지에서 데이터베이스 덤프 작업이 응용 프로그램을 유지 관리 모드로 전환하고 소비자 큐 프로세스를 중지하며 덤프가 시작되기 전에 cron 작업을 비활성화함을 명확히 설명했습니다.<!--MCLOUD-5324, MCLOUD-2062-->

   - ![고정 아이콘](../../assets/fix.svg) 스테이징 및 프로덕션 환경에 배포할 때 프로젝트 URL이 올바르게 업데이트되도록 문제를 해결했습니다. 자, `ece-tools` 를 사용하는 경로에 대한 URL을 사용합니다. `primary:true` 프로젝트 경로 구성에 설정된 속성입니다. 다음을 참조하십시오 [변수 배포](../environment/variables-deploy.md#update_urls).<!--MCLOUD-5883-->

   - ![고정 아이콘](../../assets/fix.svg) 을(를) 업데이트함 `generate.xml` 패치를 적용하는 시나리오 워크플로우를 빌드합니다. Adobe Commerce을 업데이트하려면 패치를 먼저 적용해야 다음을 초래할 수 있는 문제를 해결할 수 있습니다. `di:compile` 및 `module:refresh` 실패 단계.<!--MCLOUD-5941-->

   - ![고정 아이콘](../../assets/fix.svg) 설치 프로세스에서 를 잘못 반환하는 문제가 해결되었습니다. `Crypt key missing` 오류. 다음 `crypt/key` 설치 중에 값이 자동으로 생성됩니다.<!--MCLOUD-6120-->

- ![새 아이콘](../../assets/new.svg) **서비스 업데이트**—

   - ![새 아이콘](../../assets/new.svg) PHP 7.4 및 MariaDB 10.4에 대한 지원을 추가했습니다.<!--MAGECLOUD-2957, MCLOUD-4144-->

- ![새 아이콘](../../assets/new.svg) **환경 변수 업데이트**—

   - ![새 아이콘](../../assets/new.svg) 을(를) 추가함 **SCD_USE_BALER** 변수를 사용하여 Adobe Commerce on cloud infrastructure 빌드 프로세스 중에 Baler module for JavaScript 번들링을 활성화할 수 있습니다. 에서 변수 설명을 참조하십시오. [변수 작성](../environment/variables-build.md#scd_use_baler).<!-- MCLOUD-3456, MCLOUD-3457-->

   - ![새 아이콘](../../assets/new.svg) 을(를) 추가함 **REDIS_BACKEND** Adobe Commerce 2.3.5 이상의 Redis 캐시에 대한 Redis 백엔드 모델을 구성하는 환경 변수입니다. 에서 변수 설명을 참조하십시오. [변수 배포](../environment/variables-deploy.md#redis_backend).<!--MCLOUD-5721, MCLOUD-5865-->

- ![새 아이콘](../../assets/new.svg) **CLI 명령 업데이트**—

   - ![새 아이콘](../../assets/new.svg) 자세한 로깅을 위해 다음 CLI 명령을 옵션으로 업데이트했습니다.

      - `app:config:dump`
      - `app:config:import`
      - `module:enable`

     각 호출에 대한 로깅 수준은 의 구성에 의해 결정됩니다. [`VERBOSE_COMMANDS`](../environment/variables-build.md#verbose_commands) 의 변수 `.magento.env.yaml` 파일.<!--MCLOUD-3503-->

- ![새 아이콘](../../assets/new.svg) **유효성 검사 개선 사항**—

   - ![새 아이콘](../../assets/new.svg) **Elasticsearch 7.x 호환성 검사**—Elasticsearch 7.x 소프트웨어 호환성 검사에 대한 Elasticsearch 유효성 검사를 업데이트했습니다.<!--MCLOUD-5542-->

   - ![새 아이콘](../../assets/new.svg) **서비스 버전 및 EOL 유효성 검사 업데이트**—Adobe Commerce 2.4에 대해 설치된 서비스 버전을 확인하는 유효성 검사가 업데이트되었습니다. 요구 사항<!--MCLOUD-6144-->

   - ![고정 아이콘](../../assets/fix.svg) 다음의 배포 후 경고 메시지가 표시되는 경우에만 유효성 검사 문제를 해결했습니다. `post-deploy` 후크구성이에서 누락되었습니다. `.magento.app.yaml` 파일:

     ```text
     Your application does not have the "post_deploy" hook enabled.
     ```

     <!--MCLOUD-4077-->

   - ![새 아이콘](../../assets/new.svg) **Zend 프레임워크 종속 항목에 대한 유효성 검사가 추가되었습니다.**- Laminas 프로젝트로 마이그레이션된 Zend 프레임워크에 대한 작성기 종속성 유효성 검사를 추가했습니다. 필요한 종속성이 누락된 경우 빌드 프로세스 중에 다음 오류 메시지가 표시됩니다.

     ```text
     Required configuration is missing from the autoload section of the composer.json file.
     Add ("Laminas\Mvc\Controller\Zend\": "setupsrc/ Zend/Mvc/Controller/") to
     the `autoload -> psr-4` section. Then, re-run the "composer update" command locally, and
     commit the updated composer.json and composer.lock files.
     ```

     다음을 참조하십시오 [Zend 프레임워크 종속성 확인](../development/commerce-version.md#verify-zend-framework-composer-dependencies).<!--MCLOUD-4094-->

   - ![새 아이콘](../../assets/new.svg) **에 대한 유효성 검사가 추가됨 `env.php` 파일 및 데이터**- 다음에 대한 검사를 추가했습니다. `env.php` 설치 및 업그레이드 프로세스 중 파일 및 데이터.<!--MCLOUD-5991-->

      - 다음과 같은 경우 `env.php` 파일이 설치에서 누락되었습니다. `crypt/key` 값이 다음에 지정되지 않음 `.magento.app.yaml` 파일이 배포에 실패하고 다음 알림이 표시됩니다.

        ```text
        The crypt/key key value does not exist in the ./app/etc/env.php file or the CRYPT_KEY cloud environment variable``Missing crypt key for upgrading Magento`.
        ```

      - 설치에 이 포함되어 있지 않으면 `env.php` 파일 또는 구성에 캐시 유형이 하나만 포함되어 있는 경우 `cron:enable` 업그레이드 프로세스 중에 명령을 실행하여 모두로 파일을 복원합니다. `cache_types`. 다음 알림이 로그에 추가됩니다.

        ```text
        Magento state indicated as installed but configuration file app/etc/env.php was empty or did not exist.
        Required data will be restored from environment configurations and from the .magento.env.yaml file.
        ```

## v2002.1.0

릴리스 날짜: 2020년 2월 6일

- ![새 아이콘](../../assets/new.svg) **인프라 업데이트**—

   - ![새 아이콘](../../assets/new.svg) **Cloud Docker for Commerce에 별도의 패키지를 추가했습니다.**- Docker 패키지를 `ece-tools` 코드 품질을 유지하고 독립 릴리스를 제공하기 위한 패키지입니다. 관련 업데이트 및 수정 사항 `ece-tools` 에서 관리됩니다. [magento-cloud-docker](https://github.com/magento/magento-cloud-docker) GitHub 리포지토리.<!--MAGECLOUD-2927-->

   - ![새 아이콘](../../assets/new.svg) **업데이트된 패치 기능**- ECE-Tools 패키지에서 별도의 패키지로 패치 기능을 이동했습니다. [magento-cloud-패치](https://github.com/magento/magento-cloud-patches) 패키지. 배포 중, `ece-tools` 는 새 패키지를 사용하여 패치를 적용합니다. 다음을 참조하십시오 [Cloud 패치 릴리스 정보](cloud-patches.md).<!--MAGECLOUD-4567-->

   - ![새 아이콘](../../assets/new.svg) **업데이트된 작성기 종속성**- 를 업데이트했습니다. `composer.json` 다음에 대한 종속성이 있는 클라우드 인프라의 Adobe Commerce 파일 `magento/magento-cloud-docker` 패키지. 자, `ece-tools` 의 모든 패키지에 대한 종속성 포함 [`Cloud Tools Suite for Commerce`](cloud-tools-suite.md). 이러한 패키지는 설치 또는 업데이트 시 자동으로 설치 및 업데이트됩니다 `ece-tools`.

- ![새 아이콘](../../assets/new.svg) **시나리오 기반 배포 지원**—<!--MAGECLOUD-4101-->

   - ![새 아이콘](../../assets/new.svg) 이제 XML 구성 파일을 사용하여 빌드, 배포 및 배포 후 프로세스를 사용자 정의하여 기본 구성을 재정의하거나 사용자 정의할 수 있습니다.

   - ![새 아이콘](../../assets/new.svg) **을(를) 변경함 `hooks` 에서 구성`.magento.app.yaml`**- 다음을 업데이트했습니다. `hooks` 시나리오 기반 배포를 지원하는 구성 형식입니다. 이전 ECE-Tools 2002.0.x 릴리스의 레거시 형식은 여전히 지원됩니다. 그러나 시나리오 기반 배포 기능을 사용하려면 새 형식으로 업데이트해야 합니다. 다음을 참조하십시오 [시나리오 기반 배포](../deploy/scenario-based.md#add-scenarios-using-build-and-deploy-hooks).

>[!NOTE]
>
>ECE-Tools 버전 2002.1.0으로 업데이트하기 전에 [이전 버전과 호환 불가능한 변경 사항](backward-incompatible-changes.md) 클라우드 인프라 프로젝트 구성 또는 프로세스에서 Adobe Commerce을 업데이트해야 할 수 있는 변경 사항에 대해 알아봅니다.

- ![새 아이콘](../../assets/new.svg) **서비스 업데이트**—

   - ![새 아이콘](../../assets/new.svg) PHP 7.3에 대한 지원이 추가되었습니다.<!--MAGECLOUD-4022-->

   - ![새 아이콘](../../assets/new.svg) RabbitMQ 3.8에 대한 지원이 추가되었습니다.<!--MAGECLOUD-4674-->

   - ![새 아이콘](../../assets/new.svg) 각 서비스의 EOL 날짜에 대해 설치된 서비스 버전을 확인하는 유효성 검사가 추가되었습니다. 이제 고객은 서비스 버전이 EOL 날짜의 3개월 이내인 경우 알림을 받고, EOL 날짜가 과거인 경우 경고를 받습니다.<!--MAGECLOUD-4076-->

   - ![고정 아이콘](../../assets/fix.svg) 모든 환경에서 올바른 Elasticsearch 설정이 구성되도록 Elasticsearch 구성 문제를 해결했습니다.<!--MAGECLOUD-4474-->

>[!NOTE]
>
>다음을 참조하십시오 [서비스 버전](../services/services-yaml.md#service-versions) 클라우드 인프라의 Adobe Commerce에서 사용되는 서비스 목록 및 클라우드 템플릿과의 버전 호환성.

- ![새 아이콘](../../assets/new.svg) **환경 변수 업데이트**—

   - ![새 아이콘](../../assets/new.svg) 의 기능을 확장했습니다 `WARM_UP_PAGES` 특정 제품 페이지에 대한 캐시 미리 로드를 지원하는 환경 변수입니다. 에서 확장된 정의를 참조하십시오. [배포 후 변수](../environment/variables-post-deploy.md#warm_up_pages) 주제.<!--MAGECLOUD-4444-->

   - ![새 아이콘](../../assets/new.svg) 을(를) 추가함 `ERROR_REPORT_DIR_NESTING_LEVEL` 에서 오류 보고서 데이터 관리를 단순화하는 환경 변수 `<magento_root>/var/report/` 디렉토리. 에서 변수 설명을 참조하십시오. [변수 작성](../environment/variables-build.md#error_report_dir_nesting_level) 주제.

   - ![고정 아이콘](../../assets/fix.svg) 을(를) 제거함 `SCD_EXCLUDE_THEMES`, `STATIC_CONTENT_THREADS`,`DO_DEPLOY_STATIC_CONTENT`, 및 `STATIC_CONTENT_SYMLINK` 환경 변수. 다음을 참조하십시오 [이전 버전과 호환 불가능한 변경 사항](backward-incompatible-changes.md#environment-configuration-changes).<!--MAGECLOUD-4407, MAGECLOUD-3873-->

   - ![고정 아이콘](../../assets/fix.svg) 를 구성할 때 기본 구성이 예상대로 덮어쓰기되도록 Elastic Suite 구성 프로세스의 문제를 수정했습니다. `ELASTICSUITE_CONFIGURATION` 를 사용하지 않고 변수 배포 `_merge` 옵션을 선택합니다.<!--MAGECLOUD-4388-->

- ![새 아이콘](../../assets/new.svg) **CLI 명령 업데이트**—

   - ![새 아이콘](../../assets/new.svg) **새 cron 명령**—이제 를 사용하여 Adobe Commerce on cloud infrastructure 환경에서 cron 처리를 수동으로 관리할 수 있습니다. `cron:disable` 및 `cron:enable` 명령입니다. disable 명령을 사용하여 모든 활성 cron 프로세스를 중지하고 모든 cron 작업을 비활성화합니다. 준비가 되면 cron 작업을 다시 활성화하려면 enable 명령을 사용합니다. 다음을 참조하십시오 [cron 작업 비활성화](../application/crons-property.md#disable-cron-jobs).

   - ![새 아이콘](../../assets/new.svg) **향상된 오류 보고**—ECE-Tools 처리 중에 발생하는 CLI 명령 장애에 대한 로깅이 향상되었습니다.<!--MAGECLOUD-4849-->

   - ![새 아이콘](../../assets/new.svg) **더 이상 사용되지 않는 빌드 명령 제거**— 다음 빌드 명령을 제거했습니다. `m2-ece-build`, `m2-ece-deploy`, `m2-ece-scd-dump`, 및 이름이 변경됨 `ece-tools docker` 명령 `ece-docker`. 다음을 참조하십시오 [이전 버전과 호환 불가능한 변경 사항](backward-incompatible-changes.md)<!--MAGECLOUD-4392-->

- ![새 아이콘](../../assets/new.svg) 더 이상 사용되지 않는 항목을 제거했습니다. `build_options.ini` 파일이 있는 경우 빌드에 실패하도록 파일 및 추가된 유효성 검사. 사용 [.magento.env.yaml](../environment/configure-env-yaml.md) 빌드 옵션을 구성하는 파일입니다.

- ![고정 아이콘](../../assets/fix.svg) 에서 빌드 프로세스가 실패하는 문제를 해결했습니다. `config.php` 파일이 비어 있습니다.<!--MAGECLOUD-4127-->

## 2002.0.23

릴리스 날짜: 2020년 2월 27일

- ![고정 아이콘](../../assets/fix.svg) 과의 호환성 문제가 해결되었습니다. `ece-tools` 프로덕션 모드에서 온디맨드 정적 콘텐츠 생성을 성공적으로 완료할 수 없는 2002.0.x 릴리스입니다.

## 이전 릴리스

다음을 참조하십시오. [릴리스 정보 아카이브](cloud-release-archive.md) 버전 2002.0.22 및 이전 버전
