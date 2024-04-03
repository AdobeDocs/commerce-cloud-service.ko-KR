---
title: 배포 모범 사례
description: 클라우드 인프라에 Adobe Commerce을 배포하기 위한 모범 사례를 살펴보십시오.
feature: Cloud, Deploy, Best Practices
exl-id: bac3ca83-0eee-4fda-9a5c-a84ab25a837a
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '1904'
ht-degree: 0%

---

# 배포 모범 사례

빌드 및 배포 스크립트는 원격 환경에 코드를 병합할 때 활성화됩니다. 이러한 스크립트는 환경을 사용합니다. [구성 파일](../environment/overview.md) 및 애플리케이션 코드를 사용하여 적절한 데이터 및 서비스를 클라우드 인프라에 프로비저닝할 수 있습니다. 또한 이러한 스크립트는 클라우드 환경에서 Adobe Commerce 애플리케이션, 서드파티 서비스 및 사용자 정의 확장을 설치하거나 업데이트하는 데 사용됩니다.

빌드 및 배포 프로세스는 각 플랜에 따라 약간 다릅니다.

- **스타터 플랜**—통합 환경의 경우 모든 활성 분기가 액세스 및 테스트를 위해 전체 환경에 빌드 및 배포됩니다. 를 로 병합한 후 코드를 완전히 테스트합니다. `staging` 분기입니다. 사이트를 시작하려면 푸시 `staging` 끝 `master` 를 클릭하여 프로덕션 환경에 배포합니다. 를 통해 모든 분기에 액세스할 수 있습니다. [!DNL Cloud Console] 및 CLI 명령을 사용할 수 있습니다.

- **Pro 플랜**—통합 환경의 경우 모든 활성 분기가 액세스 및 테스트를 위해 전체 환경에 빌드 및 배포됩니다. 에 코드 병합 `integration` 스테이징 및 프로덕션 환경으로 병합하기 전에 분기합니다. 를 사용하여 스테이징 및 프로덕션 환경에 병합할 수 있습니다. [!DNL Cloud Console] 또는 SSH 및 `magento-cloud` CLI 명령

## 프로세스 추적

터미널 또는 을 사용하여 실시간으로 빌드 및 배포 작업을 추적할 수 있습니다. [!DNL Cloud Console] 상태 메시지—`in-progress`, `pending`, `success`, 또는 `failed`- 배포 프로세스 중에 표시됩니다. 로그 파일에서 세부 사항을 볼 수 있습니다. 다음을 참조하십시오 [로그 보기](../test/log-locations.md).

외부 GitHub 저장소를 사용하는 경우 작업 로그가 GitHub 세션에 표시되지 않습니다. 그러나 외부 저장소 및 의 인터페이스에서 활동을 따를 수 있습니다. [!DNL Cloud Console]. 다음을 참조하십시오 [통합](../integrations/overview.md).

>[!NOTE]
>
>통합 환경에서는 의 배포 로그를 볼 수 없습니다. [!DNL Cloud Console]. 이 기능은 프로덕션 및 스테이징 환경에만 사용할 수 있습니다. 그러나 를 사용하여 모든 환경에서 배포의 모든 단계에 대한 로그를 볼 수 있습니다. [빌드 및 배포](../test/log-locations.md#build-and-deploy-logs) 로그. 문제 해결 정보는 [배포 오류 참조](../dev-tools/error-reference.md).

다음을 활성화할 수 있습니다. [New Relic을 사용하여 배포 추적](../monitor/track-deployments.md) 배포 이벤트를 모니터링하고 배포 간 성능을 분석합니다.

## 빌드 및 배포에 대한 우수 사례

배포 프로세스에 대한 다음 모범 사례 및 고려 사항을 검토하십시오.

- **의 최신 버전을 실행 중인지 확인합니다. `ece-tools` 패키지**

  다음을 참조하십시오 [ECE-Tools 릴리스 정보](../release-notes/ece-tools-package.md).

- **빌드 및 배포 프로세스를 따릅니다**

  환경 간에 코드를 병합할 때 구성을 덮어쓰지 않도록 각 환경에 올바른 코드가 있는지 확인하십시오. 예를 들어 모든 환경에 구성 변경 사항을 적용하려면 원격 통합 환경에 배포하기 전에 로컬 환경에서 변경 사항을 수정하고 테스트합니다. 그런 다음 프로덕션에 배포하기 전에 스테이징 환경에서 변경 사항을 배포하고 테스트합니다. 한 환경에서 다른 환경으로 병합하는 경우 배포는 환경별 구성 및 설정을 제외한 환경의 모든 코드를 덮어씁니다.

- **여러 환경에서 동일한 변수 사용**

  이러한 변수의 값은 환경에 따라 다를 수 있지만 일반적으로 각 환경에서 동일한 변수가 필요합니다. 다음을 참조하십시오 [저장소 설정에 대한 구성 관리](../store/store-settings.md).

- **중요한 구성 값 및 데이터를 환경별 변수에 유지**

  이러한 값에는 Cloud CLI를 사용하여 지정된 변수인 [!DNL Cloud Console]또는에 추가됨 `env.php` 파일. 다음을 참조하십시오 [변수 수준](../environment/variable-levels.md).

- **환경 분기에서 모든 코드를 사용할 수 있는지 확인합니다.**

  비공개 분기와 같은 다른 분기의 코드를 참조하면 빌드 및 배포 프로세스 중에 문제가 발생할 수 있습니다. 예를 들어 개인 분기의 테마를 참조하는 경우 테마에 액세스할 수 없어 애플리케이션 코드로 빌드할 수 없습니다.

- **반복된 분기에 확장, 통합 및 코드 추가**

  로컬에서 변경 작업을 수행하고 테스트하여으로 푸시 `integration`, 그런 다음 로 `staging` 및 `production`. 업데이트를 다음 환경에 병합하기 전에 각 환경에서 문제를 테스트하고 해결합니다. 일부 확장 및 통합은 종속성으로 인해 특정 순서로 활성화하고 구성해야 합니다. 그룹에 추가하고 테스트하면 빌드 및 배포 프로세스가 훨씬 쉬워지고 문제 발생 위치를 확인하는 데 도움이 됩니다.

- **서비스 버전, 관계 및 연결 기능 확인**

  응용 프로그램에서 사용할 수 있는 서비스를 확인하고 호환되는 최신 버전을 사용 중인지 확인하십시오. 다음을 참조하십시오 [서비스 관계](../services/services-yaml.md#service-relationships) 및 [시스템 요구 사항](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html) 다음에서 _설치 안내서_ 권장 버전.

- **스테이징 및 프로덕션에 배포하기 전에 로컬에서 및 통합 환경에서 테스트합니다.**

  로컬 및 통합 환경의 문제를 식별하고 수정하여 스테이징 및 프로덕션 환경에 배포할 때 가동 중지 시간이 길어지는 것을 방지합니다.

  >[!TIP]
  >
  >다음 항목이 있습니다. [스마트 마법사](../deploy/smart-wizards.md) 클라우드 프로젝트 구성이 SCD(정적 콘텐츠 배포) 전략을 비롯한 빌드 및 배포 구성에 대한 모범 사례를 따르는지 확인하는 데 사용할 수 있는 명령입니다.

- **로컬 및 통합 환경에서 테스트를 완료한 후 스테이징 환경에서 배포 및 테스트**

  다음을 참조하십시오 [스테이징 및 프로덕션 테스트](../test/staging-and-production.md).

- **프로덕션 환경 구성 확인**

  프로덕션에 배포하기 전에 다음 작업을 완료하십시오.

   - 를 사용하여 프로덕션 환경의 세 노드 모두에 연결할 수 있는지 확인합니다. [SSH](../development/secure-connections.md).

   - 인덱서가 로 설정되어 있는지 확인합니다. _일정에 따라 업데이트_. 다음을 참조하십시오 [색인 생성 모드](https://developer.adobe.com/commerce/php/development/components/indexing/) 다음에서 _확장 개발자 안내서_.

   - 프로덕션 코드에서 환경별 변수를 업데이트하고, 서비스 가용성 및 호환성을 확인하고, 기타 필요한 구성을 변경하여 환경을 준비합니다.

- **배포 프로세스 모니터링**

  배포 상태 메시지를 검토하고 필요에 따라 문제를 완화할 수 있습니다. 클라우드 검토 [로그](../test/log-locations.md#) 자세한 로그 메시지.

## 5단계 통합 구축 및 구축

다음 단계는 로컬 개발 환경 및 통합 환경에서 발생합니다. Pro 플랜의 경우 이러한 초기 단계에서 코드가 스테이징 또는 프로덕션 환경에 배포되지 않습니다.

### 1단계: 코드 및 구성 유효성 검사

처음에 프로젝트를 설정할 때, [클라우드 인프라 템플릿](https://github.com/magento/magento-cloud) 는 코드 파일에 대한 기반을 제공합니다. 이 코드 리포지토리는 프로젝트로 복제됩니다. `master` 분기입니다.

- **스타터용**—`master` 분기는 프로덕션 환경입니다.
- **Pro용**—`master` 는 통합 환경의 원본 분기로 시작됩니다.

에서 분기 만들기 `master` 사용자 지정 코드, 확장 및 모듈, 타사 통합. 클라우드에서 코드를 테스트하기 위한 원격 통합 환경이 있습니다.

로컬 작업 영역에서 원격 저장소로 코드를 푸시하면 빌드 및 배포 스크립트가 시작되기 전에 일련의 확인 및 코드 유효성 검사가 완료됩니다. 기본 제공 Git 서버는 푸시하고 있는 내용을 확인하고 변경합니다. 예를 들어 OpenSearch 서비스를 추가하는 경우, 기본 제공 Git 서버는 클러스터의 토폴로지가 그에 따라 수정되었는지 확인합니다.

구성 파일에 구문 오류가 있으면 Git 서버가 푸시를 거부합니다. 다음을 참조하십시오 [보호 블록](../development/protective-block.md).

이 단계도 실행됩니다 `composer install` 종속성을 검색합니다.

### 2단계: 빌드

>[!NOTE]
>
>빌드 단계에서 사이트는 유지 관리 모드가 아니며 오류 또는 문제가 발생하는 경우 종료되지 않습니다. 이전 빌드 이후에 변경된 코드만 빌드합니다.

이 단계에서는 코드베이스를 빌드하고 `build` 섹션 / `.magento.app.yaml`. 기본 빌드 후크는 `php ./vendor/bin/ece-tools` 명령을 실행하고 다음을 수행합니다.

- 에서 패치 적용 `vendor/magento/ece-patches`및 의 프로젝트 특정 패치(선택 사항) `m2-hotfixes`
- 코드 및 를 다시 생성합니다. [종속성 삽입](https://experienceleague.adobe.com/docs/commerce-operations/operational-playbook/glossary.html) 구성(즉, `generated/` 디렉터리, 다음 포함 `generated/code` 및 `generated/metapackage`) 사용 `bin/magento setup:di:compile`.
- 다음을 확인함: [`app/etc/config.php`](../store/store-settings.md) 파일이 코드 베이스에 있습니다. Adobe Commerce은 빌드 단계에서 이 파일을 감지하지 못하고 모듈 및 확장 목록이 포함된 경우 이 파일을 자동으로 생성합니다. 존재하는 경우 빌드 단계는 정상적으로 계속되고 GZIP을 사용하여 정적 파일을 압축하고 배포하므로 배포 단계에서 다운타임이 줄어듭니다. 을(를) 참조하십시오 [빌드 옵션](../environment/variables-build.md) 파일 압축 사용자 지정 또는 비활성화에 대해 알아봅니다.

>[!WARNING]
>
>이때 클러스터가 생성되지 않았으므로 데이터베이스에 연결하거나 활성 데몬 프로세스가 있다고 가정하지 마십시오.

애플리케이션이 빌드되면 **읽기 전용 파일 시스템**. 읽기/쓰기를 수행할 특정 마운트 지점을 구성할 수 있습니다. 서버에 FTP를 보내고 모듈을 추가할 수 없습니다. 대신 로컬 저장소에 코드를 추가하고 를 실행해야 합니다 `git push`: 환경을 빌드하고 배포합니다. 프로젝트 구조에 대해서는 를 참조하십시오. [로컬 프로젝트 디렉토리 구조](../project/file-structure.md).

### 3단계: 슬러그 준비

빌드 단계의 결과는 라고 하는 읽기 전용 파일 시스템입니다. _개요_. 이 단계에서는 아카이브를 만들고 슬러그를 영구 저장소에 배치합니다. 다음에 코드를 푸시할 때 서비스가 변경되지 않으면 아카이브의 슬러그가 사용됩니다.

- 변경되지 않은 코드를 재사용하여 지속적인 통합을 보다 빠르게 빌드
- 코드가 변경되면 재사용할 다음 빌드의 슬러그를 업데이트합니다.
- 필요한 경우 배포를 즉시 되돌릴 수 있습니다.
- 다음과 같은 경우 정적 파일을 포함합니다. `app/etc/config.php` 파일이 코드 베이스에 있음

슬러그에는 모든 파일과 폴더가 포함됩니다 **다음을 제외** 마운트 구성 `magento.app.yaml`:

- `"var": "shared:files/var"`
- `"app/etc": "shared:files/etc"`
- `"pub/media": "shared:files/media"`
- `"pub/static": "shared:files/static"`

### 4단계: 슬러그 및 클러스터 배포

애플리케이션 및 모든 [백엔드](https://experienceleague.adobe.com/docs/commerce-operations/operational-playbook/glossary.html) 다음과 같은 서비스 제공:

- 웹 서버, OpenSearch, OpenSearch 등의 컨테이너에 각 서비스를 마운트합니다. [!DNL RabbitMQ]
- 읽기-쓰기 파일 시스템(고가용성 분산 스토리지 그리드에 마운트) 마운트
- 서비스가 서로(그리고 서로) &quot;확인&quot;할 수 있도록 네트워크를 구성합니다.

>[!NOTE]
>
>모든 빌드 및 배포가 완료되고 다시 푸시한 후 Git 분기에서 변경 작업을 수행합니다. 모든 환경 파일 시스템은 _읽기 전용_. 읽기 전용 시스템은 파일 시스템에 쓸 수 있는 프로세스가 없으므로 결정론적 배포를 보장하고 사이트 보안을 크게 향상시킵니다. 또한 코드가 통합, 스테이징 및 프로덕션 환경에서 동일한지 확인하는 데도 작동합니다.

### 5단계: 배포 후크

>[!NOTE]
>
>이 단계에서는 [!DNL Commerce] 배포가 완료될 때까지 응용 프로그램이 유지 관리 모드에 있습니다.

마지막 단계에서는 개발 환경에서 데이터를 익명화하고, 캐시를 지우고, 외부 연속 통합 도구를 쿼리하는 데 사용할 수 있는 배포 스크립트를 실행합니다. 이 스크립트가 실행되면 Redis와 같은 환경의 모든 서비스에 액세스할 수 있습니다.

다음과 같은 경우 `app/etc/config.php` 파일이 코드 베이스에 없습니다. 정적 파일은 다음을 사용하여 압축됩니다. `gzip` 배포 단계 및 사이트 유지 관리 기간이 늘어나는 이 단계에서 배포됩니다.

>[!NOTE]
>
>을(를) 참조하십시오 [변수 배포](../environment/variables-deploy.md) 파일 압축 사용자 지정 또는 비활성화에 대해 알아봅니다.

두 개의 배포 후크가 있습니다. 다음 `pre-deploy.php` 후크는 빌드 후크에서 생성된 리소스 및 코드에 대한 필요한 정리 및 검색을 완료합니다. 다음 `php ./vendor/bin/ece-tools deploy` hook은 일련의 명령과 스크립트를 실행합니다.

- Adobe Commerce이 **설치되지 않음**, 와 함께 설치됩니다. `bin/magento setup:install`, 배포 구성 업데이트, `app/etc/env.php`및 지정된 환경에 대한 데이터베이스(예: Redis 및 웹 사이트 URL). **중요 사항:** 완료 시 [최초 배포](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/launch/overview.html) 설치하는 동안 Adobe Commerce이 모든 환경에 설치되고 배포되었습니다.

- If Adobe Commerce **이(가) 설치됨**&#x200B;필요한 업그레이드를 수행합니다. 배포 스크립트가 실행됩니다 `bin/magento setup:upgrade` 데이터베이스 스키마 및 데이터(확장 또는 코어 코드 업데이트 후 필요)를 업데이트하고 배포 구성도 업데이트하려면 `app/etc/env.php`및 사용자 환경에 대한 데이터베이스를 추가합니다. 마지막으로 배포 스크립트는 Adobe Commerce 캐시를 지웁니다.

- 스크립트는 선택적으로 명령을 사용하여 정적 웹 콘텐츠를 생성합니다 `magento setup:static-content:deploy`.

- 범위 사용(`-s` 기본 설정인 빌드 스크립트의 플래그 `quick` 정적 콘텐츠 배포 전략의 경우. 환경 변수를 사용하여 전략을 사용자 정의할 수 있습니다 [`SCD_STRATEGY`](../environment/variables-deploy.md#scd_strategy). 이러한 옵션 및 기능에 대한 자세한 내용은 [정적 파일 배포 전략](../deploy/static-content.md) 및 `-s` 플래그 [정적 보기 파일 배포](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html).

>[!NOTE]
>
>배포 스크립트는에서 구성 파일에 정의된 값을 사용합니다. `.magento` 디렉토리가 표시되면 스크립트는 디렉토리와 해당 컨텐트를 삭제합니다. 로컬 개발 환경은 영향을 받지 않습니다.

### 배포 후: 라우팅 구성

배포가 실행되는 동안 프로세스는 시작 지점에서 60초 동안 들어오는 트래픽을 중지하고 웹 트래픽이 새로 만든 클러스터에 도달하도록 라우팅을 다시 구성합니다.

배포에 성공하면 유지 관리 모드가 제거되어 일반 액세스가 허용되고 `app/etc/env.php` 및 `app/etc/config.php` 구성 파일입니다.

를 사용하여 정적 콘텐츠 생성 활성화 `SCD_ON_DEMAND` 변수 및 구성 [`post_deploy` 후크](../application/hooks-property.md) 캐시를 지우고 캐시를 미리 로드(웜)합니다. _이후_ 컨테이너가 연결을 수락하기 시작하고 _다음 기간 동안_ 일반, 들어오는 트래픽.

빌드 및 배포 로그를 검토하려면 다음을 참조하십시오 [로그 보기](../test/log-locations.md#view-and-manage-logs).
