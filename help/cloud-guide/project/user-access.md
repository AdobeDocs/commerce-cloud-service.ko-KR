---
title: 사용자 액세스 관리
description: 클라우드 인프라 프로젝트 및 환경에서 Adobe Commerce에 대한 사용자 액세스를 관리하는 방법을 알아봅니다.
role: Admin
feature: Cloud, Roles/Permissions
last-substantial-update: 2023-06-27T00:00:00Z
topic: Security
exl-id: 3357a3ea-bf86-4a65-95d1-6b24f1152248
source-git-commit: 13e76d3e9829155995acbb72d947be3041579298
workflow-type: tm+mt
source-wordcount: '1441'
ht-degree: 0%

---

# 사용자 액세스 관리

클라우드 인프라의 Adobe Commerce 프로젝트는 역할 기반 액세스를 사용합니다. 프로젝트 수준에서 사용할 수 있는 두 가지 역할이 있습니다.

- **프로젝트 관리자**—모든 프로젝트 환경에 대한 쓰기 액세스 권한으로 사용자, 푸시 코드 및 프로젝트 설정을 관리할 수 있습니다.
- **프로젝트 뷰어**- 모든 프로젝트 환경에 대한 보기 전용 액세스

프로젝트 뷰어는 환경에서 작업을 수행할 수 없지만 프로젝트 뷰어에게 특정 환경 유형에 대한 쓰기 액세스 권한을 부여할 수 있습니다.

환경 수준 액세스는 환경 유형(프로덕션, 스테이징 및 개발)을 기반으로 합니다. 사용자 부여 _조회자_ 권한 대상 _개발_ 환경은 사용자가 볼 수 있음을 의미합니다. **모두** 프로젝트의 개발 환경. 다음 표에서는 각 권한 수준에 부여된 기능을 설명합니다.

| 권한 수준 | 액세스 | SSH 액세스 |
| ------------------ | ----------- | :----------: |
| **관리자** | 상위 환경과의 병합을 포함하여 설정 변경, 푸시 코드, 작업 수행 및 분기 관리와 같은 관리자 작업 수행 | 예 |
| **참여자** | 푸시 코드 및 환경 분기. 설정을 변경하거나 작업을 실행할 수 없음 | 예 |
| **뷰어** | 환경 유형에 대한 보기 전용 액세스 | 아니요 |
| **액세스 권한 없음** | 환경 유형에 대한 액세스 권한 없음 | 아니요 |

{style="table-layout:auto"}

다음을 사용하여 사용자를 추가하고 역할을 할당할 수 있습니다. `magento-cloud` CLI 또는 [!DNL Cloud Console].

>[!BEGINSHADEBOX]

**사전 요구 사항:**

- Adobe ID에 등록된 사용자. 사용자는 [Adobe 계정에 등록](https://account.adobe.com) 그런 다음 [클라우드 계정 초기화](https://console.adobecommerce.com) 먼저 이를 클라우드 프로젝트에 추가할 수 있습니다.
- 사용자에게 할당됨 **관리자** 역할에서 다음을 가진 사용자를 관리할 수 없음: `magento-cloud` CLI. 권한이 부여된 사용자만 **계정 소유자** 역할은 사용자를 관리할 수 있습니다.

>[!ENDSHADEBOX]

## CLI를 사용하여 사용자 관리

사용 `magento-cloud` 사용자를 관리하고 자동화된 시스템과 통합하는 CLI:

- `magento-cloud user:add`-프로젝트에 사용자 추가
- `magento-cloud user:delete`-사용자 삭제
- `magento-cloud user:list [users]`-목록 프로젝트 사용자
- `magento-cloud user:role`-사용자 역할 보기 또는 변경
- `magento-cloud user:update`-프로젝트에서 사용자 역할 업데이트

다음 예제에서는 `magento-cloud` CLI를 사용하여 사용자를 추가하고, 역할을 구성하고, 프로젝트 할당을 수정하고, 사용자 역할을 할당합니다.

**사용자를 추가하고 역할을 할당하려면**:

1. 사용 `magento-cloud` 사용자를 추가하는 CLI입니다.

   ```bash
   magento-cloud user:add
   ```

   >[!IMPORTANT]
   >
   >사용자에게 Adobe ID이 있어야 합니다. 다음을 참조하십시오. [전제 조건](#add-users-and-manage-access).

1. 프롬프트에 따라 사용자 이메일 주소를 지정하고 프로젝트 및 환경 유형 역할을 설정한 다음 사용자를 추가합니다.

   > 샘플 프롬프트

   ```terminal
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

```terminal
Current role(s) of User (alice@example.com) on Production (project_id):
  Project role: admin
```

### 여러 환경에 사용자 추가

사용자를 다음으로 추가하려면 `viewer` 에 `Production` 환경 및 `contributor` 에 `Integration` 환경:

```bash
magento-cloud user:add alice@example.com -r production:v -r integration:c
```

### 사용자 환경 권한 업데이트

사용자 환경 권한을 다음으로 업데이트하려면 `admin` 다음에 있음 `Production` 환경:

```bash
magento-cloud user:update alice@example.com -r production:a
```

## 에서 사용자 관리 [!DNL Cloud Console]

다음을 사용할 수 있습니다. [[!DNL Cloud Console]](../../get-started/cloud-console.md) 권한을 추가하고 _편집_ 기존 사용자의 권한을 수정하는 기능입니다.

>[!IMPORTANT]
>
>사용자에게 Adobe ID이 있어야 합니다. 다음을 참조하십시오. [전제 조건](#add-users-and-manage-access).

### 프로젝트에 사용자 추가

1. 에 로그인합니다 [[!DNL Cloud Console]](https://console.adobecommerce.com/).

1. 다음에서 프로젝트 선택 _모든 프로젝트_ 목록을 표시합니다.

1. 프로젝트 대시보드에서 오른쪽 상단의 구성 아이콘을 클릭합니다.

1. 아래 _프로젝트 설정_, 클릭 **[!UICONTROL Access]**.

1. 다음에서 _액세스_ 보기, 클릭 **[!UICONTROL Add]**.

1. 다음을 완료합니다. _[!UICONTROL Add User]_양식:

   - 사용자 이메일 주소를 입력합니다.

   - **[!UICONTROL Project admin]**—모든 설정 및 환경 유형에 관리자 권한을 부여합니다.

   - **[!UICONTROL Environment types and permissions]**—특정 환경 유형에 액세스 권한 및 특정 권한 수준을 부여합니다. _액세스 권한 없음_, _관리자_ (설정 변경, 작업 실행, 코드 병합), _참여자_ (푸시 코드) 또는 _뷰어_ (보기 전용).

   >[!TIP]
   >
   >만 **프로젝트 관리자** 는 모든 환경에서 사용자를 관리할 수 있습니다. 사용자에게 다음에 대한 액세스 권한을 부여하려면 **액세스** 탭, 다른 탭 **프로젝트 관리자** 또는 **계정 소유자** 은(는) 해당 사용자에게 을(를) 할당해야 합니다. **프로젝트 관리자** 역할.

1. 클릭 **[!UICONTROL Add User]**.

1. 사용자를 추가한 후 모든 환경을 재배포하여 변경 사항을 적용합니다. 사용자를 추가해도 배포가 자동으로 트리거되지 않습니다. 재배포는 사용자가 SSH를 사용하여 환경에 액세스할 수 있도록 하는 중요한 단계입니다.

사용자를 추가하면 Adobe은 클라우드 인프라 프로젝트에서 Adobe Commerce에 액세스하기 위한 지침과 함께 지정된 주소로 이메일을 전송합니다.

## 사용자 인증 요구 사항

보안 강화를 위해 Adobe은 클라우드 인프라 프로젝트 소스 코드 및 환경에서 Adobe Commerce에 대한 SSH 액세스에 TFA(2단계 인증)가 필요하도록 프로젝트 수준의 MFA(Multi-Factor Authentication) 적용을 제공합니다. 다음을 참조하십시오 [SSH용 MFA 활성화](multi-factor-authentication.md).

Adobe Commerce on cloud infrastructure 프로젝트에서 MFA 시행이 활성화되면 해당 프로젝트의 환경에 대한 SSH 액세스 권한이 있는 모든 사용자는 Adobe Commerce on cloud infrastructure 계정에서 TFA를 활성화해야 합니다. 자동화된 프로세스의 경우 명령줄에서 인증할 컴퓨터 사용자 및 API 토큰을 만들 수 있습니다.

클라우드 프로젝트에 사용자를 추가한 후 사용자에게 계정 보안 설정을 검토하고 필요에 따라 다음 보안 구성을 추가하도록 요청합니다.

- **TFA 활성화**—이중 인증을 구성하여 보안 및 규정 준수 표준을 충족합니다. 다음으로 구성된 프로젝트 [MFA 적용](multi-factor-authentication.md) 프로젝트에 액세스하기 위해 SSH를 사용하는 계정의 TFA가 필요합니다.

- **SSH 키 사용**—클라우드 인프라 소스 코드 저장소에서 Adobe Commerce에 액세스해야 하는 사용자는 자신의 계정에서 SSH 키를 활성화해야 합니다. 다음을 참조하십시오 [보안 연결](../development/secure-connections.md).

- **API 토큰 만들기**—환경에 대한 SSH 액세스에 사용되는 API 토큰을 생성해야 합니다. 자동화된 프로세스에 대한 인증 워크플로를 활성화하려면 토큰이 필요합니다.

  MFA 시행이 활성화된 프로젝트에서 API 토큰을 사용하여 자동화된 계정의 SSH 액세스 요청을 인증해야 합니다. 이 토큰을 사용하면 자동화된 프로세스에서 TFA가 필요한 인증 워크플로를 우회할 수 있습니다.

### 클라우드 계정용 TFA 활성화

Adobe Commerce on cloud infrastructure는 다음 애플리케이션 중 하나를 사용하여 TFA를 지원합니다.

- [Google Authenticator(Android/iPhone)](https://support.google.com/accounts/answer/1066447?hl=en)
- [인증(Android/iPhone)](https://authy.com/features/)
- [FreeOTP(Android)](https://play.google.com/store/apps/details?id=org.fedorahosted.freeotp)
- [GAuth 인증자(Firefox OS, 데스크탑 등)](https://github.com/gbraad-apps/gauth)

인증자 애플리케이션 설치 및 TFA 사용 지침은 _계정 설정_ 페이지의 [!DNL Cloud Console].

**사용자 계정에서 TFA를 사용하려면**:

1. 에 로그인 [내 계정](https://console.adobecommerce.com).

1. 오른쪽 위 계정 메뉴에서 **[!UICONTROL My Profile]**.

1. 다음에서 _보안_ 탭을 클릭하고 **[!UICONTROL Set up application]**.

1. 모바일 장치에 승인된 인증자 응용 프로그램이 없는 경우 연결된 지침을 사용하여 응용 프로그램을 설치합니다.

1. 인증자 애플리케이션에 클라우드 인프라의 Adobe Commerce 계정을 추가합니다.

   - 모바일 장치에서 인증자 애플리케이션을 엽니다. 그런 다음 응용 프로그램에 설정 코드를 추가합니다.

   - 다음에서 [!UICONTROL **[!UICONTROL TFA set up - Application]**] 페이지에서 모바일 장치의 TFA 코드를 **[!UICONTROL Application verification code]** 필드.

   - 클릭 **[!UICONTROL Verify and save]**.

     코드가 유효하면 Adobe이 계정 이메일 주소로 알림이 전송되어 계정에 TFA가 있음을 확인할 수 있습니다.

1. 선택 사항입니다. 사용 _신뢰할 수 있는 브라우저_ 30일 동안 브라우저에서 인증 코드를 캐시하는 설정입니다.

   이 구성은 프로젝트 로그인 중 인증 문제 수를 줄입니다.

1. 클릭 **저장** 또는 **건너뛰기**.

1. 복구 코드를 저장합니다.

   - 다음에서 _TFA 설정 - 복구_ 코드 페이지 - 모바일 장치나 인증 애플리케이션에 액세스할 수 없는 경우 Adobe Commerce on cloud infrastructure 프로젝트에 로그인할 수 있도록 복구 코드를 복사하여 저장합니다.

   - 복구 코드를 다른 위치에 복사하거나 장치 또는 인증 응용 프로그램에 대한 액세스 권한을 잃어 버릴 경우에 대비해 기록하십시오.

   - 클릭 **저장** 계정 보안 설정에서 코드를 보고 관리할 수 있도록 계정에 코드를 저장합니다.

     >[!WARNING]
     >
     >TFA가 있는 계정에 대한 액세스 권한을 상실하고 복구 코드 목록이 없는 경우 프로젝트 관리자에게 문의하거나 [Adobe Commerce 지원 티켓 제출](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) 를 클릭하여 TFA 응용 프로그램을 재설정합니다.

1. TFA 설정을 완료한 후 다음을 클릭합니다. **저장** 계정을 업데이트합니다.

1. TFA를 사용하여 현재 세션을 인증합니다.

   - 계정에서 로그아웃합니다.
   - 사용자 이름과 암호로 로그인합니다.
   - 메시지가 표시되면 다음에 대한 TFA 코드를 입력합니다. `accounts.magento.cloud` 모바일 장치의 인증자 응용 프로그램 항목.

### TFA 구성 및 복구 코드 관리

에서 클라우드 인프라 계정의 Adobe Commerce에 대한 TFA 구성을 관리할 수 있습니다. _보안_ 다음에 대한 섹션 _내 프로필_ 페이지를 가리키도록 업데이트하는 중입니다.

1. 에 로그인 [내 계정](https://console.adobecommerce.com).

1. 오른쪽 위 계정 메뉴에서 **[!UICONTROL My Profile]**.

1. 다음에서 _내 프로필_ 페이지를 클릭하고 **[!UICONTROL Security]** 탭.

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

1. 에 로그인 [내 계정](https://console.adobecommerce.com).

1. 오른쪽 위 계정 메뉴에서 **[!UICONTROL My Profile]**.

1. 다음에서 _내 프로필_ 페이지를 클릭하고 **[!UICONTROL API tokens]** 탭.

1. 클릭 **[!UICONTROL Create API token]** 이름을 입력합니다. 예를 들어, 컴퓨터 사용자 또는 API 토큰을 사용하는 자동화된 프로세스와 일치하는 이름을 지정합니다.

   ![API 토큰](../../assets/api-token-name.png)

1. 클릭 **[!UICONTROL Create API token]**.
