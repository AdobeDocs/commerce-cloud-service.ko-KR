---
title: 데이터 수집
description: New Relic에서 Commerce 데이터 수집을 보고 관리하는 방법을 알아봅니다.
feature: Cloud, Observability
exl-id: f88bf20c-604b-4986-b71c-bb726b2f00b8
source-git-commit: bf3debc5986d51a721537b52ffced58b2ee521ea
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 0%

---

# 데이터 수집

New Relic은 효과적인 모니터링 및 분석을 제공하기 위해 풍부한 데이터에 의존하지만, 대규모 데이터 세트는 시기 적절한 결과, 성능 및 규정 준수에 영향을 줄 수 있습니다. 이 항목에서는 데이터 수집 관리에 대한 몇 가지 지침과 데이터를 가장 효과적으로 미세 조정하는 전략을 제공합니다.

New Relic에서는 데이터 소스별 플랜 사용량을 요약하는 _데이터 관리_ 보기를 제공합니다.

**수집 데이터 및 소스를 보려면**:

1. New Relic 사용자 메뉴에서 **[!UICONTROL Manage your data]**&#x200B;을(를) 클릭합니다.
1. _관리_ 목록에서 **[!UICONTROL Data management]**&#x200B;을(를) 클릭합니다.

   ![데이터 관리](../../assets/new-relic/data-ingestion.png)

   **[!UICONTROL Data ingestion]** 탭에는 해당 날짜에 대해 수집된 데이터와 데이터 원본이 표시됩니다.
데이터 보존 탭에는 데이터가 저장되는 기간이 표시되고 제어됩니다.

1. **[!UICONTROL Limits]** 탭을 선택하고 계정에 대한 제한을 확인합니다.

Adobe Commerce의 데이터 소스는 다음과 같습니다.

- **APM 이벤트** - 차트 및 대시보드에 사용된 이벤트 데이터
- **인프라**—프로세스 및 호스트 지표(예: CPU, 스토리지, 네트워킹)
- **로깅**—CDN, APM 및 응용 프로그램 서버에 대한 로그

로그 데이터는 수집의 많은 부분에 기여합니다. [로그 데이터를 보고 분석](log-management.md#view-and-analyze-log-data)하는 방법을 확인하고 Adobe 담당자와 협력하여 데이터 수집 및 유지 요구 사항에 대한 전략을 수립합니다. _New Relic 설명서_&#x200B;에서 [데이터 수집 관리](https://docs.newrelic.com/docs/data-apis/manage-data/manage-data-coming-new-relic/)에 대해 자세히 알아보세요.
