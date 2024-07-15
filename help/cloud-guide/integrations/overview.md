---
title: 통합 개요
description: Adobe Commerce on cloud infrastructure 프로젝트의 서드파티 통합 옵션에 대해 알아봅니다.
role: Developer
feature: Cloud, Integration
last-substantial-update: 2024-02-06T00:00:00Z
exl-id: 2dddba73-5b88-4b5d-a0e1-2f1c1f52354c
source-git-commit: abe9aa36b907be8bdfdf42e6f28f1e1eac68fecf
workflow-type: tm+mt
source-wordcount: '564'
ht-degree: 0%

---

# 통합 개요

통합은 Git 호스팅 또는 Slack 봇과 같은 외부 서비스를 사용하고 GitHub의 코드 검토 가져오기 요청 기능을 사용하는 등 현재 개발 프로세스를 유지 관리하는 데 유용합니다. Adobe Commerce on cloud infrastructure 프로젝트에 다음 통합을 추가할 수 있습니다.

![통합](/help/assets/integrations.png)

>[!BEGINTABS]

>[!TAB CLI]

**Cloud CLI를 사용하여 통합을 추가하려면**:

다음 명령은 새 통합에 대한 유형 및 옵션을 선택하는 대화형 프롬프트를 시작합니다.

```bash
magento-cloud integration:add
```

**프로젝트에 대해 구성된 통합을 나열하려면**:

```bash
magento-cloud integration:list
```

샘플 응답:

```terminal
+----------+--------------+---------------------------------------------------------------------------+
| ID       | Type         | Summary                                                                   |
+----------+--------------+---------------------------------------------------------------------------+
| <int-id> | bitbucket    | Repository: user/magento-int                                              |
|          |              | Hook URL:                                                                 |
|          |              | https://magento-url.cloud/api/projects/projectID/integrations/int-ID/hook |
| <int-id> | health.email | From: you@example.com                                                     |
|          |              | To: them@example.com                                                      |
+----------+--------------+---------------------------------------------------------------------------+
```

>[!TAB 콘솔]

**[!DNL Cloud Console]**&#x200B;을(를) 사용하여 통합을 추가하려면:

1. _프로젝트 설정_&#x200B;에서 **[!UICONTROL Integrations]**&#x200B;을(를) 클릭합니다.

1. 통합 유형을 클릭하거나 **[!UICONTROL Add integration]**&#x200B;을(를) 클릭합니다.

1. 통합 유형 선택 및 구성 단계를 단계별로 안내합니다.

1. 통합을 추가하면 통합 보기의 목록에 표시됩니다.

>[!ENDTABS]

## Commerce webhooks

[ENABLE_WEBHOOKS 전역 변수](../environment/variables-global.md#enable_webhooks)을(를) 사용하여 클라우드 프로젝트에서 Commerce 웹후크를 구성할 수 있습니다. Commerce 웹후크는 Commerce에서 생성한 이벤트에 대한 응답으로 외부 서버에 요청을 보냅니다. [_Webhooks 안내서_](https://developer.adobe.com/commerce/extensibility/webhooks)에서는 이 기능에 대해 자세히 설명합니다.

## 일반 웹후크

_webhook_ URL에 대한 `POST` JSON 메시지에 사용자 지정 Webhook 통합을 사용하여 Cloud 인프라 및 저장소 이벤트를 캡처하고 보고할 수 있습니다.

**웹후크 URL을 추가하려면 다음 구문을 사용합니다**.

```bash
magento-cloud integration:add --type=webhook --url=https://hook-url.example.com
```

- `type` - `webhook` 통합 유형을 지정합니다.
- `url`—JSON 메시지를 받을 수 있는 웹후크 URL을 제공합니다.

샘플 응답에는 통합을 사용자 지정할 수 있는 기회를 제공하는 일련의 프롬프트가 표시됩니다. 기본(빈) 응답을 사용하면 프로젝트의 모든 환경에 있는 모든 이벤트에 대한 메시지를 보냅니다.

코드를 분기로 푸시하는 것과 같이, 특정 [이벤트](#events-to-report)를 보고하도록 통합을 사용자 지정할 수 있습니다. 예를 들어 사용자가 코드를 분기로 푸시할 때 메시지를 보내는 `environment.push` 이벤트를 지정할 수 있습니다.

```terminal
Events to report (--events)
A list of events to report, e.g. environment.push
Default: *
Enter comma-separated values (or leave this blank)
>
```

`pending`, `in_progress` 또는 `complete` 상태의 이벤트를 보고하도록 선택할 수 있습니다.

```terminal
States to report (--states)
A list of states to report, e.g. pending, in_progress, complete
Default: complete
Enter comma-separated values (or leave this blank)
>
```

특정 환경에 대해 _포함_ 또는 _제외_ 메시지를 사용할 수 있습니다.

```terminal
Included environments (--environments)
The environment IDs to include
Default: *
Enter comma-separated values (or leave this blank)
>

Excluded environments (--excluded-environments)
The environment IDs to exclude
Enter comma-separated values (or leave this blank)
>
```

통합이 완료되면 다음과 같은 값의 요약을 받게 됩니다.

```terminal
Created integration integration-ID (type: webhook)
+-----------------------+------------------------------+
| Property              | Value                        |
+-----------------------+------------------------------+
| id                    | integration-ID               |
| type                  | webhook                      |
| events                | - '*'                        |
| environments          | - '*'                        |
| excluded_environments | {  }                         |
| states                | - complete                   |
| url                   | https://hook-url.example.com |
+-----------------------+------------------------------+
```

### 기존 통합 업데이트

기존 통합을 업데이트할 수 있습니다. 예를 들어 다음을 사용하여 상태를 `complete`에서 `pending`(으)로 변경합니다.

```bash
magento-cloud integration:update --states=pending <int-id>
```

샘플 응답:

```terminal
Integration integration-ID (webhook) updated
+-----------------------+------------------------------+
| Property              | Value                        |
+-----------------------+------------------------------+
| id                    | integration-ID               |
| type                  | webhook                      |
| events                | - '*'                        |
| environments          | - '*'                        |
| excluded_environments | {  }                         |
| states                | - pending                    |
| url                   | https://hook-url.example.com |
+-----------------------+------------------------------+
```

### 보고할 이벤트

| 이벤트 | 설명 |
| ----- | :-----------|
| `environment.access.add` | 사용자에게 환경에 대한 액세스 권한이 부여되었습니다. |
| `environment.access.remove` | 사용자가 환경에서 제거되었습니다. |
| `environment.activate` | 환경이 &quot;활성화&quot;되었습니다. |
| `environment.backup` | 사용자가 스냅샷을 트리거했습니다. |
| `environment.branch` | Management Console을 사용하여 분기가 생성되었습니다 |
| `environment.deactivate` | 분기가 &quot;비활성화되었습니다.&quot; 코드는 그대로 있지만 환경은 파괴되었습니다 |
| `environment.delete` | 분기가 삭제되었습니다. |
| `environment.initialize` | 첫 번째 커밋으로 초기화된 프로젝트의 `master` 분기 |
| `environment.merge` | 활성 분기가 관리 콘솔 또는 API를 사용하여 병합되었습니다. |
| `environment.push` | 사용자가 코드를 분기로 푸시함 |
| `environment.restore` | 사용자가 스냅샷을 복원했습니다. |
| `environment.route.create` | 관리 콘솔을 사용하여 경로를 만들었습니다. |
| `environment.route.delete` | 관리 콘솔을 사용하여 경로가 삭제되었습니다. |
| `environment.route.update` | 관리 콘솔을 사용하여 경로가 수정되었습니다. |
| `environment.subscription.update` | 구독이 변경되어 `master` 환경의 크기가 조정되었지만 콘텐츠 변경 내용은 없습니다 |
| `environment.synchronize` | 환경에 상위 환경에서 데이터 또는 코드가 다시 복사되었습니다. |
| `environment.update.http_access` | 환경에 대한 HTTP 액세스 규칙이 수정되었습니다. |
| `environment.update.restrict_robots` | 모든 로봇 차단 기능이 활성화되었거나 비활성화되었습니다. |
| `environment.update.smtp` | 환경에 대해 이메일 전송이 활성화되거나 비활성화되었습니다. |
| `environment.variable.create` | 변수가 생성되었습니다. |
| `environment.variable.delete` | 변수가 삭제되었습니다. |
| `environment.variable.update` | 변수가 수정되었습니다. |
| `project.domain.create` | 도메인이 만들어지고 프로젝트에 추가되었습니다 |
| `project.domain.delete` | 프로젝트와 연결된 도메인이 제거되었습니다. |
| `project.domain.update` | 프로젝트와 연계된 도메인이 업데이트되었습니다. |
