---
title: 사용자 액세스 관리
description: 클라우드 인프라 프로젝트 및 환경에서 Adobe Commerce에 대한 사용자 액세스를 관리하는 방법을 알아봅니다.
role: Admin
feature: Cloud, Roles/Permissions
last-substantial-update: 2023-06-27T00:00:00Z
topic: Security
exl-id: 3357a3ea-bf86-4a65-95d1-6b24f1152248
source-git-commit: b49a51aba56f79b5253eeacb1adf473f42bb8959
workflow-type: tm+mt
source-wordcount: '1454'
ht-degree: 0%

---

# 사용자 액세스 관리

클라우드 인프라의 Adobe Commerce 프로젝트는 역할 기반 액세스를 사용합니다. 프로젝트 수준에서 사용할 수 있는 두 가지 역할이 있습니다.

- **프로젝트 관리자** - 모든 프로젝트 환경에 대한 쓰기 액세스 권한으로 사용자, 푸시 코드 및 프로젝트 설정을 관리할 수 있습니다.
- **프로젝트 뷰어**—모든 프로젝트 환경에 대한 보기 전용 액세스 권한.

프로젝트 뷰어는 환경에서 작업을 수행할 수 없지만 프로젝트 뷰어에게 특정 환경 유형에 대한 쓰기 액세스 권한을 부여할 수 있습니다.

환경 수준 액세스는 환경 유형(프로덕션, 스테이징 및 개발)을 기반으로 합니다. _개발_ 환경에 _뷰어_ 권한을 부여하면 프로젝트에서 **모두** 개발 환경을 볼 수 있습니다. 다음 표에서는 각 권한 수준에 부여된 기능을 설명합니다.

| 권한 수준 | 액세스 | SSH 액세스 |
| ------------------ | ----------- | :----------: |
| **관리자** | 상위 환경과의 병합을 포함하여 설정 변경, 푸시 코드, 작업 수행 및 분기 관리와 같은 관리자 작업 수행 | 예 |
| **참가자** | 푸시 코드 및 환경 분기. 설정을 변경하거나 작업을 실행할 수 없음 | 예 |
| **뷰어** | 환경 유형에 대한 보기 전용 액세스 | 아니요 |
| **액세스 권한 없음** | 환경 유형에 대한 액세스 권한 없음 | 아니요 |

{style="table-layout:auto"}

`magento-cloud` CLI 또는 [!DNL Cloud Console]을(를) 사용하여 사용자를 추가하고 역할을 할당할 수 있습니다.

>[!BEGINSHADEBOX]

**필수 구성 요소:**

- Adobe ID에 등록된 사용자. 클라우드 프로젝트에 추가하려면 먼저 사용자가 [Adobe 계정에 등록](https://account.adobe.com)한 다음 [클라우드 계정을 초기화](https://console.adobecommerce.com)해야 합니다.
- **관리자** 역할을 할당받은 사용자가 `magento-cloud` CLI로 사용자를 관리할 수 없습니다. **계정 소유자** 역할이 부여된 사용자만 사용자를 관리할 수 있습니다.

>[!ENDSHADEBOX]

## CLI를 사용하여 사용자 관리

`magento-cloud` CLI를 사용하여 사용자를 관리하고 자동화된 시스템과 통합합니다.

- `magento-cloud user:add` 프로젝트에 사용자 추가
- `magento-cloud user:delete`-사용자 삭제
- `magento-cloud user:list [users]` 목록 프로젝트 사용자
- `magento-cloud user:role` 보기 또는 사용자 역할 변경
- 프로젝트에서 `magento-cloud user:update`-사용자 역할 업데이트

다음 예제에서는 `magento-cloud` CLI를 사용하여 사용자를 추가하고, 역할을 구성하고, 프로젝트 할당을 수정하고, 사용자 역할을 할당합니다.

**사용자를 추가하고 역할을 할당하려면**:

1. `magento-cloud` CLI를 사용하여 사용자를 추가하십시오.

   ```bash
   magento-cloud user:add
   ```

   >[!IMPORTANT]
   >
   >사용자에게 Adobe ID이 있어야 합니다. [필수 구성 요소](#add-users-and-manage-access)를 참조하세요.

1. 프롬프트에 따라 사용자 이메일 주소를 지정하고 프로젝트 및 환경 유형 역할을 설정한 다음 사용자를 추가합니다.

   > 샘플 프롬프트

   ```
   Enter the user's email address: alice@example.com
   
   Email address: alice@example.com
   
   The user's project role can be admin (a) or viewer (v).
   
   Project role (default: viewer) [a/v]: viewer
   
   The user's environment type role(s) can be admin (a), viewer (v), contributor (c) or none (n).
   
   Role on type development (default: none) [a/v/c/n]: none
   Role on type production (default: none) [a/v/c/n]: admin
   Role on type staging (default: none) [a/v/c/n]: admin
   
   Adding the user alice@example.com to (project_id):
   Project role: viewer
     Role on type production: admin
     Role on type staging: admin
   
   Are you sure you want to add this user? [Y/n] y
   Adding the user to the project
   ```

   사용자를 추가하면 Adobe은 클라우드 인프라 프로젝트에서 Adobe Commerce에 액세스하기 위한 지침과 함께 지정된 주소로 이메일을 전송합니다.

### 사용자의 프로젝트 역할 보기

```bash
magento-cloud user:get alice@example.com
```

>샘플 응답:

```
Current role(s) of User (alice@example.com) on Production (project_id):
  Project role: admin
```

### 여러 환경에 사용자 추가

사용자를 `Production` 환경에서는 `viewer`(으)로, `Integration` 환경에서는 `contributor`(으)로 추가하려면 다음 작업을 수행하십시오.

```bash
magento-cloud user:add alice@example.com -r production:v -r integration:c
```

### 사용자 환경 권한 업데이트

`Production` 환경에서 사용자 환경 권한을 `admin`(으)로 업데이트하려면:

```bash
magento-cloud user:update alice@example.com -r production:a
```

## [!DNL Cloud Console]에서 사용자 관리

[[!DNL Cloud Console]](../../get-started/cloud-console.md)을(를) 사용하여 권한을 추가하고 _편집_ 기능을 사용하여 기존 사용자의 권한을 수정할 수 있습니다.

>[!IMPORTANT]
>
>사용자에게 Adobe ID이 있어야 합니다. [필수 구성 요소](#add-users-and-manage-access)를 참조하세요.

### 프로젝트에 사용자 추가

1. [[!DNL Cloud Console]](https://console.adobecommerce.com/)에 로그인합니다.

1. _모든 프로젝트_ 목록에서 프로젝트를 선택하십시오.

1. 프로젝트 대시보드에서 오른쪽 상단의 구성 아이콘을 클릭합니다.

1. _프로젝트 설정_&#x200B;에서 **[!UICONTROL Access]**&#x200B;을(를) 클릭합니다.

1. _액세스_ 보기에서 **[!UICONTROL Add]**&#x200B;을(를) 클릭합니다.

1. _[!UICONTROL Add User]_양식을 작성합니다.

   - 사용자 이메일 주소를 입력합니다.

   - **[!UICONTROL Project admin]**—모든 설정 및 환경 유형에 관리자 권한을 부여합니다.

   - **[!UICONTROL Environment types and permissions]**—특정 환경 유형에 액세스 권한 및 특정 권한 수준을 부여합니다. _액세스 권한 없음_, _관리자_(설정 변경, 작업 실행, 코드 병합), _기여자_(푸시 코드) 또는 _뷰어_(보기 전용).

   >[!TIP]
   >
   >**프로젝트 관리자**&#x200B;만 모든 환경에서 사용자를 관리할 수 있습니다. 사용자에게 **액세스** 탭에 대한 액세스 권한을 부여하려면 다른 **프로젝트 관리자** 또는 **계정 소유자**&#x200B;가 해당 사용자에게 **프로젝트 관리자** 역할을 할당해야 합니다.

1. **[!UICONTROL Add User]**&#x200B;을(를) 클릭합니다.

   >[!IMPORTANT]
   >
   >사용자를 추가해도 배포가 자동으로 트리거되지 않습니다.

1. 사용자를 추가한 후 모든 환경을 재배포하여 변경 사항을 적용합니다. 사용자를 추가해도 배포가 자동으로 트리거되지 않습니다. 재배포는 사용자가 SSH를 사용하여 환경에 액세스하거나 관리자 작업을 수행할 수 있도록 하는 중요한 단계입니다.

사용자를 추가하면 Adobe은 클라우드 인프라 프로젝트에서 Adobe Commerce에 액세스하기 위한 지침과 함께 지정된 주소로 이메일을 전송합니다.

## 사용자 인증 요구 사항

보안 강화를 위해 Adobe은 클라우드 인프라 프로젝트 소스 코드 및 환경에서 Adobe Commerce에 대한 SSH 액세스에 TFA(2단계 인증)가 필요하도록 프로젝트 수준의 MFA(Multi-Factor Authentication) 적용을 제공합니다. [SSH용 MFA 사용](multi-factor-authentication.md)을 참조하세요.

Adobe Commerce on cloud infrastructure 프로젝트에서 MFA 시행이 활성화되면 해당 프로젝트의 환경에 대한 SSH 액세스 권한이 있는 모든 사용자는 Adobe Commerce on cloud infrastructure 계정에서 TFA를 활성화해야 합니다. 자동화된 프로세스의 경우 명령줄에서 인증할 컴퓨터 사용자 및 API 토큰을 만들 수 있습니다.

클라우드 프로젝트에 사용자를 추가한 후 사용자에게 계정 보안 설정을 검토하고 필요에 따라 다음 보안 구성을 추가하도록 요청합니다.

- **TFA 사용**—2단계 인증을 구성하여 보안 및 규정 준수 표준을 충족합니다. [MFA 적용](multi-factor-authentication.md)으로 구성된 프로젝트에서는 프로젝트에 액세스하기 위해 SSH를 사용하는 계정에 TFA가 필요합니다.

- **SSH 키 사용** - 클라우드 인프라 소스 코드 리포지토리에서 Adobe Commerce에 액세스해야 하는 사용자는 계정에서 SSH 키를 사용해야 합니다. [보안 연결](../development/secure-connections.md)을 참조하세요.

- **API 토큰 만들기** - 사용자는 환경에 대한 SSH 액세스에 사용되는 API 토큰을 생성해야 합니다. 자동화된 프로세스에 대한 인증 워크플로를 활성화하려면 토큰이 필요합니다.

  MFA 시행이 활성화된 프로젝트에서 API 토큰을 사용하여 자동화된 계정의 SSH 액세스 요청을 인증해야 합니다. 이 토큰을 사용하면 자동화된 프로세스에서 TFA가 필요한 인증 워크플로를 우회할 수 있습니다.

### 클라우드 계정용 TFA 활성화

Adobe Commerce on cloud infrastructure는 다음 애플리케이션 중 하나를 사용하여 TFA를 지원합니다.

- [Google Authenticator(Android/iPhone)](https://support.google.com/accounts/answer/1066447?hl=en)
- [인증(Android/iPhone)](https://authy.com/features/)
- [FreeOTP(Android)](https://play.google.com/store/apps/details?id=org.fedorahosted.freeotp)
- [GAuth 인증자(Firefox OS, 데스크톱 등)](https://github.com/gbraad-apps/gauth)

인증자 응용 프로그램을 설치하고 TFA를 사용하도록 설정하는 방법은 [!DNL Cloud Console]의 _계정 설정_ 페이지에서 확인할 수 있습니다.

**사용자 계정에서 TFA를 활성화하려면**:

1. [계정](https://console.adobecommerce.com)에 로그인합니다.

1. 오른쪽 위 계정 메뉴에서 **[!UICONTROL My Profile]**&#x200B;을(를) 클릭합니다.

1. _보안_ 탭에서 **[!UICONTROL Set up application]**&#x200B;을(를) 클릭합니다.

1. 모바일 장치에 승인된 인증자 응용 프로그램이 없는 경우 연결된 지침을 사용하여 응용 프로그램을 설치합니다.

1. 인증자 애플리케이션에 클라우드 인프라의 Adobe Commerce 계정을 추가합니다.

   - 모바일 장치에서 인증자 애플리케이션을 엽니다. 그런 다음 응용 프로그램에 설정 코드를 추가합니다.

   - [!UICONTROL **[!UICONTROL TFA set up - Application]**] 페이지의 **[!UICONTROL Application verification code]** 필드에 모바일 장치의 TFA 코드를 입력합니다.

   - **[!UICONTROL Verify and save]**&#x200B;을(를) 클릭합니다.

     코드가 유효하면 Adobe이 계정 이메일 주소로 알림이 전송되어 계정에 TFA가 있음을 확인할 수 있습니다.

1. 선택 사항입니다. _신뢰할 수 있는 브라우저_ 설정을 사용하여 30일 동안 브라우저에서 인증 코드를 캐시합니다.

   이 구성은 프로젝트 로그인 중 인증 문제 수를 줄입니다.

1. **저장** 또는 **건너뛰기**&#x200B;를 클릭합니다.

1. 복구 코드를 저장합니다.

   - _TFA 설정 - 복구_ 코드 페이지에서 모바일 장치나 인증 애플리케이션에 액세스할 수 없는 경우 클라우드 인프라 프로젝트에서 Adobe Commerce에 로그인할 수 있도록 복구 코드를 복사하여 저장합니다.

   - 복구 코드를 다른 위치에 복사하거나 장치 또는 인증 응용 프로그램에 대한 액세스 권한을 잃어 버릴 경우에 대비해 기록하십시오.

   - 계정 보안 설정에서 코드를 보고 관리할 수 있도록 **저장**&#x200B;을 클릭하여 계정에 코드를 저장합니다.

     >[!WARNING]
     >
     >TFA가 있는 계정에 액세스할 수 없고 복구 코드 목록이 없는 경우 프로젝트 관리자에게 문의하거나 [Adobe Commerce 지원 티켓을 제출](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)하여 TFA 애플리케이션을 재설정해야 합니다.

1. TFA 설정을 완료한 후 **저장**&#x200B;을 클릭하여 계정을 업데이트합니다.

1. TFA를 사용하여 현재 세션을 인증합니다.

   - 계정에서 로그아웃합니다.
   - 사용자 이름과 암호로 로그인합니다.
   - 메시지가 표시되면 모바일 장치의 인증자 응용 프로그램에서 `accounts.magento.cloud` 항목에 대한 TFA 코드를 입력합니다.

### TFA 구성 및 복구 코드 관리

_내 프로필_ 페이지의 _보안_ 섹션에서 클라우드 인프라 계정의 Adobe Commerce에 대한 TFA 구성을 관리할 수 있습니다.

1. [계정](https://console.adobecommerce.com)에 로그인합니다.

1. 오른쪽 위 계정 메뉴에서 **[!UICONTROL My Profile]**&#x200B;을(를) 클릭합니다.

1. _내 프로필_ 페이지에서 **[!UICONTROL Security]** 탭을 클릭합니다.

1. 사용 가능한 링크를 사용하여 클라우드 인프라 계정의 Adobe Commerce에 대한 TFA 설정을 업데이트합니다.

   - TFA 비활성화
   - 인증자 애플리케이션 재설정
   - 신뢰할 수 있는 브라우저 추가 또는 제거
   - 계정에 있는 TFA 복구 코드 보기 또는 새로 고침

### API 토큰 만들기

API 토큰은 OAuth 2 액세스 토큰으로 교환된 다음 요청을 인증하는 데 사용할 수 있습니다.

MFA 적용이 활성화된 프로젝트의 경우 컴퓨터 사용자 및 자동화된 프로세스에 대한 SSH 액세스를 활성화하기 위해 API 토큰이 있어야 합니다.

>[!IMPORTANT]
>
>계정에 대한 Protect API 토큰 값입니다. 코드 샘플, 화면 캡처 또는 비보안 클라이언트-서버 통신에 값을 표시하지 마십시오. 또한 공개 저장소에 저장된 소스 코드의 값을 노출하지 마십시오.

**API 토큰을 만들려면**:

1. [계정](https://console.adobecommerce.com)에 로그인합니다.

1. 오른쪽 위 계정 메뉴에서 **[!UICONTROL My Profile]**&#x200B;을(를) 클릭합니다.

1. _내 프로필_ 페이지에서 **[!UICONTROL API tokens]** 탭을 클릭합니다.

1. **[!UICONTROL Create API token]**&#x200B;을(를) 클릭하고 이름을 입력합니다. 예를 들어, 컴퓨터 사용자 또는 API 토큰을 사용하는 자동화된 프로세스와 일치하는 이름을 지정합니다.

   ![API 토큰](../../assets/api-token-name.png)

1. **[!UICONTROL Create API token]**&#x200B;을(를) 클릭합니다.
