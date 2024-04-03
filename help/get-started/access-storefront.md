---
title: Commerce 관리 패널 액세스
description: Commerce 관리 패널에 액세스하는 방법을 알아봅니다.
recommendations: noDisplay, catalog
exl-id: 9a8a0a49-b108-48bd-b413-ec9431370c06
source-git-commit: 85ff1283f773823ff2c6e6ab8f391fd5b4aa00e4
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 0%

---

# Commerce 관리 패널 액세스

Commerce 관리 패널에 대한 관리 액세스 권한이 있는 사용자는 사용자를 추가하고, 스토어 서비스를 구성하고, 스토어 설정 및 사용자 지정 작업을 완료하는 등의 작업을 수행할 수 있습니다.

새 프로젝트의 경우 시작 이메일을 받은 후 첫 번째 단계는 라이선스 소유자 계정의 암호를 변경하여 프로젝트에 대한 관리자 액세스 권한을 보호하는 것입니다. 이 계정의 기본 사용자 이름은 라이선스 소유자 이메일 주소입니다.

다음 방법 중 하나를 사용하여 암호 변경 요청을 제출할 수 있습니다.

- 라이선스 소유자 이메일 주소로 전송된 시작 이메일을 찾고 링크를 따라 암호를 변경합니다.

- 에서 스토어 URL 복사 [[!DNL Cloud Console]](../cloud-guide/project/overview.md) 을 브라우저에 추가합니다. 그런 다음 를 추가합니다 `/admin` 를 URL 끝에 추가하여 로그인 페이지를 엽니다. 다음을 클릭합니다. **암호를 잊으셨습니까?** 라이선스 소유자 이메일 주소로 암호 변경 요청을 전송하는 링크입니다.

암호 변경 요청을 제출한 후, 암호 재설정 알림에 대해 이메일을 확인하십시오. 이메일을 받지 못한 경우 스팸 폴더를 확인합니다.

>[!TIP]
>
>암호 재설정에 실패하거나 관리 패널에 로그인할 수 없는 경우 관리자 액세스 권한을 가진 사용자는 SSH를 사용하여 프로젝트에 연결할 수 있고 `admin:user:create` CLI 명령입니다. 다음을 참조하십시오 [관리자 계정 만들기, 편집 또는 잠금 해제](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/admin.html) 다음에서 _설치 안내서_.
