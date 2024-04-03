---
title: 배포 프로세스
description: 클라우드 인프라 프로젝트에서 Adobe Commerce에 대한 배포가 작동하는 방식을 알아봅니다.
feature: Cloud, Build, Deploy, SCD
exl-id: 378fa290-5a71-4ac2-816a-a7c837e45b2f
source-git-commit: 3d9e3294872fedcc43744f54a71c71d8951ed853
workflow-type: tm+mt
source-wordcount: '396'
ht-degree: 0%

---

# 배포 프로세스

배포 프로세스는 환경의 병합, 푸시 또는 동기화를 수행하거나 를 트리거할 때 시작됩니다. [수동 재배포](../dev-tools/cloud-cli-overview.md#redeploy-the-environment). 배포 프로세스에 시간이 걸리지만 개발 및 테스트 중인지 또는 라이브 사이트를 사용하여 작업하고 있는지에 따라 배포를 최적화하는 방법이 있습니다. 가장 주목할 만한 사실은 [정적 콘텐츠 배포](static-content.md).

배포 프로세스에는 빌드, 배포 및 배포 후 배포의 세 가지 단계가 있습니다. 각 단계는 제한된 리소스로 특정 작업을 수행합니다.

## ![빌드 단계](../../assets/status-build.png) 빌드 단계

다음 _빌드_ 단계 는 구성 파일에 정의된 서비스에 대한 컨테이너를 어셈블하고 `composer.lock` 파일에 정의된 빌드 후크를 실행하고 `.magento.app.yaml` 파일. 서비스에 연결하거나 데이터베이스에 액세스할 수 없는 경우 빌드 단계는 환경에 제한된 리소스에 따라 다릅니다.

## ![배포 단계](../../assets/status-deploy.png) 배포 단계

다음 _배포_ 이 단계에서는 들어오는 요청에 대해 임시 보류를 지정한 다음 사이트로 전환합니다. [유지 관리 모드](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/setup/application-modes.html). 배포 단계에서는 새 컨테이너를 사용하고 파일 시스템을 마운트한 후 네트워크 연결을 열고 `relationships` 의 섹션 `.magento.app.yaml` 파일에 정의된 배포 후크를 실행합니다. `.magento.app.yaml` 파일. 모든 것이 _읽기 전용_&#x200B;에 정의된 디렉토리 제외 `.magento.app.yaml` 파일. 기본적으로 [`mounts` 속성](../application/properties.md#mounts) 에는 다음 디렉터리가 포함되어 있습니다.

- `app/etc`- 다음을 포함합니다. `env.php` 및 `config.php` 구성 파일
- `pub/media`—제품 또는 범주와 같은 모든 미디어 데이터를 포함합니다.
- `pub/static`- 생성된 정적 파일을 포함합니다.
- `var`- 런타임 중에 생성된 임시 파일을 포함합니다.

다른 모든 디렉터리에는 읽기 전용 권한이 있습니다. 새 사이트는 유지 관리 모드에서 전환하고 수신 요청에 대한 임시 보류를 해제하면 배포 단계가 끝날 때 활성화됩니다.

배포 단계에서 `app/etc/config.php` 및 `app/etc/env.php` 배포 구성 파일은 BAK 확장명으로 저장됩니다. 다음을 참조하십시오 [스토어 설정](../store/store-settings.md#restore-configuration-files) 이러한 파일을 복원하는 방법에 대해 알아봅니다.

## ![배포 후 단계](../../assets/status-post-deploy.png) 배포 후 단계

다음 _사후 배포_ 단계는 다음에 정의된 후 배포 후크를 실행합니다. `.magento.app.yaml` 파일. 이 단계에서 작업을 수행하면 사이트 성능에 영향을 줄 수 있지만 [WARM_UP_PAGES](../environment/variables-post-deploy.md#warmuppages) 환경 변수를 사용하여 캐시를 채웁니다.

## ![상태 확인](../../assets/status-verify.png) 구성 확인

를 실행하여 프로젝트 상태에 대한 최적의 구성을 테스트할 수 있습니다. [스마트 마법사](smart-wizards.md).

>[!NOTE]
>
>포함 `ece-tools` 2002.1.0 이상 버전에서는 시나리오 기반 배포 기능을 사용하여 Adobe Commerce on cloud infrastructure 프로젝트의 빌드, 배포 및 배포 후 프로세스를 사용자 지정할 수 있습니다. 다음을 참조하십시오 [시나리오 기반 배포](scenario-based.md).
