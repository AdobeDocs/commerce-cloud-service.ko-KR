---
title: Commerce 버전 업그레이드
description: 클라우드 인프라 프로젝트에서 Adobe Commerce 버전을 업그레이드하는 방법에 대해 알아봅니다.
feature: Cloud, Upgrade
exl-id: 87821007-4979-4a20-940b-aa3c82c192d8
source-git-commit: 99272d08a11f850a79e8e24857b7072d1946f374
workflow-type: tm+mt
source-wordcount: '1439'
ht-degree: 0%

---

# Commerce 버전 업그레이드

Adobe Commerce 코드 베이스를 최신 버전으로 업그레이드할 수 있습니다. 프로젝트를 업그레이드하기 전에 [시스템 요구 사항](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html) 다음에서 _설치_ 최신 소프트웨어 버전 요구 사항에 대한 안내서입니다.

프로젝트 구성에 따라 업그레이드 작업에는 다음이 포함될 수 있습니다.

- MariaDB(MySQL), OpenSearch, RabbitMQ 및 Redis와 같은 업데이트 서비스를 사용하여 새 Adobe Commerce 버전과의 호환성을 유지합니다.
- 이전 구성 관리 파일을 변환합니다.
- 업데이트 `.magento.app.yaml` 후크와 환경 변수에 대한 새 설정이 있는 파일입니다.
- 타사 확장을 지원되는 최신 버전으로 업그레이드하십시오.
- 업데이트 `.gitignore` 파일.

{{upgrade-tip}}

{{pro-update-service}}

## 이전 버전에서 업그레이드

2.1 이전 버전의 Commerce에서 업그레이드를 시작하는 경우 Adobe Commerce 코드 기반의 일부 제한 사항이 다음 기능에 영향을 줄 수 있습니다. _업데이트_ 특정 ECE-Tools 릴리스 또는 _업그레이드_ 지원되는 다음 상거래 버전으로 이동합니다. 다음 표를 사용하여 최상의 경로를 확인하십시오.

| 현재 버전 | 업그레이드 경로 |
| ----------------- | ------------ |
| 2.1.3 및 이전 | 계속하기 전에 Adobe Commerce을 버전 2.1.4 이상으로 업그레이드하십시오. 그런 다음 [ECE-Tools 설치를 위한 1회 업그레이드](../dev-tools/install-package.md). |
| 2.1.4 - 2.1.14 | [ECE-Tools 업데이트](../dev-tools/update-package.md) 패키지.<br>의 릴리스 정보 를 참조하십시오. [2002.0.9](../release-notes/cloud-release-archive.md#v200209) 및 2002.0.x 이후 릴리스 |
| 2.1.15 - 2.1.16 | [ECE-Tools 업데이트](../dev-tools/update-package.md) 패키지.<br>의 릴리스 정보 를 참조하십시오.[2002.0.9](../release-notes/cloud-release-archive.md#v200209) 나중에. |
| 2.2.x 이상 | [ECE-Tools 업데이트](../dev-tools/update-package.md) 패키지.<br>의 릴리스 정보 를 참조하십시오.[2002.0.8](../release-notes/cloud-release-archive.md#v200208) 나중에. |

{style="table-layout:auto"}

{{ece-tools-package}}

## 구성 관리

2.1.4 이상 또는 2.2.x 이상과 같은 이전 버전의 Adobe Commerce은 `config.local.php` 구성 관리용 파일입니다. Adobe Commerce 버전 2.2.0 이상에서는 `config.php` 파일, 즉 `config.local.php` 사용 가능한 모듈 목록과 추가 구성 옵션을 포함하는 다양한 구성 설정이 있습니다.

이전 버전에서 업그레이드하는 경우 `config.local.php` 최신 파일을 사용할 파일 `config.php` 파일. 다음 단계를 사용하여 구성 파일을 백업하고 만듭니다.

**임시 항목을 만들려면 `config.php` 파일**:

1. 사본 만들기 `config.local.php` 파일 및 이름 지정 `config.php`.

1. 에 이 파일 추가 `app/etc` 프로젝트의 폴더입니다.

1. 파일을 추가하여 분기에 커밋합니다.

1. 파일을 통합 분기에 푸시합니다.

1. 업그레이드 프로세스를 계속합니다.

>[!WARNING]
>
>업그레이드 후 다음을 제거할 수 있습니다. `config.php` 파일을 만들고 새로운 전체 파일을 생성합니다. 이 파일은 한 번만 삭제하여 바꿀 수 있습니다. 새 전체 생성 후 `config.php` 파일을 삭제하면 새 파일을 생성할 수 없습니다. 다음을 참조하십시오 [구성 관리 및 파이프라인 배포](../store/store-settings.md).

### Zend 프레임워크 작성기 종속성 확인

로 업그레이드할 때 **2.2.x에서 2.3.x 이상**, Zend 프레임워크 종속성이 `autoload` 의 속성 `composer.json` Laminas를 지원하는 파일입니다. 이 플러그인은 Laminas 프로젝트로 마이그레이션된 Zend 프레임워크에 대한 새로운 요구 사항을 지원합니다. 다음을 참조하십시오 [Zend Framework의 Laminas 프로젝트로의 마이그레이션](https://community.magento.com/t5/Magento-DevBlog/Migration-of-Zend-Framework-to-the-Laminas-Project/ba-p/443251) 다음에 있음 _Magento DevBlog_.

**을(를) 확인하려면 `auto-load:psr-4` 구성**:

1. 로컬 워크스테이션에서 프로젝트 디렉터리로 변경합니다.

1. 통합 분기를 확인하십시오.

1. 를 엽니다. `composer.json` 파일을 텍스트 편집기에 넣습니다.

1. 다음 확인: `autoload:psr-4` 컨트롤러 종속성에 대한 Zend plugin manager 구현 섹션.

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

1. Zend 종속성이 누락된 경우 `composer.json` 파일:

   - 다음 줄을 추가합니다. `autoload:psr-4` 섹션.

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

애플리케이션을 업그레이드하기 전에 클라우드 인프라 또는 애플리케이션에서 Adobe Commerce의 기본 구성 설정을 변경하기 위해 프로젝트 구성 파일을 업데이트해야 합니다. 최신 기본값은 [magento-cloud GitHub 저장소](https://github.com/magento/magento-cloud).

### .magento.app.yaml

항상 다음에 포함된 값 검토 [.magento.app.yaml](../application/configure-app-yaml.md) 애플리케이션이 클라우드 인프라에 빌드하고 배포하는 방식을 제어하기 때문에 설치된 버전에 대한 파일입니다. 다음 예제는 버전 2.4.7용이며 Composer 2.7.2를 사용합니다. 다음 `build: flavor:` 속성은 Composer 2.x에 사용되지 않습니다. 참조 [Composer 2 설치 및 사용](../application/properties.md#installing-and-using-composer-2).

**를 업데이트하려면 `.magento.app.yaml` 파일**:

1. 로컬 워크스테이션에서 프로젝트 디렉터리로 변경합니다.

1. 를 열고 편집합니다. `magento.app.yaml` 파일.

1. PHP 옵션을 업데이트합니다.

   ```yaml
   type: php:8.3
   
   build:
       flavor: none
   dependencies:
       php:
           composer/composer: '2.7.2'
   ```

1. 수정 `hooks` 속성 `build` 및 `deploy` 명령입니다.

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

업그레이드하기 전에 항상 의 종속성이 `composer.json` 파일은 Adobe Commerce 버전과 호환됩니다.

**를 업데이트하려면 `composer.json` Adobe Commerce 버전 2.4.4 이상용 파일**:

1. 다음 추가 `allow-plugins` (으)로 `config` 섹션:

   ```json
   "config": {
      "allow-plugins": {
         "dealerdirect/phpcodesniffer-composer-installer": true,
         "laminas/laminas-dependency-plugin": true,
         "magento/*": true
      }
   },
   ```

1. 에 다음 플러그인을 추가합니다 `require` 섹션:

   ```json
   "require": {
       "magento/composer-root-update-plugin": "^2.0.3"
   },
   ```

1. 에 다음 구성 요소 추가 `extra:component_paths` 섹션:

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
   >다음 `magento-cloud db:dump` 명령을 실행하면 [mysqldump](https://dev.mysql.com/doc/refman/8.0/en/mysqldump.html) 명령을 사용하여 `--single-transaction` 플래그를 사용하여 테이블을 잠그지 않고 데이터베이스를 백업할 수 있습니다.

1. 코드 및 미디어를 백업합니다.

   ```bash
   php bin/magento setup:backup --code [--media]
   ```

   선택적으로 다음을 생략할 수 있습니다. `[--media]` 소스 제어에 이미 많은 정적 파일이 있는 경우

**배포하기 전에 스테이징 또는 프로덕션 환경 데이터베이스를 백업하려면**:

1. SSH를 사용하여 원격 환경에 로그인합니다.

1. 만들기 [데이터베이스 덤프](../storage/database-dump.md). DB 덤프의 대상 디렉토리를 선택하려면 `--dump-directory` 옵션을 선택합니다.

   ```bash
   vendor/bin/ece-tools db-dump
   ```

   덤프 작업은 `dump-<timestamp>.sql.gz` 원격 프로젝트 디렉터리에 파일 보관 다음을 참조하십시오 [데이터베이스 백업](../storage/database-dump.md).

## 애플리케이션 업그레이드

리뷰 [서비스 버전](../services/services-yaml.md#service-versions) 응용 프로그램을 업그레이드하기 전에 최신 소프트웨어 버전 요구 사항에 대한 정보입니다.

**응용 프로그램 버전을 업그레이드하려면**:

1. 로컬 워크스테이션에서 프로젝트 디렉터리로 변경합니다.

1. 다음을 사용하여 업그레이드 버전 설정 [버전 제약 조건 구문](overview.md#cloud-metapackage).

   ```bash
   composer require "magento/magento-cloud-metapackage":">=CURRENT_VERSION <NEXT_VERSION" --no-update
   ```

   >[!NOTE]
   >
   >를 성공적으로 업데이트하려면 버전 제한 구문을 사용해야 합니다. `ece-tools` 패키지. 다음에서 버전 제약 조건을 찾을 수 있습니다. `composer.json` 버전 파일 [애플리케이션 템플릿](https://github.com/magento/magento-cloud/blob/master/composer.json) 업그레이드에 을(를) 사용하고 있습니다.

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

   `git add -A` Composer가 기본 패키지를 마샬링하는 방식으로 인해 변경된 모든 파일을 소스 제어에 추가해야 합니다. 모두 `composer install` 및 `composer update` 기본 패키지에서 파일 마샬링(`magento/magento2-base` 및 `magento/magento2-ee-base`)를 패키지 루트에 추가합니다.

   작성기가 마샬링하는 파일은 새 버전의 Adobe Commerce에 속하므로 동일한 파일의 오래된 버전을 덮어씁니다. 현재 Adobe Commerce에서는 마샬링을 사용할 수 없으므로 마샬링된 파일을 소스 제어에 추가해야 합니다.

1. 배포가 완료될 때까지 기다립니다.

1. SSH를 사용하여 로그인하고 버전을 확인하여 통합, 스테이징 또는 프로덕션 환경에서 업그레이드를 확인합니다.

   ```bash
   php bin/magento --version
   ```

### config.php 파일 만들기

에서 언급된 바와 같이 [구성 관리](#configuration-management)를 설치한 후 업데이트된 를 만들어야 합니다 `config.php` 파일. 통합 환경의 관리를 통해 추가 구성 변경 사항을 완료합니다.

**시스템별 구성 파일을 만들려면**:

1. 터미널에서 SSH 명령을 사용하여 `/app/etc/config.php` 환경용 파일입니다.

   ```bash
   ssh <SSH-URL> "<Command>"
   ```

   예를 들어 Pro의 경우 `scd-dump` 다음에 있음 `integration` 분기:

   ```bash
   ssh <project-id-integration>@ssh.us.magentosite.cloud "php vendor/bin/ece-tools config:dump"
   ```

1. 전송 `config.php` 를 사용하여 로컬 워크스테이션에 파일을 추가합니다. `rsync` 또는 `scp`. 이 파일은 로컬에서만 분기에 추가할 수 있습니다.

   ```bash
   rsync <SSH-URL>:app/etc/config.php ./app/etc/config.php
   ```

1. 코드 변경 사항을 추가, 커밋 및 푸시합니다.

   ```bash
   git add app/etc/config.php && git commit -m "Add system-specific configuration" && git push origin master
   ```

   업데이트됨 `/app/etc/config.php` 모듈 목록과 구성 설정이 있는 파일입니다.

>[!WARNING]
>
>업그레이드를 위해 다음을 삭제합니다. `config.php` 파일. 이 파일이 코드에 추가되면 **아님** 삭제하십시오. 설정을 제거하거나 편집해야 하는 경우 파일을 수동으로 편집하십시오.

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

Adobe은 프로덕션 환경을 업그레이드할 것을 강력히 권장합니다 _다음 이전_ 사이트 실행 프로세스에 업그레이드된 확장을 포함합니다.

>[!NOTE]
>
>애플리케이션 버전을 업그레이드하면 업그레이드 프로세스가 최신 버전의 [Fastly CDN 모듈](../cdn/fastly.md#fastly-cdn-module-for-magento-2) 자동으로 표시됩니다.

## 업그레이드 문제 해결

업그레이드가 실패하는 경우 브라우저에 상점 또는 관리 패널에 액세스할 수 없다는 오류 메시지가 표시됩니다.

```terminal
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

1. 를 엽니다. `./app/var/report/<error number>` 파일.

1. [로그 검사](../test/log-locations.md) 및 문제의 원인을 확인합니다.

1. 코드 변경 사항을 추가, 커밋 및 푸시합니다.

   ```bash
   git add -A && git commit -m "Fixed deployment failure" && git push origin <branch-name>
   ```
