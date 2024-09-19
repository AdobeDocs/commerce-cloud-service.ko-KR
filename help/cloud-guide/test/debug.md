---
title: ' [!DNL Xdebug] 구성'
description: 클라우드 인프라 프로젝트 개발에서 Adobe Commerce을 디버깅하기 위해 Xdebug 확장을 구성하는 방법을 알아봅니다.
exl-id: bf2d32d8-fab7-439e-8df3-b039e53009d4
source-git-commit: 7b42174663b79b673ee5af05b794090ddc5bdd75
workflow-type: tm+mt
source-wordcount: '1765'
ht-degree: 0%

---

# Xdebug 구성

[!DNL Xdebug]은(는) PHP를 디버깅하기 위한 확장입니다. 선택한 IDE를 사용할 수 있지만, 다음은 로컬 환경에서 디버깅하도록 [!DNL Xdebug] 및 [!DNL PhpStorm]을(를) 구성하는 방법을 설명합니다.

>[!NOTE]
>
>클라우드 인프라 프로젝트 구성에서 Adobe Commerce을 변경하지 않고 로컬 디버깅을 위해 Cloud Docker 환경에서 실행되도록 [!DNL Xdebug]을(를) 구성할 수 있습니다. [Docker용 Xdebug 구성](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug/)을 참조하십시오.

[!DNL Xdebug]을(를) 사용하려면 Git 저장소에서 파일을 구성하고 IDE를 구성하고 포트 전달을 설정해야 합니다. `magento.app.yaml` 파일에서 일부 설정을 구성할 수 있습니다. 편집한 후 모든 Starter 환경 및 Pro 통합 환경에서 Git 변경 사항을 푸시하여 [!DNL Xdebug]을(를) 사용하도록 설정합니다. [!DNL Xdebug]은(는) Pro 스테이징 및 프로덕션 환경에서 이미 사용할 수 있습니다.

구성하고 나면 CLI 명령, 웹 요청 및 코드를 디버깅할 수 있습니다. 모든 클라우드 인프라 환경은 읽기 전용입니다. 코드를 로컬 개발 환경에 복제하여 디버깅을 수행합니다. Pro 스테이징 및 프로덕션 환경의 경우 [!DNL Xdebug]에 대한 [추가 지침](#debug-for-pro-staging-and-production)을 참조하세요.

## 요구 사항

[!DNL Xdebug]을(를) 실행하고 사용하려면 환경에 대한 SSH URL이 필요합니다. [[!DNL Cloud Console]](../project/overview.md) 또는 [!DNL Cloud Onboarding UI]을(를) 통해 정보를 찾을 수 있습니다.

## Xdebug 구성

[!DNL Xdebug]을(를) 구성하려면 다음 단계를 수행합니다.

- [분기에서 작업하여 파일 업데이트 푸시](#get-started-with-a-branch)
- [환경에 대해  [!DNL Xdebug] 사용](#enable-xdebug-in-your-environment)
- [IDE 구성](#configure-phpstorm)
- [포트 전달 설정](#set-up-port-forwarding)

### 분기 시작

Adobe [!DNL Xdebug]을(를) 추가하려면 [개발 분기](../dev-tools/cloud-cli-overview.md#create-an-environment-branch)에서 작업하는 것이 좋습니다.

### 환경에서 Xdebug 활성화

모든 시작 환경 및 Pro 통합 환경에 직접 [!DNL Xdebug]을(를) 사용하도록 설정할 수 있습니다. 이 구성 단계는 Pro 프로덕션 및 스테이징 환경에는 필요하지 않습니다. [Pro 스테이징 및 프로덕션용 디버그](#debug-for-pro-staging-and-production)를 참조하십시오.

프로젝트에 대해 [!DNL Xdebug]을(를) 사용하려면 `.magento.app.yaml` 파일의 `runtime:extensions` 섹션에 `xdebug`을(를) 추가하십시오.

**Xdebug를 사용하려면**:

1. 로컬 터미널에서 텍스트 편집기에서 `.magento.app.yaml` 파일을 엽니다.

1. `runtime` 섹션의 `extensions`에서 `xdebug`을(를) 추가합니다. For example:

   ```yaml
   runtime:
       extensions:
           - redis
           - xsl
           - newrelic
           - sodium
           - xdebug
   ```

1. `.magento.app.yaml` 파일에 변경 내용을 저장하고 텍스트 편집기를 종료합니다.

1. 변경 사항을 추가, 커밋 및 푸시하여 환경을 재배포합니다.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Add xdebug"
   ```

   ```bash
   git push origin <environment-ID>
   ```

Starter 환경 및 Pro 통합 환경에 배포되면 이제 [!DNL Xdebug]을(를) 사용할 수 있습니다. IDE 구성을 계속합니다. PhpStorm의 경우 [PhpStorm 구성](#configure-phpstorm)을 참조하십시오.

### PhpStorm 구성

[!DNL Xdebug]에서 제대로 작동하도록 [PhpStorm](https://www.jetbrains.com/phpstorm/) IDE를 구성해야 합니다.

**Xdebug에서 작동하도록 PhpStorm을 구성하려면**:

1. PhpStorm 프로젝트에서 **설정** 패널을 엽니다.

   - _macOS_—**PhpStorm** > **환경 설정**&#x200B;을 선택합니다.
   - _Windows/Linux_ - **파일** > **설정**&#x200B;을 선택합니다.

1. _설정_ 패널에서 **언어 및 프레임워크** > **PHP** > **서버** 섹션을 확장하고 찾습니다.

1. 서버 구성을 추가하려면 **+**&#x200B;을(를) 클릭하십시오. 프로젝트 이름이 맨 위에 회색으로 표시됩니다.

1. [선택 사항] 새 서버 구성에 대해 다음 설정을 구성합니다. _PHPStorm_ 설명서에서 [구성된 디버그 서버가 없음](https://www.jetbrains.com/help/phpstorm/troubleshooting-php-debugging.html#no-debug-server-is-configured)을 참조하십시오.

   - **이름**—호스트 이름과 동일하게 입력합니다. 이 값은 디버깅에 CLI를 사용하려면 [Debug CLI 명령](#debug-cli-commands)의 `PHP_IDE_CONFIG` 변수 값과 일치해야 합니다.
   - **호스트**—호스트 이름을 입력하십시오.
   - **포트**—`443`을(를) 입력하십시오.
   - **디버거**—`Xdebug`을(를) 선택합니다.

1. **경로 매핑 사용**&#x200B;을 선택하십시오. _파일/디렉터리_ 창에 `serverName`에 대한 프로젝트의 루트가 표시됩니다.

1. **서버의 절대 경로** 열에서 **편집** 아이콘을 클릭하고 환경에 따라 설정을 추가합니다.

   - 모든 Starter 환경 및 Pro 통합 환경의 경우 원격 경로는 `/app`입니다.
   - Pro 스테이징 및 프로덕션 환경의 경우:

      - 프로덕션: `/app/<project_code>/`
      - 준비 중: `/app/<project_code>_stg/`

1. **언어 및 프레임워크** > **PHP** > **디버그** > **Xdebug** > **디버그 포트** 패널에서 [!DNL Xdebug] 포트를 9000으로 변경합니다.

1. **적용**&#x200B;을 클릭합니다.

### 포트 전달 설정

서버에서 로컬 시스템에 `XDEBUG` 연결을 매핑합니다. 모든 유형의 디버깅을 수행하려면 Adobe Commerce on cloud infrastructure 서버에서 로컬 시스템으로 포트 9000을 전달해야 합니다. 다음 섹션 중 하나를 참조하십시오.

- [Mac 또는 UNIX의 포트 전달](#port-forwarding-on-mac-or-unix)
- [Windows에서 포트 전달](#port-forwarding-on-windows)

#### Mac 또는 UNIX에서 포트 ®

**Mac 또는 UNIX® 환경에서 포트 전달을 설정하려면**:

1. 터미널을 엽니다.

1. SSH를 사용하여 연결을 설정하십시오.

   ```bash
   ssh -R 9000:localhost:9000 <ssh url>
   ```

   소켓이 전달되는 포트에 연결될 때마다 터미널에 표시되도록 `-v`(verbose) 옵션을 사용합니다.

   &quot;연결할 수 없음&quot; 또는 &quot;원격으로 포트를 수신 대기할 수 없음&quot; 오류가 표시되면 포트 9000을 사용 중인 서버에 다른 활성 SSH 세션이 남아 있을 수 있습니다. 해당 연결이 사용되고 있지 않으면 연결을 종료할 수 있습니다.

**연결 문제 해결**:

1. SSH를 사용하여 원격 통합, 스테이징 또는 프로덕션 환경에 로그인합니다.

1. SSH 세션 목록 보기: `who`

1. 사용자별 기존 SSH 세션을 봅니다. 자신 이외의 사용자에게 영향을 주지 않도록 주의하십시오!

   - 통합: 사용자 이름이 `dd2q5ct7mhgus`과(와) 비슷합니다.
   - 스테이징: 사용자 이름이 `dd2q5ct7mhgus_stg`과(와) 비슷합니다.
   - 프로덕션: 사용자 이름이 `dd2q5ct7mhgus`과(와) 비슷합니다.

1. 기존 세션보다 오래된 사용자 세션의 경우 `pts/0`과(와) 같은 의사 터미널(PTS) 값을 찾습니다.

1. PTS 값에 해당하는 프로세스 ID(PID)를 종료합니다.

   ```bash
   ps aux | grep ssh
   kill <PID>
   ```

   샘플 응답:

   ```
   dd2q5ct7mhgus        5504  0.0  0.0  82612  3664 ?      S    18:45   0:00 sshd: dd2q5ct7mhgus@pts/0
   ```

   연결을 종료하려면 프로세스 ID(PID)와 함께 종료 명령을 입력합니다.

   ```bash
   kill 3664
   ```

#### Windows에서 포트 전달

Windows에서 포트 전달(SSH 터널링)을 설정하려면 Windows 터미널 응용 프로그램을 구성해야 합니다. 이 예제에서는 [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)을(를) 사용하여 SSH 터널을 만드는 과정을 단계별로 설명합니다. Cygwin과 같은 다른 응용 프로그램을 사용할 수 있습니다. 다른 애플리케이션에 대한 자세한 내용은 해당 애플리케이션과 함께 제공되는 공급업체 설명서를 참조하십시오.

**Putty를 사용하여 Windows에서 SSH 터널을 설정하려면**:

1. 아직 다운로드하지 않았다면 [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)을(를) 다운로드하십시오.

1. 퍼티 시작해요

1. 범주 창에서 **세션**&#x200B;을 클릭합니다.

1. 다음 정보를 입력합니다.

   - **호스트 이름(또는 IP 주소)** 필드: 클라우드 서버의 [SSH URL](../development/secure-connections.md#connect-to-a-remote-environment)을(를) 입력하십시오.
   - **포트** 필드: `22` 입력

   ![Putty 설정](../../assets/xdebug/putty-session.png)

1. _범주_ 창에서 **연결** > **SSH** > **터널**&#x200B;을 클릭합니다.

1. 다음 정보를 입력합니다.

   - **Source 포트** 필드: `9000` 입력
   - **대상** 필드: `127.0.0.1:9000` 입력
   - **원격** 클릭

1. **추가**&#x200B;를 클릭합니다.

   ![Putty에서 SSH 터널 만들기](../../assets/xdebug/putty-tunnels.png)

1. _범주_ 창에서 **세션**&#x200B;을 클릭합니다.

1. **저장된 세션** 필드에 이 SSH 터널의 이름을 입력하십시오.

1. **저장**&#x200B;을 클릭합니다.

   ![SSH 터널 저장](../../assets/xdebug/putty-session-save.png)

1. SSH 터널을 테스트하려면 **로드**&#x200B;를 클릭한 다음 **열기**&#x200B;를 클릭합니다.

   &quot;연결할 수 없음&quot; 오류가 표시되면 다음을 확인하십시오.

   - 모든 퍼티 설정이 올바릅니다.
   - 클라우드 인프라의 개인 Adobe Commerce SSH 키가 있는 컴퓨터에서 Putty를 실행하고 있습니다.

## Xdebug 환경에 대한 SSH 액세스

디버깅을 시작하고 설정을 수행하는 등의 작업을 수행하려면 환경에 액세스하기 위한 SSH 명령이 필요합니다. [[!DNL Cloud Console]](../development/secure-connections.md#use-an-ssh-command) 및 프로젝트 스프레드시트를 통해 이 정보를 가져올 수 있습니다.

Starter 환경 및 Pro 통합 환경의 경우 다음 `magento-cloud` CLI 명령을 사용하여 해당 환경에 SSH할 수 있습니다.

```bash
magento-cloud environment:ssh --pipe -e <environment-ID>
```

[!DNL Xdebug]을(를) 사용하려면 다음과 같이 환경에 SSH를 사용하십시오.

```bash
ssh -R <xdebug listen port>:<host>:<xdebug listen port> <SSH-URL>
```

For example,

```bash
ssh -R 9000:localhost:9000 pwga8A0bhuk7o-mybranch@ssh.us.magentosite.cloud
```

## Pro Staging 및 프로덕션에 대한 디버그

>[!NOTE]
>
>Pro 스테이징 및 프로덕션 환경에서는 [!DNL Xdebug]에 대한 특별 설정이 있으므로 [!DNL Xdebug]을(를) 항상 사용할 수 있습니다. 모든 일반 웹 요청이 [!DNL Xdebug]이(가) 없는 전용 PHP 프로세스로 라우팅됩니다. 따라서 이러한 요청은 정상적으로 처리되며 [!DNL Xdebug]이(가) 로드될 때 성능 저하의 영향을 받지 않습니다. [!DNL Xdebug] 키가 있는 웹 요청이 전송되면 [!DNL Xdebug]이(가) 로드된 별도의 PHP 프로세스로 라우팅됩니다.

특별히 Pro 계획 스테이징 및 프로덕션 환경에서 [!DNL Xdebug]을(를) 사용하려면 액세스 권한이 있는 사용자에게만 별도의 SSH 터널 및 웹 세션을 만듭니다. 이 사용법은 일반적인 액세스와는 다르며, 모든 사용자가 아닌 귀하에게만 액세스를 제공합니다.

다음이 필요합니다.

- 환경에 액세스하기 위한 SSH 명령. [[!DNL Cloud Console]](../project/overview.md) 또는 [!DNL Cloud Onboarding UI]을(를) 통해 이 정보를 가져올 수 있습니다.
- 스테이징 및 Pro 환경을 구성할 때 설정된 `xdebug_key` 값입니다.

  SSH를 사용하여 기본 노드에 로그인하고 다음을 실행하여 `xdebug_key`을(를) 찾을 수 있습니다.

  ```bash
  cat /etc/platform/*/nginx.conf | grep xdebug.sock | head -n1
  ```

**스테이징 또는 프로덕션 환경에 SSH 터널을 설정하려면**:

1. 터미널을 엽니다.

1. 클러스터의 각 웹 노드에 대한 모든 SSH 세션을 정리합니다.

   ```bash
   ssh USERNAME@CLUSTER.ent.magento.cloud 'rm /run/platform/USERNAME/xdebug.sock'
   ```

1. 클러스터의 각 웹 노드에 대해 Xdebug에 대한 SSH 터널을 설정합니다.

   ```bash
   ssh -R /run/platform/USERNAME/xdebug.sock:localhost:9000 -N USERNAME@CLUSTER.ent.magento.cloud
   ```

>[!NOTE]
>
>`USERNAME@CLUSTER.ent.magento.cloud`에 대한 올바른 값을 얻으려면:
>- 방법 1: magento-cloud CLI: `magento-cloud ssh --all`
>- 메서드 2: Commerce 콘솔: https://CONSOLE-URL/ENVIRONMENT, `SSH v` 드롭다운을 클릭합니다.

**환경 URL을 사용하여 디버깅을 시작하려면**:

1. 원격 디버깅을 사용하도록 설정합니다. 브라우저의 사이트를 방문하여 `KEY`이(가) `xdebug_key`의 값인 URL에 다음을 추가하십시오.

   ```http
   ?XDEBUG_SESSION_START=KEY
   ```

   이 단계에서는 [!DNL Xdebug]을(를) 트리거하기 위해 브라우저 요청을 보내는 쿠키를 설정합니다.

1. [!DNL Xdebug](으)로 디버깅을 완료합니다.

1. 세션을 종료할 준비가 되면 다음 명령을 사용하여 쿠키를 제거하고 브라우저를 통해 디버깅을 종료합니다. 여기서 `KEY`은(는) `xdebug_key`의 값입니다.

   ```http
   ?XDEBUG_SESSION_STOP=KEY
   ```

   >[!NOTE]
   >
   >`POST` 요청에 의해 전달된 `XDEBUG_SESSION_START`은(는) 지원되지 않습니다.

## CLI 명령 디버그

이 섹션에서는 CLI 명령 디버깅에 대해 설명합니다.

CLI 명령을 디버깅하려면

1. CLI 명령을 사용하여 디버그하려는 서버에 SSH를 연결합니다.

1. 다음 환경 변수를 만듭니다.

   ```bash
   export XDEBUG_CONFIG='PHPSTORM'
   ```

   ```bash
   export PHP_IDE_CONFIG="serverName=<name of the server that is configured in PHPSTORM>"
   ```

   이 변수는 SSH 세션이 종료되면 제거됩니다.

1. 디버깅 시작

   Starter 환경 및 Pro 통합 환경에서 CLI 명령을 실행하여 디버그합니다.
다음과 같은 런타임 옵션을 추가할 수 있습니다.

   ```bash
   php -d xdebug.profiler_enable=On -d xdebug.max_nesting_level=9999 bin/magento cache:clean
   ```

   Pro Staging 및 Production 환경에서는 다음과 같이 CLI 명령을 디버깅할 때 [!DNL Xdebug] PHP 구성 파일의 경로를 지정해야 합니다.

   ```bash
   php -c /etc/platform/USERNAME/php.xdebug.ini bin/magento cache:clean
   ```

## 웹 요청 디버그

다음 단계는 웹 요청을 디버깅하는 데 도움이 됩니다.

1. _확장_ 메뉴에서 **디버그**&#x200B;를 클릭하여 사용하도록 설정합니다.

1. 마우스 오른쪽 단추를 클릭하고 옵션 메뉴를 선택한 다음 IDE 키를 **PHPSTORM**(으)로 설정합니다.

1. 브라우저에 [!DNL Xdebug] 클라이언트를 설치합니다. 구성 및 활성화합니다.

### 예: Chrome 설정

이 섹션에서는 [!DNL Xdebug] Helper 확장을 사용하여 Chrome에서 [!DNL Xdebug]을(를) 사용하는 방법에 대해 설명합니다. 다른 브라우저의 [!DNL Xdebug] 도구에 대한 자세한 내용은 브라우저 설명서를 참조하십시오.

**Chrome에서 Xdebug 도우미를 사용하려면**:

1. 클라우드 서버에 대한 [SSH 터널](#ssh-access-to-xdebug-environments)을(를) 만듭니다.

1. Chrome 스토어에서 [Xdebug Helper 확장 기능](https://chromewebstore.google.com/detail/eadndfjplgieldjbigjakmdgkmoaaaoc)을 설치하십시오.

1. 다음 그림과 같이 Chrome에서 확장을 활성화합니다.

   ![Chrome에서 Xdebug 확장 사용](../../assets/xdebug/enable-chrome-ext.png)

1. Chrome에서 Chrome 도구 모음의 녹색 도우미 아이콘을 마우스 오른쪽 단추로 클릭합니다.

1. 팝업 메뉴에서 **옵션**&#x200B;을 클릭합니다.

1. _IDE 키_ 목록에서 **PhpStorm**&#x200B;을 클릭합니다.

1. **저장**&#x200B;을 클릭합니다.

   ![Xdebug 도우미 옵션](../../assets/xdebug/helper-options.png)

1. PhpStorm 프로젝트를 엽니다.

1. 위쪽 탐색 모음에서 **수신 시작** 아이콘을 클릭합니다.

   탐색 모음이 표시되지 않으면 **보기** > **탐색 모음**&#x200B;을 클릭하세요.

1. PhpStorm 탐색 창에서 테스트할 PHP 파일을 두 번 클릭합니다.

## 로컬 코드 디버그

읽기 전용 환경으로 인해 디버깅을 수행하려면 코드를 환경 또는 특정 Git 분기에서 로컬 워크스테이션으로 가져와야 합니다.

선택하는 방법은 사용자가 결정합니다. 다음과 같은 옵션이 있습니다.

- Git에서 코드를 체크아웃하고 `composer install`을(를) 실행합니다.

  이 메서드는 `composer.json`이(가) 액세스 권한이 없는 개인 저장소의 패키지를 참조하지 않는 한 작동합니다. 이 메서드는 전체 Adobe Commerce 코드베이스를 가져옵니다.

- `vendor`, `app`, `pub`, `lib` 및 `setup` 디렉터리 복사

  이 방법을 사용하면 테스트할 수 있는 모든 코드가 있습니다. 보유한 정적 에셋 수에 따라 대량의 파일로 장기간 전송할 수 있습니다.

- `vendor` 디렉터리만 복사

  대부분의 코드가 `vendor` 디렉터리에 있으므로 이 메서드는 전체 코드베이스를 테스트하지는 않지만 양호한 테스트를 초래할 수 있습니다.

**파일을 압축하여 로컬 컴퓨터에 복사하려면**:

1. SSH를 사용하여 원격 환경에 로그인합니다.

1. 파일을 압축합니다.

   ```bash
   tar -czf /tmp/<file-name>.tgz <directory list>
   ```

   예를 들어 `vendor` 디렉터리만 압축하려면 다음을 수행합니다.

   ```bash
   tar -czf /tmp/vendor.tgz vendor
   ```

1. 로컬 환경에서 PhpStorm을 사용하여 파일을 압축합니다.

   ```bash
   cd <phpstorm project root dir>
   ```

   ```bash
   rsync <SSH-URL>:/tmp/<file-name>.tgz .
   ```

   ```bash
   tar xzf <file-name>.tgz
   ```
