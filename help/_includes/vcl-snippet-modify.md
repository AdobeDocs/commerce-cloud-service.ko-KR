---
source-git-commit: 2d902a3926c6bbc6a9dc8afcbd667eddeaf3be7e
workflow-type: tm+mt
source-wordcount: '112'
ht-degree: 0%

---
# Fastly에 대한 사용자 지정 VCL을 수정하기 위한 파일 포함

## 사용자 지정 VCL 코드 조각 수정

1. [로그인](/help/get-started/onboarding.md#access-your-admin-panel) 관리자에게 문의하십시오.

1. 클릭 **스토어** > **설정** > **구성** > **고급** > **시스템**.

1. 확장 **전체 페이지 캐시** > **Fastly 구성** > **사용자 지정 VCL 코드 조각**.

   ![사용자 지정 VCL 코드 조각 관리](/help/assets/cdn/fastly-manage-snippets.png)

1. 다음에서 _작업_ 열에서 편집할 코드 조각 옆에 있는 설정 아이콘을 클릭합니다.

1. 페이지를 다시 로드한 후 **VCL을 Fastly에 업로드** 다음에서 _Fastly 구성_ 섹션.

1. 업로드가 완료되면 페이지 상단의 알림에 따라 캐시를 새로 고칩니다.

>[!WARNING]
>
>다음 _사용자 지정 VCL 코드 조각_ UI 옵션은 Adobe Commerce 관리자를 통해 추가된 코드 조각만 표시합니다. Fastly API를 사용하여 스니펫을 추가하는 경우 API를 사용하여 다음을 수행합니다 [관리](/help/cloud-guide/cdn/fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api).
