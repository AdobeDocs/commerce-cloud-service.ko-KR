---
title: 발신 이메일 구성
description: 클라우드 인프라에서 Adobe Commerce에 대해 발신 이메일을 활성화하는 방법을 알아봅니다.
exl-id: 814fe2a9-15bf-4bcb-a8de-ae288fd7f284
source-git-commit: 59f82d891bb7b1953c1e19b4c1d0a272defb89c1
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 0%

---

# 발신 이메일 구성

에서 각 환경에 대해 발신 이메일을 활성화 및 비활성화할 수 있습니다. [!DNL Cloud Console] 또는 를 명령줄에서 선택할 수 있습니다. 통합 및 스테이징 환경에 대해 발신 이메일을 활성화하여 Cloud 프로젝트 사용자에 대해 이중 인증 또는 암호 재설정 이메일을 전송합니다.

기본적으로 발신 이메일은 프로덕션 및 스테이징 환경에서 활성화됩니다. 그러나 [!UICONTROL Enable outgoing emails] 을(를) 설정할 때까지 환경 설정에서 을(를) 비활성화할 수 있습니다. `enable_smtp` 속성을 통해 [명령줄](#enable-emails-in-the-cli) 또는 [클라우드 콘솔](outgoing-emails.md#enable-emails-in-the-cloud-console).

업데이트 중 [!UICONTROL enable_smtp] 속성 값 기준 [명령줄](#enable-emails-in-the-cli) 또한 [!UICONTROL Enable outgoing emails] Cloud Console에서 이 환경에 대한 값 설정.

{{redeploy-warning}}

## Cloud Console에서 이메일 활성화

사용 **[!UICONTROL Outgoing emails]** 에서 전환 _환경 구성_ 이메일 지원을 활성화하거나 비활성화하려면 을(를) 봅니다.

Pro 프로덕션 또는 스테이징 환경에서 발신 이메일을 비활성화하거나 다시 활성화해야 하는 경우 [Adobe Commerce 지원 티켓](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide).

>[!TIP]
>
>발신 이메일 상태가 Cloud Console의 Pro 환경에 반영되지 않을 수 있습니다. 대신 [명령줄](#enable-emails-in-the-cli) 발신 이메일을 활성화하고 테스트하기 위해.

**에서 이메일 지원을 관리하려면[!DNL Cloud Console]**:

1. 에 로그인합니다 [[!DNL Cloud Console]](https://console.adobecommerce.com).
1. 다음에서 프로젝트 선택 _모든 프로젝트_ 목록을 표시합니다.
1. 프로젝트 대시보드에서 오른쪽 상단의 구성 아이콘을 클릭합니다.
1. 클릭 **[!UICONTROL Environments]** 목록에서 특정 환경을 선택합니다.
1. 발신 이메일을 활성화하거나 비활성화하려면 _발신 이메일 활성화_ **날짜** 또는 **끔**.

   ![발신 이메일 구성 활성화](../../assets/outgoing-emails.png)

설정을 변경하면 환경이 빌드되고 새 구성으로 배포됩니다.

## CLI에서 이메일 활성화

를 사용하여 활성 환경에 대한 이메일 구성을 변경할 수 있습니다. `magento-cloud` CLI `environment:info` 를 설정하는 명령 `enable_smtp` 속성. SMTP 업데이트 사용 `MAGENTO_CLOUD_SMTP_HOST` 메일을 보낼 SMTP 호스트의 IP 주소가 있는 환경 변수입니다.

**명령줄에서 이메일 지원을 관리하려면**:

1. 로컬 워크스테이션에서 프로젝트 디렉터리로 변경합니다.

1. 환경에 대한 발신 이메일 설정을 확인합니다.

   ```bash
   magento-cloud environment:info -e <environment-id> | grep enable_smtp
   ```

1. 다음을 설정하여 이메일 지원 구성 변경 `enable_smtp` 환경 변수 대상 `true` 또는 `false`.

   ```bash
   magento-cloud environment:info --refresh -e <environment-id> enable_smtp true
   ```

   환경이 빌드되고 배포될 때까지 기다립니다.

1. SSH를 사용하여 원격 환경에 로그인합니다.

1. 이메일이 작동하는지 확인합니다. 확인할 수 있는 주소로 테스트 이메일을 보냅니다.

   ```bash
   php -r 'mail("mail@example.com", "test message", "just testing", "From: tester@example.com");'
   ```

1. SendGrid에서 이메일을 선택했는지 확인합니다.

   ```bash
   grep mail@example.com /var/log/mail.log
   ```
