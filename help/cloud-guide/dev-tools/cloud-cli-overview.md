---
title: Cloud CLI
description: magento-cloud CLI를 통해 Adobe Commerce on cloud infrastructure 프로젝트의 로컬 개발 환경을 관리하는 방법을 알아봅니다.
exl-id: 70dddd62-0269-4af4-bd2a-1a4fbf11a131
source-git-commit: 13e76d3e9829155995acbb72d947be3041579298
workflow-type: tm+mt
source-wordcount: '741'
ht-degree: 0%

---


# Cloud CLI

다음 `magento-cloud` 개발자와 시스템 관리자는 CLI 툴을 사용하여 클라우드 프로젝트 및 환경을 관리하고 루틴을 수행하며 자동화 작업을 수행할 수 있습니다. 다음 `magento-cloud` CLI는 다음과 같은 기능과 특징을 확장합니다. [[!DNL Cloud Console]](../../get-started/cloud-console.md). 를 설치한 후 `magento-cloud` 로컬 워크스테이션에서 CLI를 사용하여 클라우드 인프라 Starter 및 Pro 통합 환경에서 Adobe Commerce을 관리할 수 있습니다.

**을(를) 설치하려면 `magento-cloud` CLI**:

1. 로컬 워크스테이션에서 클라우드 프로젝트를 복제하려는 디렉터리와 [파일 시스템 소유자](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html) 이(가) _쓰기_ 액세스 권한.

1. 설치 `magento-cloud` CLI.

   ```bash
   curl -sS https://accounts.magento.cloud/cli/installer | php
   ```

1. 추가 `magento-cloud` Bash 프로필에 대한 CLI.

   ```bash
   export PATH=$PATH:$HOME/.magento-cloud/bin
   ```

1. 업데이트된 bash 프로필을 다시 로드합니다.

   ```bash
   . ~/.bash_profile
   ```

1. CLI를 시작하려면 `magento-cloud` 메시지가 표시되면 Cloud 계정 자격 증명을 입력합니다.

   ```bash
   magento-cloud
   ```

   ```terminal
   Welcome to Magento Cloud!
   Please log in using your Magento Cloud account.
   Your email address or username:
   ```

1. 확인 `magento-cloud` 명령이 경로에 있습니다. 다음 예제에서는 사용 가능한 명령을 나열합니다.

   ```bash
   magento-cloud list
   ```

## 일반 명령

Adobe은 Cloud 통합 환경을 관리하도록 이러한 명령을 설계했으며 `magento-cloud` 를 생략할 수 있도록 프로젝트 디렉토리에서 CLI `-p <project-ID>` 매개 변수.

일반적으로 사용되는 다음 목록 `magento-cloud` CLI 명령에는 필수 옵션만 포함됩니다. 다음을 사용할 수 있습니다. `--help` 추가 정보를 보려면 모든 명령과 함께 옵션을 선택합니다.

| 명령 | 설명 |
| ------------------------------------ | -------------------------------------------------- |
| `magento-cloud login` | 프로젝트에 로그인. |
| `magento-cloud list` | CLI 도구에 사용할 수 있는 명령을 나열합니다. |
| `magento-cloud environment:list` | 현재 프로젝트의 환경을 나열합니다. |
| `magento-cloud environment:checkout` | 기존 환경을 확인합니다. |
| `magento-cloud environment:merge -e` | 이 환경의 변경 내용을 상위 항목과 병합합니다. |
| `magento-cloud variables` | 이 환경의 목록 변수입니다. |
| `magento-cloud ssh` | SSH를 사용하여 원격 환경에 연결합니다. |
| `magento-cloud url` | 브라우저에서 Adobe Commerce 상점을 엽니다. |
| `magento-cloud web` | 를 엽니다. [!DNL Cloud Console]. |

## 환경 명령

환경 _이름_ 환경과 다름 _ID_ 환경 이름에 공백이나 대문자를 사용하는 경우에만 해당됩니다. 환경 ID는 모든 소문자, 숫자 및 허용되는 기호로 구성됩니다. 환경 이름의 대문자는 ID에서 소문자로 변환되고 환경 이름의 공백은 대시로 변환됩니다.

환경 이름 _할 수 없음_ Linux 셸 또는 정규 표현식에 예약된 문자를 포함합니다. 금지된 문자에는 중괄호(`{ }`), 괄호, 별표(`*`), 꺾쇠 괄호(`< >`), 앰퍼샌드(`&`),% (`%`) 및 기타 문자를 포함합니다.

다음 `magento-cloud environment:list` 명령은 환경 계층을 표시하지만 `git branch` 그렇지 않습니다. 중첩된 환경이 있는 경우 다음을 사용합니다.

```bash
magento-cloud environment:list
```

### 환경 재배포

푸시를 사용하지 않고 재배포를 트리거합니다. 재배포할 환경을 확인하고 확인합니다. 보류 중인 상태의 빌드가 있는 경우 재배포를 사용하지 마십시오.

```bash
magento-cloud environment:redeploy
```

샘플 응답:

```terminal
Are you sure you want to redeploy the environment <environment-name>? [Y/n]
```

{{redeploy-warning}}

## Git 명령

이러한 명령 중 일부는 Git 명령과 유사합니다. 다음 `magento-cloud` 명령은 추가 기능을 사용하여 Git 기반 클라우드 프로젝트에 직접 연결합니다. 를 사용하지 않고 분기를 만드는 경우 `magento-cloud` CLI는 &quot;활성화&quot;되지 않으며 변경 사항을 원격 환경에 푸시할 때 자동으로 빌드되지 않습니다. 다음 `magento-cloud` CLI 명령에 활성화가 포함됩니다.

분기를 만들려면 `magento-cloud` 명령을 실행하여 분기가 활성화되도록 합니다.

```bash
magento-cloud environment:branch <new-name> <parent-branch>
```

분기 상태의 경우:

- 사용 `magento-cloud env` 환경 분기의 목록과 해당 상태를 보기 위한 명령: 활성 또는 비활성.
- 사용 `magento-cloud environment:activate` 환경 분기를 활성화하는 명령입니다.

빈 Git 커밋을 푸시하여 배포를 트리거합니다. For example:

```bash
git commit --allow-empty -m "redeploy" && git push <branch-name>
```

사용자 추가와 같은 일부 작업은 배포로 이어지지 않습니다.

### 환경 분기 만들기

다음 단계에서는 CLI 및 Git 명령을 상호 교환하여 로컬 환경을 관리하는 방법을 보여 줍니다.

1. 로컬 워크스테이션에서 프로젝트 디렉터리로 변경합니다.

1. 다음으로 전환 [파일 시스템 소유자](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html).

1. 프로젝트에 로그인.

   ```bash
   magento-cloud login
   ```

1. 프로젝트를 나열합니다.

   ```bash
   magento-cloud project:list
   ```

1. 프로젝트의 환경을 나열합니다. 모든 환경에는 코드, 데이터베이스, 환경 변수, 구성 및 서비스를 포함하는 활성 Git 분기가 포함되어 있습니다.

   ```bash
   magento-cloud environment:list
   ```

   >[!NOTE]
   >
   >를 사용하는 것이 중요합니다. `magento-cloud environment:list` 명령은 환경 계층을 표시하지만 `git branch` 명령이 실행되지 않습니다.

1. 원본 분기를 가져와 최신 코드를 가져옵니다.

   ```bash
   git fetch origin
   ```

1. 특정 분기 및 환경을 체크아웃하거나 로 전환합니다.

   ```bash
   magento-cloud environment:checkout <environment-ID>
   ```

   Git 명령은 Git 분기만 체크아웃합니다. 다음 `magento-cloud checkout` 명령은 분기를 체크아웃하고 활성 환경으로 전환합니다.

   >[!TIP]
   >
   >를 사용하여 환경 분기를 만들 수 있습니다. `magento-cloud environment:branch <environment-name> <parent-environment-ID>` 명령 구문. 환경 분기를 만들고 활성화하는 데 약간의 추가 시간이 걸릴 수 있습니다.

1. 환경 ID를 사용하여 업데이트된 코드를 로컬로 가져옵니다. 환경 분기가 새로운 경우에는 필요하지 않습니다.

   ```bash
   git pull origin <environment-ID>
   ```

1. (_선택 사항_) 만들기 [스냅샷](../storage/snapshots.md) 을 백업으로 사용.

   ```bash
   magento-cloud snapshot:create -e <environment-ID>
   ```

## CLI 업데이트

다음 `magento-cloud` CLI는 로그인할 때 사용 가능한 업데이트를 확인하지만 `self:update` 명령입니다. 업데이트를 사용할 수 있는 경우 지침에 따라 CLI를 업데이트합니다.

다음의 경우 `magento-cloud` CLI가 최신 상태이면 다음 응답이 표시됩니다.

```bash
magento-cloud update
```

```terminal
Checking for Magento Cloud CLI updates (current version: X.XX.X)
No updates found
```
