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

- **인증 파일**- Adobe Commerce이 포함된 파일 [인증 자격 증명](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/authentication-keys.html) Adobe Commerce on cloud infrastructure 루트 디렉터리에서
- **환경 변수**—Adobe Commerce on cloud infrastructure 프로젝트에서 인증 키를 설정하여 실수로 노출되는 것을 방지하는 환경 변수입니다.

>[!BEGINSHADEBOX]

**보안 메모**

Adobe은 [환경 변수](#composer-auth-environment-variable) 인증 자격 증명의 우발적 노출을 방지하기 위해 클라우드 프로젝트와 함께 사용하는 방법입니다.

인증 파일 방법은 Cloud Docker for Commerce를 로컬 개발 도구로 사용할 때 이상적이지만 `auth.json` 파일을 공용 Git 기반 리포지토리에 추가합니다. 다음을 추가할 수 있습니다. `auth.json` 에 파일 [`.gitignore` 파일](../project/file-structure.md#ignoring-files).

>[!ENDSHADEBOX]

## 인증 파일

**을(를) 만들려면 `auth.json` 파일**:

1. 다음 항목이 없는 경우: `auth.json` 프로젝트 루트 디렉토리에 파일을 생성합니다.

   - 텍스트 편집기를 사용하여 `auth.json` 프로젝트 루트 디렉토리에 있는 파일입니다.
   - 의 내용을 복사합니다. [샘플 `auth.json`](https://github.com/magento/magento2/blob/2.3/auth.json.sample) 을(를) 새 항목으로 `auth.json` 파일.

1. 바꾸기 `<public-key>` 및 `<private-key>` Adobe Commerce 인증 자격 증명으로 바꿉니다.

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

1. 다음에서 _[!DNL Cloud Console]_프로젝트 탐색 오른쪽의 구성 아이콘을 클릭합니다.

   ![프로젝트 구성](../../assets/icon-configure.png){width="36"}

1. 다음에서 _프로젝트 설정_ 목록, 클릭 **[!UICONTROL Variables]**.

1. 클릭 **[!UICONTROL Create variable]**.

1. 다음에서 **[!UICONTROL Variable name]** 필드, 입력 `env:COMPOSER_AUTH`.

1. 다음에서 _값_ 필드, 다음을 추가하고 바꾸기 `<public-key>` 및 `<private-key>` Adobe Commerce 인증 자격 증명을 사용하여 다음을 수행합니다.

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

1. 선택 **[!UICONTROL Available during buildtime]** 및 선택 취소 **[!UICONTROL Available during runtime]**.

1. 클릭 **[!UICONTROL Create variable]**.

1. 제거 `auth.json` 각 환경의 파일입니다.
