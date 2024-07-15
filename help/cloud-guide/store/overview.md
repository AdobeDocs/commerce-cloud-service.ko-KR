---
title: 저장소 옵션 및 구성 관리 개요
description: 클라우드 인프라에서 Adobe Commerce 스토어를 사용자 지정합니다.
feature: Cloud, Configuration, Services
exl-id: 06d477e4-02de-4742-8495-541458400e93
source-git-commit: 8a0523f1714b6ea41887e99b5c31294cf5e5255e
workflow-type: tm+mt
source-wordcount: '191'
ht-degree: 0%

---

# 저장소 옵션 및 구성 관리 개요

사용자 지정 테마 추가, 확장 설치 또는 클라우드 인프라 환경 전반에 대한 특정 구성 적용과 같이 스토어를 사용자 지정하는 방법에는 여러 가지가 있습니다. 스테이징 및 프로덕션 환경에서 직접 특정 서비스에 대한 설정을 구성할 수 있습니다. 여러 웹 사이트와 스토어를 설정할 수 있습니다. 저장소 구성은 로컬 워크스테이션에서 이러한 옵션을 구성하고 환경 간에 특정 설정을 배포하는 데 도움이 됩니다.

상점에 액세스하려면 `magento-cloud url` 명령을 사용하고 프롬프트에 응답하십시오. 또는 **사이트 액세스**&#x200B;의 [!DNL Cloud Console]에서 URL을 찾을 수 있습니다.

## 저장소 옵션 구성

저장소 옵션에는 다음이 포함됩니다.

* [B2B(Business-to-Business) 모듈](b2b-module.md)
* [사용자 정의 테마](custom-theme.md)
* [확장](extensions.md)
* [여러 사이트](multiple-sites.md)
* [결제 서비스](paypal.md)

## 서비스 및 통합 구성

원격 환경에 대한 특정 배포 동작을 관리하는 특정 [구성 파일](../environment/overview.md)이 있습니다. 이러한 항목은 별도로 검토할 수 있습니다.

* [응용 프로그램 배포](../application/configure-app-yaml.md)
* [환경 빌드 및 배포 작업](../environment/configure-env-yaml.md)
* [수신 요청 경로](../routes/routes-yaml.md)
* [지원되는 서비스](../services/services-yaml.md)

## 구성 관리

저장소 옵션, 서비스 및 통합을 구성한 후 구성 관리를 사용하여 가동 중단을 최소화하면서 일관되게 모든 환경에 이러한 구성을 배포합니다. [구성 관리](store-settings.md)를 참조하십시오.
