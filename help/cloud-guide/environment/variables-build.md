---
title: 변수 작성
description: Adobe Commerce on cloud infrastructure 빌드 단계의 작업을 제어하는 환경 변수 목록을 참조하십시오.
feature: Cloud, Configuration, Build, SCD, Upgrade
recommendations: noDisplay, catalog
role: Developer
exl-id: 243aaa45-a5ef-4ed2-8800-3d0f07bf3740
source-git-commit: 7b9c6a4cd17069c25455195bd8f273664b8a29eb
workflow-type: tm+mt
source-wordcount: '920'
ht-degree: 0%

---

# 변수 작성

다음 _build_ 변수는 빌드 단계에서 작업을 제어하며 [전역 변수](variables-global.md)에서 값을 상속하고 재정의할 수 있습니다. `.magento.env.yaml` 파일의 `build` 단계에 다음 변수를 삽입합니다.

```yaml
stage:
  build:
    BUILD_VARIABLE_NAME: value
```

빌드 및 배포 프로세스 사용자 지정에 대한 자세한 내용은 다음을 참조하십시오.

- [배포 구성](configure-env-yaml.md)
- [배포 프로세스](../deploy/process.md)

다음 변수가 v2.2에서 제거되었습니다.

- `skip_di_clearing`
- `skip_di_compilation`

## `ERROR_REPORT_DIR_NESTING_LEVEL`

- **기본값**—`1`
- **버전**—Adobe Commerce 2.1.4 이상

보고서 디렉터리에 수만 개의 파일이 채워지지 않도록 오류 보고서 파일을 저장하기 위해 디렉터리 중첩 수준을 설정합니다. 이렇게 하면 데이터를 관리하고 검토하는 데 어려움이 있을 수 있습니다. 이 설정의 기본값은 `1`입니다. 일반적으로 `<magento_root>/var/report/` 디렉터리에서 오류 보고서 파일을 관리하는 데 문제가 없으면 기본값을 변경할 필요가 없습니다.

```yaml
stage:
  build:
    ERROR_REPORT_DIR_NESTING_LEVEL: 2
```

## `QUALITY_PATCHES`

- **기본값**—_설정되지 않음_
- **버전**—Adobe Commerce 2.1.4 이상

배포 중에 적용할 Adobe Commerce 품질 패치 목록을 지정합니다.

```yaml
stage:
  build:
    QUALITY_PATCHES: [ ]
```

다음 예제에서는 배포 중에 적용할 세 개의 패치를 지정합니다.

```yaml
stage:
  build:
    QUALITY_PATCHES:
      - MC-31387
      - MDVA-4567
      - MC-456345
```

[패치 적용](../development/apply-patches.md)을 참조하세요.

## `SCD_COMPRESSION_LEVEL`

- **기본값**—`6`
- **버전**—Adobe Commerce 2.1.4 이상

정적 콘텐츠를 압축할 때 사용할 [gzip](https://www.gnu.org/software/gzip) 압축 수준(`0` ~ `9`)을 지정합니다. `0`은(는) 압축을 사용하지 않도록 설정합니다.

```yaml
stage:
  build:
    SCD_COMPRESSION_LEVEL: 4
```

## `SCD_COMPRESSION_TIMEOUT`

- **기본값**—`600`
- **버전**—Adobe Commerce 2.1.4 이상

정적 자산을 압축하는 데 걸리는 시간이 압축 시간 제한 을 초과하면 배포 프로세스를 중단합니다. 정적 콘텐츠 압축 명령의 최대 실행 시간(초)을 설정합니다.

```yaml
stage:
  build:
    SCD_COMPRESSION_TIMEOUT: 800
```

## `SCD_NO_PARENT`

- **기본값**—`false`
- **버전**—Adobe Commerce 2.4.2 이상

빌드 단계에서 상위 테마에 대한 정적 콘텐츠를 생성하지 않도록 하려면 `true`(으)로 설정하십시오.

상위 테마에 대한 정적 콘텐츠를 생성해도 사이트 배포에 영향을 주지 않거나 불필요한 사이트 다운타임이 발생하지 않도록 빌드 단계 동안 `SCD_NO_PARENT: false`을(를) 설정하십시오. [정적 콘텐츠 배포](../deploy/static-content.md)를 참조하세요.

```yaml
stage:
  build:
    SCD_NO_PARENT: false
```

## `SCD_MATRIX`

- **기본값**—_설정되지 않음_
- **버전**—Adobe Commerce 2.1.4 이상

테마당 여러 로케일을 구성할 수 있습니다. 이 맞춤화는 불필요한 테마 파일의 수를 줄여 빌드 프로세스를 가속화하는 데 도움이 됩니다. 예를 들어 _magento/backend_ 테마는 영어로 작성하고 사용자 지정 테마는 다른 언어로 작성할 수 있습니다.

다음 예제에서는 세 개의 로케일로 `Magento/backend` 테마를 빌드합니다.

```yaml
stage:
  build:
    SCD_MATRIX:
      "Magento/backend":
        language:
          - en_US
          - fr_FR
          - af_ZA
```

다음 예제에서는 로케일이 세 개인 세 개의 테마를 빌드합니다.

```yaml
stage:
  build:
    SCD_MATRIX:
      "Magento/backend":
        language:
          - en_US
          - fr_FR
          - af_ZA
      "Magento/blank":
        language:
          - en_US
          - fr_FR
          - af_ZA
      "Magento/luma":
        language:
          - en_US
          - fr_FR
          - af_ZA
```

또는 테마를 _배포할 수 없음_&#x200B;을 선택할 수 있습니다.

```yaml
stage:
  build:
    SCD_MATRIX:
      "Magento/backend": [ ]
```

## `SCD_MAX_EXECUTION_TIME`

- **기본값**—_설정되지 않음_
- **버전**—Adobe Commerce 2.2.0 이상

정적 콘텐츠 배포에 대한 최대 예상 실행 시간을 늘릴 수 있습니다.

기본적으로 클라우드 인프라의 Adobe Commerce은 예상 최대 실행을 900초로 설정하지만, 일부 시나리오에서는 클라우드 프로젝트에 대한 정적 콘텐츠 배포를 완료하는 데 더 많은 시간이 필요할 수 있습니다.

```yaml
stage:
  build:
    SCD_MAX_EXECUTION_TIME: 3600
```

{{scd-timing-warning}}

## `SCD_STRATEGY`

- **기본값**—`quick`
- **버전**—Adobe Commerce 2.2.0 이상

정적 콘텐츠에 대한 [배포 전략](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-strategy.html)을(를) 사용자 지정하십시오. [정적 보기 파일 배포](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html)를 참조하세요.

로케일이 두 개 이상인 경우 _only_ 옵션을 사용합니다.

- `standard` - 모든 패키지에 대한 모든 정적 보기 파일을 배포합니다.
- `quick`—(_default_)은(는) 배포 시간을 최소화합니다.
- `compact` - 서버의 디스크 공간을 절약합니다. Adobe Commerce 버전 2.2.4 및 이전 버전에서 이 설정은 값이 `1`인 `scd_threads`의 값을 재정의합니다.

```yaml
stage:
  build:
    SCD_STRATEGY: "compact"
```

## `SCD_THREADS`

- **기본값**—자동
- **버전**—Adobe Commerce 2.1.4 이상

정적 콘텐츠 배포를 위한 스레드 수를 설정합니다. 기본값은 감지된 CPU 스레드 수를 기준으로 설정되며 값 4를 초과하지 않습니다. 스레드 수를 늘리면 정적 콘텐츠 배포 속도가 빨라지고 스레드 수를 줄이면 속도가 느려집니다. 다음과 같이 스레드 값을 설정할 수 있습니다.

```yaml
stage:
  build:
    SCD_THREADS: 2
```

배포 시간을 더 줄이려면 `scd-dump` 명령과 함께 [구성 관리](../store/store-settings.md)를 사용하여 정적 배포를 빌드 단계로 이동하십시오.

## `SCD_USE_BALER`

- **기본값**—_설정되지 않음_
- **버전**—Adobe Commerce 2.3.0 이상

[Baler](https://github.com/magento/baler)은(는) 생성된 JavaScript 코드를 스캔하고 최적화된 JavaScript 번들을 만듭니다. 사이트에 최적화된 번들을 배포하면 사이트를 로드할 때 네트워크 요청 수를 줄이고 페이지 로드 시간을 향상시킬 수 있습니다.

정적 콘텐츠 배포를 수행한 후 발레를 실행하려면 `true`(으)로 설정합니다.

```yaml
stage:
  build:
    SCD_USE_BALER: true
```

>[!NOTE]
>
>Baler는 알파 릴리스이므로 프로덕션 환경에서 사용하는 것은 권장되지 않습니다.

## `SKIP_COMPOSER_DUMP_AUTOLOAD`

- **기본값**— _설정되지 않음_
- **버전**—Adobe Commerce 2.1.4 이상

Cloud Docker 설치 중에 `composer dump-autoload` 명령을 건너뛰려면 `true`(으)로 설정하십시오. 이 변수는 쓰기 가능한 파일 시스템이 있는 Cloud Docker 컨테이너에만 관련이 있습니다. 이러한 경우 명령을 건너뛰면 삭제된 `generated` 디렉터리의 코드에 액세스하려는 다른 명령에 오류가 발생하지 않습니다.

Adobe Commerce에서 `composer dump-autoload`을(를) 실행하면 `generated` 폴더에 생성된 클래스에 대한 링크가 있는 자동 로드 파일이 생성되며, 이는 읽기 전용 파일 시스템을 사용하는 프로덕션 환경에서는 문제가 되지 않습니다. 그러나 쓰기 가능한 파일 시스템이 있는 Cloud Docker 설치(`./vendor/bin/ece-docker build:compose --with-test`을(를) 사용하여 테스트 및 개발용으로만 생성됨)의 경우 `generated` 디렉터리를 삭제하는 `--keep-generated` 옵션 없이 `bin/magento -n setup:upgrade` 명령을 실행할 수 있습니다. 디렉터리를 삭제하면 자동 로드에 삭제된 디렉터리에 있는 파일에 대한 링크가 포함되어 있으므로 `composer dump-autoload` 명령이 실패합니다.

```yaml
stage:
  build:
    SKIP_COMPOSER_DUMP_AUTOLOAD: true
```

## `SKIP_SCD`

- **기본값**— _설정되지 않음_
- **버전**—Adobe Commerce 2.1.4 이상

빌드 단계 동안 정적 콘텐츠 배포를 건너뛰려면 `true`(으)로 설정하십시오.

[구성 관리](../store/store-settings.md)를 사용하여 빌드 단계 중에 정적 콘텐츠를 이미 배포한 경우 빠른 빌드 테스트를 위해 정적 콘텐츠 배포를 건너뛸 수 있습니다.

빌드 단계에서 정적 콘텐츠 빌드가 빌드 단계 중에 발생하도록 `SKIP_SCD: false`을(를) 설정합니다. 이 단계에서 프로세스가 사이트 배포에 영향을 주지 않거나 불필요한 사이트 가동 중지 시간이 발생하지 않습니다. [정적 콘텐츠 배포](../deploy/static-content.md)를 참조하세요.

```yaml
stage:
  build:
    SKIP_SCD: false
```

## `VERBOSE_COMMANDS`

- **기본값**—_설정되지 않음_
- **버전**—Adobe Commerce 2.1.4 이상

배포 단계에서 수행되는 `bin/magento` CLI 명령에 대해 [Symfony](https://symfony.com/doc/current/console/verbosity.html) 디버그 세부 정보 수준을 활성화하거나 비활성화합니다.

>[!NOTE]
>
>VERBOSE_COMMANDS를 사용하여 성공 및 실패한 `bin/magento` CLI 명령에 대한 명령 출력의 세부 정보를 제어하려면 [MIN_LOGGING_LEVEL](variables-global.md#minlogginglevel) `debug`을(를) 설정해야 합니다.

로그에 제공되는 세부 사항 수준을 선택합니다.

- `-v`= 일반 출력
- `-vv`= 더 자세한 출력
- `-vvv` = 디버그에 적합한 세부 정보 출력

```yaml
stage:
  build:
    VERBOSE_COMMANDS: "-vv"
```
