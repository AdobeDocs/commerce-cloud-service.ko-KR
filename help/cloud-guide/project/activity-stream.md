---
title: 활동 스트림
description: ' [!DNL Cloud Console] 또는 클라우드 인프라의 Adobe Commerce용 Cloud CLI에서 활동 스트림을 읽는 방법을 알아봅니다.'
last-substantial-update: 2024-02-06T00:00:00Z
exl-id: ffef5ab4-ef40-4073-adc8-a44c61c0d77b
source-git-commit: 85ff1283f773823ff2c6e6ab8f391fd5b4aa00e4
workflow-type: tm+mt
source-wordcount: '449'
ht-degree: 0%

---

# 활동 스트림

각 환경의 기본 보기에는 Git 로그와 유사한 기록 이벤트의 **활동** 목록이 표시됩니다. 활동 목록은 활성 환경에 대한 최근 이벤트 스트림입니다. 다음은 활동 스트림에 표시되는 활동 유형 및 아이콘 목록입니다.

![활동 유형](../../assets/activity-types.svg){width="500" align="center"}

## 로그 보기

활동 목록에서 활동의 상태 아이콘을 클릭하여 로그를 확인합니다. 또는 ![자세히](../../assets/icon-more.png){width="32"}(_자세히_) 메뉴를 클릭하여 활동을 관리하는 더 많은 옵션에 액세스하십시오. 다음은 백업을 만드는 짧은 로그를 보여 줍니다. [Cloud CLI를 사용](#activity-stream-with-cloud-cli)하여 동일한 로그를 볼 수 있습니다.

![로그 보기](../../assets/log-view.png)

## 활동 관리

일부 활동이 _실행 중_ 또는 _보류 중_ 상태입니다. 실행 중인 배포 취소와 같이 실행 중인 활동에 대해 작업을 수행할 수 있습니다. 다음 탭은 활동을 취소하는 두 가지 방법을 보여 줍니다. [!DNL Cloud Console] 또는 Cloud CLI.

>[!BEGINTABS]

>[!TAB 콘솔]

**[!DNL Cloud Console]**&#x200B;에서 활동을 취소하려면 다음을 수행하십시오.

![자세히](../../assets/icon-more.png){width="32"}(_자세히_) 메뉴에 액세스하고 `Cancel` 또는 `View log`과(와) 같은 작업을 선택하여 실행 중인 활동에 대한 작업을 수행할 수 있습니다. 이 예제에서는 실행 중인 활동을 중지하려면 **취소** 옵션을 선택하십시오.

모든 활동에 취소 옵션이 있는 것은 아닙니다. 예를 들어 응용 프로그램 배포를 취소하는 옵션은 _빌드_ 단계에서만 나타납니다. 응용 프로그램이 _배포_ 단계로 이동한 후에는 더 이상 활동을 취소할 수 없습니다. 다른 단계에 대한 자세한 내용은 [배포 프로세스](../deploy/process.md)를 참조하세요.

![활동 취소](../../assets/activity-icons/cancel-activity.png){width="450" align="center"}

배포 활동을 실행하는 터미널이 있는 경우 [!DNL Cloud Console]에서 취소하면 터미널에서 취소됩니다.

![터미널](../../assets/activity-icons/activity-cancelled.png){width="300"}에서 활동이 취소됨

>[!TAB CLI]

**Cloud CLI에서 활동을 취소하려면**:

1. 실행 중인 활동을 식별하고 활동 ID를 선택합니다.

   ```bash
   magento-cloud activity:list --state=in_progress
   ```

1. 활동 ID를 사용하여 활동을 취소합니다.

   ```bash
   magento-cloud activity:cancel wvl5wm7s5vkhy
   ```

>[!ENDTABS]

## 활동 스트림 필터링

활동 목록을 필터링하는 기능은 백업 또는 병합 이벤트와 같은 특정 항목을 찾고 있을 때 유용합니다.

**[!DNL Cloud Console]**&#x200B;에서 활동 목록을 필터링하려면:

1. 전체 이벤트 내역을 포함하려면 환경을 선택하고 활동 **[!UICONTROL All]** 보기를 선택하십시오.

1. ![필터링 기준](../../assets/icon-filterby.png){width="32"}을 클릭하고 **[!UICONTROL Filter by]** 옵션을 선택합니다.

   ![활동 필터링](../../assets/activity-filter.png)

1. **[!UICONTROL Recent]** 활동 보기를 선택하고 목록을 다시 설정합니다.

## Cloud CLI로 스트림 보기

`magento-cloud` CLI는 [!DNL Cloud Console]과(와) 거의 동일한 기능을 제공합니다. `activity` 명령은 다음을 수행할 수 있습니다.

- 환경에 대한 활동 스트림 `list`
- 특정 활동에 대한 `get` 세부 정보
- 특정 활동에 대한 `log` 표시
- `cancel` 활동

**Cloud CLI로 활동 스트림을 보려면**:

1. 현재 환경에 대한 활동을 나열합니다.

   ```bash
   magento-cloud activity:list
   ```

1. 각 활동에는 고유한 ID가 있습니다. 이전 목록에서 ID를 선택하고 해당 활동에 대한 세부 정보를 봅니다.

   ```bash
   magento-cloud activity:get wvl5wm7s5vkhy
   ```

1. 해당 활동에 대한 전체 로그를 봅니다.

   ```bash
   magento-cloud activity:log wvl5wm7s5vkhy
   ```

   샘플 응답:

   ```bash
   Activity ID: wvl5wm7s5vkhy
   Type: environment.backup
   Description: User created a backup of Master
   Created: 2023-09-08T14:03:33+00:00
   State: complete
   Log:
   Creating backup of master
   Created backup eg5pu63egt2dcojkljalzjdopa
   ```
