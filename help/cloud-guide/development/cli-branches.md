---
title: CLI를 사용하여 분기 관리
description: Cloud CLI를 사용하여 클라우드 인프라에서 Adobe Commerce의 환경 분기를 관리하는 방법을 알아봅니다.
role: Developer
feature: Cloud, Install
exl-id: a871c7e2-4506-4a05-8fc2-fc5ef2afe609
source-git-commit: b49a51aba56f79b5253eeacb1adf473f42bb8959
workflow-type: tm+mt
source-wordcount: '676'
ht-degree: 0%

---

# CLI를 사용하여 분기 관리

`magento-cloud` CLI를 설치하려면 [Cloud CLI 참조](../dev-tools/cloud-cli-overview.md)를 참조하십시오. `magento-cloud` CLI를 설치하고 클라우드 인프라에 대한 원격 액세스를 위해 SSH 키를 설정한 후에는 `magento-cloud` CLI 명령을 사용하여 프로젝트의 환경을 관리할 수 있습니다. 환경 아키텍처에 대한 자세한 내용은 [Starter 아키텍처](../architecture/starter-architecture.md) 또는 [Pro 아키텍처](../architecture/pro-architecture.md)를 참조하십시오.

[!DNL Cloud Console]을(를) 사용하여 분기 및 환경을 관리하려면 [을(를) 사용하여 분기 관리 [!DNL Cloud Console]](../project/console-branches.md)를 참조하십시오.

## CLI 명령 사용

`magento-cloud` CLI 명령은 Git 명령과 유사합니다. 이를 사용하여 프로젝트에 연결하고 환경을 관리할 수 있습니다. 모든 디렉터리에서 명령을 실행할 수 있지만 프로젝트 디렉터리에서 실행하는 것이 좋습니다. 프로젝트 디렉터리에서 실행할 때는 `-p <project-ID>` 매개 변수를 생략할 수 있습니다. [Cloud CLI 참조](../dev-tools/cloud-cli-overview.md)를 참조하십시오.

## 프로젝트 복제

다음 지침은 `magento-cloud` CLI 명령과 Git 명령을 조합하여 프로젝트를 로컬 워크스테이션에 복제합니다. `magento-cloud` CLI 명령의 전체 목록을 보려면 `magento-cloud list` 명령을 사용하십시오.

>[!IMPORTANT]
>
>일부 Git 명령은 Adobe Commerce on cloud infrastructure 프로젝트에서 작업을 완료할 수 없습니다. 예를 들어 Git 명령을 사용하여 분기를 만들 수는 있지만 새 환경을 만들고 활성화할 수는 없습니다. `magento-cloud environment:branch <branch-name>` 명령을 사용하여 환경을 만들어야 환경이 _활성_ 상태가 됩니다. 또는 [!DNL Cloud Console]을(를) 사용하여 활성 환경을 만들 수 있습니다. [Cloud CLI 참조](../dev-tools/cloud-cli-overview.md#git-commands)를 참조하십시오.

**프로젝트 `master` 환경을 복제하려면**:

1. [파일 시스템 소유자](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html) 계정으로 로컬 워크스테이션에 로그인합니다.

1. 웹 서버 또는 가상 호스트 _docroot_ 디렉터리로 변경합니다.

1. `magento-cloud` CLI를 사용하여 로그인합니다.

   ```bash
   magento-cloud login
   ```

1. 프로젝트를 나열합니다.

   ```bash
   magento-cloud project:list
   ```

1. 프로젝트 복제

   ```bash
   magento-cloud project:get <project-ID>
   ```

   메시지가 표시되면 디렉토리 이름을 입력합니다.

1. `magento2` 디렉터리로 변경합니다.

1. 프로젝트에 사용할 수 있는 환경을 나열합니다.

   ```bash
   magento-cloud environment:list
   ```

   >[!IMPORTANT]
   >
   >`magento-cloud environment:list` 명령은 환경 계층을 표시하지만 `git branch` 명령은 표시하지 않습니다.

1. 원격 분기를 가져옵니다.

   ```bash
   git fetch origin
   ```

1. 업데이트된 코드를 가져옵니다.

   ```bash
   git pull origin <environment-ID>
   ```

>[!TIP]
>
>클라우드 인프라에서 Adobe Commerce과 함께 Git 기반 호스팅 서비스를 사용하는 방법에 대한 자세한 내용은 [통합](../integrations/overview.md)을 참조하십시오.

## 개발을 위한 분기 만들기

프로젝트를 복제하고 Adobe Commerce 관리자 계정 구성을 업데이트하면 개발을 위해 분기할 수 있습니다. 앞에서 설명한 대로 환경이 _활성_&#x200B;이 되도록 하려면 `magento-cloud environment:branch <branch-name>` 명령 또는 [!DNL Cloud Console]을(를) 사용하여 환경을 만들어야 합니다.

- [Starter](../architecture/starter-develop-deploy-workflow.md#clone-and-branch)의 경우 `staging`에 대한 분기를 만든 다음 `staging` 분기를 기반으로 개발 분기를 만드십시오.
- [Pro](../architecture/pro-develop-deploy-workflow.md#development-workflow)의 경우 `Integration` 분기를 기반으로 개발 분기를 만드십시오.

**개발 분기를 만들려면**:

1. 로컬 워크스테이션에서 프로젝트 디렉터리로 변경합니다.

1. 프로젝트 워크플로에 권장되는 분기를 기반으로 환경을 만듭니다.

   ```bash
   magento-cloud branch <new-environment-name> integration
   ```

1. 종속성을 업데이트합니다.

   ```bash
   composer --no-ansi --no-interaction install --no-progress --prefer-dist --optimize-autoloader
   ```

1. [_선택 사항_] 환경의 [백업](../storage/snapshots.md)을 만듭니다.

### 분기 병합

개발을 완료한 후 이 분기를 상위에 병합합니다.

1. 코드 변경 내용 커밋 및 푸시:

   ```bash
   git add -A && git commit -m "Add message here"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. 상위 환경과 병합:

   ```bash
   magento-cloud environment:merge <environment-ID>
   ```

### 환경 삭제

더 이상 필요하지 않은 환경만 삭제합니다. 환경을 삭제한 후에는 환경을 복구할 수 없습니다.

>[!WARNING]
>
>프로젝트의 `master` 분기를 삭제할 수 없습니다.

이 작업을 수행하려면 프로젝트 관리자, 환경 관리자 또는 계정 소유자여야 합니다. [클라우드 프로젝트에 대한 사용자 액세스 관리](../project/user-access.md)를 참조하십시오.

환경을 삭제하면 환경이 _비활성_(으)로 설정됩니다. 코드는 Git 분기에서 계속 사용할 수 있지만 더 이상 서비스나 데이터베이스를 포함하지 않습니다. 환경을 완전히 삭제하려면 해당 원격 Git 분기도 삭제해야 합니다.

**환경을 삭제하려면**:

1. 로컬 워크스테이션에서 프로젝트 디렉터리로 변경합니다.

1. 원격 서버에서 업데이트를 가져옵니다.

   ```bash
   git fetch
   ```

1. 환경 분기를 삭제합니다.

   ```bash
   magento-cloud environment:delete <environment-ID>
   ```

   선택적으로, 여러 환경 ID를 delete 명령에 추가하여 한 번에 두 개 이상의 환경을 삭제할 수 있습니다.

   ```bash
   magento-cloud environment:delete <environment-1-ID> <environment-2-ID>
   ```

1. 프롬프트에 응답하여 로컬 환경과 해당 원격 환경을 삭제합니다.

   ```
   The environment <environment-ID> is currently active: deleting it will delete all associated data.
   Are you sure you want to delete the environment <environment-ID>? [Y/n]
   ```

   환경을 삭제하면 _비활성_ 상태가 됩니다.

   ```
   Delete the remote Git branch too? [Y/n]
   ```

   원격 Git 분기를 삭제하면 프로젝트에서 환경이 제거됩니다.

1. 환경이 삭제될 때까지 기다립니다.

   ```
   Deleting environment <environment-ID>
   Waiting for the activity...
     Deleting environment <project-id>-<environment-ID>-xxxxxx
   
     [============================]  1 min (complete)
   Activity ID succeeded
   Deleted remote Git branch <environment-ID>
   Run git fetch --prune to remove deleted branches from your local cache.
   ```

>[!TIP]
>
>비활성 환경을 활성화하려면 `magento-cloud environment:activate` 명령을 사용하십시오.

## 원격 환경과 상호 작용

[SSH 키를 설정](../development/secure-connections.md)한 후 [로컬 작업 공간에서 원격 환경에 연결](../development/secure-connections.md#connect-to-a-remote-environment)하고 프로젝트 서비스와 상호 작용하고 설정을 수정할 수 있습니다.
