---
title: 보안 연결
description: Adobe Commerce on cloud infrastructure 프로젝트에 SSH 키를 적용하고 원격 환경에 로그인하는 방법에 대해 알아봅니다.
role: Developer
feature: Cloud, Security
topic: Security
exl-id: b5780e8e-e3da-4b10-8ca3-2778085acd4a
source-git-commit: b49a51aba56f79b5253eeacb1adf473f42bb8959
workflow-type: tm+mt
source-wordcount: '976'
ht-degree: 0%

---

# 원격 환경에 대한 보안 연결

SSH(Secure Shell)는 원격 서버 및 시스템에 안전하게 로그인하는 데 사용되는 일반적인 프로토콜입니다. SSH를 사용하여 Adobe Commerce 애플리케이션 관리 및 원격 환경 로그 액세스를 위해 원격 환경에 액세스할 수 있습니다. Adobe은 SSH 공개 키를 사용하는 보안 FTP(sFTP) 연결만 지원합니다. FTP 연결은 지원되지 않습니다.

## SSH 키 쌍 생성

프로젝트 소스 코드 및 환경에 액세스해야 하는 모든 시스템 및 작업 영역에 SSH 키 쌍을 만듭니다. SSH 키를 사용하면 사용자 이름과 암호를 지속적으로 제공할 필요 없이 GitHub에 연결하여 소스 코드를 관리하고 클라우드 서버에 연결할 수 있습니다. SSH 키 쌍을 만드는 방법에 대한 자세한 내용은 [SSH를 사용하여 GitHub에 연결](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)을 참조하십시오.

- _공개 키_&#x200B;은(는) 사이트, SSH 및 sFTP에 액세스하는 데 안전하게 사용할 수 있습니다.
- _개인 키_&#x200B;는 워크스테이션에서 비공개로 유지됩니다.

>[!CAUTION]
>
>**개인 키를 공유하지 마십시오.** 티켓에 추가하지 않거나 채팅에 복사하거나 전자 메일에 첨부하지 않습니다.

## 계정에 SSH 공개 키 추가

Adobe Commerce on cloud infrastructure 계정에 SSH 공개 키를 추가한 후 계정에 모든 활성 환경을 다시 배포하여 키를 설치합니다.

Cloud CLI 또는 [!DNL Cloud Console] 방법 중 하나를 사용하여 계정에 SSH 키를 추가할 수 있습니다.

>[!BEGINTABS]

>[!TAB CLI]

### Cloud CLI를 사용하여 SSH 키 추가

1. 로컬 워크스테이션에서 프로젝트 디렉터리로 변경합니다.

1. 프로젝트에 로그인:

   ```bash
   magento-cloud login
   ```

1. 공개 키를 추가합니다.

   ```bash
   magento-cloud ssh-key:add ~/.ssh/id_rsa.pub
   ```

>[!TIP]
>
>Cloud CLI 명령 `ssh-key:list` 및 `ssh-key:delete`을(를) 사용하여 SSH 키를 나열하고 삭제할 수 있습니다.

>[!TAB 콘솔]

### [!DNL Cloud Console]을(를) 사용하여 SSH 키 추가

**새 프로젝트에 SSH 키를 추가하려면**:

1. [[!DNL Cloud Console]](https://console.adobecommerce.com)에 로그인합니다.

1. **[!UICONTROL No SSH key]**&#x200B;을(를) 클릭합니다. 이 아이콘은 명령 필드 오른쪽에 있으며 프로젝트에 SSH 키가 없을 때 표시됩니다.

1. 공개 SSH 키의 내용을 복사하여 **공개 키** 필드에 붙여넣으십시오.

1. 나머지 프롬프트를 따릅니다.

**클라우드 프로필에 SSH 키를 추가하려면**:

1. [[!DNL Cloud Console]](https://console.adobecommerce.com)에 로그인합니다.

1. 오른쪽 상단 계정 메뉴에서 **내 프로필**&#x200B;을 클릭합니다.

1. _SSH 키_ 보기에서 **공개 키 추가**&#x200B;를 클릭합니다.

1. _SSH 키 추가_ 양식에서 키에 **제목**&#x200B;을 지정하고 공개 SSH 키를 **키** 필드에 붙여 넣으십시오.

1. **저장**&#x200B;을 클릭합니다.

>[!ENDTABS]

## 원격 환경에 연결

`magento-cloud` CLI 또는 SSH 명령을 사용하여 원격 환경에 연결할 수 있습니다. `magento-cloud` CLI 명령은 Starter 및 Pro 통합 환경에서만 사용할 수 있습니다.

### Cloud CLI 사용

**원격 통합 환경에 로그인하려면**:

1. 로컬 워크스테이션에서 프로젝트 디렉터리로 변경합니다.

1. 해당 프로젝트의 환경을 나열합니다.

   ```bash
   magento-cloud environment:list -p <project-ID>
   ```

1. SSH를 사용하여 원격 환경에 로그인합니다.

   ```bash
   magento-cloud ssh -p <project-ID> -e <environment-ID>
   ```

### SSH 명령 사용

[!DNL Cloud Console]에는 각 환경에 대한 웹 및 SSH 액세스 명령 목록이 포함되어 있습니다.

**SSH 명령을 복사하려면**:

1. [[!DNL Cloud Console]](https://console.adobecommerce.com)에 로그인합니다.

1. _모든 프로젝트_ 목록에서 프로젝트를 선택하십시오.

1. 환경을 선택합니다.

1. **[!UICONTROL SSH]**&#x200B;을(를) 클릭합니다.

1. _SSH_ 탭에서 복사 단추를 클릭하여 전체 SSH 명령을 클립보드에 복사합니다.

1. 터미널을 열고 SSH 명령을 붙여넣어 연결을 만듭니다.

   ```bash
   ssh abcdefg123abc-branch-a12b34c--mymagento@ssh.us-2.magento.cloud
   ```

>[!TIP]
>
>Pro 스테이징 및 프로덕션 환경의 경우 SSH 명령은 다음과 같을 수 있습니다.
>
>```bash
>ssh <node>.ent-<project-ID>-<environment>-<user-ID>@ssh.<region>.magento.com
>```

## sFTP

Adobe Commerce on cloud infrastructure는 SSH 인증이 있는 sFTP(보안 FTP)를 사용하여 환경에 액세스할 수 있도록 지원합니다. sFTP용 SSH 키 인증을 지원하는 클라이언트를 사용하고 공개 SSH 키를 사용합니다. 공개 SSH 키를 대상 환경에 추가해야 합니다. Starter 환경 및 Pro 통합 환경의 경우 [다음을 통해 추가 [!DNL Cloud Console]](#add-your-ssh-key-using-the-project-web-interface)할 수 있습니다.

읽기 전용 sFTP 연결은 _지원되지 않습니다_. sFTP 액세스는 기본적으로 _쓰기_ 권한으로 제공됩니다.

sFTP를 구성할 때 SSH 액세스 환경 명령의 정보를 사용하십시오. `<project-id>-<environment-id>--<app-name>@ssh<cloud-host>`

- **사용자 이름**: SSH 액세스 대상의 `@` 앞에 있는 모든 콘텐츠.
- **암호**: sFTP에는 암호가 필요하지 않습니다. sFTP 액세스는 SSH 키 인증을 사용합니다.
- **호스트**: SSH 액세스에서 `@` 이후의 모든 콘텐츠.
- **포트**: 22(기본 SSH 포트).
- **SSH** 개인 키: 필요한 경우 sFTP 클라이언트에 개인 키의 위치를 제공하십시오. 기본적으로 개인 키는 `~/.ssh` 디렉터리에 저장됩니다.

클라이언트에 따라 sFTP에 대한 SSH 인증을 완료하려면 추가 옵션이 필요할 수 있습니다. 선택한 클라이언트에 대한 설명서를 검토하십시오.

**Starter 환경 및 Pro 통합 환경**&#x200B;의 경우 [특정 디렉터리에 액세스할 수 있도록 `mount`](../application/properties.md#mounts)을(를) 추가하는 것도 고려해 볼 수 있습니다. 마운트를 `.magento.app.yaml` 파일에 추가합니다. 쓰기 가능한 디렉터리 목록은 [프로젝트 구조](../project/file-structure.md)를 참조하십시오. 이 마운트 지점은 해당 환경에서만 작동합니다.

**Pro 스테이징 및 프로덕션 환경**&#x200B;의 경우 환경에 대한 SSH 액세스 권한이 없는 경우 [Adobe Commerce 지원 티켓을 제출](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)하여 sFTP 액세스를 요청하고 탑재 지점에서 특정 폴더(예: `pub/media`)에 액세스해야 합니다.

>[!NOTE]
>Pro Staging 및 프로덕션의 경우, sFTP 연결이 **not**&#x200B;이(가) 필요한 _일반_ 사용자용인 경우[클라우드 프로젝트에 추가](../project/user-access.md)해야 합니다. **공개** 키가 첨부된 [Adobe Commerce 지원 티켓을 제출](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)해야 합니다. **개인 SSH 키를 제공하지 않습니다.**

## SSH 터널링

SSH 터널링을 사용하여 서비스가 로컬인 것처럼 로컬 개발 환경에서 서비스에 연결할 수 있습니다. 터널링하기 전에 [SSH](#add-an-ssh-public-key-to-your-account)을(를) 구성하십시오.

터미널 응용 프로그램을 사용하여 로그인하고 명령을 실행합니다.

```bash
magento-cloud login
```

을 사용하여 열려 있는 터널이 있는지 확인합니다.

```bash
magento-cloud tunnel:list
```

터널을 만들려면 [응용 프로그램 이름](../application/properties.md#name)을 알아야 합니다. CLI를 사용하여 애플리케이션 이름을 확인할 수 있습니다.

```bash
magento-cloud apps
```

### SSH 터널 설정

```bash
magento-cloud tunnel:open -e <environment-ID> --app <app-name>
```

예를 들어, 이름이 `mymagento`인 앱으로 프로젝트의 `sprint5` 분기에 대한 터널을 열려면 다음을 입력합니다

```bash
magento-cloud tunnel:open -e sprint5 --app mymagento
```

샘플 응답:

```
SSH tunnel opened on port 30004 to relationship: redis
SSH tunnel opened on port 30005 to relationship: database
Logs are written to: /home/magento_user/.magento/tunnels.log

List tunnels with: magento-cloud tunnels
View tunnel details with: magento-cloud tunnel:info
Close tunnels with: magento-cloud tunnel:close
```

**터널 정보를 표시하려면**:

```bash
magento-cloud tunnel:info -e <environment-ID>
```

### 서비스에 연결

SSH 터널을 설정한 후 로컬에서 실행하는 것처럼 서비스에 연결할 수 있습니다. 예를 들어 데이터베이스에 연결하려면 다음 명령을 사용합니다.

```bash
mysql --host=127.0.0.1 --user='<database-username>' --pass='<user-password>' --database='<name>' --port='<port>'
```
