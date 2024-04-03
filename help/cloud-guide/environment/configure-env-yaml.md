---
title: 환경 구성
description: 환경 변수를 사용하여 Pro Staging 및 프로덕션을 포함하여 클라우드 인프라 환경의 모든 Commerce에 걸쳐 빌드 및 배포 작업을 구성하는 방법에 대해 알아봅니다.
feature: Cloud, Build, Configuration, Deploy, SCD
role: Developer
exl-id: 66e257e2-1eca-4af5-9b56-01348341400b
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '697'
ht-degree: 0%

---

# 배포를 위한 환경 변수 구성

다음 `.magento.env.yaml` 파일은 환경 변수를 사용하여 Pro Staging 및 Production을 비롯한 모든 환경에서 빌드 및 배포 작업을 중앙 집중식으로 관리합니다. 각 환경에서 고유한 작업을 구성하려면 각 환경에서 이 파일을 수정해야 합니다.

>[!TIP]
>
>YAML 파일은 대/소문자를 구분하므로 탭을 허용하지 않습니다. 전체에서 일관된 들여쓰기를 사용하도록 주의하십시오. `.magento.env.yaml` 파일 또는 구성이 예상대로 작동하지 않을 수 있습니다. 설명서 및 샘플 파일의 예제는 _이중 공간_ 들여쓰기. 사용 [ece-tools validate 명령](#validate-configuration-file) 구성을 확인합니다.

## 파일 구조

다음 `.magento.env.yaml` 파일에는 두 개의 섹션이 포함되어 있습니다. `stage` 및 `log`. 다음 `stage` 섹션은 다음 단계 동안 발생하는 작업을 제어합니다. [클라우드 배포 프로세스](../deploy/process.md).

- `stage`- 스테이지 섹션을 사용하여 다음 배포 단계에 대한 특정 작업을 정의합니다.
   - `global`- 빌드, 배포 및 배포 후 단계 모두에서 작업을 제어합니다. 빌드, 배포 및 사후 배포 섹션에서 이러한 설정을 재정의할 수 있습니다.
   - `build`- 빌드 단계에서만 작업을 제어합니다. 이 섹션에서 설정을 지정하지 않으면 빌드 단계에서 전역 섹션의 설정을 사용합니다.
   - `deploy`- 배포 단계의 작업만 제어합니다. 이 섹션에서 설정을 지정하지 않으면 배포 단계에서 전역 섹션의 설정을 사용합니다.
   - `post-deploy`- 작업을 제어합니다. _이후_ 애플리케이션 배포 및 _이후_ 컨테이너가 연결을 수락하기 시작합니다.
- `log`—로그 섹션을 사용하여 다음을 구성합니다 [알림](set-up-notifications.md)알림 유형 및 세부 정보 수준을 포함합니다.
   - `slack`—Slack 봇에 보낼 메시지를 구성합니다.
   - `email`—하나 이상의 이메일 수신자에게 전송할 이메일을 구성합니다.
   - [로그 처리기](log-handlers.md)—원격 로깅 서버로 전송되는 하드웨어 및 소프트웨어 애플리케이션 메시지를 구성합니다.

### 환경 변수

다음 `ece-tools` 패키지는 `env.php` 의 값을 기반으로 한 파일 [클라우드 변수](variables-cloud.md), 변수에 설정된 값 [!DNL Cloud Console]및 `.magento.env.yaml` 구성 파일입니다. 의 환경 변수 `.magento.env.yaml` 파일 기존 Commerce 구성을 재정의하여 클라우드 환경을 사용자 정의합니다. 기본값이 `Not Set`, 그런 다음 `ece-tools` 패키지 테이크 **아니요** 작업 및 사용 [!DNL Commerce] 기본값 또는 MAGENTO_CLOUD_RELATIONSHIPS 구성의 값입니다. 기본값이 설정되면 `ece-tools` 패키지는 해당 기본값을 설정하는 역할을 합니다.

다음 항목에는 기본값이 설정되었는지 여부와 같이, 에서 사용할 수 있는 모든 변수에 대한 자세한 정의가 포함되어 있습니다. `.magento.env.yaml` 파일:

- [글로벌](variables-global.md)—변수는 빌드, 배포 및 배포 후 각 단계에서 작업을 제어합니다.
- [빌드](variables-build.md)—변수가 빌드 작업을 제어합니다.
- [배포](variables-deploy.md)—변수 제어 배포 작업
- [Post-deploy](variables-post-deploy.md)- 변수는 배포 후 작업을 제어합니다.

### CLI에서 구성 파일 생성

다음을 생성할 수 있습니다. `.magento.env.yaml` 다음을 사용하는 클라우드 환경용 구성 파일 `ece-tools` 명령입니다.

>구성 파일 만들기

```bash
php ./vendor/bin/ece-tools cloud:config:create `<configuration-json>`
```

>구성 파일의 값 업데이트

```bash
php ./vendor/bin/ece-tools cloud:config:update `<configuration-json>`
```

두 명령 모두 하나 이상의 빌드, 배포 또는 배포 후 변수에 대한 값을 지정하는 JSON 형식의 배열이라는 단일 인수가 필요합니다. 예를들어, 다음 명령은 `SCD_THREADS` 및 `CLEAN_STATIC_FILES` 변수:

```bash
php vendor/bin/ece-tools cloud:config:create '{"stage":{"build":{"SCD_THREADS":5}, "deploy":{"CLEAN_STATIC_FILES":false}}}'
```

및 를 만듭니다. `.magento.env.yaml` 다음 설정을 사용하는 파일:

```yaml
stage:
  build:
    SCD_THREADS: 5
  deploy:
    CLEAN_STATIC_FILES: false
```

다음을 사용할 수 있습니다. `cloud:config:update` 새 파일을 업데이트하는 명령입니다. 예를 들어 다음 명령은 `SCD_THREADS` 값을 반환하고 를 추가합니다. `SCD_COMPRESSION_TIMEOUT` 구성:

```bash
php vendor/bin/ece-tools cloud:config:update '{"stage":{"build":{"SCD_THREADS":3, "SCD_COMPRESSION_TIMEOUT":1000}}}'
```

업데이트된 파일에는 다음 구성이 포함됩니다.

```yaml
stage:
  build:
    SCD_THREADS: 3
    SCD_COMPRESSION_TIMEOUT: 1000
  deploy:
    CLEAN_STATIC_FILES: false
```

### 구성 파일 유효성 검사

다음 사용 `ece-tools` 유효성 검사 명령 `.magento.env.yaml` 원격 클라우드 환경에 변경 사항을 푸시하기 전에 구성 파일.

```bash
php ./vendor/bin/ece-tools cloud:config:validate
```

다음 샘플 응답은 수정할 항목 목록을 제공합니다.

```terminal
Environment configuration is not valid. Correct the following items in your .magento.env.yaml file:
The SCD_THREADS variable contains an invalid value of type string. Use the following type: integer.
The SCD_STRATEGY variable contains an invalid value fast. Use one of the available value options: compact, quick, standard.
The NOT_EXIST_OPTION variable is not allowed in configuration.
```

## PHP 상수

에서 PHP 상수를 사용할 수 있습니다. `.magento.env.yaml` 값을 하드 코딩하는 대신 파일 정의를 사용하십시오. 다음 예제에서는 `driver_options` php 상수 사용:

```yaml
stage:
  deploy:
    DATABASE_CONFIGURATION:
      connection:
        default:
          driver_options:
            !php/const:\PDO::MYSQL_ATTR_LOCAL_INFILE : 1
        indexer:
          driver_options:
            !php/const:\PDO::MYSQL_ATTR_LOCAL_INFILE : 1
      _merge: true
```

>[!WARNING]
>
>를 사용할 때 상수 구문 분석이 작동하지 않음 `symfony/yaml` 3.2 이전 패키지 버전.

## 오류 처리

에 예기치 않은 값이 있어 오류가 발생하는 경우 `.magento.env.yaml` 구성 파일에서 오류 메시지가 표시됩니다. 예를 들어, 다음 오류 메시지는 예기치 않은 값이 있는 각 항목에 대해 제안된 변경 사항 목록을 나타내며 경우에 따라 유효한 옵션을 제공합니다.

```terminal
- Environment configuration is not valid. Please correct .magento.env.yaml file with next suggestions:
  Item CRON_CONSUMERS_RUNNER is not supposed to be in stage build. Please move it to one of possible stages: global, deploy
  Item SKIP_SCD has unexpected type string. Please use one of next types: boolean
  Item VERBOSE_COMMANDS has unexpected type boolean. Please use one of next types: string
  Item SKIP_HTML_MINIFICATION has unexpected type string. Please use one of next types: boolean
  Item CRON_CONSUMERS_RUNNER has unexpected type boolean. Please use one of next types: array
  Item VAR_WARM_UP_PAGES is not allowed in configuration.
  Item WARM_UP_PAGES has unexpected type string. Please use one of next types: array
```

모든 수정, 커밋 및 변경 사항 푸시 오류 메시지가 표시되지 않으면 구성 파일의 변경 사항이 유효성 검사를 통과합니다.

## 구성 관리 최적화

구성을 덤프한 후 구성 관리를 활성화한 경우 SCD_* 변수를 배포에서 빌드 단계로 이동해야 합니다. 다음을 참조하십시오 [정적 콘텐츠 배포 전략](../deploy/static-content.md).

>구성 관리 전:

```yaml
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      consumers: []
    SCD_STRATEGY: compact
    SCD_MATRIX:
      ...
    REDIS_USE_SLAVE_CONNECTION: 1
```

>구성 관리를 활성화한 후 SCD_* 변수를 빌드 단계로 이동합니다.

```yaml
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      consumers: []
    REDIS_USE_SLAVE_CONNECTION: 1
  build:
    SCD_STRATEGY: compact
    SCD_MATRIX:
      ...
```
