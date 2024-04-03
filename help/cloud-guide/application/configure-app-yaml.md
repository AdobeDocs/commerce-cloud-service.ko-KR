---
title: 애플리케이션 배포 구성
description: 애플리케이션 구성 파일에서 의 방식을 제어하는 속성을 구성하는 방법을 알아봅니다. [!DNL Commerce] 애플리케이션이 클라우드 환경에 빌드하고 배포합니다.
feature: Cloud, Configuration, Build, Deploy
exl-id: 900da20d-98d2-4c9f-97ec-578aee775b55
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '142'
ht-degree: 0%

---

# 애플리케이션 배포 구성

다음 `.magento.app.yaml` 파일은 응용 프로그램이 빌드하고 배포하는 방식을 제어합니다. 클라우드 인프라의 Adobe Commerce은 프로젝트당 여러 애플리케이션을 지원하지만 일반적으로 프로젝트에는 `.magento.app.yaml` 저장소의 루트에 있는 파일입니다.

다음 `.magento.app.yaml` 에는 많은 기본값이 있습니다. 다음을 참조하십시오. [샘플 `.magento.app.yaml` 파일](https://github.com/magento/magento-cloud/blob/master/.magento.app.yaml). 항상 검토 `.magento.app.yaml` 설치 버전용입니다. 이 파일은 Adobe Commerce 클라우드 인프라 버전마다 다를 수 있습니다.

사용 `.magento.app.yaml` 파일을 사용하여 다음 구성 값을 정의할 수 있습니다.

- [속성](properties.md)- 응용 프로그램 인스턴스의 속성 값을 정의합니다.
- [변수 속성](variables-property.md)—에 필요한 환경 변수 검토 [!DNL Commerce] 애플리케이션 버전.
- [PHP 설정](php-settings.md)- 런타임 PHP 옵션을 구성합니다.
- [정적 파일에 대한 캐시 설정](set-cache.md)—미디어 및 정적 파일에 대한 캐시 TTL을 설정합니다.
