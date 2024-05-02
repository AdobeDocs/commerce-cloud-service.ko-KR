---
title: 백업 관리
description: Adobe Commerce on cloud infrastructure 프로젝트에 대한 백업을 수동으로 만들고 복원하는 방법에 대해 알아봅니다.
feature: Cloud, Paas, Snapshots, Storage
exl-id: 1cb00db7-2375-4761-9c07-1e20a74859e0
source-git-commit: 4c3f3f2775e8327476233520e52b589f7264786f
workflow-type: tm+mt
source-wordcount: '711'
ht-degree: 0%

---

# 백업 관리

다음을 사용하여 언제든지 활성 Starter 환경의 수동 백업을 수행할 수 있습니다. **[!UICONTROL Backup]** 의 단추 [!DNL Cloud Console] 또는 `magento-cloud snapshot:create` 명령입니다.

백업 또는 _스냅샷_ 는 실행 중인 서비스(MySQL 데이터베이스)의 모든 영구 데이터와 마운트된 볼륨(var, pub/media, app/etc)에 저장된 모든 파일을 포함하는 환경 데이터의 전체 백업입니다. 스냅샷은 _아님_ 코드가 Git 기반 저장소에 이미 저장되어 있으므로 코드를 포함합니다. 스냅샷의 사본은 다운로드할 수 없습니다.

백업/스냅샷 기능은 **아님** 기본적으로 재해 복구를 위해 정기적인 백업을 수신하는 Pro Staging 및 운영 환경에 적용됩니다. 을(를) 참조하십시오 [Pro 백업 및 재해 복구](../architecture/pro-architecture.md#backup-and-disaster-recovery) 추가 정보. Pro 스테이징 및 프로덕션 환경의 자동 라이브 백업과 달리 백업은 **아님** 자동. 다음과 같습니다. _본인_ 수동으로 백업을 만들거나 cron 작업을 설정하여 Starter 또는 Pro 통합 환경의 백업을 정기적으로 만들 책임.

## 수동 백업 만들기

에서 모든 활성 Starter 환경 및 Integration Pro 환경의 수동 백업을 만들 수 있습니다. [!DNL Cloud Console] 또는 Cloud CLI에서 스냅샷을 생성할 수 있습니다. 다음 항목이 있어야 합니다. [책임자 역할](../project/user-access.md) 환경용입니다.

**를 사용하여 Starter 환경의 백업을 만들려면[!DNL Cloud Console]**:

1. 에 로그인합니다 [[!DNL Cloud Console]](https://console.adobecommerce.com).
1. 프로젝트 탐색 모음에서 환경을 선택합니다. 환경이 활성화되어 있어야 합니다.
1. 다음에서 _백업_ 보기, 클릭 **[!UICONTROL Backup]**. Pro 환경에서는 이 옵션을 사용할 수 없습니다.

   ![백업](../../assets/button-backup.png){width="150"}

**를 사용하여 통합 환경의 백업을 만들려면[!DNL Cloud Console]**:

1. 에 로그인합니다 [[!DNL Cloud Console]](https://console.adobecommerce.com).
1. 프로젝트 탐색 모음에서 통합/개발 환경을 선택합니다. 환경이 활성화되어 있어야 합니다.
1. 다음 항목 선택 **[!UICONTROL Backup]** 오른쪽 상단 메뉴에서 옵션을 선택합니다. 이 옵션은 Starter 및 Pro 환경 모두에서 사용할 수 있습니다.
1. 다음을 클릭합니다. **[!UICONTROL Yes]** 단추를 클릭합니다.

**를 사용하여 스냅샷을 생성하려면 다음을 수행합니다 `magento-cloud` CLI**:

1. 로컬 워크스테이션에서 프로젝트 디렉터리로 변경합니다.
1. 스냅샷에 대한 환경 분기를 확인하십시오.
1. 스냅샷을 생성합니다.

   ```bash
   magento-cloud snapshot:create --live
   ```

   또는 `magento-cloud backup` 짧은 명령입니다. 다음 `--live` 옵션을 사용하면 다운타임을 피하기 위해 환경을 계속 실행할 수 있습니다. 옵션의 전체 목록을 보려면 다음을 입력합니다. `magento-cloud snapshot:create --help`.

   샘플 응답:

   ```terminal
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

   ```terminal
   Snapshots on the project (project-id), environment develop-branch (type: development):
   +---------------------------+----------------------+------------+
   | Created                   | Snapshot ID          | Restorable |
   +---------------------------+----------------------+------------+
   | 2023-03-08T17:07:01+00:00 | my-snapshot          | true       |
   +---------------------------+----------------------+------------+
   ```

## 수동 백업 복원

다음을 수행해야 합니다. [관리자 액세스](../project/user-access.md) 환경을 위해. 최대 개수 **7일** 끝 _복원_ 수동 백업. 백업을 복원해도 현재 git 분기의 코드는 변경되지 않습니다. 이러한 방식으로 백업을 복원하는 것은 Pro 스테이징 및 프로덕션 환경에는 적용되지 않습니다. 다음을 참조하십시오. [Pro 백업 및 재해 복구](../architecture/pro-architecture.md#backup-and-disaster-recovery).

복원 시간은 데이터베이스 크기에 따라 다릅니다.

- 대용량 데이터베이스(200GB 이상)는 5시간 소요
- 중간 데이터베이스(150GB)는 2시간 30분 정도 소요될 수 있음
- 소규모 데이터베이스(60GB)는 1시간 소요

>[!TIP]
>
>백업 없이 복원:
>
>- 이전 코드로 롤백하거나 환경에서 추가된 확장을 제거하려면 다음을 참조하십시오. [롤백 코드](#roll-back-code).
>- 다음과 같은 불안정한 환경을 복구하려면 _아님_ 백업, 참조 [환경 복원](../development/restore-environment.md).

**다음을 사용하여 백업을 복원하려면[!DNL Cloud Console]**:

1. 에 로그인합니다 [[!DNL Cloud Console]](https://console.adobecommerce.com).
1. 프로젝트 탐색 모음에서 환경을 선택합니다.
1. 다음에서 _백업_ 보기, 다음에서 백업 선택 _저장됨_ 목록을 표시합니다. 백업 기능은 다음과 같습니다 **아님** pro 환경에 적용합니다.
1. 다음에서 ![자세히](../../assets/icon-more.png){width="32"} (_기타_) 메뉴, 클릭 **복원**.
1. 백업에서 복원 정보를 검토하고 **예, 복원합니다**.

**Cloud CLI를 사용하여 스냅샷을 복원하려면**:

1. 로컬 워크스테이션에서 프로젝트 디렉터리로 변경합니다.
1. 복원할 환경 분기를 확인하십시오.
1. 사용 가능한 모든 스냅샷을 나열합니다.

   ```bash
   magento-cloud snapshot:list
   ```

   목록에서는 사용 가능한 스냅샷에 대한 정보를 반환합니다.

   ```terminal
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

Pro 스테이징 및 프로덕션 환경에서 재해 복구 스냅샷을 복원하려면 [서버에서 직접 데이터베이스 덤프 가져오기](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/how-to/restore-a-db-snapshot-from-staging-or-production#meth3).

## 롤백 코드

백업 및 스냅샷 기능 _아님_ 코드의 사본을 포함합니다. 코드가 이미 Git 기반 저장소에 저장되었으므로 Git 기반 명령을 사용하여 코드를 롤백(또는 되돌리기)할 수 있습니다. 예를 들어, `git log --oneline` 이전 커밋을 스크롤한 다음 [`git revert`](https://git-scm.com/docs/git-revert) 특정 커밋에서 코드를 복원합니다.

또한 _비활성_ 분기입니다. 를 사용하지 않고 git 명령을 사용하여 분기 만들기 `magento-cloud` 명령입니다. 정보 보기 [Git 명령](../dev-tools/cloud-cli-overview.md#git-commands) ( Cloud CLI 항목 참조)
