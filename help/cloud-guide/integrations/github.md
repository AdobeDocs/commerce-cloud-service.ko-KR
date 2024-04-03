---
title: GitHub 통합
description: Adobe Commerce on cloud infrastructure 프로젝트를 GitHub와 통합하는 방법을 알아봅니다.
feature: Cloud, Integration
last-substantial-update: 2023-05-25T00:00:00Z
exl-id: 5305452f-4c8d-438c-ac78-e2e1ec2f8cd9
source-git-commit: 4d32fc596064f01eaefe3ee509a655837fa846de
workflow-type: tm+mt
source-wordcount: '924'
ht-degree: 0%

---

# GitHub 통합

GitHub 통합을 사용하면 GitHub 저장소에서 직접 클라우드 인프라 환경의 Adobe Commerce을 관리할 수 있습니다. 통합은 GitHub에 이미 있는 콘텐츠를 관리하고 클라우드 인프라 코드 저장소의 Adobe Commerce과 동기화합니다. 본질적으로 코드 리포지토리는 GitHub 리포지토리의 미러가 됩니다.

{{private-repository}}

이 통합을 통해 다음과 같은 작업을 수행할 수 있습니다.

- 분기 생성 시 환경 생성
- 끌어오기 요청을 병합할 때 환경 재배포
- 분기 삭제 시 환경 삭제

프로세스를 계속하려면 GitHub 토큰과 웹후크를 가져와야 합니다.

## 전제 조건

- Adobe Commerce on cloud infrastructure 프로젝트에 대한 관리자 액세스
- GitHub 저장소
- GitHub 개인 액세스 토큰

## GitHub 토큰 생성

GitHub 개발자 설정에서 클래식 개인 액세스 토큰을 만듭니다. 다음과 같은 작업을 수행할 수 있도록 GitHub 저장소에 대한 쓰기 권한이 있는 그룹의 구성원이어야 합니다 _푸시_ 리포지토리에. 토큰을 만들 때 다음 범위를 포함하십시오.

- `admin:repo_hook`- 웹 후크 만들기
- `repo`- 저장소와 통합
- `read:org`—조직 저장소와 통합

다음을 참조하십시오 [GitHub: 만들기](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token).

## 저장소 준비

기존 환경에서 Adobe Commerce on cloud infrastructure 프로젝트를 복제하고 프로젝트 분기를 새로운 빈 GitHub 저장소로 마이그레이션하여 동일한 분기 이름을 유지합니다. 다음과 같습니다. **중요** Adobe Commerce on cloud infrastructure 프로젝트에서 기존 환경 또는 분기를 손실하지 않도록 동일한 Git 트리를 유지합니다.

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
   magento-cloud project:get <project-ID>
   ```

1. GitHub 저장소를 원격으로 추가합니다.

   ```bash
   git remote add origin git@github.com:<user-name>/<repo-name>.git
   ```

   원격 연결의 기본 이름은 다음과 같습니다. `origin` 또는 `magento`. If `origin` 존재하는 경우 다른 이름을 선택하거나 기존 참조의 이름을 바꾸거나 삭제할 수 있습니다. 다음을 참조하십시오 [git-remote 설명서](https://git-scm.com/docs/git-remote).

1. GitHub 원격을 올바르게 추가했는지 확인합니다.

   ```bash
   git remote -v
   ```

   예상 응답:

   ```terminal
   origin git@github.com:<user-name>/<repo-name>.git (fetch)
   origin git@github.com:<user-name>/<repo-name>.git (push)
   ```

1. 프로젝트 파일을 새 GitHub 리포지토리에 푸시합니다. 모든 분기 이름을 동일하게 유지하는 것을 잊지 마십시오.

   ```bash
   git push -u origin master
   ```

   새 GitHub 저장소로 시작하는 경우 `-f` 원격 저장소가 로컬 복사본과 일치하지 않기 때문입니다.

1. GitHub 저장소에 모든 프로젝트 파일이 포함되어 있는지 확인합니다.

## GitHub 통합 활성화

시작하기 전에 프로젝트 코드와 환경은 GitHub 저장소에 있어야 합니다. 통합을 활성화하면 GitHub 저장소가 코드 소스가 됩니다. 코드 변경 내용을 원본에 푸시하는 경우 `magento` 저장소는 코드 변경 사항을 GitHub 저장소에 푸시할 때 통합에 의해 덮어쓰여집니다.

다음은 GitHub 통합을 활성화하고 웹후크를 만들 때 사용할 페이로드 URL을 제공합니다.

>[!WARNING]
>
>다음 명령을 덮어씁니다. _모두_ 를 포함하여 모든 분기를 포함하는 GitHub 저장소의 코드와 함께 Adobe Commerce on cloud infrastructure 프로젝트의 코드 `production` 분기입니다. 이 작업은 즉시 수행되며 실행 취소할 수 없습니다. 가장 좋은 방법은 Adobe Commerce on cloud infrastructure 프로젝트에서 모든 분기를 복제하여 GitHub 저장소로 푸시하는 것입니다 **다음 이전** GitHub 통합 추가.

다음을 사용하여 CLI 프롬프트의 단계를 선택할 수 있습니다. `magento-cloud integration:add` 또는 다음 옵션을 사용하여 통합 명령을 빌드할 수 있습니다.

| 옵션 | 필수? | 설명 |
| ----------------------- | --------- | --------------------------------- |
| `--base-url` | 예 | 서버 설치의 기본 URL입니다. `https://github.com/` 또는 사용자 지정 저장소가 공용 Github로 호스팅되는 경우 이 옵션을 생략합니다. |
| `--token` | 예 | GitHub에 대해 생성한 개인 액세스 토큰 |
| `--repository` | 예 | 저장소 이름: `owner-or-organisation/repository` |
| `--build-pull-requests` | 선택 사항 | 끌어오기 요청을 병합한 후 배포하도록 클라우드 인프라의 Adobe Commerce에 지시합니다(`true` (기본적으로) |
| `--fetch-branches` | 선택 사항 | 클라우드 인프라의 Adobe Commerce이 분기를 업데이트한 후 분기를 추적하고 배포하도록 함(`true` (기본적으로) |
| `--prune-branches` | 선택 사항 | 원격에 존재하지 않는 분기를 삭제합니다(`true` (기본적으로) |

많은 옵션이 있으며 도움말 옵션을 사용하여 볼 수 있습니다.

```bash
magento-cloud integration:add --help
```

**GitHub 통합을 활성화하려면**:

1. 통합을 활성화합니다.

   ```bash
   magento-cloud integration:add --type=github --project=<project-ID> --token=<your-GitHub-token> {--repository=USER/REPOSITORY | --repository=ORGANIZATION/REPOSITORY} [--build-pull-requests={true|false} --fetch-branches={true|false}
   ```

   **예 1**: 개인 개인 저장소에 대해 GitHub 통합을 활성화합니다.

   ```bash
   magento-cloud integration:add --type=github --project=ov58dlacU2e --base-url=https://github.com --token=<token> --repository=myUserName/myrepo
   ```

   **예제 2**: 조직 저장소에 대해 GitHub 통합을 활성화합니다.

   ```bash
   magento-cloud integration:add --type=github --project=ov58dlacU2e --base-url=https://github.com --token=<token> --repository=Magento/teamrepo
   ```

1. 메시지가 표시되면 필요한 정보를 입력합니다.

1. 다음을 복사합니다. **페이로드 URL** 반환 출력으로 표시됩니다.

   ```terminal
   Created integration <integration-ID> (type: github)
   Repository: myUserName/myrepo
   Build PRs: yes
   Fetch branches: yes
   Payload URL: https://us.magento.cloud/api/projects/<project-id>/integrations/wO8a0eoamxwcg/hook
   ```

## GitHub에서 Webhook 추가

Cloud Git 서버와 푸시 등의 이벤트를 전달하려면 GitHub 저장소에 대한 웹후크를 만들어야 합니다.

1. GitHub 저장소에서 **설정** 탭.

1. 왼쪽 탐색 모음에서 를 클릭합니다. **웹훅**.

1. 다음에서 _웹훅_ 창, 클릭 **Webhook 추가**.

1. 다음에서 _Webhooks/Webhook 추가_ 양식에서 다음 필드를 편집합니다.

   - **페이로드 URL**: GitHub 통합을 활성화했을 때 반환되는 URL을 입력합니다.
   - **컨텐츠 유형**: 선택 **application/json** 목록에서 삭제할 수 있습니다.
   - **암호**: 확인 암호를 입력합니다.
   - **이 웹후크를 트리거할 이벤트를 선택하십시오.**: 선택 **모두 보내기**.
   - 다음 항목 선택 **활성** 확인란.

1. 클릭 **Webhook 추가**.

## 통합 테스트

GitHub 통합을 구성한 후 를 사용하여 통합이 작동하는지 확인할 수 있습니다. `magento-cloud` CLI:

```bash
magento-cloud integration:validate
```

또는 간단한 변경 사항을 GitHub 저장소에 푸시하여 테스트할 수 있습니다.

1. 테스트 파일을 만듭니다.

   ```bash
   touch test.md
   ```

1. 변경 사항을 커밋하고 GitHub 저장소에 푸시합니다.

   ```bash
   git add . && git commit -m "Testing GitHub integration" && git push
   ```

1. 에 로그인합니다 [[!DNL Cloud Console]](../project/overview.md) 커밋 메시지가 표시되고 프로젝트가 배포되는지 확인합니다.

## 통합 제거

코드에 영향을 주지 않고 프로젝트에서 GitHub 통합을 안전하게 제거할 수 있습니다.

**GitHub 통합을 제거하려면**:

1. 터미널에서 Adobe Commerce on cloud infrastructure 프로젝트에 로그인합니다.

1. 통합을 나열합니다. 다음 단계를 완료하려면 GitHub 통합 ID가 필요합니다.

   ```bash
   magento-cloud integration:list
   ```

1. 통합을 삭제합니다.

   ```bash
   magento-cloud integration:delete <int-ID>
   ```

또한 GitHub 계정에 로그인하고 의 웹 후크를 제거하여 GitHub 통합을 제거할 수 있습니다. _웹훅_ 저장소의 탭 _설정_.
