---
title: New Relic 모니터링
description: New Relic 대시보드에 액세스하고 Adobe Commerce on cloud infrastructure 프로젝트에서 데이터를 분석하는 방법을 알아봅니다.
feature: Cloud, Observability
topic: Performance
exl-id: 8d1e2ad6-14d0-4754-9703-960d93e135a9
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '844'
ht-degree: 0%

---

# New Relic 모니터링

New Relic은 인프라 및 [!DNL Commerce] PHP 에이전트를 사용하는 응용 프로그램입니다. 클라우드 환경이 New Relic에 연결되면 New Relic 계정에 로그인하여 에이전트가 수집한 데이터를 검토할 수 있습니다.

다음에서 _APM 및 서비스_ 페이지에서 **요약** 을 클릭하여 애플리케이션에 대한 트랜잭션 정보를 봅니다. 이 보기는 잠재적인 오류를 식별하고 애플리케이션 및 서비스의 전반적인 상태를 확인하는 데 도움이 됩니다.

![Cloud 프로젝트 New Relic 개요 페이지](../../assets/new-relic/dashboard.png)

이 보기에서 응답이 느려지거나 병목 현상이 발생하는 트랜잭션, 애플리케이션 처리량, 웹 오류 등을 추적할 수 있습니다.

추적된 데이터 검토:

- **가장 많은 시간 소요**- 요청을 동시에 추적하여 시간 소비를 결정합니다. 예를 들어 제품 및 카테고리 보기에서 가장 높은 트랜잭션 시간이 있을 수 있습니다. 고객 계정 페이지가 갑자기 시간 소비량 순위가 높은 경우 호출 또는 쿼리 드래그 성능에 영향을 받을 수 있습니다.

- **최고 처리량**- 전송된 바이트의 크기 및 빈도에 따라 가장 많은 히트를 기록한 페이지를 식별합니다.

수집된 모든 데이터는 데이터, 쿼리 또는 _레디스_ 데이터. 쿼리로 인해 문제가 발생하는 경우 New Relic은 해당 문제를 추적하고 이에 응답하는 정보를 제공합니다.

>[!TIP]
>
>이 데이터를 사용하여 응용 프로그램 성능 문제를 해결하는 방법에 대한 자세한 내용은 [New Relic을 사용하여 성능 문제 해결](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/troubleshoot-performance-using-new-relic-on-magento-commerce.html) 다음에서 _Adobe Commerce 도움말 센터_.

## 관리 경고로 성능 모니터링

Adobe은 _Adobe Commerce에 대한 관리 경고_ 성능 지표를 추적하는 경고 정책. 이 정책에는 인프라 또는 애플리케이션 문제가 사이트 성능에 영향을 줄 때 임계값을 설정하고 경고 및 중요 알림을 트리거하는 경고 모음이 포함됩니다. 이 정책은 프로덕션 환경에서 다음 지표를 추적합니다.

| 지표 | 데이터 수집 | 가용성 |
|:-------------------|:----------------|:----------------|
| [!DNL Apdex] 점수 | APM | Pro 및 Starter |
| 사용 | NRI | Pro |
| 디스크 공간 | NRI | Pro |
| 오류율 | APM | Pro 및 Starter |
| 메모리 사용 | NRI | Pro |
| MariaDB 쿼리 로드 | NRI | Pro |
| 레디스 메모리 | NRI | Pro |

사이트 인프라 또는 애플리케이션 조건에서 경고 임계값을 트리거하면 New Relic에서 사전 예방적으로 문제를 해결할 수 있도록 경고 알림을 보냅니다. 다음을 참조하십시오 [Adobe Commerce에 대한 관리 경고](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-for-magento-commerce.html) 다음에서 _Adobe Commerce 도움말 센터_ 경고 임계값 및 경고를 트리거한 문제를 해결하기 위한 문제 해결 단계에 대한 자세한 내용은 을 참조하십시오.

>[!TIP]
>
>Pro Staging 및 통합 환경과 Starter 환경의 경우 다음을 사용하십시오. [상태 알림](../integrations/health-notifications.md) 디스크 공간을 모니터링합니다.

>[!PREREQUISITES]
>
>- **New Relic 자격 증명**—클라우드 프로젝트의 New Relic 계정에 로그인하기 위한 자격 증명
>- **활성 New Relic 통합**—클라우드 환경이 New Relic에 연결되어 있는지 확인합니다.
>- **워크플로 알림**—최소 하나 이상의 구성 [워크플로우](#set-up-a-workflow-for-notifications) 경고 알림을 받으려면

**Adobe Commerce에 대한 관리 경고 정책을 검토하려면**:

1. 에 로그인 [New Relic 계정](https://login.newrelic.com/login).

1. 를 찾습니다. _Adobe Commerce에 대한 관리 경고_ 정책:

   - Explorer 탐색 메뉴에서 **[!UICONTROL Alerts & AI]**.

   - 아래 _감지_, 클릭 **[!UICONTROL Alert Conditions & Policies]**.

   - 계정 이(가) 의 맨 위에서 선택되었는지 확인합니다. _경고 조건 및 정책_ 보기.

   - 다음에서 _정책_ 목록, 선택 **Adobe Commerce에 대한 관리 경고** 정책.

     ![생성된 경고 정책](../../assets/new-relic/managed-alerts-policy.png)

     >[!NOTE]
     >
     >다음과 같은 경우 _Adobe Commerce에 대한 관리 경고_ 정책을 사용할 수 없습니다. 다음을 참조하십시오 [Adobe Commerce에 대한 관리 경고](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-for-magento-commerce.html) 다음에서 _Adobe Commerce 도움말 센터_.

1. 다음을 클릭합니다. **[!UICONTROL Alert conditions]** 정책에 정의된 경고 조건을 검토하려면 탭하십시오.

## 경고 정책 만들기

Adobe Commerce에 대한 관리 경고 정책에 포함된 경고를 수정하지 마십시오. Adobe은 시간이 지남에 따라 이 정책의 경고 조건을 업데이트하고 개선하여 정책에 추가하는 모든 사용자 지정 사항을 덮어씁니다.

기존 경고를 수정하는 대신 경고 정책을 만들 수 있습니다. 그런 다음 경고 조건을 새 정책에 복사합니다. 다음을 참조하십시오 [정책 또는 조건 업데이트](https://docs.newrelic.com/docs/alerts-applied-intelligence/new-relic-alerts/alert-policies/update-or-disable-policies-conditions/) 다음에서 _New Relic_ 설명서를 참조하십시오.

>[!TIP]
>
>다음을 참조하십시오 [경고 소개](https://docs.newrelic.com/docs/alerts-applied-intelligence/new-relic-alerts/learn-alerts/alerts-concepts-workflow/) 다음에서 _New Relic_ 경고, 경고 정책 및 워크플로에 대한 자세한 내용은 설명서 를 참조하십시오.

## 알림에 대한 워크플로우 설정

이제 다음을 설정할 수 있습니다. _워크플로우_&#x200B;이전에는 알림 채널이라고 했으며, 경고 정책과 같이 필터링된 데이터를 기반으로 사이트 성능에 대한 알림을 받습니다. 애플리케이션 또는 인프라의 조건이 경고를 트리거할 때 경고 정책과 관련된 모든 워크플로우로 성능 문제에 대한 알림이 전송됩니다. 문제가 확인 및 종료된 경우에도 알림을 받습니다.

New Relic은 이메일, Slack, PagerDuty, 웹후크 등을 포함하여 다양한 유형의 워크플로 알림을 구성하기 위한 템플릿을 제공합니다.

**워크플로우를 구성하려면**:

1. 에 로그인 [New Relic 계정](https://login.newrelic.com/login).

1. 워크플로우를 만듭니다.

   - Explorer 탐색 메뉴에서 **[!UICONTROL Alerts & AI]**.

   - 아래의 왼쪽 탐색에서 _강화 및 알림_, 클릭 **[!UICONTROL Workflows]**.

   - 클릭 **[!UICONTROL Add a workflow]** 오른편에 있습니다.

     ![New Relic 워크플로우 추가](../../assets/new-relic/add-a-workflow.png)

   - 다음에서 _워크플로우 구성_ 페이지에서 워크플로우의 이름을 입력합니다.

   - 다음에서 _데이터 필터링_ 섹션, 선택 **[!UICONTROL Managed Alerts for Adobe Commerce]** 다음에서 **[!UICONTROL Policy]** 드롭다운 목록입니다.

   - 다음에서 _알림_ 섹션에서 채널을 선택하고 지침을 따릅니다.

   - 클릭 **[!UICONTROL Test workflow]** 구성을 확인합니다.

1. 클릭 **[!UICONTROL Activate workflow]**.

다음에 대한 New Relic 설명서 참조: [워크플로](https://docs.newrelic.com/docs/alerts-applied-intelligence/applied-intelligence/incident-workflows/incident-workflows/).

>[!WARNING]
>
>Adobe Commerce에 대한 관리 경고 정책의 경고에는 클라우드 인프라 고객의 Adobe Commerce을 지원하는 Adobe 팀에 알리도록 구성된 기본 워크플로가 있습니다. 이러한 기본 채널에 대한 구성을 수정하지 말고 지정된 경고 정책을 제거하지 마십시오.
