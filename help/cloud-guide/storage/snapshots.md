---
title: 백업 관리
description: Adobe Commerce on cloud infrastructure 프로젝트에 대한 백업을 수동으로 만들고 복원하는 방법에 대해 알아봅니다.
feature: Cloud, Paas, Snapshots, Storage
exl-id: 1cb00db7-2375-4761-9c07-1e20a74859e0
source-git-commit: b49a51aba56f79b5253eeacb1adf473f42bb8959
workflow-type: tm+mt
source-wordcount: '711'
ht-degree: 0%

---

# 백업 관리

[!DNL Cloud Console]의 **[!UICONTROL Backup]** 버튼을 사용하거나 `magento-cloud snapshot:create` 명령을 사용하여 언제든지 활성 Starter 환경의 수동 백업을 수행할 수 있습니다.

백업 또는 _스냅숏_&#x200B;은(는) 실행 중인 서비스(MySQL 데이터베이스)의 모든 영구 데이터와 탑재된 볼륨(var, pub/media, app/etc)에 저장된 모든 파일을 포함하는 환경 데이터의 전체 백업입니다. Git 기반 저장소에 코드가 이미 저장되어 있으므로 스냅숏에 코드가 _포함되지_&#x200B;않습니다. 스냅샷의 사본은 다운로드할 수 없습니다.

백업/스냅숏 기능은 기본적으로 재해 복구 목적으로 일반 백업을 받는 Pro 스테이징 및 프로덕션 환경에는 **적용되지 않습니다**. 자세한 내용은 [Pro 백업 및 재해 복구](../architecture/pro-architecture.md#backup-and-disaster-recovery)를 참조하십시오. Pro 스테이징 및 프로덕션 환경의 자동 라이브 백업과 달리 백업은 **자동**&#x200B;이 아닙니다. Starter 또는 Pro 통합 환경의 백업을 주기적으로 만들려면 백업을 수동으로 만들거나 cron 작업을 설정하는 것은 _사용자_&#x200B;의 책임입니다.

## 수동 백업 만들기

[!DNL Cloud Console]에서 활성 Starter 환경 및 통합 Pro 환경의 수동 백업을 만들거나 Cloud CLI에서 스냅숏을 만들 수 있습니다. 환경에 대한 [관리자 역할](../project/user-access.md)이(가) 있어야 합니다.

**[!DNL Cloud Console]**&#x200B;을(를) 사용하여 Starter 환경의 백업을 만들려면 다음을 수행하십시오.

1. [[!DNL Cloud Console]](https://console.adobecommerce.com)에 로그인합니다.
1. 프로젝트 탐색 모음에서 환경을 선택합니다. 환경이 활성화되어 있어야 합니다.
1. _백업_ 보기에서 **[!UICONTROL Backup]**&#x200B;을(를) 클릭합니다. Pro 환경에서는 이 옵션을 사용할 수 없습니다.

   ![백업](../../assets/button-backup.png){width="150"}

**[!DNL Cloud Console]**&#x200B;을(를) 사용하여 통합 환경의 백업을 만들려면:

1. [[!DNL Cloud Console]](https://console.adobecommerce.com)에 로그인합니다.
1. 프로젝트 탐색 모음에서 통합/개발 환경을 선택합니다. 환경이 활성화되어 있어야 합니다.
1. 오른쪽 상단 메뉴에서 **[!UICONTROL Backup]** 옵션을 선택합니다. 이 옵션은 Starter 및 Pro 환경 모두에서 사용할 수 있습니다.
1. **[!UICONTROL Yes]** 단추를 클릭합니다.

**CLI를 사용하여 스냅숏을 만들려면**:`magento-cloud`

1. 로컬 워크스테이션에서 프로젝트 디렉터리로 변경합니다.
1. 스냅샷에 대한 환경 분기를 확인하십시오.
1. 스냅샷을 생성합니다.

   ```bash
   magento-cloud snapshot:create --live
   ```

   또는 `magento-cloud backup` short 명령을 사용할 수 있습니다. `--live` 옵션은 가동 중지 시간을 피하기 위해 환경을 계속 실행합니다. 전체 옵션 목록을 보려면 `magento-cloud snapshot:create --help`을(를) 입력하십시오.

   샘플 응답:

   ```
   Creating a snapshot of develop-branch
   Waiting for the activity ID (User created a backup of develop-branch):
   
   Creating backup of develop-branch
   Created backup my-snapshot
   [============================] 45 secs (complete)
   Activity ID succeeded
   Snapshot name: my-snapshot
   ```

1. 최신 스냅샷을 확인합니다.

   ```bash
   magento-cloud snapshot:list
   ```

   목록에서는 스냅샷 상태에 대한 정보를 반환합니다.

   ```
   Snapshots on the project (project-id), environment develop-branch (type: development):
   +---------------------------+----------------------+------------+
   | Created                   | Snapshot ID          | Restorable |
   +---------------------------+----------------------+------------+
   | 2023-03-08T17:07:01+00:00 | my-snapshot          | true       |
   +---------------------------+----------------------+------------+
   ```

## 수동 백업 복원

환경에 대한 [관리자 액세스 권한](../project/user-access.md)이 있어야 합니다. 수동 백업은 최대 **7일**&#x200B;에서 _복원_&#x200B;할 수 있습니다. 백업을 복원해도 현재 git 분기의 코드는 변경되지 않습니다. 이러한 방식으로 백업을 복원하는 것은 Pro 스테이징 및 프로덕션 환경에는 적용되지 않습니다. [Pro 백업 및 재해 복구](../architecture/pro-architecture.md#backup-and-disaster-recovery)를 참조하십시오.

복원 시간은 데이터베이스 크기에 따라 다릅니다.

- 대용량 데이터베이스(200GB 이상)는 5시간 소요
- 중간 데이터베이스(150GB)는 2시간 30분 정도 소요될 수 있음
- 소규모 데이터베이스(60GB)는 1시간 소요

>[!TIP]
>
>백업 없이 복원:
>
>- 이전 코드로 롤백하거나 환경에서 추가된 확장을 제거하려면 [코드 롤백](#roll-back-code)을 참조하십시오.
>- 백업이 없는 _불안정한 환경을 복원하려면 [환경 복원](../development/restore-environment.md)을 참조하십시오._

**[!DNL Cloud Console]**&#x200B;을(를) 사용하여 백업을 복원하려면:

1. [[!DNL Cloud Console]](https://console.adobecommerce.com)에 로그인합니다.
1. 프로젝트 탐색 모음에서 환경을 선택합니다.
1. _백업_ 보기의 _저장_ 목록에서 백업을 선택하십시오. 백업 기능은 Pro 환경에 적용되지 **않습니다**.
1. ![자세히](../../assets/icon-more.png){width="32"}(_자세히_) 메뉴에서 **복원**&#x200B;을 클릭합니다.
1. 백업 정보에서 복원 정보를 검토하고 **예, 복원**&#x200B;을 클릭합니다.

**Cloud CLI를 사용하여 스냅샷을 복원하려면**:

1. 로컬 워크스테이션에서 프로젝트 디렉터리로 변경합니다.
1. 복원할 환경 분기를 확인하십시오.
1. 사용 가능한 모든 스냅샷을 나열합니다.

   ```bash
   magento-cloud snapshot:list
   ```

   목록에서는 사용 가능한 스냅샷에 대한 정보를 반환합니다.

   ```
   Snapshots on the project (project-id), environment develop-branch (type: development):
   +---------------------------+----------------------+------------+
   | Created                   | Snapshot ID          | Restorable |
   +---------------------------+----------------------+------------+
   | 2023-03-08T17:07:01+00:00 | my-snapshot          | true       |
   +---------------------------+----------------------+------------+
   ```

1. 목록의 스냅샷 ID를 사용하여 스냅샷을 복원합니다.

   ```bash
   magento-cloud snapshot:restore <snapshot-id>
   ```

## 재해 복구 스냅샷 복원

Pro 스테이징 및 프로덕션 환경에서 재해 복구 스냅숏을 복원하려면 [서버에서 직접 데이터베이스 덤프를 가져옵니다](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/how-to/restore-a-db-snapshot-from-staging-or-production#meth3).

## 롤백 코드

백업 및 스냅숏에 코드 사본이 포함되어 있지 _않습니다_. 코드가 이미 Git 기반 저장소에 저장되었으므로 Git 기반 명령을 사용하여 코드를 롤백(또는 되돌리기)할 수 있습니다. 예를 들어 `git log --oneline`을(를) 사용하여 이전 커밋을 스크롤한 다음 [`git revert`](https://git-scm.com/docs/git-revert)을(를) 사용하여 특정 커밋에서 코드를 복원합니다.

또한 _비활성_ 분기에 코드를 저장하도록 선택할 수 있습니다. `magento-cloud` 명령을 사용하는 대신 git 명령을 사용하여 분기를 만드십시오. Cloud CLI 항목에서 [Git 명령](../dev-tools/cloud-cli-overview.md#git-commands) 정보를 참조하십시오.
