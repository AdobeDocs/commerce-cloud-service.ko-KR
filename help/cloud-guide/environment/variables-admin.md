---
title: 관리자 변수
description: 클라우드 인프라에 Adobe Commerce을 설치할 때 사용되는 환경 변수 목록을 참조하십시오.
feature: Cloud, Configuration, Install, Roles/Permissions
role: Developer
exl-id: 2829a9dc-40bb-4665-886e-a56d98468fc1
source-git-commit: 13e76d3e9829155995acbb72d947be3041579298
workflow-type: tm+mt
source-wordcount: '421'
ht-degree: 0%

---

# 관리자 변수

클라우드 인프라 프로젝트의 Adobe Commerce에 대한 관리 액세스 권한이 있는 사용자는 다음 프로젝트 환경 변수를 사용하여 관리 UI에 액세스하기 위한 관리 사용자 계정에 대한 구성 설정을 재정의할 수 있습니다.

## 관리자 자격 증명

다음 표의 ADMIN 변수를 사용하여 Commerce 설치 중에 관리자 사용자 자격 증명을 재정의할 수 있습니다.

설치 후 값을 변경하려면 SSH를 사용하여 환경에 연결하고 Adobe Commerce CLI를 사용하십시오 [`admin:user` 명령](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/admin.html) 관리자 자격 증명을 만들거나 편집합니다.

| 변수 | 기본값 | 설명 |
| -------------- | --------------------------- | ----------- |
| `ADMIN_USERNAME` | 라이선스 소유자 이메일 주소 | 관리 사용자를 포함하여 다른 사용자를 생성할 수 있는 관리 사용자의 사용자 이름입니다. |
| `ADMIN_EMAIL` |                             | 관리자의 이메일 주소. 이 주소는 암호 재설정 알림을 보내는 데 사용됩니다. |
| `ADMIN_PASSWORD` |                             | 관리자 암호. 프로젝트가 생성되면 무작위 암호가 생성되고 이메일이 라이선스 소유자에게 전송됩니다. 프로젝트를 만드는 동안 라이선스 소유자가 이미 암호를 변경했어야 합니다. 업데이트된 암호는 라이센스 소유자에게 문의하십시오. |
| `ADMIN_LOCALE` | `en_US` | 관리자가 사용하는 기본 로케일. |

## 관리자 URL

관리 UI에 대한 액세스 보안을 유지하려면 다음 환경 변수를 사용하십시오. 지정하면 이 값이 설치 중에 기본 URL을 재정의합니다.

`ADMIN_URL`- 관리 UI에 액세스할 수 있는 상대 URL입니다. 기본 URL은 `/admin`. 보안상의 이유로 Adobe은 기본값을 쉽게 추측할 수 없는 고유한 사용자 지정 관리자 URL로 변경할 것을 권장합니다.

### 관리자 URL 변경

Adobe은 설치 후 관리 URL에 대한 환경 수준 변수를 변경할 것을 권장합니다. 클론에서 분기하기 전에 보안상의 이유로 이 설정을 구성합니다. `master` 환경. 에서 생성된 모든 분기 `master` 분기는 환경 수준 변수와 해당 값을 상속합니다.

사용 `magento-cloud variable:update` 변수 값을 업데이트하는 명령입니다. (다음 `variable:set` 명령이 더 이상 사용되지 않으며 사용할 수 없습니다.) 다음 예제에서는 ADMIN_URL을 `newAdmin_A8v10`:

```bash
magento-cloud variable:update ADMIN_URL --value newAdmin_A8v10 -e master
```

>[!NOTE]
>
>다음 `ADMIN_URL` 값은 사용자 지정 관리자 경로에 대해 문자(a-z 또는 A-Z), 숫자(0-9) 및 밑줄 문자(_)를 허용합니다. 공백 또는 기타 문자는 다음과 같습니다 **아님** 수락됨.

**를 사용하여 URL을 변경하려면[!DNL Cloud Console]**:

1. 에 로그인합니다 [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. 다음에서 프로젝트 선택 _모든 프로젝트_ 목록을 표시합니다.

1. 프로젝트 개요에서 환경을 선택하고 구성 아이콘을 클릭합니다.

   ![프로젝트 구성](../../assets/icon-configure.png){width="36"}

1. 다음 항목 선택 **변수** 탭.

1. 클릭 **변수 만들기**.

1. 다음을 입력합니다.

   - **변수 이름** = `ADMIN_URL`
   - **값** = 새 URL. 예를 들어 관리자 URL을 로 설정합니다. `magento_A8v10`.

   기본적으로, `Available during runtime` 및 `Make inheritable` 이(가) 선택되어 있습니다.

1. 클릭 **변수 만들기** 배포가 완료될 때까지 기다립니다. 이 단추는 필수 필드에 값이 포함된 경우에만 표시됩니다.
