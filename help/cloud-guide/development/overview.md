---
title: 개발 개요
description: Adobe Commerce on cloud infrastructure 프로젝트를 통해 로컬 개발을 준비합니다.
role: Developer
feature: Cloud, Install
topic: Development
last-substantial-update: 2024-02-06T00:00:00Z
exl-id: d4452d7d-d3dc-4f8d-8bd7-76f05d89f545
source-git-commit: 99272d08a11f850a79e8e24857b7072d1946f374
workflow-type: tm+mt
source-wordcount: '548'
ht-degree: 0%

---

# 개발 개요

클라우드 인프라 원격 환경의 Adobe Commerce은 모든 Starter 환경과 모든 Pro 통합, 스테이징 및 프로덕션 환경을 포함하여 **읽기 전용**&#x200B;입니다. 로컬 개발 환경에서는 스테이징 및 프로덕션에 추가 테스트 및 배포를 위해 코드를 통합 환경에 푸시하기 전에 작성하고 테스트할 수 있습니다.

로컬 작업 영역을 준비하기 전에 [자격 증명](../../get-started/prepare-workspace.md)이 있는지 확인하십시오. [Commerce용 Cloud Docker](#docker-environment)를 사용하지 않도록 선택하지 않는 한 로컬 개발에는 PHP 및 Composer를 설치해야 합니다.

## 필수 패키지

클라우드 인프라의 Adobe Commerce은 Composer를 사용하여 프로젝트에 대한 종속성 및 업그레이드를 관리합니다. 로컬 개발을 위해 클라우드 프로젝트와 호환되는 PHP 및 Composer 버전을 설치해야 합니다. 예를 들어 [!DNL Commerce] 2.4.7 클라우드 템플릿을 사용하는 경우 [`.magento.app.yaml`](https://github.com/magento/magento-cloud/blob/2.4.7/.magento.app.yaml) 구성 파일이 **PHP 8.3** 및 **작성기 2.7.2**&#x200B;을 사용함을 확인할 수 있습니다.

Composer는 `vendor` 디렉터리에 프로젝트에 필요한 라이브러리 및 종속성을 설치합니다. 다음 필수 Composer 파일은 프로젝트 루트 디렉토리에 있습니다.

- `composer.json` - `composer.json` 파일을 사용하여 제품 설치 및 업그레이드를 관리합니다.
- `composer.lock` - `composer.lock` 파일은 프로젝트의 종속성 트리에 있는 모든 패키지에 대한 모든 요구 사항의 버전 제한을 충족하는 정확한 버전 종속성 집합을 저장합니다.

**일반 명령:**

| 명령 | 설명 |
|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| `composer update` | `composer.json` 파일에 반영된 최신 버전의 종속성에 대한 업데이트입니다. `composer.lock` 파일을 업데이트합니다. |
| `composer install` | 종속성을 다운로드할 `composer.lock` 파일을 읽습니다. 프로젝트 저장소에 `composer.lock`의 최신 복사본을 보관하는 것이 좋습니다. |

{style="table-layout:auto"}

업데이트된 코드를 추가, 커밋 및 푸시하면 [빌드 단계](../deploy/process.md#build-phase-build-phase) 동안 배포 프로세스에서 `composer install` 명령을 자동으로 실행합니다.

### 클라우드 메타패키지

클라우드 인프라의 Adobe Commerce은 `magento/product-enterprise-edition`이(가) 필요한 메타패키지를 사용합니다. 최신 버전의 Commerce에 대한 최신 업데이트를 가져오려면 다음 제한 구문을 사용하십시오.

```text
>=current_version <next_version
```

예를 들어 최신 Adobe Commerce 버전 2.4.7을 사용하려면 `composer.json` 파일에서 `2.4.7`을(를) &quot;현재&quot; 버전으로 설정하고 `2.4.8`을(를) &quot;다음&quot; 버전으로 설정합니다.

```text
"magento/magento-cloud-metapackage": ">=2.4.7 <2.4.8"
```

이 메타패키지의 기본 패키지는 다음과 같습니다.

- **vendor/magento/ece-tools** - `ece-tools` 패키지는 Adobe Commerce 버전 2.1.4 이상과 호환되므로 Adobe Commerce on cloud infrastructure 프로젝트를 관리하는 데 사용할 수 있는 다양한 기능을 제공합니다. 코드를 관리하고 프로젝트를 자동으로 빌드 및 배포하는 데 도움이 되도록 설계된 클라우드 인프라 명령의 스크립트와 Adobe Commerce이 포함되어 있습니다. [`ece-tools` 패키지 개요](../dev-tools/package-overview.md)를 참조하세요.
- **vendor/magento/product-enterprise-edition**—이 메타패키지에는 모듈, 프레임워크, 테마 등을 비롯한 애플리케이션 구성 요소가 필요합니다.
- **vendor/fastly2/magento2**—이 모듈은 Pro 스테이징 및 프로덕션 및 스타터 프로덕션 환경에 대한 Fastly CDN 및 서비스를 관리합니다. [Fastly 서비스](/help/cloud-guide/cdn/fastly.md#fastly-cdn-module-for-magento-2)를 참조하세요.
- **vendor/magento/module-paypal-on-boarding**—이 모듈은 PayPal 가맹점 계정에 연결하여 PayPal 결제 게이트웨이 체크아웃을 제공합니다. [PayPal 온보딩 도구](../store/paypal.md)를 참조하세요.

>[!TIP]
>
>종속성 및 타사 라이선스 목록은 _Adobe Commerce 릴리스 정보_&#x200B;의 [Commerce용 클라우드 패키지](/help/cloud-guide/release-notes/cloud-packages.md)를 참조하십시오.

## Docker 환경

Commerce용 Cloud Docker 도구를 사용하여 로컬 개발을 위한 클라우드 인프라 프로덕션 및 개발 환경에서 Adobe Commerce을 에뮬레이션할 수 있습니다. Commerce용 Cloud Docker에서는 PHP 및 Composer를 로컬에 설치할 필요가 없습니다.

- Adobe Developer 사이트의 [Cloud Docker를 사용한 로컬 개발](https://developer.adobe.com/commerce/cloud-tools/docker/setup/)
- [Docker 아키텍처 및 공통 명령](../dev-tools/cloud-docker.md)
- [Cloud Docker 릴리스 정보](../release-notes/cloud-docker.md)

>[!TIP]
>
>클라우드 인프라에서 Adobe Commerce과 함께 Git 기반 호스팅 서비스를 사용하는 방법에 대한 자세한 내용은 [통합](../integrations/overview.md)을 참조하십시오.
