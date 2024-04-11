---
title: ECE-Tools 패키지에 대한 오류 메시지
description: 클라우드 인프라 빌드, 배포 및 배포 후 프로세스에서 Adobe Commerce을 수행하는 동안 발생할 수 있는 오류 코드 및 메시지 목록을 참조하십시오.
recommendations: noDisplay
role: Developer
exl-id: d8cc8d49-32da-43cf-a105-aa56b5334000
source-git-commit: 9dda6fe7f6a9d6064436820a3c8426ec982b5230
workflow-type: tm+mt
source-wordcount: '2763'
ht-degree: 4%

---

# ECE-Tools용 오류 메시지

이 오류 메시지 참조는 클라우드 인프라 빌드, 배포 및 배포 후 프로세스에서 Adobe Commerce을 수행하는 동안 발생할 수 있는 오류를 해결하기 위한 정보를 제공합니다.

배포 중에 발생하는 모든 위기 및 경고 오류 메시지는 `var/log/cloud.log` 및 `/var/log/cloud.error.log` 파일. 클라우드 오류 로그 파일에는 최신 배포의 오류만 포함되어 있습니다. 빈 파일은 오류 없이 성공적으로 배포했음을 나타냅니다.

다음에서 `cloud.error.log` 각 항목의 형식은 보다 쉽게 구문 분석할 수 있도록 JSON 문자열로 지정됩니다.

```json
{"errorCode":1006,"stage":"build","step":"validate-config","suggestion":"No stores/website/locales found in config.php\n  To speed up the deploy process do the following:\n  1. Using SSH, log in to your Magento Cloud account\n  2. Run \"php ./vendor/bin/ece-tools config:dump\"\n  3. Using SCP, copy the app/etc/config.php file to your local repository\n  4. Add, commit, and push your changes to the app/etc/config.php file","title":"The configured state is not ideal","type":"warning"}
```

오류 메시지는 배포 단계인 빌드, 배포 및 사후 배포 중 하나로 분류됩니다. 각 섹션은 각 오류에 대한 다음 정보와 함께 관련 오류 목록을 제공합니다.

- **오류 코드**: 오류 메시지에 대한 Adobe Commerce 할당 식별자
- **단계**: 빌드, 배포 또는 배포 후 단계 중 오류가 발생했는지 여부를 나타냅니다
- **단계**: 오류를 반환할 수 있는 배포 시나리오의 단계를 나타냅니다. 다음과 같은 경우 _단계_ 열이 비어 있으면 이 오류는 여러 단계에서 또는 사전 처리 작업 중에 반환될 수 있는 일반적인 오류입니다. 다음을 참조하십시오 [시나리오 기반 배포](../deploy/scenario-based.md) 빌드, 배포 및 배포 후 단계에 대한 자세한 내용은 을 참조하십시오.
- **제안**: 문제를 해결하고 오류를 해결하기 위한 지침을 제공합니다
- **제목(오류 설명)**: 오류의 원인을 요약하는 설명입니다
- **유형**: 오류가 심각한 오류인지 또는 경고인지를 나타냅니다.

<!-- Note: The error code tables in this file are auto-generated from source code. To request changes to error code descriptions or suggestions, submit a GitHub issue to the magento/ece-tools repository. -->

## 심각한 오류

심각한 오류는 필요한 설정에 대한 구성이 잘못되었거나, 지원되지 않거나, 누락된 경우 등 배포 실패를 발생시키는 Commerce on cloud infrastructure 프로젝트 구성 문제를 나타냅니다. 배포하기 전에 구성을 업데이트하여 이러한 오류를 해결해야 합니다.

### 빌드 단계

| 오류 코드 | 빌드 단계 | 오류 설명(제목) | 제안된 작업 |
| - | - | - | - |
| 2 |  | 에 쓸 수 없음 `./app/etc/env.php` 파일 | 배포 스크립트는 `/app/etc/env.php` 파일. 파일 시스템 권한을 확인합니다. |
| 3 |  | 구성에 정의되지 않음 `schema.yaml` 파일 | 구성에 정의되지 않음 `./vendor/magento/ece-tools/config/schema.yaml` 파일. 구성 변수 이름이 올바르고 정의되어 있는지 확인합니다. |
| 4 |  | 구문 분석 실패 `.magento.env.yaml` 파일 | 다음 `./.magento.env.yaml` 파일 형식이 잘못되었습니다. YAML 파서를 사용하여 구문을 확인하고 오류를 수정합니다. |
| 5 |  | 을(를) 읽을 수 없음 `.magento.env.yaml` 파일 | 을(를) 읽을 수 없음 `./.magento.env.yaml` 파일. 파일 권한을 확인합니다. |
| 6 |  | 을(를) 읽을 수 없음 `.schema.yaml` 파일 | 을(를) 읽을 수 없음 `./vendor/magento/ece-tools/config/magento.env.yaml` 파일. 파일 권한 확인 및 재배포(`magento-cloud environment:redeploy`). |
| 7 | 모듈 새로 고침 | 에 쓸 수 없음 `./app/etc/config.php` 파일 | 배포 스크립트는 `/app/etc/config.php` 파일. 파일 시스템 권한을 확인합니다. |
| 8 | validate-config | 을(를) 읽을 수 없음 `composer.json` 파일 | 을(를) 읽을 수 없음 `./composer.json` 파일. 파일 권한을 확인합니다. |
| 9 | validate-config | 다음 `composer.json` 파일에 필수 자동 로드 섹션이 없습니다. | 필수 `autoload` 섹션이 다음에서 누락됨: `composer.json` 파일. 자동 로드 섹션을 `composer.json` 을(를) 클라우드 템플릿에 추가하고 누락된 구성을 추가합니다. |
| 10 | validate-config | 다음 `.magento.env.yaml` 파일에 스키마에서 선언되지 않은 옵션 또는 잘못된 값 또는 단계로 구성된 옵션이 포함되어 있습니다. | 다음 `./.magento.env.yaml` 파일에 잘못된 구성이 있습니다. 자세한 내용은 오류 로그를 확인하십시오. |
| 11 | 모듈 새로 고침 | 명령 실패: `/bin/magento module:enable --all` | 실행 시도 `composer update` 로컬로. 그런 다음 업데이트된 을 커밋하고 푸시합니다. `composer.lock` 파일. 또한 다음을 확인하십시오. `cloud.log` 추가 정보. 자세한 명령 출력을 보려면 `VERBOSE_COMMANDS: '-vvv'` 옵션 `.magento.env.yaml` 파일. |
| 12 | 패치 적용 | 패치 적용 실패 |  |
| 13 | set-report-dir-nesting-level | 파일에 쓸 수 없음 `/pub/errors/local.xml` |  |
| 14 | copy-sample-data | 샘플 데이터 파일을 복사하지 못했습니다. |  |
| 15 | 컴파일 다이 | 명령 실패: `/bin/magento setup:di:compile` | 다음 확인: `cloud.log` 추가 정보. 추가 `VERBOSE_COMMANDS: '-vvv'` 대상 `.magento.env.yaml` 를 참조하십시오. |
| 16 | dump-autoload | 명령 실패: `composer dump-autoload` | 다음 `composer dump-autoload` 명령이 실패했습니다. 다음 확인: `cloud.log` 추가 정보. |
| 17 | 런발러 | 실행할 명령 `Baler` for Javascript 번들 실패 | 다음 확인: `SCD_USE_BALER` Baler 모듈이 구성되어 있고 JS 번들링에 대해 활성화되었는지 확인하는 환경 변수입니다. Baler 모듈이 필요하지 않으면 다음을 설정합니다. `SCD_USE_BALER: false`. |
| 18 | 정적 콘텐츠 압축 | 필수 유틸리티를 찾을 수 없습니다(시간 제한, bash). |  |
| 19 | 정적 콘텐츠 배포 | 명령 `/bin/magento setup:static-content:deploy` 실패 | 다음 확인: `cloud.log` 추가 정보. 자세한 명령 출력을 보려면 `VERBOSE_COMMANDS: '-vvv'` 옵션 `.magento.env.yaml` 파일. |
| 20 | 정적 콘텐츠 압축 | 정적 콘텐츠 압축 실패 | 다음 확인: `cloud.log` 추가 정보. |
| 21 | backup-data: 정적 컨텐츠 | 정적 콘텐츠를 (으)로 복사하지 못했습니다. `init` 디렉터리 | 다음 확인: `cloud.log` 추가 정보. |
| 22 | backup-data: writable-dirs | 일부 쓰기 가능한 디렉토리를 `init` 디렉터리 | 쓰기 가능한 디렉터리를 로 복사하지 못했습니다. `./init` 폴더를 삭제합니다. 파일 시스템 권한을 확인합니다. |
| 23 |  | 로거 개체를 만들 수 없습니다. |  |
| 24 | backup-data: 정적 컨텐츠 | 을(를) 정리하지 못했습니다. `./init/pub/static/` 디렉터리 | 정리하지 못함 `./init/pub/static` 폴더를 삭제합니다. 파일 시스템 권한을 확인합니다. |
| 25 |  | 작성기 패키지를 찾을 수 없음 | GitHub 저장소에서 직접 Adobe Commerce 애플리케이션 버전을 설치한 경우 `DEPLOYED_MAGENTO_VERSION_FROM_GIT` 환경 변수가 구성되었습니다. |
| 26 | validate-config | Adobe Commerce 및 Magento Open Source 2.4 이상 버전에서 더 이상 지원되지 않는 Magento Braintree 모듈 구성을 제거합니다. | Braintree 모듈에 대한 지원은 더 이상 Magento 2.4.0 이상에 포함되지 않습니다. Braintree 의 변수 섹션에서 CONFIG__STORES__DEFAULT__PAYMENT_CHANNEL__CHANNEL 변수를 제거합니다. `.magento.app.yaml` 파일. Braintree 결제 지원의 경우 Commerce Marketplace의 공식 확장을 대신 사용하십시오. |

### 스테이지 배포

| 오류 코드 | 배포 단계 | 오류 설명(제목) | 제안된 작업 |
| - | - | - | - |
| 101 | 사전 배포: 캐시 | 잘못된 캐시 구성(포트 또는 호스트 누락) | 캐시 구성에 필수 매개 변수가 없습니다. `server` 또는 `port`. 다음 확인: `cloud.log` 추가 정보. |
| 102 |  | 에 쓸 수 없음 `./app/etc/env.php` 파일 | 배포 스크립트는 `/app/etc/env.php` 파일. 파일 시스템 권한을 확인합니다. |
| 103 |  | 구성에 정의되지 않음 `schema.yaml` 파일 | 구성에 정의되지 않음 `./vendor/magento/ece-tools/config/schema.yaml` 파일. 구성 변수 이름이 올바르고 정의되어 있는지 확인합니다. |
| 104 |  | 구문 분석 실패 `.magento.env.yaml` 파일 | 구성에 정의되지 않음 `./vendor/magento/ece-tools/config/schema.yaml` 파일. 구성 변수 이름이 올바르고 정의되어 있는지 확인합니다. |
| 105 |  | 을(를) 읽을 수 없음 `.magento.env.yaml` 파일 | 을(를) 읽을 수 없음 `./.magento.env.yaml` 파일. 파일 권한을 확인합니다. |
| 106 |  | 을(를) 읽을 수 없음 `.schema.yaml` 파일 |  |
| 107 | 사전 배포: clean-redis-cache | Redis 캐시 정리 실패 | Redis 캐시를 정리하지 못했습니다. Redis 캐시 구성이 올바르고 Redis 서비스를 사용할 수 있는지 확인합니다. 다음을 참조하십시오 [Redis 서비스 설정](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/service/redis.html). |
| 108 | 사전 배포: set-production-mode | 명령 `/bin/magento maintenance:enable` 실패 | 다음 확인: `cloud.log` 추가 정보. 자세한 명령 출력을 보려면 `VERBOSE_COMMANDS: '-vvv'` 옵션 `.magento.env.yaml` 파일. |
| 109 | validate-config | 잘못된 데이터베이스 구성 | 다음을 확인하십시오. `DATABASE_CONFIGURATION` 환경 변수가 올바르게 구성되었습니다. |
| 110 | validate-config | 잘못된 세션 구성 | 다음을 확인하십시오. `SESSION_CONFIGURATION` 환경 변수가 올바르게 구성되었습니다. 구성에는 최소한 다음 항목이 포함되어야 합니다. `save` 매개 변수. |
| 111 | validate-config | 잘못된 검색 구성 | 다음을 확인하십시오. `SEARCH_CONFIGURATION` 환경 변수가 올바르게 구성되었습니다. 구성에는 최소한 다음 항목이 포함되어야 합니다. `engine` 매개 변수. |
| 112 | validate-config | 잘못된 리소스 구성 | 다음을 확인하십시오. `RESOURCE_CONFIGURATION` 환경 변수가 올바르게 구성되었습니다. 구성에는 최소한 다음이 포함되어야 합니다. `connection` 매개 변수. |
| 113 | validate-config:elasticsuite-integrity | ElasticSuite가 설치되었지만 Elasticsearch 서비스를 사용할 수 없습니다. | 다음을 확인하십시오. `SEARCH_CONFIGURATION` 환경 변수가 올바르게 구성되어 있고 Elasticsearch 서비스를 사용할 수 있는지 확인합니다. |
| 114 | validate-config:elasticsuite-integrity | ElasticSuite가 설치되었지만 다른 검색 엔진이 사용되었습니다. | ElasticSuite가 설치되었지만 다른 검색 엔진이 구성되었습니다. 업데이트 `SEARCH_CONFIGURATION` Elasticsearch을 활성화하고 Elasticsearch 서비스 구성을 확인하는 환경 변수 `services.yaml` 파일. |
| 115 |  | 데이터베이스 쿼리 실행 실패 |  |
| 116 | install-update: setup | 명령 `/bin/magento setup:install` 실패 | 다음 확인: `cloud.log` 및 `install_upgrade.log` 추가 정보. 자세한 명령 출력을 보려면 `VERBOSE_COMMANDS: '-vvv'` 옵션 `.magento.env.yaml` 파일. |
| 117 | install-update: config-import | 명령 `app:config:import` 실패 | 다음 확인: `cloud.log` 추가 정보. 자세한 명령 출력을 보려면 `VERBOSE_COMMANDS: '-vvv'` 옵션 `.magento.env.yaml` 파일. |
| 118 |  | 필수 유틸리티를 찾을 수 없습니다(시간 제한, bash). |  |
| 119 | install-update: static-content 배포 | 명령 `/bin/magento setup:static-content:deploy` 실패 | 다음 확인: `cloud.log` 추가 정보. 자세한 명령 출력을 보려면 `VERBOSE_COMMANDS: '-vvv'` 옵션 `.magento.env.yaml` 파일. |
| 120 | 정적 콘텐츠 압축 | 정적 콘텐츠 압축 실패 | 다음 확인: `cloud.log` 추가 정보. |
| 121 | deploy-static-content:generate | 배포된 버전을 업데이트할 수 없음 | 을(를) 업데이트할 수 없음 `./pub/static/deployed_version.txt` 파일. 파일 시스템 권한을 확인합니다. |
| 122 | clean-static-content | 정적 콘텐츠 파일 정리 실패 |  |
| 123 | install-update: split-db | 명령 `/bin/magento setup:db-schema:split` 실패 | 다음 확인: `cloud.log` 추가 정보. 자세한 명령 출력을 보려면 `VERBOSE_COMMANDS: '-vvv'` 옵션 `.magento.env.yaml` 파일. |
| 124 | clean-view-preprocessed | 을(를) 정리하지 못했습니다. `var/view_preprocessed` 폴더 | 을(를) 정리할 수 없음 `./var/view_preprocessed` 폴더를 삭제합니다. 파일 시스템 권한을 확인합니다. |
| 125 | install-update: reset-password | 업데이트 실패 `/var/credentials_email.txt` 파일 | 업데이트 실패 `/var/credentials_email.txt` 파일. 파일 시스템 권한을 확인합니다. |
| 126 | install-update: update | 명령 `/bin/magento setup:upgrade` 실패 | 다음 확인: `cloud.log` 및 `install_upgrade.log` 추가 정보. 자세한 명령 출력을 보려면 `VERBOSE_COMMANDS: '-vvv'` 옵션 `.magento.env.yaml` 파일. |
| 127 | 클린 캐시 | 명령 `/bin/magento cache:flush` 실패 | 다음 확인: `cloud.log` 추가 정보. 자세한 명령 출력을 보려면 `VERBOSE_COMMANDS: '-vvv'` 옵션 `.magento.env.yaml` 파일. |
| 128 | 유지 관리 모드 비활성화 | 명령 `/bin/magento maintenance:disable` 실패 | 다음 확인: `cloud.log` 추가 정보. 추가 `VERBOSE_COMMANDS: '-vvv'` 대상 `.magento.env.yaml` 를 참조하십시오. |
| 129 | install-update: reset-password | 암호 재설정 템플릿을 읽을 수 없음 |  |
| 130 | install-update: cache_type | 명령 실패: `php ./bin/magento cache:enable` | 명령 `php ./bin/magento cache:enable` Adobe Commerce이 설치된 경우에만 실행되지만 `./app/etc/env.php` 배포 시작 시 파일이 없거나 비어 있습니다. 다음 확인: `cloud.log` 추가 정보. 추가 `VERBOSE_COMMANDS: '-vvv'` 대상 `.magento.env.yaml` 를 참조하십시오. |
| 131 | install-update | 다음 `crypt/key`  에 키 값이 없습니다. `./app/etc/env.php` 파일 또는 `CRYPT_KEY` 클라우드 환경 변수 | 이 오류는 다음 경우에 발생합니다. `./app/etc/env.php` Adobe Commerce 배포가 시작될 때 또는 `crypt/key` 값이 정의되지 않았습니다. 다른 환경에서 데이터베이스를 마이그레이션한 경우 해당 환경에서 암호화 키 값을 검색합니다. 그런 다음 값을 [CRYPT_KEY](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-deploy.html#crypt_key) 현재 환경의 클라우드 환경 변수입니다. 다음을 참조하십시오 [Adobe Commerce 암호화 키](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/develop/overview.html#gather-credentials). 실수로 `./app/etc/env.php` 다음 명령을 사용하여 이전 배포에서 만든 백업 파일에서 복원합니다. `./vendor/bin/ece-tools backup:restore` CLI 명령 .&quot; |
| 132 |  | Elasticsearch 서비스에 연결할 수 없습니다. | 유효한 Elasticsearch 자격 증명을 확인하고 서비스가 실행 중인지 확인하십시오 |
| 137 |  | OpenSearch 서비스에 연결할 수 없습니다. | 올바른 OpenSearch 자격 증명을 확인하고 서비스가 실행 중인지 확인하십시오. |
| 133 | validate-config | Adobe Commerce 또는 Magento Open Source 2.4 이상 버전에서 더 이상 지원되지 않는 Magento Braintree 모듈 구성을 제거합니다. | Braintree 모듈에 대한 지원은 더 이상 Adobe Commerce 또는 Magento Open Source 2.4.0 이상에 포함되지 않습니다. Braintree 의 변수 섹션에서 CONFIG__STORES__DEFAULT__PAYMENT_CHANNEL__CHANNEL 변수를 제거합니다. `.magento.app.yaml` 파일. Braintree 지원의 경우 대신 Commerce Marketplace의 공식 Braintree 결제 확장 프로그램을 사용하십시오. |
| 134 | validate-config | Adobe Commerce 및 Magento Open Source 2.4.0을 사용하려면 Elasticsearch 서비스를 설치해야 합니다 | Elasticsearch 서비스 설치 |
| 138 | validate-config | Adobe Commerce 및 Magento Open Source 2.4.4를 사용하려면 OpenSearch 또는 Elasticsearch 서비스를 설치해야 합니다 | OpenSearch 서비스 설치 |
| 135 | validate-config | 검색 엔진은 Adobe Commerce 및 Magento Open Source Elasticsearch >= 2.4.0으로 설정되어야 합니다. | 에 대한 SEARCH_CONFIGURATION 변수를 확인합니다. `engine` 옵션을 선택합니다. 구성된 경우 옵션을 제거하거나 값을 &quot;elasticsearch&quot;로 설정합니다. |
| 136 | validate-config | 분할 데이터베이스가 Adobe Commerce 및 Magento Open Source 2.5.0부터 제거되었습니다. | 분할 데이터베이스를 사용하는 경우 단일 데이터베이스로 되돌리거나 마이그레이션하거나 다른 방법을 사용해야 합니다. |
| 139 | validate-config | 잘못된 검색 엔진 | 이 Adobe Commerce 또는 Magento Open Source 버전은 OpenSearch를 지원하지 않습니다. 버전 2.3.7-p3, 2.4.3-p2 이상을 사용해야 합니다 |

### 배포 후 단계

| 오류 코드 | 배포 후 단계 | 오류 설명(제목) | 제안된 작업 |
| - | - | - | - |
| 201 | is-deploy-failed | 스테이지 배포 실패 |  |
| 202 |  | 다음 `./app/etc/env.php` 파일에 쓸 수 없음 | 배포 스크립트는 `/app/etc/env.php` 파일. 파일 시스템 권한을 확인합니다. |
| 203 |  | 구성에 정의되지 않음 `schema.yaml` 파일 | 구성에 정의되지 않음 `./vendor/magento/ece-tools/config/schema.yaml` 파일. 구성 변수 이름이 올바르고 정의되어 있는지 확인합니다. |
| 204 |  | 구문 분석 실패 `.magento.env.yaml` 파일 | 다음 `./.magento.env.yaml` 파일 형식이 잘못되었습니다. YAML 파서를 사용하여 구문을 확인하고 오류를 수정합니다. |
| 205 |  | 을(를) 읽을 수 없음 `.magento.env.yaml` 파일 | 파일 권한을 확인합니다. |
| 206 |  | 을(를) 읽을 수 없음 `.schema.yaml` 파일 |  |
| 207 | 준비 | 일부 준비 페이지를 미리 로드하지 못했습니다. |  |
| 208 | 첫 번째 바이트 시간 | 첫 번째 바이트까지의 시간 테스트 실패(TTFB) |  |
| 227 | 클린 캐시 | 명령 `/bin/magento cache:flush` 실패 | 다음 확인: `cloud.log` 추가 정보. 추가 `VERBOSE_COMMANDS: '-vvv'` 대상 `.magento.env.yaml` 를 참조하십시오. |

### 일반

| 오류 코드 | 일반 단계 | 오류 설명(제목) | 제안된 작업 |
| - | - | - | - |
| 243 |  | 구성에 정의되지 않음 `schema.yaml` 파일 | 구성 변수 이름이 올바르고 정의되어 있는지 확인합니다. |
| 244 |  | 구문 분석 실패 `.magento.env.yaml` 파일 | 다음 `./.magento.env.yaml` 파일 형식이 잘못되었습니다. YAML 파서를 사용하여 구문을 확인하고 오류를 수정합니다. |
| 245 |  | 을(를) 읽을 수 없음 `.magento.env.yaml` 파일 | 을(를) 읽을 수 없음 `./.magento.env.yaml` 파일. 파일 권한을 확인합니다. |
| 246 |  | 을(를) 읽을 수 없음 `.schema.yaml` 파일 |  |
| 247 |  | 이벤트용 모듈을 생성할 수 없음 | 다음 확인: `cloud.log` 추가 정보. |
| 248 |  | 이벤트용 모듈을 활성화할 수 없음 | 다음 확인: `cloud.log` 추가 정보. |
| 249 |  | AdobeCommerceWebhookPlugins 모듈 생성 실패 | 다음 확인: `cloud.log` 추가 정보. |
| 250 |  | AdobeCommerceWebhookPlugins 모듈을 활성화하지 못했습니다. | 다음 확인: `cloud.log` 추가 정보. |

## 경고 오류

경고 오류는 사이트 작업에 영향을 줄 수 있는 선택적 기능에 대한 구성 설정이 잘못되었거나, 더 이상 사용되지 않거나, 지원되지 않거나, 누락되는 것과 같이 클라우드 인프라에서 Commerce 프로젝트 구성에 문제가 있음을 나타냅니다. 경고 메시지가 배포 실패를 야기하지는 않지만 경고 메시지를 검토하고 구성을 업데이트하여 해결해야 합니다.

### 빌드 단계

| 오류 코드 | 빌드 단계 | 오류 설명(제목) | 제안된 작업 |
| - | - | - | - |
| 1001 | validate-config | 파일 app/etc/config.php 이(가) 없습니다. |  |
| 1002 | validate-config | 입니다./build_options.ini 파일은 더 이상 지원되지 않습니다. |  |
| 1003 | validate-config | 공유 구성 파일에 모듈 섹션이 없습니다. |  |
| 1004 | validate-config | 구성이 이 버전의 Magento과 호환되지 않습니다. |  |
| 1005 | validate-config | SCD 옵션이 무시됨 |  |
| 1006 | validate-config | 구성된 상태가 이상적이지 않음 |  |
| 1007 | 런발러 | Baler JS 번들링을 사용할 수 없음 |  |

### 스테이지 배포

| 오류 코드 | 배포 단계 | 오류 설명(제목) | 제안된 작업 |
| - | - | - | - |
| 2001 | 사전 배포:캐시 | 사용할 수 없는 Redis 서비스에 대해 캐시가 구성되었습니다. 구성이 무시됩니다. |  |
| 2002 | validate-config | 구성된 상태가 이상적이지 않음 |  |
| 2003 | validate-config | 오류 보고를 위한 디렉터리 중첩 수준 값이 구성되지 않았습니다 |  |
| 2004 | validate-config | 의 잘못된 구성./pub/errors/local.xml 파일입니다. |  |
| 2005 | validate-config | 관리자 데이터는 초기 설치 시에만 관리자 사용자를 만드는 데 사용됩니다. 업그레이드 프로세스 중에 관리 데이터에 대한 모든 변경 사항은 무시됩니다. | 초기 설치 후 구성에서 관리 데이터를 제거할 수 있습니다. |
| 2006 | validate-config | 관리자 이메일이 설정되지 않았으므로 관리자 사용자가 생성되지 않았습니다. | 설치 후 관리 사용자를 수동으로 만들 수 있습니다. ssh를 사용하여 환경에 연결합니다. 그런 다음 를 실행합니다 `bin/magento admin:user:create` 명령입니다. |
| 2007 | validate-config | php 버전을 권장 버전으로 업데이트 |  |
| 2008 | validate-config | Solr 지원은 Adobe Commerce 및 Magento Open Source 2.1에서 더 이상 사용되지 않습니다. |  |
| 2009 | validate-config | Adobe Commerce 및 Magento Open Source 2.2 이상에서는 Solr이 더 이상 지원되지 않습니다. |  |
| 2010 | validate-config | Elasticsearch 서비스는 인프라 계층에 설치되지만 검색 엔진으로 사용되지 않습니다. | 리소스 사용을 최적화하려면 인프라 계층에서 Elasticsearch 서비스를 제거하는 것이 좋습니다. |
| 2011 | validate-config | 인프라 계층의 Elasticsearch 서비스 버전이 Adobe Commerce 애플리케이션에서 사용하는 elasticsearch/elasticsearch 모듈의 현재 버전과 호환되지 않습니다. |  |
| 2012 | validate-config | 현재 구성은 이 버전의 Adobe Commerce과 호환되지 않습니다 |  |
| 2013 | validate-config | 빌드 단계에서 배포 프로세스가 실행되지 않았으므로 SCD 옵션이 무시되었습니다. |  |
| 2014 | validate-config | 구성에 더 이상 사용되지 않는 변수 또는 값이 포함되어 있습니다. |  |
| 2015 | validate-config | 환경 구성이 잘못되었습니다. |  |
| 2016 | validate-config | JSON 유형 구성을 디코딩할 수 없습니다. |  |
| 2017 | validate-config | 현재 구성은 이 버전의 Adobe Commerce과 호환되지 않습니다 |  |
| 2018 | validate-config | 일부 서비스가 EOL을 통과했습니다. |  |
| 2019 | validate-config | MySQL 검색 구성 옵션은 사용되지 않습니다 | 대신 Elasticsearch을 사용하십시오. |
| 2029 | validate-config | 분할 데이터베이스는 Adobe Commerce 및 Magento Open Source 2.4.2에서 더 이상 사용되지 않으며 2.5에서 제거됩니다. | 분할 데이터베이스를 사용하는 경우 단일 데이터베이스로 되돌리거나 마이그레이션하거나 다른 방법을 사용할 계획을 시작해야 합니다. |
| 2020 | install-update | Adobe Commerce 설치가 완료되었지만 `app/etc/env.php` 구성 파일이 없거나 비어 있습니다. | 필요한 데이터는 환경 구성 및 .magento.env.yaml 파일에서 복원됩니다. |
| 2021 | install-update:db-connection | 사용자 지정 연결에 사용된 분할 데이터베이스의 경우 |  |
| 2022 | install-update:db-connection | 슬레이브 연결과 호환되지 않는 데이터베이스 구성으로 변경했습니다. |  |
| 2023 | install-update:split-db | 분할 데이터베이스 활성화는 건너뜁니다. |  |
| 2024 | install-update:split-db | SPLIT_DB 변수에 분할 연결 유형에 대한 구성이 없습니다. |  |
| 2025 | install-update:split-db | 슬레이브 연결이 설정되지 않았습니다. |  |
| 2026 | 사전 배포:복원-쓰기 가능-dirs | 빌드 단계 중에 생성된 일부 데이터를 마운트된 디렉터리에 복원하지 못했습니다. | 다음 확인: `cloud.log` 추가 정보. |
| 2027 | validate-config:mage-mode-variable | MAGE_MODE 환경 변수의 모드 값이 지원되지 않음 | MAGE_MODE 환경 변수를 제거하거나 값을 &quot;production&quot;으로 변경합니다. Adobe Commerce on cloud infrastructure는 &quot;프로덕션&quot; 모드만 지원합니다. |
| 2028 | 원격 스토리지 | 원격 저장소를 사용하도록 설정할 수 없습니다. | 원격 스토리지 자격 증명을 확인합니다. |
| 2030 | validate-config | Elasticsearch 및 OpenSearch 서비스는 모두 인프라 계층에 설치됩니다. Adobe Commerce 및 Magento Open Source 2.4.4 이상 버전은 기본적으로 OpenSearch를 사용합니다. | 리소스 사용을 최적화하려면 인프라 계층에서 Elasticsearch 또는 OpenSearch 서비스를 제거하는 것이 좋습니다. |

### 배포 후 단계

| 오류 코드 | 배포 후 단계 | 오류 설명(제목) | 제안된 작업 |
| - | - | - | - |
| 3001 | validate-config | Adobe Commerce에서 디버그 로깅이 활성화되었습니다. | 디스크 공간을 절약하려면 프로덕션 환경에 대한 디버그 로깅을 활성화하지 마십시오. |
| 3002 | 준비 | 저장소 URL을 가져올 수 없음 |  |
| 3003 | 준비 | 저장소 URL을 가져올 수 없음 |  |
| 3004 | 백업 | 백업 파일을 만들 수 없음 |  |

### 일반

| 오류 코드 | 일반 단계 | 오류 설명(제목) | 제안된 작업 |
| - | - | - | - |
| 4001 |  | 시스템 프로세서 수를 가져올 수 없음: |  |
