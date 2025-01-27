---
title: Commerce 관리 패널 액세스
description: Commerce 관리 패널에 액세스하는 방법을 알아봅니다.
recommendations: noDisplay, catalog
exl-id: 9a8a0a49-b108-48bd-b413-ec9431370c06
source-git-commit: 8be5b3f33798b1f03fc85a7ac9111dabd7bfd9e5
workflow-type: tm+mt
source-wordcount: '310'
ht-degree: 0%

---

# Commerce 관리 패널 액세스

Commerce 관리 패널에 대한 관리 액세스 권한이 있는 사용자는 사용자를 추가하고, 스토어 서비스를 구성하고, 스토어 설정 및 사용자 지정 작업을 완료하는 등의 작업을 수행할 수 있습니다.

새 프로젝트의 경우 시작 이메일을 받은 후 첫 번째 단계는 라이선스 소유자 계정의 암호를 변경하여 프로젝트에 대한 관리자 액세스 권한을 보호하는 것입니다. 이 계정의 기본 사용자 이름은 라이선스 소유자 이메일 주소입니다.

다음 방법 중 하나를 사용하여 암호 변경 요청을 제출할 수 있습니다.

- 라이선스 소유자 이메일 주소로 전송된 시작 이메일을 찾고 링크를 따라 암호를 변경합니다.

- [[!DNL Cloud Console]](../cloud-guide/project/overview.md)에서 브라우저로 스토어 URL을 복사합니다. 그런 다음 URL 끝에 `/admin`을(를) 추가하여 로그인 페이지를 엽니다. **암호를 잊으셨습니까?라이선스 소유자 전자 메일 주소로 암호 변경 요청을 보내는** 링크입니다.

암호 변경 요청을 제출한 후, 암호 재설정 알림에 대해 이메일을 확인하십시오. 이메일을 받지 못한 경우 스팸 폴더를 확인합니다.

>[!TIP]
>
>암호 재설정이 실패하거나 [관리] 패널에 로그인할 수 없는 경우 관리자 액세스 권한이 있는 사용자는 SSH를 사용하여 프로젝트에 연결할 수 있고 `admin:user:create` CLI 명령을 사용하여 관리자 사용자를 추가할 수 있습니다. _설치 안내서_&#x200B;에서 [관리자 계정 만들기, 편집 또는 잠금 해제](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/admin.html)를 참조하십시오.

## 사이트 상태 모니터링

[사이트 전체 분석 도구](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/site-wide-analysis-tool/intro)는 Adobe Commerce 설치의 보안 및 운영을 보장하기 위한 자세한 시스템 통찰력과 권장 사항이 포함된 사전 예방적 셀프서비스 도구이자 중앙 저장소입니다. 24시간 연중무휴 실시간 성능 모니터링, 보고서 및 조언을 제공하여 잠재적인 문제를 식별하고 사이트 상태, 안전 및 애플리케이션 구성에 대한 가시성을 향상시킵니다. 이는 해결 시간을 줄이고 사이트 안정성과 성능을 개선하는 데 도움이 됩니다. [관리 패널](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/site-wide-analysis-tool/access#option-2-logging-in-to-your-site-wide-analysis-tool-dashboard-from-your-stores-admin-panel)에서 바로 사이트 전체 분석 도구에 액세스할 수 있습니다.
