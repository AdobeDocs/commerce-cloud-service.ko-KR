---
title: 상태 알림
description: Adobe Commerce on cloud infrastructure 프로젝트에서 디스크 공간 사용을 위한 Slack, 이메일 및 PagerDuty 알림을 구성하는 방법에 대해 알아봅니다.
feature: Cloud, Observability, Integration
exl-id: d3173098-78ed-42a8-aeb3-9ccbaccc4d32
source-git-commit: 1253d8357fd2554050d1775fefbc420a2097db5f
workflow-type: tm+mt
source-wordcount: '312'
ht-degree: 0%

---

# 상태 알림

클라우드 인프라의 Adobe Commerce은 Starter 환경 또는 Pro 통합 환경의 모든 애플리케이션 및 서비스에서 디스크 공간 사용을 모니터링합니다. 데이터베이스 디스크에 공간이 부족하면 데이터가 손상될 수 있습니다. 상태 검사는 5분마다 발생하며 이메일 또는 기타 외부 서비스로 알릴 수 있습니다. 상태 알림에 대한 세 가지 디스크 부족 경고가 있습니다.

- **경고**—사용 가능한 디스크 공간이 20% 미만으로 떨어짐
- **중요**—사용 가능한 디스크 공간이 10% 미만으로 떨어짐
- **All-clear**—디스크 부족 이벤트 후 사용 가능한 디스크 공간이 20% 이상 반환됨

>[!NOTE]
>
>Pro Production 환경에서는 New Relic용 Adobe Commerce 경고 관리 정책을 사용하여 디스크 공간을 모니터링할 수 있습니다. [관리 경고로 성능 모니터링](../monitor/investigate-performance.md#monitor-performance-with-managed-alerts)을 참조하십시오.

## 이메일 알림

상태 전자 메일 통합에는 원본 주소와 받는 사람 주소가 하나 이상 필요합니다. `from-address` 및 `recipients` 주소에 동일한 전자 메일 주소를 사용할 수 있습니다. 다음 예제에서는 두 명의 수신자에게 상태 이메일 통합을 등록합니다.

```bash
magento-cloud integration:add --type health.email --from-address you@example.com --recipients them@example.com --recipients others@example.com
```

## Slack 채널 알림

Slack은 봇이라는 인터랙티브한 앱을 이용해 대화방에 메시지를 게시하는 외부 서비스다. Slack에서 상태 알림을 받으려면 Slack 그룹에 대한 사용자 지정 [봇 사용자](https://api.slack.com/bot-users)를 만들어야 합니다. 채널 또는 채널에 대한 봇 사용자를 구성한 후 Slack이 제공한 [봇 토큰](https://api.slack.com/docs/token-types#bot)을 저장하여 통합을 등록합니다. 다음 예제에서는 상태 알림을 Slack 채널에 등록합니다.

```bash
magento-cloud integration:add --type health.slack --token SLACK_BOT_TOKEN --channel '#slack-channel-name'
```

## PagerDuity 알림

PagerDuty는 호출중인 팀원에게 중요한 문제를 알릴 수 있는 외부 서비스입니다. PagerDuty에서 상태 알림을 받으려면 먼저 이벤트 API 버전 2를 사용하는 [PagerDuty 통합](https://developer.pagerduty.com/v2/docs/integrating)을 만들어야 합니다. 통합 키 또는 _라우팅 키_&#x200B;를 사용하여 통합을 등록하십시오. 다음 예제에서는 라우팅 키를 사용하여 PagerDuty에 대한 알림을 등록합니다.

```bash
magento-cloud integration:add --type health.pagerduty --routing-key PAGERDUTY_ROUTING_KEY
```
