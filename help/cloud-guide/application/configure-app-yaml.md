---
title: 애플리케이션 배포 구성
description: ' [!DNL Commerce] 응용 프로그램이 클라우드 환경에 빌드되고 배포되는 방식을 제어하는 응용 프로그램 구성 파일의 속성을 구성하는 방법에 대해 알아봅니다.'
feature: Cloud, Configuration, Build, Deploy
exl-id: 900da20d-98d2-4c9f-97ec-578aee775b55
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '142'
ht-degree: 0%

---

# 애플리케이션 배포 구성

`.magento.app.yaml` 파일은 응용 프로그램이 빌드하고 배포하는 방법을 제어합니다. 클라우드 인프라의 Adobe Commerce은 프로젝트당 여러 애플리케이션을 지원하지만 일반적으로 프로젝트에는 리포지토리의 루트에 `.magento.app.yaml` 파일이 있는 단일 애플리케이션이 있습니다.

`.magento.app.yaml`에 많은 기본값이 있습니다. [샘플 `.magento.app.yaml` 파일](https://github.com/magento/magento-cloud/blob/master/.magento.app.yaml)을 참조하세요. 설치된 버전에 대한 `.magento.app.yaml`을(를) 항상 검토하십시오. 이 파일은 Adobe Commerce 클라우드 인프라 버전마다 다를 수 있습니다.

`.magento.app.yaml` 파일을 사용하여 다음 구성 값을 정의합니다.

- [속성](properties.md) - 응용 프로그램 인스턴스의 속성 값을 정의합니다.
- [변수 속성](variables-property.md)—[!DNL Commerce] 응용 프로그램 버전에 필요한 환경 변수를 검토합니다.
- [PHP 설정](php-settings.md)—런타임 PHP 옵션을 구성합니다.
- [정적 파일에 대한 캐시 설정](set-cache.md)—미디어 및 정적 파일에 대한 캐시 TTL을 설정합니다.
