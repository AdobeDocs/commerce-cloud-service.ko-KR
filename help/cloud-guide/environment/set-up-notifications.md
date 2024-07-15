---
title: 알림 설정
description: 클라우드 인프라 환경에서 Adobe Commerce에 대한 알림을 구성하는 방법을 알아봅니다.
feature: Cloud, Configuration, Logs
exl-id: be48abcd-4753-4e89-83fe-0aadfb0415c2
source-git-commit: 1253d8357fd2554050d1775fefbc420a2097db5f
workflow-type: tm+mt
source-wordcount: '405'
ht-degree: 0%

---

# 알림 설정

기본적으로 클라우드 인프라의 Adobe Commerce은 Adobe Commerce 루트 응용 프로그램 디렉터리 내의 `app/var/log/cloud.log` 파일에 빌드 및 배포 작업을 기록합니다. 원할 경우 Slack 및 이메일과 같은 메시징 시스템에 로그를 전송하여 실시간 알림을 받을 수 있습니다.

예를 들어 Slack 메시지를 전송하여 배포가 실패할 때 그룹의 사용자에게 경고하고 무엇이 잘못되었는지 조사할 수 있습니다.

## 플랜 알림

알림을 구성하기 전에 다음 사항을 고려하십시오.

- 어떤 종류의 알림(Slack 메시지, 이메일, 둘 다)을 수신하시겠습니까?
- 로그에서 얼마나 자세한 내용을 보고 싶으십니까?
- 알림(통합, 스테이징, 프로덕션)을 설정할 위치를 선택하십시오.

예를 들어 초기 개발 중에 통합 환경에 대한 자세한 로그를 표시하는 이메일 알림을 선호하여 스테이징 환경으로 배포하기 전에 문제를 디버깅할 수 있습니다. 스테이징 또는 프로덕션 환경에 배포할 준비가 되면 덜 상세한 정보가 포함된 Slack 메시지를 더 선호할 수 있습니다.

>[!NOTE]
>
>알림을 설정하는 데 사용되는 구성 파일은 프로젝트 디렉터리의 루트에 있으므로 변경 사항을 환경에 푸시할 때 적용됩니다. 환경별로 알림을 사용자 지정하려면 구성 파일을 해당 환경으로 푸시하기 전에 수정해야 합니다.

## 알림 구성

알림을 구성하려면:

1. 로컬 워크스테이션에서 프로젝트 디렉터리로 변경합니다.
1. 프로젝트 루트의 `.magento.env.yaml` 파일에서 기본 알림 [로그 수준](log-handlers.md#log-levels)을(를) 포함한 메시징 시스템 설정을 추가합니다.

   예를 들어 Slack _과(와)_ 전자 메일 구성을 모두 구성하려면 다음을 사용하십시오.

   ```yaml
   log:
     slack:
       token: "<your-slack-token>"
       channel: "<your-slack-channel>"
       username: "SlackHandler"
       min_level: "info"
     email:
       to: <your-email>
       from: <your-email>
       subject: "Log notification from Adobe Commerce"
       min_level: "notice"
   ```

   >[!NOTE]
   >
   >클라우드 인프라의 Adobe Commerce은 배포 단계 중에만 이메일을 전송합니다.

1. 변경 사항을 커밋하고 원격 서버에 푸시합니다.

   ```bash
   git -A && git commit -m "Configure build/deploy notifications"
   ```

   ```bash
   git push origin <branch-name>
   ```

### Slack 구성 예

다음 예는 Slack 전용 구성을 보여 줍니다.

```yaml
log:
  slack:
    token: "<your-slack-token>"
    channel: "<your-slack-channel>"
    username: "SlackHandler"
    min_level: "info"
```

- `token`—Slack [사용자 토큰](https://api.slack.com/docs/token-types#user). 사용자 토큰은 클라우드 인프라의 Adobe Commerce에서 메시지를 보내도록 승인합니다.
- `channel` - 클라우드 인프라에서 알림을 보내는 Slack 채널 Adobe Commerce의 이름입니다.
- `username` - Cloud Infrastructure의 사용자 이름 Adobe Commerce은 Slack에서 알림 메시지를 보내는 데 사용합니다.
- `min_level`—알림 메시지의 최소 로그 수준입니다. `info`을(를) 사용하는 것이 좋습니다.

### 이메일 구성 예

다음 예는 이메일 전용 구성을 보여 줍니다.

>[!NOTE]
>
>클라우드 인프라의 Adobe Commerce은 배포 단계 중에만 이메일을 전송합니다.

```yaml
log:
  email:
    to: <your-email>
    from: <your-email>
    subject: "Log notification from Adobe Commerce"
    min_level: "notice"
```

- `to` - 클라우드 인프라의 이메일 주소 Adobe Commerce에서 알림 메시지를 보냅니다.
- `from` - 받는 사람에게 알림 메시지를 보낼 전자 메일 주소입니다.
- `subject`—전자 메일에 대한 설명입니다.
- `min_level`—알림 메시지의 최소 로그 수준입니다. `notice` 또는 `warning`을(를) 사용하는 것이 좋습니다.
