---
title: 구성 파일 개요
description: 맞춤화된 Adobe Commerce 스토어의 배포 및 관리를 지원하도록 클라우드 인프라 환경을 구성하는 방법에 대해 알아봅니다.
feature: Cloud, Configuration, Services, Iaas, Paas
exl-id: f469a0ec-e459-413f-9725-66a0fbf34f01
source-git-commit: 47b66d0d2bbff14e76ce49182a68d5e6c9fb13a7
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 0%

---

# 구성 파일 개요

Adobe Commerce on cloud infrastructure의 환경에는 Adobe Commerce 애플리케이션 코드베이스 및 파일을 위한 완전한 시스템을 제공하기 위한 애플리케이션, 서비스 및 데이터베이스가 포함된 컨테이너가 포함됩니다.

다음 구성 파일을 사용하여 프로젝트 환경을 지원하도록 애플리케이션 설정, 경로, 빌드 및 배포 작업, 알림을 구성할 수 있습니다.

| 구성 | 파일 이름 | 설명 |
| ------------- | -------- | ----------- |
| [애플리케이션](../application/configure-app-yaml.md) | `.magento.app.yaml` | 서비스, 후크 및 cron 작업을 포함하여 Adobe Commerce을 빌드하고 배포하는 방법을 정의합니다. |
| [환경](configure-env-yaml.md) | `.magento.env.yaml` | 환경 변수를 사용하여 Pro Staging 및 프로덕션을 포함하여 모든 환경에서 빌드 및 배포 작업을 중앙 집중화합니다. |
| [경로](../routes/routes-yaml.md) | `.magento/routes.yaml` | 캐싱, 리디렉션 및 서버측 포함을 구성합니다. |
| [서비스](../services/services-yaml.md) | `.magento/services.yaml` | Adobe Commerce이 사용하는 서비스를 이름 및 버전별로 정의합니다. 예를 들어, 이 파일은 MariaDB, PHP 확장, Redis, RabbitMQ, Elasticsearch 또는 OpenSearch 버전을 포함할 수 있습니다. 이러한 변경 사항을 Pro 계획 스테이징 및 프로덕션 환경에 푸시하려면 지원 티켓을 열어야 합니다. |
| [PHP 설정](../application/php-settings.md#configure-php) | `php.ini` | 프로젝트에 추가할 수 있는 선택적 파일입니다. 이 파일에 포함된 설정은 클라우드 인프라에서 유지 관리하는 설정에 추가됩니다. |

{style="table-layout:auto"}

## Pro 환경에 대한 구성 업데이트

클라우드 인프라 Pro 스테이징 및 프로덕션 환경의 Adobe Commerce의 경우 로컬 개발 환경에서 많은 구성 옵션을 업데이트하고 변경 사항을 커밋하여 이러한 환경에 적용할 수 있습니다. 그러나 다음을 수행해야 합니다. [Adobe Commerce 지원 티켓 제출](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) 다음 구성 옵션을 갱신하려면

- 에서 서비스 설치 또는 업데이트 `.magento/services.yaml` 파일.
- 에 대한 구성 변경 `mounts` 및 `disk` 의 속성 `.magento.app.yaml` 파일.

{{pro-self-service-warning}}
