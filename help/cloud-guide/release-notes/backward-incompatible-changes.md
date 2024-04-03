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

이전 버전과 호환되지 않는 변경 사항을 적용하려면 최신 릴리스의 로 업그레이드할 때 기존 클라우드 프로젝트에 대한 클라우드 구성 및 프로세스를 조정해야 할 수 있습니다. `ece-tools` 패키지 또는 기타 Cloud Tools for Commerce 패키지

## 변경 사항 `ece-tools` 패키지

이전에 포함된 일부 기능 `ece-tools` 이제 패키지가 개별 패키지로 제공됩니다. 이러한 패키지는 의 작성기 종속성입니다. `ece-tools`- ece-tools를 설치하거나 업데이트할 때 자동으로 설치되고 업데이트됩니다.

새 아키텍처는 설치 또는 업데이트 프로세스에 영향을 주지 않습니다. 그러나 Adobe Commerce on cloud infrastructure 프로젝트에서 작업할 때 일부 명령 구문 및 프로세스를 변경해야 할 수 있습니다. 자세한 내용은 다음 역호환하지 않는 변경 내용 정보 및 을 검토하십시오. [Cloud Tools Suite 릴리스 노트](cloud-tools-suite.md).

### 서비스 버전 요구 사항 변경

을 사용하는 클라우드 프로젝트에 대한 최소 PHP 버전 요구 사항을 7.0.x에서 7.1.x로 변경했습니다 `ece-tools` v2002.1.0 이상 환경 구성이 PHP 7.0을 지정하는 경우 [php 구성](../application/php-settings.md) 다음에서 `.magento.app.yaml` 파일.

>[!WARNING]
>
>PHP 버전 요구 사항 변경으로 인해, `ece-tools` 2002.1.0은 Adobe Commerce 2.1.15 이상을 실행하는 클라우드 인프라 프로젝트에서 Adobe Commerce만 지원합니다. 프로젝트에서 이전 릴리스를 사용하는 경우 [업그레이드](../development/commerce-version.md) 로 업데이트하기 전에 `ece-tools` 2002.1.0.

### 환경 구성 변경 사항

다음 표에서는 제거되었거나 더 이상 사용되지 않는 환경 변수 및 기타 환경 구성 파일에 대한 정보를 제공합니다. `ece-tools` v2002.1.0.

| 항목 | 교체 |
| -------- | ----------- |
| `SCD_EXCLUDE_THEMES` 변수 | [`SCD_MATRIX`](../environment/variables-build.md#scd_matrix) |
| `STATIC_CONTENT_THREADS` 변수 | [`SCD_THREADS`](../environment/variables-build.md#scd_threads) |
| `DO_DEPLOY_STATIC_CONTENT` 변수 | [`SKIP_SCD`](../environment/variables-build.md#skip_scd) |
| `STATIC_CONTENT_SYMLINK` 변수 | 없음. 이제 빌드는 항상 정적 콘텐츠 디렉터리에 대한 symlink를 만듭니다 `pub/static`. |
| `build_options.ini` 파일 | 사용 [`.magento.env.yaml`](../application/configure-app-yaml.md) 모든 환경에서 빌드 및 배포 작업을 관리하도록 환경 변수를 구성하는 파일입니다.<p>다음을 포함하는 클라우드 환경을 빌드하는 경우 `build_options.ini` 파일, 빌드가 실패합니다. |

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

이전 ECE-Tools 릴리스에서는 `m2-ece-build` 및 `m2-ece-deploy` 에서 배포 후크를 구성하는 명령 `.magento.app.yaml` 파일. v2002.1.0으로 업데이트하면 `hooks` 에서 구성 `.magento.app.yaml` 사용되지 않는 명령에 대한 파일을 만든 다음 필요한 경우 바꿉니다.

## 클라우드 패치 변경 사항

- **다운로드한 패치 제거**-다음 `magento/magento-cloud-patches` 패키지 번들 사용 가능한 모든 패치 [소프트웨어 다운로드](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/commerce.html) 를 페이지에 추가하고 클라우드에 배포할 때 자동으로 적용합니다. ECE-Tools 2002.1.0 이상으로 업그레이드한 후 패치 충돌을 방지하려면 수동으로 프로젝트에 다운로드하여 추가한 Adobe 제공 패치를 제거합니다.

- **패치 적용 명령 업데이트**-에서 패치를 적용하는 명령을 이동했습니다. `vendor/bin/ece-tools` 디렉토리 대상: `vendor/bin/ece-patches` 디렉토리. 이 명령을 사용하여 패치를 수동으로 적용하는 경우 새 경로를 사용합니다.

  > 수동으로 패치 적용

  ```bash
  php ./vendor/bin/ece-patches apply
  ```

## Cloud Docker 변경 사항

- **최소 PHP 버전 요구 사항은 이제 PHP 7.1입니다**-Commerce용 Cloud Docker 호스트에서 이전 버전을 실행 중인 경우 PHP v7.1 이상으로 업그레이드하십시오.

- **Commerce용 Cloud Docker 명령 변경**-

   - **도커 빌드 작업을 위해 Cloud Docker for Commerce 명령 업데이트**-에서 Commerce용 Cloud Docker 명령을 이동했습니다. `vendor/bin/ece-tools` 디렉토리 대상: `vendor/bin/ece-docker` 디렉토리. 새 경로를 사용하도록 스크립트 및 명령을 업데이트합니다.

     로 업그레이드한 후 `ece-tools` 2002.1.0, 다음 명령을 사용하여 사용 가능한 `ece-docker` 명령입니다.

     ```bash
     php ./vendor/bin/ece-docker list
     ```

   - **Cloud docker-compose 명령 업데이트**-에서 명령 파일에 대한 경로 이름을 변경했습니다. `./bin/docker` 끝 `./bin/magento-docker`. 새 경로를 사용하도록 스크립트 및 명령을 업데이트합니다.

   - **Cron 컨테이너는 더 이상 기본 도커 구성에 포함되지 않습니다.**-이제 다음을 추가해야 합니다. `--with-cron` 옵션 `ece-docker build:compose` docker 환경 구성에 Cron 컨테이너를 포함하는 명령입니다. 다음을 참조하십시오 [크론 작업 관리](https://developer.adobe.com/commerce/cloud-tools/docker/configure/manage-cron-jobs/) 다음에서 _Commerce용 Cloud Docker_ 가이드.

     cron 작업이 있는 이전에 생성된 컨테이너에 이제 cron 컨테이너가 없는 스크립트입니다.

   - **임시 컨테이너 사용**-이전 버전에서 컨테이너는 `bin/magento-docker` 명령 작업은 제거되지 않았으므로 다른 작업에 사용할 수 있습니다. 이제 `magento-docker` 명령은 명령이 완료된 후 만드는 모든 컨테이너를 제거합니다.

     도커 작성 작업으로 생성된 컨테이너를 유지하려면 `docker-compose run` 명령 대신 `bin/magento-docker` 명령입니다.

   - **배포 후 후크 실행**-다음 `cloud-deploy` 명령이 더 이상 배포 후 후크를 실행하지 않습니다. 새 항목 사용 `cloud-post-deploy` 배포 후 사후 배포 후크를 실행하는 명령입니다. 스크립트를 업데이트하여 배포 후 후크를 실행하는 명령을 추가합니다.

     ```shell
     bin/magento-docker ece-deploy
     bin/magento-docker ece-post-deploy
     ```

     또는 다음을 사용하는 경우 `docker-compose` 명령을 직접 실행하고 `docker-compose run deploy cloud-post-deploy` deploy 명령 뒤에 있는 명령입니다.

- **데이터베이스 새로 고침**-이제 데이터베이스 컨테이너가 `magento-db` 영구 도커 볼륨. Docker 환경을 새로 고치면 데이터베이스가 더 이상 자동으로 삭제되지 않습니다. 필요한 경우 다음 명령 중 하나를 사용하여 수동으로 제거합니다.

   - 제거 `magento-db` 컨테이너:

     ```bash
     docker volume rm magento-db
     ```

   - Docker 컨테이너를 종료할 때 연결된 모든 볼륨을 제거합니다.

     ```bash
     docker-compose down -v
     ```

- **보관 및 백업 파일에 대한 파일 동기화 설정 무시**-Docker-sync 또는 mutagen을 사용할 때 SQL, GZ, ZIP 및 BZ2 확장명을 가진 보관 및 백업 파일이 더 이상 동기화되지 않습니다. 다른 확장자로 끝나도록 파일 이름을 변경하여 이러한 파일 유형에 대한 기본 파일 동기화를 재정의할 수 있습니다. 예: `synchronize-me.zip-backup`
