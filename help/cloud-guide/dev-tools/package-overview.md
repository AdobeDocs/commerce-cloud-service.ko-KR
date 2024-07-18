---
title: '[!DNL ECE-Tools] 패키지'
description: ' [!DNL ECE-Tools] 패키지 및 Adobe Commerce 관리 및 배포 방법에 대해 알아봅니다.'
exl-id: 5583a685-29c5-4de5-8d2e-94cff5ff37ab
source-git-commit: b49a51aba56f79b5253eeacb1adf473f42bb8959
workflow-type: tm+mt
source-wordcount: '431'
ht-degree: 0%

---

# ECE-Tools 패키지

[!DNL ECE-Tools] 패키지는 [!DNL Commerce] 응용 프로그램을 관리하고 배포하도록 설계된 스크립트 및 도구 집합입니다. `ece-tools` 패키지를 사용하면 cron 작업 관리, 프로젝트 구성 확인, Adobe 패치 및 핫픽스 적용 등 많은 프로세스가 간소화됩니다. GitHub의 [오픈 소스 [!DNL ECE-Tools] 코드 저장소][ece-repo]를 보고 기여할 수 있습니다.

{{ece-tools-package}}

`ece-tools` 패키지는 버전 2.1.4부터 Adobe Commerce과 호환되며 코드를 관리하고 프로젝트를 자동으로 빌드 및 배포하는 데 도움이 되도록 설계된 클라우드 인프라 명령의 스크립트와 Adobe Commerce을 포함합니다.

다음은 사용 가능한 `ece-tools` 명령 목록입니다.

```bash
php ./vendor/bin/ece-tools list
```

## 빌드 및 배포

`ece-tools` 패키지에는 클라우드 인프라 응용 프로그램에서 Adobe Commerce을 시작하는 빌드, 배포 및 배포 후 단계에 대한 작업을 수행하는 명령이 포함되어 있습니다. 예를들어, `php ./vendor/bin/ece-tools build` 명령은 응용 프로그램 빌드 프로세스를 시작합니다.

기본적으로 이 `ece-tools` 명령은 `.magento.app.yaml` 구성 파일의 [hooks 속성](../application/hooks-property.md)에 있습니다.

## 도커 구성 생성기

`ece-tools` 패키지에는 클라우드 인프라에서 Adobe Commerce에 대한 도커 개발 환경을 시작할 수 있도록 도커 이미지에 대한 기능 및 구성 파일을 제공하는 [magento/magento-cloud-docker] 패키지에 대한 종속성이 포함되어 있습니다. Commerce용 Cloud Docker를 독립형 패키지로 실행할 수도 있습니다. [Docker 개발](../dev-tools/cloud-docker.md)을 참조하세요.

## 서비스, 경로 및 변수

`ece-tools` 패키지를 사용하여 모든 클라우드 환경에서 사용되는 Base64 인코딩 [클라우드 변수](../environment/variables-cloud.md)에 대한 자세한 정보를 표시할 수 있습니다. 다음 명령은 모든 서비스, 경로 및 변수를 표시합니다.

```bash
php ./vendor/bin/ece-tools env:config:show
```

특정 정보 세트를 표시하려면 다음 형식을 사용합니다.

```bash
php ./vendor/bin/ece-tools env:config:show <option>
```

- `services` - `services.yaml` 파일에 정의된 `MAGENTO_CLOUD_RELATIONSHIPS` 환경 변수의 관계 데이터를 표시합니다.
- `routes` - `MAGENTO_CLOUD_ROUTES` 환경 변수를 사용하여 프로젝트에 대해 구성된 경로를 표시합니다.
- `variables` - `MAGENTO_CLOUD_VARIABLES` 환경 변수를 사용하여 프로젝트에 대해 구성된 변수를 표시합니다.

`services` 옵션에 대한 샘플 출력:

```
Magento Cloud Services:
+-----------------------------------+----------------------------------+
| Service Configuration             | Value                            |
+-----------------------------------+----------------------------------+
| database:                                                            |
+-----------------------------------+----------------------------------+
| host                              | 127.0.0.1                        |
| password                          | <password>                       |
| port                              | 3306                             |
+-----------------------------------+----------------------------------+
| opensearch:                                                          |
+-----------------------------------+----------------------------------+
| host                              | 127.0.0.1                        |
| port                              | 9200                             |
...
```

## 환경 구성 확인

프로젝트의 구성을 평가하는 데 도움이 되는 일련의 확인 명령을 사용할 수 있습니다. 각 마법사 명령에 대한 자세한 설명은 _배포 최적화_ 섹션의 [스마트 마법사](../deploy/smart-wizards.md)를 참조하십시오. 빌드 단계에서 `wizard:ideal-state` 명령이 자동으로 실행됩니다. 프로젝트의 이상적인 상태를 확인하려면:

```bash
php ./vendor/bin/ece-tools wizard:ideal-state
```

>[!NOTE]
>
>원격 클라우드 환경에서 `wizard:ideal-state` 명령을 실행해야 합니다. 이 명령은 로컬 개발 환경에서 실행할 때 항상 `The configured state is not ideal` 오류를 반환합니다.

샘플 출력:

```
Ideal state is configured
```

[ece-tools 릴리스 정보](../release-notes/cloud-tools-suite.md)를 참조하십시오.

## Adobe 패치 및 사용자 정의 패치

`ece-tools` 패키지에는 모든 Adobe Commerce 버전과 클라우드 환경의 통합을 개선하고 중요한 수정 사항의 빠른 전달을 지원하는 Adobe 패치 및 핫픽스를 제공하는 [magento/magento-cloud-patches] 패키지에 대한 종속성이 포함되어 있습니다. 또한 은 Adobe Commerce on cloud infrastructure 프로젝트에 추가하는 사용자 정의 패치를 제공합니다. [패치 적용](../development/apply-patches.md)을 참조하세요.

<!-- link definitions -->

[ece-repo]: https://github.com/magento/ece-tools
[magento/magento-cloud-docker]: https://github.com/magento/magento-cloud-docker
[magento/magento-cloud 패치]: https://github.com/magento/magento-cloud-patches
