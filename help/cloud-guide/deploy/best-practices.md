---
title: 배포 모범 사례
description: 클라우드 인프라에 Adobe Commerce을 배포하기 위한 모범 사례를 살펴보십시오.
feature: Cloud, Deploy, Best Practices
exl-id: bac3ca83-0eee-4fda-9a5c-a84ab25a837a
source-git-commit: 269681efb9925d78ffb608ecbef657be740b5531
workflow-type: tm+mt
source-wordcount: '1904'
ht-degree: 0%

---

# 배포 모범 사례

빌드 및 배포 스크립트는 원격 환경에 코드를 병합할 때 활성화됩니다. 이러한 스크립트는 환경 [구성 파일](../environment/overview.md) 및 응용 프로그램 코드를 사용하여 적절한 데이터 및 서비스를 사용하여 클라우드 인프라를 프로비저닝합니다. 또한 이러한 스크립트는 클라우드 환경에서 Adobe Commerce 애플리케이션, 서드파티 서비스 및 사용자 정의 확장을 설치하거나 업데이트하는 데 사용됩니다.

빌드 및 배포 프로세스는 각 플랜에 따라 약간 다릅니다.

- **스타터 계획** - 통합 환경의 경우 모든 활성 분기가 빌드되고 액세스 및 테스트를 위해 전체 환경에 배포됩니다. `staging` 분기로 병합한 후 코드를 완전히 테스트합니다. 사이트를 시작하려면 `staging`을(를) `master`(으)로 푸시하여 프로덕션 환경에 배포합니다. [!DNL Cloud Console] 및 CLI 명령을 통해 모든 분기에 액세스할 수 있습니다.

- **Pro 계획** - 통합 환경의 경우 모든 활성 분기가 빌드되고 액세스 및 테스트를 위해 전체 환경에 배포됩니다. 스테이징 및 프로덕션 환경으로 병합하기 전에 `integration` 분기에 코드를 병합하십시오. [!DNL Cloud Console]을(를) 사용하거나 SSH 및 `magento-cloud` CLI 명령을 사용하여 스테이징 및 프로덕션 환경에 병합할 수 있습니다.

## 프로세스 추적

배포 프로세스 중에 표시되는 [!DNL Cloud Console] 상태 메시지(`in-progress`, `pending`, `success` 또는 `failed`)를 사용하여 빌드 및 배포 작업을 실시간으로 추적할 수 있습니다. 로그 파일에서 세부 사항을 볼 수 있습니다. [로그 보기](../test/log-locations.md)를 참조하세요.

외부 GitHub 저장소를 사용하는 경우 작업 로그가 GitHub 세션에 표시되지 않습니다. 그러나 외부 저장소 및 [!DNL Cloud Console]에 대한 인터페이스에서 활동을 따를 수 있습니다. [통합](../integrations/overview.md)을 참조하세요.

>[!NOTE]
>
>통합 환경에서는 [!DNL Cloud Console]에서 배포 로그를 볼 수 없습니다. 이 기능은 프로덕션 및 스테이징 환경에만 사용할 수 있습니다. 그러나 [빌드 및 배포](../test/log-locations.md#build-and-deploy-logs) 로그를 사용하여 모든 환경에서 배포의 모든 단계에 대한 로그를 볼 수 있습니다. 문제 해결 정보는 [배포 오류 참조](../dev-tools/error-reference.md)를 참조하십시오.

[New Relic을 사용하여 배포 추적](../monitor/track-deployments.md)을 사용하면 배포 이벤트를 모니터링하고 배포 간 성능을 분석할 수 있습니다.

## 빌드 및 배포에 대한 우수 사례

배포 프로세스에 대한 다음 모범 사례 및 고려 사항을 검토하십시오.

- **최신 버전의 `ece-tools` 패키지를 실행 중인지 확인**

  [ECE-Tools 릴리스 정보](../release-notes/ece-tools-package.md)를 참조하십시오.

- **빌드 및 배포 프로세스를 따릅니다**

  환경 간에 코드를 병합할 때 구성을 덮어쓰지 않도록 각 환경에 올바른 코드가 있는지 확인하십시오. 예를 들어 모든 환경에 구성 변경 사항을 적용하려면 원격 통합 환경에 배포하기 전에 로컬 환경에서 변경 사항을 수정하고 테스트합니다. 그런 다음 프로덕션에 배포하기 전에 스테이징 환경에서 변경 사항을 배포하고 테스트합니다. 한 환경에서 다른 환경으로 병합하는 경우 배포는 환경별 구성 및 설정을 제외한 환경의 모든 코드를 덮어씁니다.

- **여러 환경에서 동일한 변수 사용**

  이러한 변수의 값은 환경에 따라 다를 수 있지만 일반적으로 각 환경에서 동일한 변수가 필요합니다. 저장소 설정에 대한 [구성 관리](../store/store-settings.md)를 참조하세요.

- **중요한 구성 값 및 데이터를 환경별 변수에 유지**

  이러한 값에는 Cloud CLI를 사용하여 지정되거나 [!DNL Cloud Console]을(를) 사용하거나 `env.php` 파일에 추가된 변수가 포함됩니다. [변수 수준](../environment/variable-levels.md)을 참조하세요.

- **환경 분기에서 모든 코드를 사용할 수 있는지 확인**

  비공개 분기와 같은 다른 분기의 코드를 참조하면 빌드 및 배포 프로세스 중에 문제가 발생할 수 있습니다. 예를 들어 개인 분기의 테마를 참조하는 경우 테마에 액세스할 수 없어 애플리케이션 코드로 빌드할 수 없습니다.

- **반복된 분기에 확장, 통합 및 코드 추가**

  로컬에서 변경 작업을 수행하고 테스트한 후 `integration`을(를) 푸시하고 `staging` 및 `production`을(를) 푸시합니다. 업데이트를 다음 환경에 병합하기 전에 각 환경에서 문제를 테스트하고 해결합니다. 일부 확장 및 통합은 종속성으로 인해 특정 순서로 활성화하고 구성해야 합니다. 그룹에 추가하고 테스트하면 빌드 및 배포 프로세스가 훨씬 쉬워지고 문제 발생 위치를 확인하는 데 도움이 됩니다.

- **서비스 버전 및 관계 및 연결 기능을 확인합니다**

  응용 프로그램에서 사용할 수 있는 서비스를 확인하고 호환되는 최신 버전을 사용 중인지 확인하십시오. 권장 버전은 _설치 안내서_&#x200B;의 [서비스 관계](../services/services-yaml.md#service-relationships) 및 [시스템 요구 사항](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html)을 참조하십시오.

- **스테이징 및 프로덕션에 배포하기 전에 로컬에서 및 통합 환경에서 테스트합니다**

  로컬 및 통합 환경의 문제를 식별하고 수정하여 스테이징 및 프로덕션 환경에 배포할 때 가동 중지 시간이 길어지는 것을 방지합니다.

  >[!TIP]
  >
  >클라우드 프로젝트 구성이 SCD(정적 콘텐츠 배포) 전략을 포함하여 빌드 및 배포 구성에 대한 모범 사례를 따르는지 확인하는 데 사용할 수 있는 [스마트 마법사](../deploy/smart-wizards.md) 명령이 있습니다.

- **로컬 및 통합 환경에서 테스트를 완료한 후 스테이징 환경에서 배포하고 테스트하십시오**

  [스테이징 및 프로덕션 테스트](../test/staging-and-production.md)를 참조하십시오.

- **프로덕션 환경 구성 확인**

  프로덕션에 배포하기 전에 다음 작업을 완료하십시오.

   - [SSH](../development/secure-connections.md)을(를) 사용하여 프로덕션 환경의 세 노드에 모두 연결할 수 있는지 확인하십시오.

   - 인덱서가 _일정에 따라 업데이트_(으)로 설정되어 있는지 확인하십시오. _Extension Developer Guide_&#x200B;에서 [인덱싱 모드](https://developer.adobe.com/commerce/php/development/components/indexing/)를 참조하십시오.

   - 프로덕션 코드에서 환경별 변수를 업데이트하고, 서비스 가용성 및 호환성을 확인하고, 기타 필요한 구성을 변경하여 환경을 준비합니다.

- **배포 프로세스 모니터링**

  배포 상태 메시지를 검토하고 필요에 따라 문제를 완화할 수 있습니다. 자세한 로그 메시지는 클라우드 [로그](../test/log-locations.md#)를 검토하십시오.

## 5단계 통합 구축 및 구축

다음 단계는 로컬 개발 환경 및 통합 환경에서 발생합니다. Pro 플랜의 경우 이러한 초기 단계에서 코드가 스테이징 또는 프로덕션 환경에 배포되지 않습니다.

### 1단계: 코드 및 구성 유효성 검사

처음 프로젝트를 설정할 때 [클라우드 인프라 템플릿](https://github.com/magento/magento-cloud)에서 코드 파일에 대한 기준을 제공합니다. 이 코드 리포지토리는 프로젝트에 `master` 분기로 복제됩니다.

- **Starter**—`master` 분기는 프로덕션 환경입니다.
- **Pro**—`master`의 경우 통합 환경의 원본 분기로 시작됩니다.

사용자 지정 코드, 확장 및 모듈, 타사 통합을 위해 `master`에서 분기를 만듭니다. 클라우드에서 코드를 테스트하기 위한 원격 통합 환경이 있습니다.

로컬 작업 영역에서 원격 저장소로 코드를 푸시하면 빌드 및 배포 스크립트가 시작되기 전에 일련의 확인 및 코드 유효성 검사가 완료됩니다. 기본 제공 Git 서버는 푸시하고 있는 내용을 확인하고 변경합니다. 예를 들어 OpenSearch 서비스를 추가하는 경우, 기본 제공 Git 서버는 클러스터의 토폴로지가 그에 따라 수정되었는지 확인합니다.

구성 파일에 구문 오류가 있으면 Git 서버가 푸시를 거부합니다. [보호 차단](../development/protective-block.md)을 참조하세요.

이 단계는 또한 `composer install`을(를) 실행하여 종속성을 검색합니다.

### 2단계: 빌드

>[!NOTE]
>
>빌드 단계에서 사이트는 유지 관리 모드가 아니며 오류 또는 문제가 발생하는 경우 종료되지 않습니다. 이전 빌드 이후에 변경된 코드만 빌드합니다.

이 단계에서는 코드 베이스를 빌드하고 `.magento.app.yaml`의 `build` 섹션에서 후크를 실행합니다. 기본 빌드 후크는 `php ./vendor/bin/ece-tools` 명령이며 다음을 수행합니다.

- `vendor/magento/ece-patches`에 패치를 적용하고 `m2-hotfixes`에 프로젝트별 선택적 패치를 적용합니다.
- `bin/magento setup:di:compile`을(를) 사용하여 코드와 [종속성 삽입](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/glossary) 구성(즉, `generated/code` 및 `generated/metapackage`을(를) 포함하는 `generated/` 디렉터리)을 다시 생성합니다.
- [`app/etc/config.php`](../store/store-settings.md) 파일이 코드 베이스에 있는지 확인합니다. Adobe Commerce은 빌드 단계에서 이 파일을 감지하지 못하고 모듈 및 확장 목록이 포함된 경우 이 파일을 자동으로 생성합니다. 존재하는 경우 빌드 단계는 정상적으로 계속되고 GZIP을 사용하여 정적 파일을 압축하고 배포하므로 배포 단계에서 다운타임이 줄어듭니다. 파일 압축 사용자 지정 또는 비활성화에 대한 자세한 내용은 [빌드 옵션](../environment/variables-build.md)을 참조하세요.

>[!WARNING]
>
>이때 클러스터가 생성되지 않았으므로 데이터베이스에 연결하거나 활성 데몬 프로세스가 있다고 가정하지 마십시오.

응용 프로그램이 빌드되면 **읽기 전용 파일 시스템**&#x200B;에 탑재됩니다. 읽기/쓰기를 수행할 특정 마운트 지점을 구성할 수 있습니다. 서버에 FTP를 보내고 모듈을 추가할 수 없습니다. 대신 로컬 리포지토리에 코드를 추가하고 환경을 빌드하고 배포하는 `git push`을(를) 실행해야 합니다. 프로젝트 구조는 [로컬 프로젝트 디렉터리 구조](../project/file-structure.md)를 참조하십시오.

### 3단계: 슬러그 준비

빌드 단계의 결과는 _slug_(으)로 참조되는 읽기 전용 파일 시스템입니다. 이 단계에서는 아카이브를 만들고 슬러그를 영구 저장소에 배치합니다. 다음에 코드를 푸시할 때 서비스가 변경되지 않으면 아카이브의 슬러그가 사용됩니다.

- 변경되지 않은 코드를 재사용하여 지속적인 통합을 보다 빠르게 빌드
- 코드가 변경되면 재사용할 다음 빌드의 슬러그를 업데이트합니다.
- 필요한 경우 배포를 즉시 되돌릴 수 있습니다.
- `app/etc/config.php` 파일이 코드 베이스에 있는 경우 정적 파일을 포함합니다.

슬러그에는 `magento.app.yaml`에 구성된 **마운트를 제외한**&#x200B;모든 파일 및 폴더가 포함되어 있습니다.

- `"var": "shared:files/var"`
- `"app/etc": "shared:files/etc"`
- `"pub/media": "shared:files/media"`
- `"pub/static": "shared:files/static"`

### 4단계: 슬러그 및 클러스터 배포

응용 프로그램 및 모든 [백엔드](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/glossary) 서비스가 다음과 같이 프로비저닝됩니다.

- 웹 서버, OpenSearch, [!DNL RabbitMQ]과(와) 같은 컨테이너에 각 서비스를 탑재합니다.
- 읽기-쓰기 파일 시스템(고가용성 분산 스토리지 그리드에 마운트) 마운트
- 서비스가 서로(그리고 서로) &quot;확인&quot;할 수 있도록 네트워크를 구성합니다.

>[!NOTE]
>
>모든 빌드 및 배포가 완료되고 다시 푸시한 후 Git 분기에서 변경 작업을 수행합니다. 모든 환경 파일 시스템이 _읽기 전용_&#x200B;입니다. 읽기 전용 시스템은 파일 시스템에 쓸 수 있는 프로세스가 없으므로 결정론적 배포를 보장하고 사이트 보안을 크게 향상시킵니다. 또한 코드가 통합, 스테이징 및 프로덕션 환경에서 동일한지 확인하는 데도 작동합니다.

### 5단계: 배포 후크

>[!NOTE]
>
>이 단계에서는 배포가 완료될 때까지 [!DNL Commerce] 응용 프로그램을 유지 관리 모드로 전환합니다.

마지막 단계에서는 개발 환경에서 데이터를 익명화하고, 캐시를 지우고, 외부 연속 통합 도구를 쿼리하는 데 사용할 수 있는 배포 스크립트를 실행합니다. 이 스크립트가 실행되면 Redis와 같은 환경의 모든 서비스에 액세스할 수 있습니다.

`app/etc/config.php` 파일이 코드 베이스에 없는 경우 정적 파일은 `gzip`을(를) 사용하여 압축되고 이 단계 동안 배포되므로 배포 단계 및 사이트 유지 관리의 길이가 늘어납니다.

>[!NOTE]
>
>파일 압축 사용자 지정 또는 비활성화에 대한 자세한 내용은 [변수 배포](../environment/variables-deploy.md)를 참조하세요.

두 개의 배포 후크가 있습니다. `pre-deploy.php` 후크는 빌드 후크에서 생성된 리소스 및 코드의 필요한 정리 및 검색을 완료합니다. `php ./vendor/bin/ece-tools deploy` 후크는 일련의 명령과 스크립트를 실행합니다.

- Adobe Commerce이 **설치되지 않음**&#x200B;인 경우 `bin/magento setup:install`과(와) 함께 설치되고 배포 구성, `app/etc/env.php` 및 지정한 환경에 대한 데이터베이스를 업데이트합니다(예: Redis 및 웹 사이트 URL). **중요:** 설치 중에 [처음 배포](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/launch/overview.html)를 완료하면 Adobe Commerce이 모든 환경에 설치되고 배포되었습니다.

- Adobe Commerce **이(가) 설치**&#x200B;된 경우 필요한 업그레이드를 수행하십시오. 배포 스크립트는 `bin/magento setup:upgrade`을(를) 실행하여 데이터베이스 스키마 및 데이터(확장 또는 핵심 코드 업데이트 후 필요)를 업데이트하고 환경에 대한 배포 구성, `app/etc/env.php` 및 데이터베이스도 업데이트합니다. 마지막으로 배포 스크립트는 Adobe Commerce 캐시를 지웁니다.

- 스크립트는 선택적으로 `magento setup:static-content:deploy` 명령을 사용하여 정적 웹 콘텐츠를 생성합니다.

- 정적 콘텐츠 배포 전략에 기본 설정인 `quick`의 범위(빌드 스크립트의 `-s` 플래그)를 사용합니다. 환경 변수 [`SCD_STRATEGY`](../environment/variables-deploy.md#scd_strategy)을(를) 사용하여 전략을 사용자 지정할 수 있습니다. 이러한 옵션 및 기능에 대한 자세한 내용은 [정적 파일 배포 전략](../deploy/static-content.md) 및 [정적 보기 파일 배포](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html)에 대한 `-s` 플래그를 참조하십시오.

>[!NOTE]
>
>배포 스크립트는 `.magento` 디렉터리의 구성 파일에 정의된 값을 사용한 다음, 스크립트는 디렉터리와 해당 내용을 삭제합니다. 로컬 개발 환경은 영향을 받지 않습니다.

### 배포 후: 라우팅 구성

배포가 실행되는 동안 프로세스는 시작 지점에서 60초 동안 들어오는 트래픽을 중지하고 웹 트래픽이 새로 만든 클러스터에 도달하도록 라우팅을 다시 구성합니다.

배포가 완료되면 유지 관리 모드가 제거되어 일반 액세스가 허용되고 `app/etc/env.php` 및 `app/etc/config.php` 구성 파일에 대한 백업(BAK) 파일이 만들어집니다.

`SCD_ON_DEMAND` 변수를 사용하여 정적 콘텐츠 생성을 사용하도록 설정하고, [`post_deploy` 후크](../application/hooks-property.md)를 구성하여 캐시를 지우고 _후_ 컨테이너가 연결을 수락하기 시작하고 _받는 동안_&#x200B;의 정상적인 트래픽을 받는 동안 캐시를 미리 로드(Warms)합니다.

빌드 및 배포 로그를 검토하려면 [로그 보기](../test/log-locations.md#view-and-manage-logs)를 참조하세요.
