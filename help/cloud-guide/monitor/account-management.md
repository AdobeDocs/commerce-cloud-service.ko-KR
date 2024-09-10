---
title: New Relic 계정 관리
description: New Relic 계정에 액세스하고 Adobe Commerce on cloud infrastructure 프로젝트에 대한 액세스, 통합 및 도구 사용을 관리하는 방법을 알아봅니다.
feature: Cloud, Observability
role: Admin
exl-id: ee639e2e-4074-4384-8f68-152bc3bac93b
source-git-commit: 1fc488d7e13952ad70d4c864327899f38ea48af1
workflow-type: tm+mt
source-wordcount: '660'
ht-degree: 0%

---

# New Relic 계정 관리

Adobe이 클라우드 인프라 프로젝트를 프로비저닝할 때 라이선스 소유자는 New Relic 계정에서 자격 증명과 New Relic 계정 액세스에 대한 지침이 포함된 이메일을 수신합니다. 이메일을 받지 못한 경우 라이선스 소유자 이메일 주소를 사용하여 New Relic 암호를 재설정합니다.

## 사용자 액세스 관리

>[!NOTE]
>
>전체 기능 세트에 대한 액세스를 엄격히 요구하는 사용자만 전체 액세스 권한을 부여합니다.

**New Relic에서 사용자 관리에 액세스하려면**:

1. [New Relic 계정](https://login.newrelic.com/login)에 로그인합니다.

1. 왼쪽 아래에서 사용자 이름을 선택합니다.

1. **[!UICONTROL Administration]**&#x200B;을(를) 클릭하고 목록에서 다음 중 하나를 선택합니다.

   - 사용자를 추가하고 활성 사용자 및 보류 중인 초대를 관리하는 **[!UICONTROL User management]**.

   - **[!UICONTROL Access management]**: 사용자 그룹, 역할 및 계정을 관리합니다.

_New Relic_ 설명서에서 [사용자 관리](https://docs.newrelic.com/docs/accounts/accounts-billing/new-relic-one-user-management/user-management-ui-and-tasks/)를 참조하십시오.

## Starter 환경을 위한 New Relic 구성

>[!NOTE]
>
>**Pro 환경**&#x200B;은(는) New Relic 서비스를 사용하도록 사전 구성되어 있으며 사용 및 연결 지침을 건너뛸 수 있습니다. New Relic APM이 스테이징 및 프로덕션 환경에 설치되지 않았거나 New Relic 인프라를 프로덕션 환경에서 사용할 수 없는 경우 [Adobe Commerce 지원 티켓을 제출](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)하여 설치를 요청하십시오.

Starter 환경의 경우 `.magento.app.yaml` 파일을 확인하여 `runtime` 섹션에 New Relic 확장이 포함되어 있는지 확인해야 합니다. 확장이 구성되지 않은 경우 다음을 추가하십시오.

> `.magento.app.yaml`

```yaml
runtime:
    extensions:
        - newrelic
```

### 라이선스 키 적용

클라우드 환경을 New Relic에 연결하려면 환경에 New Relic 라이선스 키를 추가하십시오.

- **Pro 프로젝트**&#x200B;의 경우 Adobe은 프로비저닝 프로세스 중에 프로덕션 및 스테이징 환경에 라이선스 키를 추가합니다. [New Relic 계정](https://login.newrelic.com/login)에 로그인하여 클라우드 인프라 사이트의 Adobe Commerce과 New Relic 간의 연결을 확인할 수 있습니다.

- **시작 프로젝트**&#x200B;의 경우 최대 _3_ 환경을 지원하는 New Relic 라이선스 키가 있습니다. 환경 구성에 키를 수동으로 추가해야 합니다. 스타터 환경은 New Relic 서비스를 사용하도록 사전 프로비저닝되지 않습니다.

스타터 환경의 경우 환경 구성에 New Relic 라이선스 키를 추가하여 New Relic 통합을 활성화합니다. 키를 스테이징 및 프로덕션 환경과 선택한 다른 환경에 추가합니다. 구성에는 New Relic 라이선스 키만 필요합니다. 추가 구성 옵션에 대한 정보는 _New Relic 사용 안내서_&#x200B;의 [Adobe Commerce 보고](https://experienceleague.adobe.com/docs/commerce-admin/config/general/new-relic-reporting.html) 항목에서 찾을 수 있습니다.

{{redeploy-warning}}

>[!PREREQUISITES]
>
>- Adobe Commerce 계정 페이지 또는 프로젝트와 연결된 New Relic 라이선스의 로그인 자격 증명
>- 구성할 스타터 환경에 대한 [관리자 수준 액세스](../project/user-access.md)
>- 환경의 [관리자](https://experienceleague.adobe.com/docs/commerce-admin/systems/user-accounts/permissions.html)에 액세스하기 위한 자격 증명

**스타터 환경을 위한 New Relic을 구성하려면**:

1. [!DNL Cloud Console] 또는 Cloud CLI에서 New Relic 라이선스 키를 찾습니다.

   **[!DNL Cloud Console]메서드**:

   - 클라우드 프로젝트 [계정 페이지](https://accounts.magento.cloud/user)를 엽니다.

   - _프로젝트_ 탭에서 프로젝트를 찾습니다.

   - 프로젝트 인프라 정보를 보려면 **세부 정보 보기**&#x200B;를 클릭하십시오.

   - **New Relic 서비스** 섹션을 확장하여 라이선스 키를 봅니다.

   - 라이선스 키를 복사합니다.

   **Cloud CLI 메서드**:

   ```bash
   magento-cloud subscription:info services.newrelic
   ```

1. `magento-cloud` CLI를 사용하여 환경에 New Relic 라이선스 키를 추가합니다.

   - 라이센스 키가 필요한 환경으로 변경합니다.
   - 다음 `magento-cloud` CLI 명령을 사용하여 변수 값을 업데이트합니다.

     ```bash
     magento-cloud variable:update php:newrelic.license --value <newrelic-license-key>
     ```

   선택적으로 [Commerce 관리자](https://experienceleague.adobe.com/docs/commerce-admin/start/reporting/new-relic-reporting.html#step-3%3A-configure-your-store)에서 추가할 수 있습니다.

1. [New Relic 계정](https://login.newrelic.com/login)에 로그인하여 Adobe Commerce 환경에서 데이터를 볼 수 있는지 확인하십시오. [성능 조사](investigate-performance.md)를 참조하십시오.

### 라이선스 키 제거

New Relic 라이선스 키는 3개의 활성 환경에서만 사용할 수 있습니다. 키가 세 가지 환경에서 사용 중인 경우 다른 환경에 키를 추가하려면 먼저 환경 중 하나에서 키를 제거해야 합니다.

**환경에서 라이선스 키를 제거하려면**:

1. 환경 변수를 나열합니다.

   ```bash
   magento-cloud variable:list
   ```

   샘플 응답:

   ```
    +----------------------+-------------+----------------------+---------+
    | Name                 | Level       | Value                | Enabled |
    +----------------------+-------------+----------------------+---------+
    | php:newrelic.license | environment | newrelic-license-key | true    |
    +----------------------+-------------+----------------------+---------+
   ```

   >[!WARNING]
   >
   >라이선스 키를 _프로젝트_ 변수로 추가한 경우 해당 프로젝트 수준 변수를 제거해야 합니다. 프로젝트 변수는 만들어진 _모든_ 환경 분기에 라이선스를 추가하며 라이선스 한도를 소비하거나 초과할 수 있습니다. 프로젝트 변수를 나열하려면: `magento-cloud variable:list --level project`

1. 라이선스 변수를 삭제합니다.

   ```bash
   magento-cloud variable:delete php:newrelic.license
   ```
