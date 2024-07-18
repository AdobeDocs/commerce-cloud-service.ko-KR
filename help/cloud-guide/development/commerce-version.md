---
title: Commerce 버전 업그레이드
description: 클라우드 인프라 프로젝트에서 Adobe Commerce 버전을 업그레이드하는 방법에 대해 알아봅니다.
feature: Cloud, Upgrade
exl-id: 87821007-4979-4a20-940b-aa3c82c192d8
source-git-commit: b49a51aba56f79b5253eeacb1adf473f42bb8959
workflow-type: tm+mt
source-wordcount: '1439'
ht-degree: 0%

---

# Commerce 버전 업그레이드

Adobe Commerce 코드 베이스를 최신 버전으로 업그레이드할 수 있습니다. 프로젝트를 업그레이드하기 전에 _설치_ 안내서의 [시스템 요구 사항](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html)에서 최신 소프트웨어 버전 요구 사항을 검토하십시오.

프로젝트 구성에 따라 업그레이드 작업에는 다음이 포함될 수 있습니다.

- MariaDB(MySQL), OpenSearch, RabbitMQ 및 Redis와 같은 업데이트 서비스를 사용하여 새 Adobe Commerce 버전과의 호환성을 유지합니다.
- 이전 구성 관리 파일을 변환합니다.
- 후크 및 환경 변수에 대한 새로운 설정으로 `.magento.app.yaml` 파일을 업데이트합니다.
- 타사 확장을 지원되는 최신 버전으로 업그레이드하십시오.
- `.gitignore` 파일을 업데이트합니다.

{{upgrade-tip}}

{{pro-update-service}}

## 이전 버전에서 업그레이드

2.1 이전 버전의 Commerce에서 업그레이드를 시작하는 경우 Adobe Commerce 코드 기반의 일부 제한 사항이 특정 ECE-Tools 릴리스로 _업데이트_&#x200B;하거나 지원되는 다음 Commerce 버전으로 _업그레이드_&#x200B;하는 기능에 영향을 줄 수 있습니다. 다음 표를 사용하여 최상의 경로를 확인하십시오.

| 현재 버전 | 업그레이드 경로 |
| ----------------- | ------------ |
| 2.1.3 및 이전 | 계속하기 전에 Adobe Commerce을 버전 2.1.4 이상으로 업그레이드하십시오. 그런 다음 [1회 업그레이드를 수행하여 ECE-Tools](../dev-tools/install-package.md)를 설치하십시오. |
| 2.1.4 - 2.1.14 | [ECE-Tools 업데이트](../dev-tools/update-package.md) 패키지<br>릴리스 정보 [2002.0.9](../release-notes/cloud-release-archive.md#v200209) 및 이후 2002.0.x 릴리스를 참조하십시오. |
| 2.1.15 - 2.1.16 | [ECE-Tools 업데이트](../dev-tools/update-package.md) 패키지<br>릴리스 정보[2002.0.9](../release-notes/cloud-release-archive.md#v200209) 이상 |
| 2.2.x 이상 | [ECE-Tools 업데이트](../dev-tools/update-package.md) 패키지<br>릴리스 정보[2002.0.8](../release-notes/cloud-release-archive.md#v200208) 이상 |

{style="table-layout:auto"}

{{ece-tools-package}}

## 구성 관리

2.1.4 이상 또는 2.2.x 이상과 같은 이전 버전의 Adobe Commerce은 구성 관리에 `config.local.php` 파일을 사용했습니다. Adobe Commerce 버전 2.2.0 이상에서는 `config.php` 파일을 사용하는데, 이 파일은 `config.local.php` 파일처럼 작동하지만 사용 가능한 모듈 목록과 추가 구성 옵션을 포함하는 다른 구성 설정이 있습니다.

이전 버전에서 업그레이드하는 경우 `config.local.php` 파일을 마이그레이션하여 최신 `config.php` 파일을 사용해야 합니다. 다음 단계를 사용하여 구성 파일을 백업하고 만듭니다.

**임시 `config.php` 파일을 만들려면**:

1. `config.local.php` 파일의 복사본을 만들고 이름을 `config.php`(으)로 지정합니다.

1. 이 파일을 프로젝트의 `app/etc` 폴더에 추가합니다.

1. 파일을 추가하여 분기에 커밋합니다.

1. 파일을 통합 분기에 푸시합니다.

1. 업그레이드 프로세스를 계속합니다.

>[!WARNING]
>
>업그레이드한 후 `config.php` 파일을 제거하고 새로운 전체 파일을 생성할 수 있습니다. 이 파일은 한 번만 삭제하여 바꿀 수 있습니다. 새로운 전체 `config.php` 파일을 생성한 후에는 파일을 삭제하여 새 파일을 생성할 수 없습니다. [구성 관리 및 파이프라인 배포](../store/store-settings.md)를 참조하세요.

### Zend 프레임워크 작성기 종속성 확인

2.2.x **에서** 2.3.x 이상으로 업그레이드할 때 Zend 프레임워크 종속성이 `composer.json` 파일의 `autoload` 속성에 추가되어 Laminas를 지원하는지 확인하십시오. 이 플러그인은 Laminas 프로젝트로 마이그레이션된 Zend 프레임워크에 대한 새로운 요구 사항을 지원합니다. _Magento DevBlog_&#x200B;에서 [Laminas 프로젝트에 Zend 프레임워크를 마이그레이션](https://community.magento.com/t5/Magento-DevBlog/Migration-of-Zend-Framework-to-the-Laminas-Project/ba-p/443251)을 참조하십시오.

**`auto-load:psr-4` 구성을 확인하려면**:

1. 로컬 워크스테이션에서 프로젝트 디렉터리로 변경합니다.

1. 통합 분기를 확인하십시오.

1. 텍스트 편집기에서 `composer.json` 파일을 엽니다.

1. 컨트롤러 종속성에 대한 Zend 플러그 인 관리자 구현에 대한 `autoload:psr-4` 섹션을 확인하십시오.

   ```json
    "autoload": {
       "psr-4": {
          "Magento\\Framework\\": "lib/internal/Magento/Framework/",
          "Magento\\Setup\\": "setup/src/Magento/Setup/",
          "Magento\\": "app/code/Magento/",
          "Zend\\Mvc\\Controller\\": "setup/src/Zend/Mvc/Controller/"
       },
   }
   ```

1. Zend 종속성이 없으면 `composer.json` 파일을 업데이트하십시오.

   - `autoload:psr-4` 섹션에 다음 줄을 추가하십시오.

     ```json
     "Zend\\Mvc\\Controller\\": "setup/src/Zend/Mvc/Controller/"
     ```

   - 프로젝트 종속성을 업데이트합니다.

     ```bash
     composer update
     ```

   - 코드 변경 사항을 추가, 커밋 및 푸시합니다.

     ```bash
     git add -A
     ```

     ```bash
     git commit -m "Add Zend plugin manager implementation for controllers dependency for Laminas support"
     ```

     ```bash
     git push origin <branch-name>
     ```

   - 변경 사항을 스테이징 환경에 병합한 다음 프로덕션에 병합합니다.

## 구성 파일

애플리케이션을 업그레이드하기 전에 클라우드 인프라 또는 애플리케이션에서 Adobe Commerce의 기본 구성 설정을 변경하기 위해 프로젝트 구성 파일을 업데이트해야 합니다. 최신 기본값은 [magento-cloud GitHub 저장소](https://github.com/magento/magento-cloud)에서 찾을 수 있습니다.

### .magento.app.yaml

응용 프로그램이 클라우드 인프라에 빌드하고 배포하는 방식을 제어하므로 설치된 버전에 대해 [.magento.app.yaml](../application/configure-app-yaml.md) 파일에 포함된 값을 항상 검토하십시오. 다음 예제는 버전 2.4.7용이며 Composer 2.7.2를 사용합니다. `build: flavor:` 속성은 Composer 2.x에 사용되지 않습니다. [Composer 2 설치 및 사용](../application/properties.md#installing-and-using-composer-2)을 참조하십시오.

**`.magento.app.yaml` 파일을 업데이트하려면**:

1. 로컬 워크스테이션에서 프로젝트 디렉터리로 변경합니다.

1. `magento.app.yaml` 파일을 열고 편집합니다.

1. PHP 옵션을 업데이트합니다.

   ```yaml
   type: php:8.3
   
   build:
       flavor: none
   dependencies:
       php:
           composer/composer: '2.7.2'
   ```

1. `hooks` 속성 `build` 및 `deploy` 명령을 수정합니다.

   ```yaml
   hooks:
       # We run build hooks before your application has been packaged.
       build: |
           set -e
           composer install
           php ./vendor/bin/ece-tools run scenario/build/generate.xml
           php ./vendor/bin/ece-tools run scenario/build/transfer.xml
       # We run deploy hook after your application has been deployed and started.
       deploy: |
           php ./vendor/bin/ece-tools run scenario/deploy.xml
       # We run post deploy hook to clean and warm the cache. Available with ECE-Tools 2002.0.10.
       post_deploy: |
           php ./vendor/bin/ece-tools run scenario/post-deploy.xml
   ```

1. 파일 끝에 다음 환경 변수를 추가합니다.

   Adobe Commerce 2.2.x - 2.3.x-

   ```yaml
   variables:
       env:
           CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
           CONFIG__STORES__DEFAULT__PAYMENT__BRAINTREE__CHANNEL: 'Magento_Enterprise_Cloud_BT'
           CONFIG__STORES__DEFAULT__PAYPAL__NOTATION_CODE: 'Magento_Enterprise_Cloud'
   ```

   Adobe Commerce 2.4.x의 경우-

   ```yaml
   variables:
       env:
           CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
           CONFIG__STORES__DEFAULT__PAYPAL__NOTATION_CODE: 'Magento_Enterprise_Cloud'
   ```

1. 파일을 저장합니다. 아직 원격 환경에 변경 사항을 커밋하거나 푸시하지 마십시오.

1. 업그레이드 프로세스를 계속합니다.

### composer.json

업그레이드하기 전에 항상 `composer.json` 파일의 종속성이 Adobe Commerce 버전과 호환되는지 확인하십시오.

**Adobe Commerce 버전 2.4.4 이상의 `composer.json` 파일을 업데이트하려면**:

1. `config` 섹션에 다음 `allow-plugins`을(를) 추가합니다.

   ```json
   "config": {
      "allow-plugins": {
         "dealerdirect/phpcodesniffer-composer-installer": true,
         "laminas/laminas-dependency-plugin": true,
         "magento/*": true
      }
   },
   ```

1. `require` 섹션에 다음 플러그인을 추가하십시오.

   ```json
   "require": {
       "magento/composer-root-update-plugin": "^2.0.3"
   },
   ```

1. `extra:component_paths` 섹션에 다음 구성 요소를 추가합니다.

   ```json
   "extra": {
      "component_paths": {
         "tinymce/tinymce": "lib/web/tiny_mce_5"
      },
   },
   ```

1. 파일을 저장합니다. 아직 분기에 변경 사항을 커밋하거나 푸시하지 마십시오.

1. 업그레이드 프로세스를 계속합니다.

## 프로젝트 백업

업그레이드하기 전에 프로젝트의 백업을 만드는 것이 좋습니다. 다음 단계를 사용하여 통합, 스테이징 및 프로덕션 환경을 백업합니다.

**통합 환경 데이터베이스 및 코드를 백업하려면**:

1. 원격 데이터베이스의 로컬 백업을 만듭니다.

   ```bash
   magento-cloud db:dump
   ```

   >[!NOTE]
   >
   >`magento-cloud db:dump` 명령은 테이블을 잠그지 않고 데이터베이스를 백업할 수 있도록 `--single-transaction` 플래그를 사용하여 [mysqldump](https://dev.mysql.com/doc/refman/8.0/en/mysqldump.html) 명령을 실행합니다.

1. 코드 및 미디어를 백업합니다.

   ```bash
   php bin/magento setup:backup --code [--media]
   ```

   소스 제어에 이미 많은 정적 파일이 있는 경우 `[--media]`을(를) 생략할 수도 있습니다.

**배포 전에 스테이징 또는 프로덕션 환경 데이터베이스를 백업하려면**:

1. SSH를 사용하여 원격 환경에 로그인합니다.

1. [데이터베이스 덤프](../storage/database-dump.md)를 만듭니다. DB 덤프의 대상 디렉터리를 선택하려면 `--dump-directory` 옵션을 사용합니다.

   ```bash
   vendor/bin/ece-tools db-dump
   ```

   덤프 작업에서 원격 프로젝트 디렉터리에 `dump-<timestamp>.sql.gz` 보관 파일을 만듭니다. [데이터베이스 백업](../storage/database-dump.md)을 참조하십시오.

## 애플리케이션 업그레이드

응용 프로그램을 업그레이드하기 전에 [서비스 버전](../services/services-yaml.md#service-versions) 정보에서 최신 소프트웨어 버전 요구 사항을 검토하십시오.

**응용 프로그램 버전을 업그레이드하려면**:

1. 로컬 워크스테이션에서 프로젝트 디렉터리로 변경합니다.

1. [버전 제약 조건 구문](overview.md#cloud-metapackage)을 사용하여 업그레이드 버전을 설정합니다.

   ```bash
   composer require "magento/magento-cloud-metapackage":">=CURRENT_VERSION <NEXT_VERSION" --no-update
   ```

   >[!NOTE]
   >
   >`ece-tools` 패키지를 업데이트하려면 버전 제약 조건 구문을 사용해야 합니다. `composer.json` 파일에서 업그레이드에 사용하는 [응용 프로그램 템플릿](https://github.com/magento/magento-cloud/blob/master/composer.json) 버전에 대한 버전 제약 조건을 찾을 수 있습니다.

1. 프로젝트를 업데이트합니다.

   ```bash
   composer update
   ```

1. 코드 변경 사항을 추가, 커밋 및 푸시합니다.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Upgrade"
   ```

   ```bash
   git push origin <branch-name>
   ```

   Composer가 기본 패키지를 마샬링하는 방식 때문에 변경된 모든 파일을 소스 제어에 추가하려면 `git add -A`이(가) 필요합니다. 기본 패키지(`magento/magento2-base` 및 `magento/magento2-ee-base`)의 `composer install` 및 `composer update` 파일을 모두 패키지 루트로 마샬링합니다.

   작성기가 마샬링하는 파일은 새 버전의 Adobe Commerce에 속하므로 동일한 파일의 오래된 버전을 덮어씁니다. 현재 Adobe Commerce에서는 마샬링을 사용할 수 없으므로 마샬링된 파일을 소스 제어에 추가해야 합니다.

1. 배포가 완료될 때까지 기다립니다.

1. SSH를 사용하여 로그인하고 버전을 확인하여 통합, 스테이징 또는 프로덕션 환경에서 업그레이드를 확인합니다.

   ```bash
   php bin/magento --version
   ```

### config.php 파일 만들기

[구성 관리](#configuration-management)에서 설명한 대로 업그레이드한 후 업데이트된 `config.php` 파일을 만들어야 합니다. 통합 환경의 관리를 통해 추가 구성 변경 사항을 완료합니다.

**시스템별 구성 파일을 만들려면**:

1. 터미널에서 SSH 명령을 사용하여 환경에 대한 `/app/etc/config.php` 파일을 생성합니다.

   ```bash
   ssh <SSH-URL> "<Command>"
   ```

   예를 들어 Pro의 경우 `integration` 분기에서 `scd-dump`을(를) 실행하려면 다음을 수행합니다.

   ```bash
   ssh <project-id-integration>@ssh.us.magentosite.cloud "php vendor/bin/ece-tools config:dump"
   ```

1. `rsync` 또는 `scp`을(를) 사용하여 `config.php` 파일을 로컬 워크스테이션에 전송하십시오. 이 파일은 로컬에서만 분기에 추가할 수 있습니다.

   ```bash
   rsync <SSH-URL>:app/etc/config.php ./app/etc/config.php
   ```

1. 코드 변경 사항을 추가, 커밋 및 푸시합니다.

   ```bash
   git add app/etc/config.php && git commit -m "Add system-specific configuration" && git push origin master
   ```

   모듈 목록과 구성 설정을 사용하여 업데이트된 `/app/etc/config.php` 파일을 생성합니다.

>[!WARNING]
>
>업그레이드를 위해 `config.php` 파일을 삭제합니다. 이 파일이 코드에 추가되면 **삭제할 수 없습니다**. 설정을 제거하거나 편집해야 하는 경우 파일을 수동으로 편집하십시오.

### 확장 업그레이드

Marketplace 또는 기타 회사 사이트에서 타사 확장 및 모듈 페이지를 검토하고 클라우드 인프라에서 Adobe Commerce 및 Adobe Commerce에 대한 지원을 확인하십시오. Adobe 타사 확장 및 모듈을 업그레이드해야 하는 경우 확장이 비활성화된 새 통합 분기에서 작업하는 것이 좋습니다.

**확장을 확인하고 업그레이드하려면**:

1. 로컬 워크스테이션에 분기를 만듭니다.

1. 필요에 따라 확장을 비활성화합니다.

1. 사용 가능한 경우 확장 업그레이드를 다운로드합니다.

1. 타사 설명서에 따라 업그레이드를 설치합니다.

1. 확장을 활성화하고 테스트합니다.

1. 코드 변경 사항을 원격에 추가, 커밋 및 푸시합니다.

1. 을 푸시하고 통합 환경에서 테스트합니다.

1. 스테이징 환경으로 푸시하여 사전 프로덕션 환경에서 테스트합니다.

Adobe 사이트 시작 프로세스에서 업그레이드된 확장을 포함하여 프로덕션 환경을 _before_ 업그레이드하는 것이 좋습니다.

>[!NOTE]
>
>응용 프로그램 버전을 업그레이드하면 업그레이드 프로세스가 [Fastly CDN 모듈](../cdn/fastly.md#fastly-cdn-module-for-magento-2)의 최신 버전으로 자동으로 업데이트됩니다.

## 업그레이드 문제 해결

업그레이드가 실패하는 경우 브라우저에 상점 또는 관리 패널에 액세스할 수 없다는 오류 메시지가 표시됩니다.

```
There has been an error processing your request
Exception printing is disabled by default for security reasons.
  Error log record number: <error-number>
```

**오류를 해결하려면**:

1. 로컬 워크스테이션에서 프로젝트 디렉터리로 변경합니다.

1. SSH를 사용하여 원격 환경에 로그인합니다.

   ```bash
   magento-cloud ssh
   ```

1. `./app/var/report/<error number>` 파일을 엽니다.

1. [로그를 검사하고](../test/log-locations.md) 문제의 원인을 확인합니다.

1. 코드 변경 사항을 추가, 커밋 및 푸시합니다.

   ```bash
   git add -A && git commit -m "Fixed deployment failure" && git push origin <branch-name>
   ```
