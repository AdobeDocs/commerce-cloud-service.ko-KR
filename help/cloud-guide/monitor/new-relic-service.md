---
title: New Relic 서비스
description: Adobe Commerce on cloud infrastructure 프로젝트에서 사용할 수 있는 New Relic 서비스에 대해 알아봅니다.
feature: Cloud, Observability
last-substantial-update: 2023-09-06T00:00:00Z
exl-id: 613f0694-5338-4037-8ee4-ac5eca376159
source-git-commit: b53d5a8d06e0ed249bd44b6f38f837a257cbd315
workflow-type: tm+mt
source-wordcount: '369'
ht-degree: 0%

---

# New Relic 서비스 개요

클라우드 인프라의 모든 Adobe Commerce 프로젝트에는 성능을 모니터링하고 [!DNL Commerce] 응용 프로그램 및 클라우드 인프라의 이벤트를 조사하는 데 도움이 되는 New Relic 서비스에 대한 액세스 권한이 포함되어 있습니다.

다음 New Relic 기능은 프로덕션 및 스테이징 환경에서 사용할 수 있습니다.

- [New Relic APM](#new-relic-apm)(Pro 및 Starter)
- [New Relic 인프라](#new-relic-infrastructure)(Pro 전용)
- [New Relic 로그 관리](#new-relic-log-management)(Pro 전용)

>[!INFO]
>
>다른 New Relic 기능은 Adobe Commerce 프로젝트에서 사용할 수 없습니다.

## NEW RELIC API

[APM(응용 프로그램 성능 관리)용 New Relic](https://docs.newrelic.com/introduction-apm/)은 응용 프로그램 상호 작용을 분석하고 개선하는 데 도움이 되는 소프트웨어 분석 제품입니다. New Relic APM은 모든 Adobe Commerce on cloud infrastructure 프로젝트에서 사용할 수 있으며, 다음과 같은 기능을 제공합니다.

- **특정 거래에 집중** - 장바구니에 추가, 체크아웃 또는 결제 처리와 같은 사이트의 주요 고객 작업을 적극적으로 표시하고 모니터링합니다.
- **데이터베이스 쿼리 모니터링**—성능에 영향을 주는 데이터베이스 쿼리를 찾아 모니터링합니다.
- **앱 맵** - 사이트, 확장 및 외부 서비스 내의 모든 응용 프로그램 종속성을 봅니다.
- **[!DNL Apdex]점수** - 성능을 평가하고 문제를 식별하고 발생 시 알리는 경고(예: 플래시 판매나 웹 이벤트의 영향을 받는 사이트 성능)를 만듭니다. [Apdex 점수](https://docs.newrelic.com/docs/apm/new-relic-apm/apdex/apdex-measure-user-satisfaction/)를 참조하세요.
- **Adobe Commerce에 대한 관리 경고**-이 New Relic 경고 정책을 사용하여 업계 모범 사례를 기반으로 응용 프로그램 및 인프라 성능을 모니터링합니다. [Adobe Commerce 경고 정책에 대한 관리 경고를 사용하여 성능 모니터링](investigate-performance.md/#monitor-performance-with-managed-alerts)을 참조하십시오.
- **배포 추적** - 배포 이벤트를 모니터링하고 배포가 전체 성능에 미치는 영향을 분석합니다. [배포 추적](track-deployments.md)을 참조하세요.

Adobe Commerce on cloud infrastructure 프로젝트에는 라이선스 키와 함께 New Relic APM 서비스용 소프트웨어가 포함되어 있습니다. 추가 소프트웨어를 구입하거나 설치할 필요가 없습니다.

## New Relic 인프라

Pro 프로젝트에는 동적 서버 모니터링을 제공하기 위해 응용 프로그램 데이터 및 성능 분석과 자동으로 연결되는 [New Relic 인프라(NRI)](https://docs.newrelic.com/docs/infrastructure/infrastructure-monitoring/get-started/get-started-infrastructure-monitoring/) 서비스가 포함됩니다. 이 서비스는 프로프로덕션 및 스테이징 환경에서 사용할 수 있습니다.

## New Relic 로그 관리

모든 클라우드 인프라 프로젝트에는 [New Relic 로그 관리](log-management.md)가 포함됩니다. 이 서비스는 스테이징 및 프로덕션 환경에서 모든 로그 데이터를 집계하고 중앙 집중식 로그 관리 대시보드에 표시하도록 사전 구성되어 있습니다.
