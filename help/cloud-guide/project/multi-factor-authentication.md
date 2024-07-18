---
title: SSH 액세스에 다단계 인증 사용
description: 클라우드 인프라 환경에서 Adobe Commerce에 대한 SSH 액세스를 위한 인증 요구 사항을 관리하는 방법을 알아봅니다.
feature: Cloud, Security
topic: Security
exl-id: 754b2c22-f197-49be-a699-fb3bedf053fc
source-git-commit: b49a51aba56f79b5253eeacb1adf473f42bb8959
workflow-type: tm+mt
source-wordcount: '1051'
ht-degree: 0%

---

# SSH 액세스에 다단계 인증 사용

보안 강화를 위해 클라우드 인프라의 Adobe Commerce에서는 클라우드 환경에 대한 SSH 액세스를 위한 인증 요구 사항을 관리하기 위한 MFA(Multi-Factor Authentication) 적용을 제공합니다.

프로젝트에서 MFA가 활성화되면 SSH 액세스 권한이 있는 모든 사용자 계정은 환경에 액세스하려면 2단계 인증(TFA) 코드 또는 API 토큰 및 SSH 인증서가 필요합니다.

>[!NOTE]
>
>MFA는 기본적으로 클라우드 프로젝트에서 활성화되지 않습니다. 클라우드 인프라 프로젝트의 Adobe Commerce 계정 소유자가 활성화하려면 [Adobe Commerce 지원 티켓을 제출](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)해야 합니다. MFA가 활성화되면 프로젝트 환경에 대한 SSH 액세스를 위해 모든 사용자는 Adobe Commerce on cloud infrastructure 계정에서 TFA(2단계 인증)가 활성화되어 있어야 합니다.

## SSH 액세스용 인증서

MFA를 통해 사용자는 OAUTH 액세스 토큰을 Adobe 클라우드 인증자 API에서 생성된 단기 SSH 인증서와 교환할 수 있습니다. 사용자에게 관리자 또는 기여자 역할, 유효한 SSH 키 및 유효한 TFA 코드나 API 토큰이 있는 경우, 클라우드 인프라의 Adobe Commerce은 이러한 자격 증명을 사용하여 임시 SSH 인증서를 생성합니다. 인증서 만료는 1시간으로 설정되지만 현재 세션 중에 자동으로 새로 고쳐집니다.

MFA를 사용하여 프로젝트에 로그인한 후 사용자는 `magento-cloud` CLI를 사용하여 SSH 인증서를 생성해야 합니다.

```bash
magento-cloud ssh-cert:load
```

`ssh-cert:load` 명령은 SSH 인증서를 생성하여 로컬 사용자의 SSH 에이전트에 설치합니다.

### 로그인 시 인증서 자동 생성

`magento-cloud` CLI에 인증할 때 SSH 인증서를 자동으로 생성하도록 로컬 환경을 구성할 수 있습니다.

**SSH 인증서 자동 생성을 `magento-cloud` CLI 구성에 추가하려면**:

1. 로컬 워크스테이션에서 홈 디렉터리의 `.magento-cloud` 폴더에 이름이 `config.yaml`인 파일이 없는 경우 만듭니다.

   ```bash
   touch ~/.magento-cloud/config.yaml
   ```

1. `config.yaml` 파일에 다음 구성을 추가합니다.

   ```yaml
   api:
      auto_load_ssh_cert: true
   ```

1. `magento-cloud` CLI를 사용하여 다시 인증하십시오.

   >로그아웃:

   ```bash
   magento-cloud logout
   ```

   >로그인:

   ```bash
   magento-cloud login
   ```

   >다음 응답을 따릅니다.

   ```
   Please open the following URL in a browser and log in:
   http://127.0.0.1:5000
   
   Help:
     Leave this command running during login.
     If you need to quit, use Ctrl+C.
   
     To log in using an API token, run: magento-cloud auth:api-token-login
   
   Login information received. Verifying...
   You are logged in.
   
   Generating SSH certificate...
   A new SSH certificate has been generated.
   It will be automatically refreshed when necessary.
   The certificate is included in your SSH configuration: /Users/<user-name>/.ssh/config
   ```

## TFA와 함께 SSH를 사용하여 환경에 연결

프로젝트에서 MFA가 활성화되어 있으면 SSH를 사용하여 원격 환경에 연결하려면 먼저 계정에서 TFA가 활성화되어 있어야 합니다. [TFA 사용](user-access.md#enable-tfa-for-cloud-accounts)을 참조하십시오.

>[!BEGINSHADEBOX]

**필수 구성 요소:**

MFA 적용으로 활성화된 프로젝트의 경우 SSH 액세스를 사용하려면 다음 권한 및 계정 설정이 필요합니다.

- [환경에 대한 관리자 또는 기여자 액세스](user-access.md)
- [계정에 SSH 액세스 키 구성됨](../development/secure-connections.md#add-an-ssh-public-key-to-your-account)
- [TFA 계정 사용](user-access.md#enable-tfa-for-cloud-accounts)

>[!ENDSHADEBOX]

**TFA 사용자 계정 자격 증명과 함께 SSH를 사용하여 연결하려면**:

1. [계정](https://console.adobecommerce.com)에 로그인합니다.

1. 로컬 워크스테이션에서 `magento-cloud` CLI를 사용하여 SSH 인증서를 생성합니다.

   ```bash
   magento-cloud ssh-cert:load
   ```

   > 샘플 응답:

   ```
   Generating SSH certificate...
     Expires at: 2020-07-13T15:28:13-04:00
     Multi-factor authentication: verified
     Mode: interactive
   The certificate will be automatically refreshed when necessary.
   Checking SSH configuration file: /Users/<user-name>/.ssh/config
   Do you want to update the file automatically? [Y/n] Y
   Configuration file updated successfully: /Users/<user-name>/.ssh/config
   ```

1. SSH를 사용하여 원격 환경에 연결합니다.

   ```bash
   ssh abcdef7uyxabce-master-7rqtwti--mymagento@ssh.us-5.magento.cloud
   ```

   ```
    __  __                   _          ___ _             _
   |  \/  |__ _ __ _ ___ _ _| |_ ___   / __| |___ _  _ __| |
   | |\/| / _` / _` / -_) ' \  _/ _ \ | (__| / _ \ || / _` |
   |_|  |_\__,_\__, \___|_||_\__\___/  \___|_\___/\_,_\__,_|
               |___/
   
    Welcome to Magento Cloud.
   
    This is environment master-7rqtwti
    of project abcdef7uyxabce.
   
   web@mymagento.0:~$
   ```

## TFA와 함께 SSH를 사용하여 소스 코드 관리

클라우드 인프라 프로젝트에서 Adobe Commerce에 대한 소스 코드를 관리할 때 SSH를 사용하여 프로젝트에 대한 Git 저장소를 인증합니다. 프로젝트에 MFA 강제 적용이 활성화된 경우 Git 저장소를 사용하여 명령줄 작업을 수행하려면 먼저 SSH 인증서를 생성해야 합니다.

**TFA 사용자 계정 자격 증명과 함께 SSH를 사용하여 연결하려면**:

1. [계정](https://console.adobecommerce.com)에 로그인하고 TFA를 사용하여 인증합니다.

   >[!NOTE]
   >
   >계정에 TFA가 활성화되지 않은 경우 활성화해야 합니다. [클라우드 계정에서 TFA 사용](user-access.md#enable-tfa-for-cloud-accounts)을 참조하십시오.

1. 로컬 워크스테이션에서 `magento-cloud` CLI를 사용하여 SSH 인증서를 생성합니다.

   ```bash
   magento-cloud ssh-cert:load
   ```

   > 샘플 응답:

   ```
   Generating SSH certificate...
     Expires at: 2020-07-13T15:28:13-04:00
     Multi-factor authentication: verified
     Mode: interactive
   The certificate will be automatically refreshed when necessary.
   Checking SSH configuration file: /Users/<user-name>/.ssh/config
   Do you want to update the file automatically? [Y/n] Y
   Configuration file updated successfully: /Users/<user-name>/.ssh/config
   ```

1. 프로젝트 환경에 대한 Git 저장소를 복제합니다.

   ```bash
   git clone --branch integration abcdef7uyxabce@git.us-3.magento.cloud:abcdef7uyxabce.git myproject
   ```

   > 샘플 응답:

   ```
   Cloning into 'myproject'...
   Connection to git.us-3.magento.cloud port 22 [tcp/ssh] succeeded!
   remote: counting objects: 22, done.
   Receiving objects: 100% (22/22), 82.42 KiB | 16.48 MiB/s, done.
   ```

## API 토큰과 함께 SSH를 사용하여 환경에 연결

프로젝트에서 MFA가 활성화되면 클라우드 환경에 대한 SSH 액세스가 필요한 자동화된 프로세스에는 API 토큰이 필요합니다. 프로젝트에 대한 관리자 또는 기여자 액세스 권한을 사용하여 Adobe Commerce on cloud infrastructure 계정에서 토큰을 생성할 수 있습니다.

API 토큰으로 인증하려면 SSH 인증서를 생성해야 합니다. 자동화된 프로세스는 SSH 인증서 생성도 자동화해야 합니다.

>[!BEGINSHADEBOX]

**필수 구성 요소:**

- [클라우드 인프라 환경의 Adobe Commerce에 대한 관리자 또는 기여자 액세스](user-access.md)
- [계정에서 사용할 수 있는 유효한 API 토큰](user-access.md#create-an-api-token)

>[!ENDSHADEBOX]

**API 토큰 자격 증명과 함께 SSH를 사용하여 연결하려면**:

1. API 키 인증을 사용하여 Cloud 프로젝트에 로그인합니다.

   ```bash
   magento-cloud auth:api-token
   ```

1. 프롬프트에서 유효한 API 토큰에 대한 값을 입력합니다.

   ```
   Please enter an API token:
   >
   
   The API token is valid.
   You are logged in.
   ```

### 예: 자동화된 SSH 스크립트

API 토큰을 저장하는 두 가지 옵션이 있습니다.

>[!NOTE]
>
>API 토큰이 저장된 경우 `magento-cloud` CLI가 자동으로 인증되며 `magento-cloud login` 명령을 수행할 필요가 없습니다.

**옵션 1**: API 토큰을 저장할 환경 변수를 만듭니다.

bash_profile에 토큰 작성

```bash
echo "export MAGENTO_CLOUD_CLI_TOKEN=<your api token>" >> ~/.bash_profile
```

**옵션 2**: 토큰을 `config.yaml` 파일에 추가

1. 로컬 워크스테이션에서 홈 디렉터리의 `.magento-cloud` 폴더에 이름이 `config.yaml`인 파일이 없는 경우 만듭니다.

   ```bash
   touch ~/.magento-cloud/config.yaml
   ```

1. `config.yaml` 파일에 다음 구성을 추가합니다.

   ```yaml
   api:
      token: <your api token>
   ```

>샘플 bash 스크립트

```shell
#!/bin/bash
magento-cloud ssh-cert:load
ssh abcdef7uyxabce-master-7rqtabc--mymagento@ssh.us-3.magento.cloud "tail -n 10 ~/var/log/cloud.log"
```

## 문제 해결

`access requires MFA` 또는 `permission denied`과(와) 같은 인증 오류로 인해 SSH 연결 요청 오류를 해결하려면 다음 정보를 사용하십시오.

### 요청이 올바른 인증서를 제공하지 않습니다.

요청이 유효한 인증서를 제공하지 않으면 다음과 유사한 메시지가 표시됩니다.

```
to Hello user-test (UUID: abaacca12-5cd1-4b123-9096-411add578998), you successfully
authenticated, but could not connect to service abcdef7uyxabce-master-7rqtabc--mymagento@ssh.us-3.magento.cloud:>
(reason: access requires MFA)
```

다음 문제 해결 절차를 수행하여 연결 문제를 해결하십시오.

- 계정 TFA 구성 확인
- 다시 인증한 다음 인증서를 다시 로드합니다

**TFA 구성 및 인증을 확인하려면**:

1. [계정](https://console.adobecommerce.com)에 로그인합니다.

1. 오른쪽 위 계정 메뉴에서 **[!UICONTROL My Profile]**&#x200B;을(를) 클릭합니다.

1. _내 프로필_ 페이지에서 **[!UICONTROL Security]** 탭을 클릭합니다.

   TFA가 활성화된 경우 보안 섹션에서 TFA 구성을 관리할 수 있는 옵션을 제공합니다.

1. TFA가 설정되지 않은 경우 **[!UICONTROL Set up application]**&#x200B;을(를) 클릭하고 지침에 따라 활성화하십시오. [TFA 사용](user-access.md#enable-tfa-for-cloud-accounts)을 참조하십시오.

1. TFA가 구성된 경우 인증을 다시 시도하십시오.

**SSH 인증서를 인증하고 다시 로드하려면**:

1. `magento-cloud` CLI를 사용하여 다시 인증하십시오.

   ```bash
   magento-cloud logout
   ```

   ```bash
   magento-cloud login
   ```

1. SSH 인증서를 다시 로드합니다.

   ```bash
   magento-cloud ssh-cert:load
   ```

### 권한 거부됨

SSH 키가 없거나 잘못된 경우 SSH 연결 요청이 `Permission denied (publickey)` 오류를 반환합니다.

```
Hello user-test (UUID: abaacca12-5cd1-4b123-9096-411add578998), you successfully authenticated, but could not connect to service oh2wi6klp5ytk-mc-35985-integration-nnulm4a--mymagento (reason: service doesn't exist or you do not have access to it)
oh2wi6klp5ytk-mc-35985-integration-nnulm4a--mymagento@ssh.eu-3.magento.cloud: Permission denied (publickey).
```

문제를 해결하려면 현재 세션에 SSH 키를 추가하거나 SSH 구성 파일을 업데이트하여 SSH 키를 자동으로 로드합니다. [공개 SSH 키 추가](../development/secure-connections.md#add-an-ssh-public-key-to-your-account)를 참조하십시오.

### MFA가 없는 프로젝트에 액세스할 수 없음

MFA(Multi-Factor Authentication)가 활성화된 프로젝트를 인증하는 경우 MFA가 필요하지 않은 다른 프로젝트에 연결할 때 다음 오류가 발생할 수 있습니다.

```bash
ssh abcdef7uyxabce-master-7rqtabc--mymagento@ssh.us-3.magento.cloud
```

샘플 응답:

```
abcdef7uyxabce-master-7rqtabc--mymagento@ssh.us-3.magento.cloud: Permission denied (publickey).
```

SSH 인증서를 생성하는 동안 `magento-cloud` CLI는 로컬 환경에 추가 SSH 키를 추가합니다. 로컬 SSH 구성에 프로젝트 액세스용 SSH 키가 포함되지 않은 경우 기본적으로 이 키가 사용됩니다.

**로컬 구성에 SSH 키를 추가하려면**:

1. `config` 파일이 없으면 만듭니다.

   ```bash
   touch ~/.ssh/config
   ```

1. `IdentityFile` 구성을 추가합니다.

   ```yaml
   Host *
     IdentityFile ~/.ssh/id_rsa
   ```

>[!NOTE]
>
>구성에 여러 `IdentityFile` 항목을 추가하여 여러 SSH 키를 지정할 수 있습니다.
