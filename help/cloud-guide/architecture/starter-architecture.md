---
title: 스타터 아키텍처
description: Starter 아키텍처에서 지원하는 환경에 대해 알아봅니다.
feature: Cloud, Paas
exl-id: 03365d32-4eb4-42d4-82a7-771df5e7b3da
source-git-commit: c61d711b1041ecf76ec6468cd225a34fd77c24b1
workflow-type: tm+mt
source-wordcount: '903'
ht-degree: 0%

---

# 스타터 아키텍처

Adobe Commerce on cloud infrastructure Starter 아키텍처는 최대 **4** 환경(다음 포함) `master` 초기 프로젝트 코드, 스테이징 환경 및 최대 2개의 통합 환경이 포함된 환경.

모든 환경은 PaaS(Platform as a Service) 컨테이너에 있습니다. 이러한 컨테이너는 서버 그리드의 매우 제한된 컨테이너 내에 배포됩니다. 이러한 환경은 로컬 작업 영역에서 푸시된 분기의 배포된 코드 변경 사항을 수락하는 읽기 전용입니다. 각 환경은 데이터베이스와 웹 서버를 제공합니다.

원하는 개발 및 분기 방법을 사용할 수 있습니다. 프로젝트에 대한 초기 액세스 권한을 얻으면 `staging` 의 환경 `master` 환경. 그런 다음 `integration` 에서 분기하는 환경 `staging`.

## 초보자 환경 아키텍처

다음 다이어그램은 Starter 환경의 계층적 관계를 보여 줍니다.

![스타터 프로젝트에 대한 높은 수준 보기](../../assets/starter/architecture.png)

## 프로덕션 환경

프로덕션 환경은 공개 대상 단일 및 다중 사이트 스토어를 실행하는 클라우드 인프라에 Adobe Commerce을 배포하기 위한 소스 코드를 제공합니다. 프로덕션 환경에서는 `master` 분기 를 사용하여 웹 서버, 데이터베이스, 구성된 서비스 및 애플리케이션 코드를 구성하고 활성화할 수 있습니다.

이유: `production` 환경은 읽기 전용입니다. `integration` 코드 변경을 수행할 환경에서 `integration` 끝 `staging`, 그리고 마지막으로 `production` 환경. 다음을 참조하십시오 [스토어 배포](../deploy/staging-production.md) 및 [사이트 시작](../launch/overview.md).

Adobe은 `staging` 로 푸시하기 전에 분기 `master` 분기: `production` 환경.

## 스테이징 환경

Adobe은 라는 분기를 만들 것을 권장합니다. `staging` 출처: `master`. 다음 `staging` branch는 스테이징 환경에 코드를 배포하여 코드, 모듈 및 확장, 결제 게이트웨이, 배송, 제품 데이터 등을 테스트하는 사전 프로덕션 환경을 제공합니다. 이 환경은 Fastly, New Relic APM 및 검색을 포함하여 프로덕션 환경에 맞게 모든 서비스를 구성할 수 있도록 제공합니다.

이 안내서의 추가 섹션에서는 보안 스테이징 환경에서 최종 코드 배포 및 프로덕션 수준 상호 작용을 테스트하는 방법에 대한 지침을 제공합니다. 최상의 성능 및 기능 테스트를 위해 데이터베이스를 스테이징 환경으로 복제하십시오.

>[!WARNING]
>
>Adobe은 프로덕션 환경에 배포하기 전에 스테이징 환경에서 모든 판매자와 고객 상호 작용을 테스트할 것을 권장합니다. 다음을 참조하십시오 [스토어 배포](../deploy/staging-production.md) 및 [배포 테스트](../test/staging-and-production.md).

## 통합 환경

개발자는 `integration` 개발, 배포 및 테스트할 환경:

- Adobe Commerce 애플리케이션 코드

- 사용자 지정 코드

- 확장

- 서비스

**권장 사용 사례:**

통합 환경은 제한된 테스트 및 개발을 위해 설계되었습니다. 예를 들어 통합 환경을 사용하여 다음 작업을 완료할 수 있습니다.

- CI(지속적 통합) 프로세스에 대한 변경 사항이 클라우드와 호환되는지 확인

- 홈, 카테고리, PDP(제품 세부 사항 페이지), 체크아웃 및 관리자와 같은 주요 페이지에서 중요한 워크플로우를 테스트합니다

통합 환경에서 최상의 성능을 얻으려면 다음 모범 사례를 따르십시오.

- 카탈로그 크기 제한

- 1명 또는 2명의 동시 사용자로 사용 제한

- cron 작업을 비활성화하고 필요에 따라 수동으로 실행

최대 개의 **2** 활성 통합 환경. 에서 분기를 생성하여 통합 환경을 만듭니다. `staging` 분기입니다. 통합 환경을 만들 때 환경 이름이 분기 이름과 일치합니다. 통합 환경은 웹 서버와 데이터베이스를 포함한다. 모든 서비스가 포함되지 않습니다. 예를 들어 Fastly CDN 및 New Relic은 사용할 수 없습니다.

코드 스토리지에 대한 비활성 분기의 수는 제한이 없습니다. 비활성 분기에 액세스하고, 확인하고, 테스트하려면 해당 분기를 활성화해야 합니다

{{enhanced-integration-envs}}

## 프로덕션 및 스테이징 기술 스택

프로덕션 및 스테이징 환경에는 다음 기술이 포함됩니다. 다음을 통해 이러한 기술을 수정하고 구성할 수 있습니다. [`.magento.app.yaml`](../application/configure-app-yaml.md) 파일.

- HTTP 캐싱 및 CDN용 Fastly
- PHP-FPM과 대화하는 Nginx 웹 서버, 여러 작업자가 있는 하나의 인스턴스
- Redis 서버
- Adobe Commerce 2.2 - 2.4.3-p2에 대한 카탈로그 검색 Elasticsearch
- Adobe Commerce 2.3.7-p3, 2.4.3-p2 및 2.4.4 이상에 대한 카탈로그 검색 OpenSearch
- 이그레스 필터링(아웃바운드 방화벽)

### 서비스

클라우드 인프라의 Adobe Commerce은 현재 PHP, MySQL(MariaDB), Elasticsearch(Adobe Commerce 2.2 ~ 2.4.3-p2), OpenSearch(2.3.7-p3, 2.4.3-p2, 2.4.4 이상), Redis 및 [!DNL RabbitMQ].

각 서비스는 별도의 보안 컨테이너에서 실행됩니다. 컨테이너는 프로젝트에서 함께 관리됩니다. 다음과 같은 일부 서비스는 표준입니다.

- HTTP 라우터(수신 요청을 처리하지만 캐싱 및 리디렉션도 처리)

- PHP 애플리케이션 서버

- Git

- SSH(Secure Shell)

### 소프트웨어 버전

클라우드 인프라의 Adobe Commerce은 Debian GNU/Linux 운영 체제와 NGINX 웹 서버를 사용합니다. 이 소프트웨어를 업그레이드할 수 없지만 다음에 대한 버전을 구성할 수 있습니다.

- [PHP](../application/php-settings.md)

- [MySQL](../services/mysql.md)

- [레디스](../services/redis.md)

- [RabbitMQ](../services/rabbitmq.md)

- [Elasticsearch](../services/elasticsearch.md)

- [OpenSearch](../services/opensearch.md)

스테이징 및 프로덕션 환경에서는 CDN 및 캐싱에 Fastly를 사용합니다. Fastly CDN 확장의 최신 버전은 프로젝트의 초기 프로비저닝 중에 설치됩니다. 확장을 업그레이드하여 최신 버그 수정 및 개선 사항을 얻을 수 있습니다. 다음을 참조하십시오 [Magento 2용 Fastly CDN 모듈](https://github.com/fastly/fastly-magento2). 또한 다음에 대한 액세스 권한이 있습니다. [New Relic](../monitor/account-management.md) 성능 모니터링용.

다음 파일을 사용하여 구현에 사용할 소프트웨어 버전을 구성합니다.

- [`.magento.app.yaml`](../application/configure-app-yaml.md)

- [&#39;route.yaml&#39;](../routes/routes-yaml.md)

- [`services.yaml`](../services/services-yaml.md)

### 백업 및 재해 복구

다음을 사용하여 데이터베이스 및 파일 시스템의 백업을 만들 수 있습니다. [!DNL Cloud Console] 또는 CLI를 사용할 수 있습니다. 다음을 참조하십시오 [백업 관리](../storage/snapshots.md).

## 개발 준비

다음 워크플로는 코드를 분기하고 스토어를 개발 및 배포하는 프로세스를 요약합니다.

1. 로컬 환경 설정

1. 복제 `master` 로컬 환경으로 분기

1. 만들기 `staging` 에서 분기 `master`

1. 에서 개발을 위한 분기 만들기 `staging`

1. 테스트를 위해 환경에 빌드하고 배포하는 Git에 코드 푸시

스토어를 개발, 테스트 및 배포하는 자세한 지침 및 연습은 다음 섹션을 참조하십시오.

- [초급 개발 및 배포 워크플로](starter-develop-deploy-workflow.md)

- [도커 개발](../dev-tools/cloud-docker.md) (Cloud Docker for Commerce에서 사용할 수 있는 로컬 개발 환경)

- [분기 관리](../project/console-branches.md)

- [스토어 배포](../deploy/staging-production.md)

- [사이트 시작](../launch/overview.md)
