---
title: GitLab 통합
description: Adobe Commerce on cloud infrastructure 프로젝트를 GitLab과 통합하는 방법을 알아봅니다.
feature: Cloud, Integration
exl-id: 37fda8a0-7274-422f-9049-243f2e409f26
source-git-commit: 13e76d3e9829155995acbb72d947be3041579298
workflow-type: tm+mt
source-wordcount: '777'
ht-degree: 0%

---

# GitLab 통합

코드 변경 사항을 푸시할 때 환경을 자동으로 빌드하고 배포하도록 GitLab 저장소를 구성할 수 있습니다. 이 통합은 GitLab 저장소를 클라우드 인프라 계정의 Adobe Commerce과 동기화합니다.

{{private-repository}}

이 통합을 통해 다음과 같은 작업을 수행할 수 있습니다.

- 분기 생성 시 환경 생성
- 끌어오기 요청을 병합할 때 환경 재배포
- 분기 삭제 시 환경 삭제

프로세스를 계속하려면 GitLab 토큰과 웹후크를 가져와야 합니다.

## 전제 조건

- Adobe Commerce on cloud infrastructure 프로젝트에 대한 관리자 액세스
- [`magento-cloud` CLI](../dev-tools/cloud-cli-overview.md) 로컬 환경의 도구
- GitLab 계정
- GitLab 저장소에 대한 쓰기 권한이 있는 GitLab 개인 액세스 토큰입니다. 선택한 범위는 최소 다음 이상이어야 합니다. `api` 및 `read_repository`.

## 저장소 준비

기존 환경에서 Adobe Commerce on cloud infrastructure 프로젝트를 복제하고 프로젝트 분기를 새로운 빈 GitLab 저장소로 마이그레이션하여 동일한 분기 이름을 유지합니다. 다음과 같습니다. **중요** Adobe Commerce on cloud infrastructure 프로젝트에서 기존 환경 또는 분기를 손실하지 않도록 동일한 Git 트리를 유지합니다.

1. 터미널에서 Adobe Commerce on cloud infrastructure 프로젝트에 로그인합니다.

   ```bash
   magento-cloud login
   ```

1. 프로젝트 나열 및 프로젝트 ID 복사

   ```bash
   magento-cloud project:list
   ```

1. 프로젝트를 로컬 환경에 복제합니다.

   ```bash
   magento-cloud project:get <project-id>
   ```

1. GitLab 저장소를 원격으로 추가합니다(GitLab이 해당 SaaS 버전에서 사용된다고 가정).

   ```bash
   git remote add origin git@gitlab.com:<user-name>/<repo-name>.git
   ```

   원격 연결의 기본 이름은 다음과 같습니다. `origin` 또는 `magento`. If `origin` 존재하는 경우 다른 이름을 선택하거나 기존 참조의 이름을 바꾸거나 삭제할 수 있습니다. 다음을 참조하십시오 [git-remote 설명서](https://git-scm.com/docs/git-remote).

1. GitLab 리모콘을 올바르게 추가했는지 확인합니다.

   ```bash
   git remote -v
   ```

   예상 응답:

   ```terminal
   origin git@gitlab.com:<user-name>/<repo-name>.git (fetch)
   origin git@gitlab.com:<user-name>/<repo-name>.git (push)
   ```

1. 프로젝트 파일을 새 GitLab 리포지토리에 푸시합니다. 모든 분기 이름을 동일하게 유지하는 것을 잊지 마십시오.

   ```bash
   git push -u origin master
   ```

   새 GitLab 저장소로 시작하는 경우 `-f` 원격 저장소가 로컬 복사본과 일치하지 않기 때문입니다.

1. GitLab 저장소에 모든 프로젝트 파일이 포함되어 있는지 확인합니다.

## GitLab 통합 활성화

사용 `magento-cloud integration` 명령을 사용하여 GitLab 통합을 활성화하고 GitLab 웹후크의 페이로드 URL을 가져와서 GitLab에서 클라우드 인프라 프로젝트의 Adobe Commerce으로 업데이트를 전송합니다.

```bash
magento-cloud integration:add --type=gitlab --project=<project-ID> --token=<your-GitLab-token> [--base-url=<GitLab-url> --server-project=<GitLab-project> --build-merge-requests={true|false} --merge-requests-clone-parent-data={true|false} --fetch-branches={true|false} --prune-branches={true|false}]
```

| 옵션 | 설명 |
| ------ | ----------- |
| `<project-ID>` | Adobe Commerce on cloud infrastructure 프로젝트 ID |
| `<your-GitLab-token>` | GitLab에 대해 생성한 개인 액세스 토큰 |
| `--base-url` | GitLab의 URL(`https://gitlab.com/` gitLab을 SaaS 버전에서 사용하는 경우) |
| `--server-project` | GitLab의 프로젝트 이름(기본 URL 뒤 부분) |
| `--build-merge-requests` | An _선택 사항_ 클라우드 인프라의 Adobe Commerce이 모든 병합 요청에 대해 새 환경을 구축하도록 지시하는 매개 변수(`true` (기본적으로) |
| `--merge-requests-clone-parent-data` | An _선택 사항_ 클라우드 인프라의 Adobe Commerce이 병합 요청에 대해 상위 환경의 데이터를 복제하도록 지시하는 매개 변수(`true` (기본적으로) |
| `--fetch-branches` | An _선택 사항_ 클라우드 인프라의 Adobe Commerce이 원격에서(비활성 환경으로) 모든 분기를 가져오도록 하는 매개 변수(`true` (기본적으로) |
| `--prune-branches` | An _선택 사항_ 클라우드 인프라의 Adobe Commerce이 원격에 존재하지 않는 분기를 삭제하도록 지시하는 매개 변수(`true` (기본적으로) |

>[!WARNING]
>
>다음 `magento-cloud integration` 명령 덮어쓰기 _모두_ gitLab 저장소의 코드와 함께 Adobe Commerce on cloud infrastructure 프로젝트의 코드. 여기에는 다음을 포함한 모든 분기가 포함됩니다. `production` 분기입니다. 이 작업은 즉시 수행되며 실행 취소할 수 없습니다. GitLab 통합을 추가하기 전에 Adobe Commerce on cloud infrastructure 프로젝트에서 모든 분기를 복제하고 GitLab 저장소로 푸시하는 것이 가장 중요합니다.

**GitLab 통합을 활성화하려면**:

1. 터미널에서 Adobe Commerce on cloud infrastructure 프로젝트에 GitLab 통합을 추가합니다.

   ```bash
   magento-cloud integration:add --type gitlab --project=3txxjf32gtryos --token=qVUfeEn4ouze7A7JH --base-url=https://gitlab.com/ --server-project=my-agency/project-name --build-merge-requests=false --merge-requests-clone-parent-data=false --fetch-branches=true --prune-branches=true
   ```

1. 메시지가 표시되면 을 입력합니다. `y` 통합을 추가합니다.

   ```terminal
   Warning: adding a 'gitlab' integration will automatically synchronize code from the external Git repository.
   This means it can overwrite all the code in your project.
   Are you sure you want to continue? [y/N] y
   ```

1. 다음을 복사합니다. **후크 URL** 반환 출력으로 표시됩니다.

   ```terminal
   Hook URL: https://eu-3.magento.cloud/api/projects/3txxjf32gtryos/integrations/eolmpfizzg9lu/hook
   Created integration eolmpfizzg9lu (type: gitlab)
   +----------------------------------+---------------------------------------------------------------------------------------+
   | Property                         | Value                                                                                 |
   +----------------------------------+---------------------------------------------------------------------------------------+
   | id                               | <integration-id>                                                                      |
   | type                             | gitlab                                                                                |
   | token                            | ******                                                                                |
   | base_url                         | https://gitlab.com/                                                                   |
   | project                          | my-agency/project-name                                                                |
   | fetch_branches                   | true                                                                                  |
   | prune_branches                   | true                                                                                  |
   | build_merge_requests             | false                                                                                 |
   | merge_requests_clone_parent_data | false                                                                                 |
   | hook_url                         | https://eu-3.magento.cloud/api/projects/<project-id>/integrations/<integration-id>/hook |
   +----------------------------------+---------------------------------------------------------------------------------------+
   ```

### GitLab에 webhook 추가

푸시 또는 병합 요청과 같은 이벤트를 Cloud Git 서버와 전달하려면 다음을 수행해야 합니다 [webhook 만들기](https://docs.gitlab.com/ee/user/project/integrations/webhooks.html#overview) GitLab 저장소용

1. GitLab 저장소에서 **설정** 탭.

1. 왼쪽 탐색 모음에서 를 클릭합니다. **웹훅**.

1. 다음에서 _웹훅_ 양식에서 다음 필드를 편집합니다.

   - **URL**: 다음을 입력합니다. `Hook URL` GitLab 통합을 활성화하면 가 반환됩니다.
   - **비밀 토큰**: 필요한 경우 확인 암호를 입력합니다.
   - **트리거**: 확인 `Merge request events` 및/또는 `Push events` 필요에 따라.
   - **SSL 인증 활성화**: 이 옵션을 선택해야 합니다.

1. 클릭 **Webhook 추가**.

### 통합 테스트

GitLab 통합을 구성한 후 다음을 사용하여 통합이 작동하는지 확인할 수 있습니다. `magento-cloud` CLI:

```bash
magento-cloud integration:validate
```

또는 간단한 변경 사항을 GitLab 저장소에 푸시하여 테스트할 수 있습니다.

1. 테스트 파일을 만듭니다.

   ```bash
   touch test.md
   ```

1. 변경 사항을 커밋하고 GitLab 저장소에 푸시합니다.

   ```bash
   git add . && git commit -m "Testing GitLab integration" && git push
   ```

1. 에 로그인합니다 [[!DNL Cloud Console]](../project/overview.md) 커밋 메시지가 표시되고 프로젝트가 배포되는지 확인합니다.

## 클라우드 분기 만들기

사용 `magento-cloud` CLI `environment:push` 새 환경을 만들고 활성화하는 명령 다음을 참조하십시오 [클라우드 분기 만들기](bitbucket.md#create-a-cloud-branch).

## 통합 제거

사용 `magento-cloud` CLI `integration:delete` 통합을 제거하는 명령입니다. 다음을 참조하십시오 [통합 제거](bitbucket.md#remove-the-integration).
