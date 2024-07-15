---
title: 이전 버전과 호환 불가능한 변경 사항
description: 기존 클라우드 프로젝트를 업그레이드할 때 이전 버전과의 호환성에 대해 알아봅니다.
feature: Cloud, Release Notes
recommendations: noDisplay, catalog
exl-id: 9847c565-6d59-429a-9593-c2eba5cf2da4
source-git-commit: f9edcc85c14354a2eddacbc5219557e107a367cb
workflow-type: tm+mt
source-wordcount: '791'
ht-degree: 0%

---

# 이전 버전과 호환 불가능한 변경 사항

`ece-tools` 패키지의 최신 릴리스나 Commerce용 클라우드 도구 세트 패키지로 업그레이드할 때 이전 버전과 호환되지 않는 변경 사항에 따라 기존 클라우드 프로젝트에 대한 클라우드 구성 및 프로세스를 조정해야 할 수 있습니다.

## `ece-tools` 패키지 변경 내용

이전에 `ece-tools` 패키지에 포함되었던 일부 기능은 이제 별도의 패키지로 제공됩니다. 이 패키지는 ece-tools를 설치하거나 업데이트할 때 자동으로 설치 및 업데이트되는 `ece-tools`에 대한 작성기 종속성입니다.

새 아키텍처는 설치 또는 업데이트 프로세스에 영향을 주지 않습니다. 그러나 Adobe Commerce on cloud infrastructure 프로젝트에서 작업할 때 일부 명령 구문 및 프로세스를 변경해야 할 수 있습니다. 자세한 내용은 이전 버전과 호환되지 않는 다음 변경 내용 정보와 [Cloud Tools Suite 릴리스 정보](cloud-tools-suite.md)를 검토하십시오.

### 서비스 버전 요구 사항 변경

`ece-tools` v2002.1.0 이상을 사용하는 클라우드 프로젝트의 최소 PHP 버전 요구 사항을 7.0.x에서 7.1.x로 변경했습니다. 환경 구성에 PHP 7.0이 지정되어 있으면 `.magento.app.yaml` 파일에서 [php 구성](../application/php-settings.md)을 업데이트합니다.

>[!WARNING]
>
>PHP 버전 요구 사항 변경으로 인해 `ece-tools` 2002.1.0에서는 Adobe Commerce 2.1.15 이상을 실행하는 클라우드 인프라 프로젝트에서 Adobe Commerce만 지원합니다. 프로젝트에서 이전 릴리스를 사용하는 경우 `ece-tools` 2002.1.0으로 업데이트하기 전에 [업그레이드](../development/commerce-version.md)해야 합니다.

### 환경 구성 변경 사항

다음 표에서는 `ece-tools` v2002.1.0에서 제거되었거나 더 이상 사용되지 않는 환경 변수 및 기타 환경 구성 파일에 대한 정보를 제공합니다.

| 항목 | 교체 |
| -------- | ----------- |
| `SCD_EXCLUDE_THEMES` 변수 | [`SCD_MATRIX`](../environment/variables-build.md#scd_matrix) |
| `STATIC_CONTENT_THREADS` 변수 | [`SCD_THREADS`](../environment/variables-build.md#scd_threads) |
| `DO_DEPLOY_STATIC_CONTENT` 변수 | [`SKIP_SCD`](../environment/variables-build.md#skip_scd) |
| `STATIC_CONTENT_SYMLINK` 변수 | 없음. 이제 빌드는 항상 정적 콘텐츠 디렉터리 `pub/static`에 대한 symlink를 만듭니다. |
| `build_options.ini`개 파일 | [`.magento.env.yaml`](../application/configure-app-yaml.md) 파일을 사용하여 모든 환경에서 빌드 및 배포 작업을 관리하도록 환경 변수를 구성합니다.<p>`build_options.ini` 파일이 포함된 클라우드 환경을 빌드하면 빌드가 실패합니다. |

### CLI 명령 변경 사항

다음 표에는 명령이나 스크립트를 업데이트해야 할 수 있는 ECE-Tools v2002.1.0의 CLI 명령 변경 사항이 요약되어 있습니다.

| 명령 | 교체 |
|-------- | ----------- |
| `m2-ece-build` | `vendor/bin/ece-tools build` |
| `m2-ece-deploy` | `vendor/bin/ece-tools deploy` |
| `m2-ece-scd-dump` | `vendor/bin/ece-tools config:dump` |
| `vendor/bin/ece-tools patch` | `vendor/bin/ece-patches apply` |
| `vendor/bin/ece-tools docker:build` | `vendor/bin/ece-docker build:compose` |
| `vendor/bin/ece-tools docker:config:convert` | `vendor/bin/ece-docker  image:generate:php` |

이전 ECE-Tools 릴리스에서는 `m2-ece-build` 및 `m2-ece-deploy` 명령을 사용하여 `.magento.app.yaml` 파일에서 배포 후크를 구성할 수 있습니다. v2002.1.0으로 업데이트할 때 `.magento.app.yaml` 파일의 `hooks` 구성에서 사용되지 않는 명령을 확인하고 필요한 경우 바꿉니다.

## 클라우드 패치 변경 사항

- **다운로드한 패치 제거**-`magento/magento-cloud-patches` 패키지는 [소프트웨어 다운로드](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/commerce.html) 페이지에서 사용할 수 있는 모든 패치를 번들로 제공하고 클라우드에 배포할 때 자동으로 적용합니다. ECE-Tools 2002.1.0 이상으로 업그레이드한 후 패치 충돌을 방지하려면 수동으로 프로젝트에 다운로드하여 추가한 Adobe 제공 패치를 제거합니다.

- **패치 적용 명령 업데이트**-패치 적용 명령을 `vendor/bin/ece-tools` 디렉터리에서 `vendor/bin/ece-patches` 디렉터리로 이동했습니다. 이 명령을 사용하여 패치를 수동으로 적용하는 경우 새 경로를 사용합니다.

  > 수동으로 패치 적용

  ```bash
  php ./vendor/bin/ece-patches apply
  ```

## Cloud Docker 변경 사항

- **최소 PHP 버전 요구 사항이 이제 PHP 7.1**&#x200B;입니다.-Commerce용 Cloud Docker 호스트에서 이전 버전을 실행 중인 경우 PHP v7.1 이상으로 업그레이드하십시오.

- **Commerce 명령 변경용 Cloud Docker**-

   - **도커 빌드 작업을 위해 Commerce용 Cloud Docker 명령을 업데이트하는 중**-Commerce용 Cloud Docker 명령을 `vendor/bin/ece-tools` 디렉터리에서 `vendor/bin/ece-docker` 디렉터리로 이동했습니다. 새 경로를 사용하도록 스크립트 및 명령을 업데이트합니다.

     `ece-tools` 2002.1.0으로 업그레이드한 후 다음 명령을 사용하여 사용 가능한 `ece-docker` 명령을 확인합니다.

     ```bash
     php ./vendor/bin/ece-docker list
     ```

   - **Cloud Docker 구성 명령 업데이트**-명령 파일에 대한 경로 이름을 `./bin/docker`에서 `./bin/magento-docker`(으)로 변경했습니다. 새 경로를 사용하도록 스크립트 및 명령을 업데이트합니다.

   - **Cron 컨테이너가 더 이상 기본 Docker 구성에 포함되지 않습니다**-이제 `ece-docker build:compose` 명령에 `--with-cron` 옵션을 추가하여 Cron 컨테이너를 Docker 환경 구성에 포함해야 합니다. _Commerce용 Cloud Docker_ 안내서의 [크론 작업 관리](https://developer.adobe.com/commerce/cloud-tools/docker/configure/manage-cron-jobs/)를 참조하십시오.

     cron 작업이 있는 이전에 생성된 컨테이너에 이제 cron 컨테이너가 없는 스크립트입니다.

   - **임시 컨테이너 사용**-이전 버전에서는 `bin/magento-docker` 명령 작업으로 만든 컨테이너가 제거되지 않았으므로 다른 작업에 사용할 수 있습니다. 이제 `magento-docker` 명령은 명령이 완료된 후 만들어진 모든 컨테이너를 제거합니다.

     도커 작성 작업으로 생성된 컨테이너를 유지하려면 `bin/magento-docker` 명령 대신 `docker-compose run` 명령을 사용합니다.

   - **배포 후 후크 실행** - `cloud-deploy` 명령은 더 이상 배포 후 후크를 실행하지 않습니다. 배포한 후 새 `cloud-post-deploy` 명령을 사용하여 배포 후 후크를 실행합니다. 스크립트를 업데이트하여 배포 후 후크를 실행하는 명령을 추가합니다.

     ```shell
     bin/magento-docker ece-deploy
     bin/magento-docker ece-post-deploy
     ```

     또는 `docker-compose` 명령을 직접 사용하는 경우 deploy 명령 뒤에 `docker-compose run deploy cloud-post-deploy` 명령을 실행합니다.

- **데이터베이스를 새로 고치는 중**-데이터베이스 컨테이너가 이제 `magento-db` 영구 도커 볼륨에 저장됩니다. Docker 환경을 새로 고치면 데이터베이스가 더 이상 자동으로 삭제되지 않습니다. 필요한 경우 다음 명령 중 하나를 사용하여 수동으로 제거합니다.

   - `magento-db` 컨테이너 제거:

     ```bash
     docker volume rm magento-db
     ```

   - Docker 컨테이너를 종료할 때 연결된 모든 볼륨을 제거합니다.

     ```bash
     docker-compose down -v
     ```

- **보관 및 백업 파일에 대한 파일 동기화 설정 무시**-docker-sync 또는 mutagen을 사용할 때 다음 확장명을 가진 보관 및 백업 파일은 더 이상 동기화되지 않습니다(SQL, GZ, ZIP 및 BZ2). 다른 확장자로 끝나도록 파일 이름을 변경하여 이러한 파일 유형에 대한 기본 파일 동기화를 재정의할 수 있습니다. 예: `synchronize-me.zip-backup`
