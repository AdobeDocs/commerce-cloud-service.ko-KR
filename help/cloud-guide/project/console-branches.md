---
title: "을(를) 사용하여 분기 관리 [!DNL Cloud Console]"
description: 다음을 사용하여 클라우드 인프라에서 Adobe Commerce의 환경 분기를 관리하는 방법을 알아봅니다. [!DNL Cloud Console].
role: Developer
feature: Cloud, Install
exl-id: 2c4ef149-fdb9-473f-91fd-5e6421ac5a43
source-git-commit: a5af3cc6e174a731a8f2ff198acd794384ef4585
workflow-type: tm+mt
source-wordcount: '1589'
ht-degree: 0%

---

# 을(를) 사용하여 분기 관리 [!DNL Cloud Console]

다음 중 하나를 사용하여 환경을 관리할 수 있습니다. [!DNL Cloud Console] 또는 `magento-cloud` CLI. 프로젝트 파일은 Git 저장소에 저장됩니다. Git 명령을 사용하여 코드를 관리할 수 있지만 `magento-cloud` CLI는 플랫폼 기능과 상호 작용하도록 설계된 반면 Git 명령은 상호 작용하지 않습니다. 다음을 참조하십시오 [Git 명령](../dev-tools/cloud-cli-overview.md#git-commands) ( cloud CLI 항목 참조)

이 항목에서는 [!DNL Cloud Console] 끝:

- 환경 추가 또는 삭제
- 동기화 (`git pull`상위 환경의 )
- 병합(`git push`) 상위 환경으로

>[!TIP]
>
>Pro 스테이징 및 프로덕션 환경에서는 분기를 만들 수 없습니다. 에서 분기할 수 있습니다. `master` 분기입니다.

## 환경 만들기

분기 전략은 개발 분기에 코드를 개발하고 확장을 추가하는 일반적인 Git 워크플로를 사용합니다. 다음을 참조하십시오 [스타터](../architecture/starter-architecture.md) 및 [Pro](../architecture/starter-develop-deploy-workflow.md) 아키텍처 개요.

- Starter의 경우 `staging` 에서 분기 `master` 분기, 다음에서 분기 `staging` 개발용.
- Pro의 경우 `Integration` 환경.

귀하의 계정은 제한된 수의 ![활성 분기](../../assets/icon-active.png){width="32"} (active) and an unlimited number of ![inactive branch](../../assets/icon-inactive.png){width="32"} (비활성) 개발 분기. 만 사용하여 분기를 추가하거나 삭제하여 활성 및 비활성 분기를 관리합니다. [!DNL Cloud Console] 또는 Cloud CLI를 사용할 수 있습니다. 분기를 삭제하려면 먼저 의 저장소에 있는 분기를 비활성화합니다 _환경_ 다음으로 나열 _비활성_. 나중에 분기를 다시 활성화하거나 [분기 삭제](../dev-tools/cloud-cli-overview.md#) 환경 설정 또는 Cloud CLI 사용

개발을 위해 추가 활성 환경이 필요한 경우 [지원 티켓](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket).

**분기를 추가하려면**:

1. 에 로그인합니다 [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. 다음에서 프로젝트 선택 _모든 프로젝트_ 목록을 표시합니다.

1. 환경을 선택합니다.

   >[!TIP]
   >
   >새 분기가 이 환경에서 복제됩니다. 만들려는 환경과 유사한 상위 환경을 선택합니다.

1. 클릭 **[!UICONTROL Branch]**.

   ![분기 만들기](../../assets/button-branch.png){width="150"}

1. 다음에서 _에서 분기 중..._ 양식에서 분기 이름을 입력합니다.

   환경 _이름_ 환경과 다름 _ID_ 환경 이름에 공백이나 대문자를 사용하는 경우에만 해당됩니다. 환경 ID는 모든 소문자, 숫자 및 허용되는 기호로 구성됩니다. 환경 이름의 대문자는 ID에서 소문자로 변환되고 환경 이름의 공백은 대시로 변환됩니다.

   환경 이름 **할 수 없음** Linux 셸 또는 정규 표현식에 예약된 문자를 포함합니다. 금지된 문자에는 중괄호(`{ }`), 괄호, 별표(`*`), 꺾쇠 괄호(`>`), 앰퍼샌드(`&`),% (<code>%</code>) 및 기타 문자를 포함합니다.

1. 선택 **[!UICONTROL Environment type]**.

1. 클릭 **[!UICONTROL Create Branch]**.

1. 환경이 배포되는 동안 잠시 기다려 주십시오.

   배포 중 환경 상태는 다음과 같습니다.  **진행 중**. 배포가 성공하면 상태가 다음에 대한 녹색 확인 표시로 변경됩니다. **성공**.

## 비활성 분기 만들기

Adobe Commerce Cloud 콘솔 또는 CLI에서는 비활성 분기를 만들 수 없습니다. 비활성 분기를 만들려면 Git 저장소에서 만들고 를 사용하여 푸시합니다 `environment.Parent` 옵션입니다.

```bash
git push -o "environment.Parent=<parent branch>" <origin> <branch>
```

## 환경 삭제

환경을 삭제하려면 먼저 환경을 비활성화해야 합니다. 환경이 비활성화되면 삭제할 수 있습니다.

**환경을 비활성화하려면**:

1. 에 로그인합니다 [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. 다음에서 프로젝트 선택 _모든 프로젝트_ 목록을 표시합니다.

1. 탐색 모음에서 환경 선택 _환경_ 목록을 표시합니다.

1. 상단 탐색 막대의 오른쪽에 있는 구성 아이콘을 클릭하면 환경 설정이 열립니다.

1. 다음에서 _[!UICONTROL General]_탭, 아래로 스크롤하여_[!UICONTROL Deactivate environment]_ 섹션 및 클릭 **[!UICONTROL Deactivate environment and delete data]** 지침을 따르십시오.

## 환경 동기화

환경(또는 분기) 동기화는 와 동일합니다. `git pull origin <parent>`. 상위 환경에서 업데이트된 코드를 동기화할 수 있습니다. 다음을 통해 이 기능을 사용할 수 있습니다. [!DNL Cloud Console] 모든 Starter 및 Pro 환경에 적합합니다.

Pro 플랜의 경우 스테이징 및 프로덕션에서 와(과) 동기화할 수 있습니다. `master` 분기입니다. 이 동기화는 데이터를 가져오지 않고 코드만 가져오고 푸시합니다. 데이터를 동기화하려면 데이터베이스 데이터를 덤프하고 다른 환경의 데이터베이스로 푸시합니다. 다음을 참조하십시오 [정적 파일 및 데이터 마이그레이션 및 배포](/help/cloud-guide/deploy/staging-production.md#migrate-static-files).

**환경을 동기화하려면**:

1. 에 로그인합니다 [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. 다음에서 프로젝트 선택 _모든 프로젝트_ 목록을 표시합니다.

1. 환경 목록에서 동기화할 분기의 이름을 클릭합니다.

1. (동기화)를 클릭합니다.

   ![환경 동기화](../../assets/button-sync.png){width="150"}

1. 동기화할 항목을 선택하십시오.

   - 데이터 바꾸기 - (데이터 및 파일) 상위 분기의 데이터베이스 및 컨텐츠 파일에서 변경 사항을 동기화합니다.
   - 병합—(코드) 상위 분기에서 업데이트된 코드를 동기화합니다.

   또한 CLI 명령을 만들어 복사 및 사용할 수 있습니다.

1. 클릭 **동기화**.

## 상위 환경과 병합

환경(또는 분기) 병합은 와 동일합니다. `git push origin`. 를 병합하여 업데이트된 코드를 환경에서 상위 환경으로 푸시합니다. 이 코드를에 병합할 수 있습니다. `master`. 를 사용하여 스테이징 및 프로덕션에 배포할 수 있습니다. `merge` 명령입니다.

**상위 환경과 병합하려면**:

1. 에 로그인합니다 [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. 다음에서 프로젝트 선택 _모든 프로젝트_ 목록을 표시합니다.

1. 환경 목록에서 병합할 분기의 이름을 클릭합니다.

1. (병합)을 클릭합니다.

   ![환경 병합](../../assets/button-merge.png){width="150"}

1. 클릭 **병합** 작업을 확인합니다.

## 로그 보기

다음을 통해 [!DNL Cloud Console], 빌드, 배포 및 배포 기록을 포함한 환경에 대한 다양한 로그를 검토할 수 있습니다.

대상 **스타터**, 빌드 및 배포 로그와 배포 기록을 검토할 수 있습니다. 이러한 환경에는 `master` (프로덕션) 분기 및 그로부터 생성된 모든 분기.

대상 **Pro**, 각 환경에서 다음 로그를 검토할 수 있습니다.

- 통합 - 구축 및 구축, 구축 기록
- 스테이징 - 로그 및 배포 내역을 빌드합니다. SSH를 사용하여 서버에 로그인하여 배포 로그를 확인합니다.
- 프로덕션 - 로그 및 배포 내역을 작성합니다. SSH를 사용하여 서버에 로그인하여 배포 로그를 확인합니다.

**에서 로그를 보려면[!DNL Cloud Console]**:

1. 에 로그인합니다 [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. 다음에서 프로젝트 선택 _모든 프로젝트_ 목록을 표시합니다.

1. 환경을 선택합니다.

   환경 보기에서 다음을 제공합니다 [활동 목록](activity-stream.md) 다음을 표시합니다. _최근_ 이벤트, 동기화, 병합, 분기, 백업 등 작업당 하나의 항목을 시도했습니다. 클릭 **모두** 전체 배포 내역에 대해 설명합니다.

1. 빌드 로그를 보려면 계정에서 배포 레코드당 성공 또는 실패 링크를 선택합니다.

>[!TIP]
>
>다음을 클릭합니다. **필터링 기준** 아이콘 드롭다운 목록을 표시한 다음 보려는 메시지 유형을 선택합니다.

## 개인 Git 저장소에서 코드 가져오기

Adobe Commerce on cloud infrastructure 프로젝트에는 개인 Git 저장소의 코드가 포함될 수 있습니다. 예를 들어 개인 저장소에 사용자 지정 모듈 또는 테마에 대한 코드가 있을 수 있습니다. 이렇게 하려면 프로젝트의 공개 SSH 키를 개인 Git 저장소에 추가하고 프로젝트를 업데이트해야 합니다 `composer.json` 파일.

배포 키를 개인 GitHub 저장소에 추가하려면 해당 저장소의 관리자여야 합니다. GitHub에서는 한 저장소에만 배포 키를 사용할 수 있습니다.

프로젝트에서 여러 저장소에 액세스하려는 경우 SSH 키를 자동화된 사용자 계정에 첨부할 수 있습니다. 이 계정은 사람이 사용하지 않으므로 사용자라고 합니다. [컴퓨터 사용자](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/managing-deploy-keys). 공동 작업자로 컴퓨터 계정을 추가하거나 저장소에 액세스할 수 있는 팀에 컴퓨터 사용자를 추가합니다.

>[!INFO]
>
>Adobe은 이 코드를 프로젝트 Git 저장소에 추가하고 병합하는 것을 권장합니다. 연결을 구성하지 않으면 빌드 문제가 발생할 수 있습니다.

**SSH 공개 키를 찾으려면**:

1. 에 로그인합니다 [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. 다음에서 프로젝트 선택 _모든 프로젝트_ 목록을 표시합니다.

1. 맨 위 탐색 막대의 오른쪽에 있는 구성 아이콘을 클릭합니다.

1. 위치 _프로젝트 설정_, 클릭 **[!UICONTROL Deploy Key]**.

1. 다음 Git 기반 방법 중 하나에 사용할 배포 키를 클립보드에 복사합니다.

>[!BEGINTABS]

>[!TAB GitHub]

### GitHub 배포 키 입력

GitHub에서 배포 키는 기본적으로 읽기 전용입니다.

**프로젝트 공개 키를 GitHub 배포 키로 입력하려면**:

1. 관리자로 GitHub 저장소에 로그인합니다.
1. 저장소를 클릭합니다 **[!UICONTROL Settings]** 탭.

   >[!NOTE]
   >
   >이 옵션이 표시되지 않으면 저장소 관리자로 로그인되지 않은 것이므로 이 작업을 완료할 수 없습니다. GitHub 저장소 관리자에게 이 작업을 수행하도록 요청하십시오.

1. 다음에서 _설정_ 왼쪽 탐색에서 탭을 클릭하고 **[!UICONTROL Deploy Keys]**.
1. 클릭 **[!UICONTROL Add deploy key]**.
1. 나타나는 메시지를 따릅니다.

위치 `composer.json`, 사용 `<user>@<host>:<.git</code>` 형식 또는 `ssh://<user>@<host>:<port>/<path>.git` 비표준 포트를 사용하는 경우.

>[!TAB Bitbucket]

### Bitbucket 배포 키 입력

**프로젝트 공개 키를 Bitbucket 배포 키로 입력하려면**:

1. 관리자로 Bitbucket 저장소에 로그인합니다.
1. 왼쪽 탐색에서 **[!UICONTROL Settings]**.
1. 일반 을 클릭합니다. **[!UICONTROL Deployment Keys]**.
1. 클릭 **[!UICONTROL Add Key]**.
1. 나타나는 메시지를 따릅니다.

>[!TAB GitLab]

### GitLab 배포 키 입력

**프로젝트에 대한 공개 SSH 키를 GitLab 배포 키로 추가하려면**:

1. GitLab 저장소에 소유자로 로그인합니다.
1. 다음을 확인합니다 _파이프라인_ 옵션은 프로젝트에 대해 활성화되어 있습니다.

   1. 프로젝트 설정에서 **[!UICONTROL Visibility, project, features, permissions]** 섹션.
   1. 필요한 경우 **[!UICONTROL Pipelines]** 옵션을 활성화하려면 다음을 수행하십시오.

1. 공개 SSH 키를 CI/CD 설정에 추가합니다.

   1. 왼쪽 탐색에서 설정 > **[!UICONTROL CI / CD]**.
   1. 배포 키 를 클릭합니다 **확장** 키를 구성합니다.
   1. 다음에서 _배포 키_ 양식,에 배포 키 이름 추가 **[!UICONTROL Title]** 필드에 공개 SSH 키를 붙여 넣습니다. **[!UICONTROL Key]** 필드.
   1. 클릭 **[!UICONTROL Add Key]** 구성을 저장합니다.

>[!ENDTABS]

## 안전한 환경 및 분기

를 사용하여 웹 브라우저를 통해 모든 위치에서 프로젝트 및 환경에 액세스할 수 있습니다 [!DNL Cloud Console]. 프로덕션 환경, 스토어 및 사이트에 대한 보안 설정이 있을 수 있습니다. 이 섹션은 개발자, DBA 등을 위한 통합 및 스테이징 환경을 보호하는 데 도움이 됩니다.

>[!WARNING]
>
>**금지** Pro Staging 및 프로덕션 환경의 보안을 유지하려면 다음 방법을 사용하십시오. 이는 Fastly 캐싱을 중단합니다. 사용 [차단](../cdn/fastly-vcl-blocking.md) Adobe Commerce용 Fastly CDN에서 사용할 수 있는 기능입니다.

**환경을 보호하려면**:

1. 에 로그인합니다 [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. 다음에서 프로젝트 선택 _모든 프로젝트_ 목록을 표시합니다.

1. 환경을 선택하고 탐색 모음에서 구성 아이콘을 클릭합니다.

1. 환경 설정에서 _일반_ 탭을 클릭하고 **날짜** 대상 **[!UICONTROL HTTP access control enabled]** 보안 액세스를 활성화합니다. 자격 증명과 IP 주소 중에서 선택하여 액세스를 필터링할 수 있습니다.

1. 자격 증명으로 필터링하려면 **[!UICONTROL Add Login]**&#x200B;사용자 이름과 암호를 입력하고 **[!UICONTROL Add Login]** 추가.

1. IP 주소별로 필터링하려면 목록에 다음 IP 주소를 입력합니다. `deny` 또는 `allow`. For example:

   ```text
   123.456.789.111/29 allow
   123.456.789.112/29 allow
   234.123.567.111/29 allow
   0.0.0.0/0 deny
   ```

1. 클릭 **[!UICONTROL Save]**. 이렇게 하면 환경이 다시 배포되어 보안 및 설정이 업데이트됩니다. Adobe은 보안 설정을 완료한 후 환경을 테스트할 것을 권장합니다.
