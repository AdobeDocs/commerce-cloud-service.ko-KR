---
title: 인증 키
description: 클라우드 인프라의 Adobe Commerce에서 개발 프로젝트에 인증 키를 적용하는 방법을 알아봅니다.
feature: Cloud, Security
topic: Security
exl-id: b05cd4c2-0804-49c8-980a-4c7b6932082b
source-git-commit: 13e76d3e9829155995acbb72d947be3041579298
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 0%

---

# 인증 키

Adobe Commerce 저장소에 액세스하고 Adobe Commerce on cloud infrastructure 프로젝트에 대한 설치 및 업데이트 명령을 활성화하려면 인증 키가 있어야 합니다. 작성기 인증 자격 증명을 지정하는 방법에는 두 가지가 있습니다.

- **인증 파일**—cloud infrastructure 루트 디렉터리의 Adobe Commerce에 Adobe Commerce [인증 자격 증명](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/authentication-keys.html)이 들어 있는 파일입니다.
- **환경 변수** - 실수로 노출되는 것을 방지하기 위해 Adobe Commerce on cloud infrastructure 프로젝트에서 인증 키를 설정하는 환경 변수입니다.

>[!BEGINSHADEBOX]

**보안 메모**

Adobe은 인증 자격 증명이 실수로 노출되지 않도록 클라우드 프로젝트에 [환경 변수](#composer-auth-environment-variable) 메서드를 사용하는 것을 권장합니다.

인증 파일 메서드는 Commerce용 Cloud Docker를 로컬 개발 도구로 사용하는 경우에 이상적이지만, `auth.json` 파일을 공개 Git 기반 저장소에 업로드하지 않도록 주의하십시오. `auth.json` 파일을 [`.gitignore` 파일](../project/file-structure.md#ignoring-files)에 추가할 수 있습니다.

>[!ENDSHADEBOX]

## 인증 파일

**`auth.json` 파일을 만들려면**:

1. 프로젝트 루트 디렉터리에 `auth.json` 파일이 없으면 파일을 만드십시오.

   - 텍스트 편집기를 사용하여 프로젝트 루트 디렉터리에 `auth.json` 파일을 만듭니다.
   - [샘플 `auth.json`](https://github.com/magento/magento2/blob/2.3/auth.json.sample)의 내용을 새 `auth.json` 파일에 복사합니다.

1. `<public-key>` 및 `<private-key>`을(를) Adobe Commerce 인증 자격 증명으로 바꾸십시오.

   ```json
   {
       "http-basic": {
           "repo.magento.com": {
               "username": "<public-key>",
               "password": "<private-key>"
           }
       }
   }
   ```

1. 변경 사항을 저장하고 텍스트 편집기를 종료합니다.

## 작성기 인증 환경 변수

다음 방법은 공개 Git 기반 저장소에서 중요한 자격 증명이 실수로 노출되지 않도록 하는 가장 좋은 방법입니다.

**환경 변수를 사용하여 인증 키를 추가하려면**:

1. _[!DNL Cloud Console]_에서 프로젝트 탐색 오른쪽의 구성 아이콘을 클릭합니다.

   ![프로젝트 구성](../../assets/icon-configure.png){width="36"}

1. _프로젝트 설정_ 목록에서 **[!UICONTROL Variables]**&#x200B;을(를) 클릭합니다.

1. **[!UICONTROL Create variable]**&#x200B;을(를) 클릭합니다.

1. **[!UICONTROL Variable name]** 필드에 `env:COMPOSER_AUTH`을(를) 입력합니다.

1. _값_ 필드에 다음을 추가하고 `<public-key>` 및 `<private-key>`을(를) Adobe Commerce 인증 자격 증명으로 바꿉니다.

   ```json
   {
       "http-basic": {
           "repo.magento.com": {
               "username": "<public-key>",
               "password": "<private-key>"
           }
       }
   }
   ```

1. **[!UICONTROL Available during buildtime]**&#x200B;을(를) 선택하고 **[!UICONTROL Available during runtime]**&#x200B;을(를) 선택 취소합니다.

1. **[!UICONTROL Create variable]**&#x200B;을(를) 클릭합니다.

1. 각 환경에서 `auth.json` 파일을 제거합니다.
