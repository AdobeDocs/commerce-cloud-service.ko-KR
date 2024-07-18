---
title: 환경 구성
description: 환경 변수를 사용하여 Pro 스테이징 및 프로덕션을 포함하여 클라우드 인프라 환경의 모든 Commerce에 빌드 및 배포 작업을 구성하는 방법에 대해 알아봅니다.
feature: Cloud, Build, Configuration, Deploy, SCD
role: Developer
exl-id: 66e257e2-1eca-4af5-9b56-01348341400b
source-git-commit: b49a51aba56f79b5253eeacb1adf473f42bb8959
workflow-type: tm+mt
source-wordcount: '697'
ht-degree: 0%

---

# 배포를 위한 환경 변수 구성

`.magento.env.yaml` 파일은 환경 변수를 사용하여 Pro Staging 및 Production을 비롯한 모든 환경에서 빌드 및 배포 작업을 중앙 집중식으로 관리합니다. 각 환경에서 고유한 작업을 구성하려면 각 환경에서 이 파일을 수정해야 합니다.

>[!TIP]
>
>YAML 파일은 대/소문자를 구분하므로 탭을 허용하지 않습니다. `.magento.env.yaml` 파일 전체에서 일관된 들여쓰기를 사용하십시오. 그렇지 않으면 구성이 예상대로 작동하지 않을 수 있습니다. 설명서 및 샘플 파일의 예제에서는 _두 개의 공간_ 들여쓰기를 사용합니다. [ece-tools validate 명령](#validate-configuration-file)을 사용하여 구성을 확인하십시오.

## 파일 구조

`.magento.env.yaml` 파일에 두 개의 섹션(`stage` 및 `log`)이 있습니다. `stage` 섹션은 [클라우드 배포 프로세스](../deploy/process.md)의 단계 동안 발생하는 작업을 제어합니다.

- `stage` - 단계 섹션을 사용하여 다음 배포 단계에 대한 특정 작업을 정의합니다.
   - `global` - 빌드, 배포 및 배포 후 단계 모두에서 작업을 제어합니다. 빌드, 배포 및 사후 배포 섹션에서 이러한 설정을 재정의할 수 있습니다.
   - `build` - 빌드 단계에서만 작업을 제어합니다. 이 섹션에서 설정을 지정하지 않으면 빌드 단계에서 전역 섹션의 설정을 사용합니다.
   - `deploy` - 배포 단계의 작업만 제어합니다. 이 섹션에서 설정을 지정하지 않으면 배포 단계에서 전역 섹션의 설정을 사용합니다.
   - `post-deploy` - 응용 프로그램을 배포하는 _후_ 및 컨테이너가 연결을 수락하는 _후_ 작업을 제어합니다.
- `log`—로그 섹션을 사용하여 알림 유형 및 세부 정보 수준을 포함하여 [알림](set-up-notifications.md)을(를) 구성합니다.
   - `slack`—Slack 봇에 보낼 메시지를 구성합니다.
   - `email` - 하나 이상의 전자 메일 받는 사람에게 보낼 전자 메일을 구성합니다.
   - [로그 처리기](log-handlers.md)—원격 로깅 서버로 전송되는 하드웨어 및 소프트웨어 응용 프로그램 메시지를 구성합니다.

### 환경 변수

`ece-tools` 패키지는 [클라우드 변수](variables-cloud.md), [!DNL Cloud Console]에 설정된 변수 및 `.magento.env.yaml` 구성 파일의 값을 기반으로 `env.php` 파일에 값을 설정합니다. `.magento.env.yaml` 파일의 환경 변수는 기존 Commerce 구성을 재정의하여 클라우드 환경을 사용자 지정합니다. 기본값이 `Not Set`이면 `ece-tools` 패키지는 **NO** 동작을 수행하고 [!DNL Commerce] 기본값 또는 MAGENTO_CLOUD_RELATIONSHIPS 구성의 값을 사용합니다. 기본값이 설정되면 `ece-tools` 패키지가 해당 기본값을 설정하는 역할을 합니다.

다음 항목에는 `.magento.env.yaml` 파일에서 사용할 수 있는 모든 변수에 대해 기본값이 설정되었는지 여부와 같은 자세한 정의가 포함됩니다.

- [전역](variables-global.md)—변수는 빌드, 배포 및 배포 후 각 단계에서 작업을 제어합니다.
- [빌드](variables-build.md)—변수가 빌드 작업을 제어합니다.
- [배포](variables-deploy.md)—변수가 배포 작업을 제어합니다.
- [사후 배포](variables-post-deploy.md)—변수는 배포 후 작업을 제어합니다.

### CLI에서 구성 파일 생성

다음 `ece-tools` 명령을 사용하여 클라우드 환경에 대한 `.magento.env.yaml` 구성 파일을 생성할 수 있습니다.

>구성 파일 만들기

```bash
php ./vendor/bin/ece-tools cloud:config:create `<configuration-json>`
```

>구성 파일의 값 업데이트

```bash
php ./vendor/bin/ece-tools cloud:config:update `<configuration-json>`
```

두 명령 모두 하나 이상의 빌드, 배포 또는 배포 후 변수에 대한 값을 지정하는 JSON 형식의 배열이라는 단일 인수가 필요합니다. 예를 들어 다음 명령은 `SCD_THREADS` 및 `CLEAN_STATIC_FILES` 변수에 대한 값을 설정합니다.

```bash
php vendor/bin/ece-tools cloud:config:create '{"stage":{"build":{"SCD_THREADS":5}, "deploy":{"CLEAN_STATIC_FILES":false}}}'
```

다음 설정으로 `.magento.env.yaml` 파일을 만듭니다.

```yaml
stage:
  build:
    SCD_THREADS: 5
  deploy:
    CLEAN_STATIC_FILES: false
```

`cloud:config:update` 명령을 사용하여 새 파일을 업데이트할 수 있습니다. 예를 들어 다음 명령은 `SCD_THREADS` 값을 변경하고 `SCD_COMPRESSION_TIMEOUT` 구성을 추가합니다.

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

원격 클라우드 환경에 변경 사항을 푸시하기 전에 다음 `ece-tools` 명령을 사용하여 `.magento.env.yaml` 구성 파일의 유효성을 검사하십시오.

```bash
php ./vendor/bin/ece-tools cloud:config:validate
```

다음 샘플 응답은 수정할 항목 목록을 제공합니다.

```
Environment configuration is not valid. Correct the following items in your .magento.env.yaml file:
The SCD_THREADS variable contains an invalid value of type string. Use the following type: integer.
The SCD_STRATEGY variable contains an invalid value fast. Use one of the available value options: compact, quick, standard.
The NOT_EXIST_OPTION variable is not allowed in configuration.
```

## PHP 상수

하드 코딩 값 대신 `.magento.env.yaml` 파일 정의에 PHP 상수를 사용할 수 있습니다. 다음 예제에서는 PHP 상수를 사용하여 `driver_options`을(를) 정의합니다.

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
>3.2 이전 버전의 `symfony/yaml` 패키지를 사용하는 경우 상수 구문 분석이 작동하지 않습니다.

## 오류 처리

`.magento.env.yaml` 구성 파일에 예기치 않은 값이 있어 오류가 발생하면 오류 메시지가 표시됩니다. 예를 들어, 다음 오류 메시지는 예기치 않은 값이 있는 각 항목에 대해 제안된 변경 사항 목록을 나타내며 경우에 따라 유효한 옵션을 제공합니다.

```
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

구성을 덤프한 후 구성 관리를 활성화한 경우 SCD_* 변수를 배포에서 빌드 단계로 이동해야 합니다. [정적 콘텐츠 배포 전략](../deploy/static-content.md)을 참조하세요.

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
