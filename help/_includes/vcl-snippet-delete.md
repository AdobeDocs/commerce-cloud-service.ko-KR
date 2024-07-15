---
source-git-commit: 8f1ed3067f6daed897151052c8b9f987d3df3a50
workflow-type: tm+mt
source-wordcount: '93'
ht-degree: 0%

---
# Fastly용 사용자 지정 VCL 삭제를 위한 파일 포함

## 사용자 지정 VCL 코드 조각 삭제

1. 책임자에 [로그인](/help/get-started/onboarding.md#access-your-admin-panel).

1. **스토어** > **설정** > **구성** > **고급** > **시스템**&#x200B;을 클릭합니다.

1. **전체 페이지 캐시** > **빠른 구성** > **사용자 지정 VCL 조각**&#x200B;을 확장합니다.

   ![사용자 지정 VCL 코드 조각 관리](/help/assets/cdn/fastly-manage-snippets.png)

1. _Action_ 열에서 삭제할 코드 조각 옆에 있는 휴지통 아이콘을 클릭합니다.

1. 다음 모달 창에서 **DELETE**&#x200B;를 클릭하고 새 버전을 활성화합니다.

>[!WARNING]
>
>_사용자 지정 VCL 코드 조각_ UI 옵션은 Adobe Commerce 관리자를 통해 추가된 코드 조각만 표시합니다. Fastly API를 사용하여 코드 조각을 추가하는 경우 API를 사용하여 [관리하세요](/help/cloud-guide/cdn/fastly-vcl-custom-snippets.md#manage-vcl-using-the-api).
