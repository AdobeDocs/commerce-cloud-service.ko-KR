---
title: 전역 변수
description: Adobe Commerce on cloud infrastructure 배포 프로세스에서 작업을 제어하는 환경 변수 목록을 참조하십시오.
feature: Cloud, Configuration, Build, Deploy, Eventing, Logs, SCD
recommendations: noDisplay, catalog
role: Developer
exl-id: 04c2861d-746d-42d4-a678-f6c7b464eb51
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '752'
ht-degree: 0%

---

# 전역 변수

전역 변수는 [!DNL Commerce] 배포 프로세스의 각 단계(빌드, 배포 및 배포 후)에서 작업을 제어합니다. 전역 변수는 모든 단계에 영향을 주므로 `.magento.env.yaml` 파일의 `global` 단계에서 전역 변수를 설정해야 합니다.

```yaml
stage:
  global:
    GLOBAL_VARIABLE_NAME: value
```

빌드 및 배포 프로세스 사용자 지정에 대한 자세한 내용은 다음을 참조하십시오.

- [배포 구성](configure-env-yaml.md)
- [배포 프로세스](../deploy/process.md)

## `ENABLE_EVENTING`

- **기본값**-_설정되지 않음_
- **버전**—Adobe Commerce 2.4.5 이상

`true`(으)로 설정된 경우 cron에서 메시지 큐 소비자를 실행할 수 있도록 설정합니다. Adobe Commerce에 대한 Adobe I/O 이벤트는 메시지 대기열을 사용하여 중요한 이벤트를 신속하게 전달할 수 있습니다.

Adobe은 `cron_run`이(가) `true`(으)로 설정된 `.magento.env.yaml` 파일의 `deploy` 단계에도 [`CRON_CONSUMERS_RUNNER`](./variables-deploy.md#cron_consumers_runner) 변수를 추가할 것을 권장합니다.

다음 예제에서는 완전히 구성된 `ENABLE_EVENTING` 변수를 보여 줍니다.

```yaml
stage:
  global:
    ENABLE_EVENTING: true
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      max_messages: 0
      consumers: []
```

## ENABLE_WEBHOOKS

- **기본값**-_설정되지 않음_
- **버전**—Adobe Commerce 2.4.4 이상

`true`(으)로 설정된 경우 Commerce 웹후크를 활성화합니다. 웹후크는 App Builder 런타임 작업 또는 서드파티 인벤토리 관리 시스템과 같은 외부 엔드포인트에서 실행됩니다. [_Webhooks 안내서_](https://developer.adobe.com/commerce/extensibility/webhooks)에서는 이 기능에 대해 자세히 설명합니다.

```yaml
stage:
  global:
    ENABLE_WEBHOOKS: true
```

## `MIN_LOGGING_LEVEL`

- **기본값**—_설정되지 않음_
- **버전**—Adobe Commerce 2.1.4 이상

코드를 변경하지 않고 모든 출력 스트림에 대한 최소 로깅 수준을 무시합니다. 이렇게 하면 배포 문제를 해결할 때 도움이 됩니다. 예를 들어 배포에 실패하는 경우 이 변수를 사용하여 로깅 세부기간을 전체적으로 늘릴 수 있습니다. [로그 수준](log-handlers.md#log-levels)을 참조하세요. 로깅 처리기의 `min_level` 값이 이 설정을 덮어씁니다.

```yaml
stage:
  global:
    MIN_LOGGING_LEVEL: debug
```

>[!WARNING]
>
>`MIN_LOGGING_LEVEL` 변수에 대한 설정은 기본적으로 `debug`(으)로 설정된 파일 처리기의 로그 수준 구성을 변경하지 않습니다.

## `SCD_ON_DEMAND`

- **기본값**—_설정되지 않음_
- **버전**—Adobe Commerce 2.1.4 이상

사용자(SCD)가 요청한 경우 정적 콘텐츠 생성을 활성화합니다. 주문형 정적 콘텐츠는 배포 시간이 감소하므로 개발 및 테스트 워크플로에 이상적입니다.

[`post_deploy` 후크](../application/hooks-property.md)을(를) 사용하여 캐시를 미리 로드하면 사이트 가동 중지 시간이 줄어듭니다. 캐시 보온은 [!DNL Cloud Console]의 스테이징 및 프로덕션 환경이 포함된 Pro 프로젝트와 시작 프로젝트에만 사용할 수 있습니다. `SCD_ON_DEMAND` 환경 변수를 `.magento.env.yaml` 파일의 `global` 단계에 추가합니다.

```yaml
stage:
  global:
    SCD_ON_DEMAND: true
```

`SCD_ON_DEMAND` 변수는 두 단계(빌드 및 배포)에서 SCD를 건너뛰고 `pub/static` 및 `var/view_preprocessed` 폴더를 지우고 `app/etc/env.php` 파일에 다음을 씁니다.

```php?start_inline=1
return array(
   ...
   'static_content_on_demand_in_production' => 1,
   ...
);
```

## `SCD_MAX_EXECUTION_TIME`

- **기본값**—_설정되지 않음_
- **버전**—Adobe Commerce 2.2.0 이상

정적 콘텐츠 배포에 대한 최대 예상 실행 시간을 늘릴 수 있습니다.

기본적으로 Adobe Commerce은 최대 예상 실행을 900초로 설정하지만, 일부 시나리오에서는 클라우드 프로젝트에 대한 정적 콘텐츠 배포를 완료하는 데 더 많은 시간이 필요할 수 있습니다.

```yaml
stage:
  global:
    SCD_MAX_EXECUTION_TIME: 3600
```

{{scd-timing-warning}}

## `SCD_NO_PARENT`

- **기본값**—_설정되지 않음_
- **버전**—Adobe Commerce 2.4.2 이상

빌드 및 배포 단계에서 부모 테마에 대한 정적 콘텐츠를 생성하지 않도록 하려면 `true`(으)로 설정하십시오. 이 옵션을 `true`(으)로 설정하면 정적 콘텐츠가 덜 생성되므로 전체 빌드 및 배포 시간이 향상됩니다.

```yaml
stage:
  global:
    SCD_NO_PARENT: true
```

## `SCD_USE_BALER`

- **기본값**—_설정되지 않음_
- **버전**—Adobe Commerce 2.3.0 이상

[Baler](https://github.com/magento/baler)은(는) 생성된 JavaScript 코드를 스캔하고 최적화된 JavaScript 번들을 만드는 모듈입니다. 사이트에 최적화된 번들을 배포하면 사이트를 로드할 때 네트워크 요청 수를 줄이고 페이지 로드 시간을 향상시킬 수 있습니다.

정적 콘텐츠 배포를 수행한 후 발레를 실행하려면 `true`(으)로 설정합니다.

```yaml
stage:
  build:
    SCD_USE_BALER: true
```

>[!NOTE]
>
>이 기능을 사용하기 전에 Baler 모듈을 설치하고 구성합니다. Baler가 알파 릴리스이므로 스테이징 환경에서만 이 옵션을 활성화합니다.

## `SKIP_HTML_MINIFICATION`

- **기본값**:
   - `true` - `ece-tools` 2002.0.13 이상
   - `false`—이전 버전의 `ece-tools`에 대해
- **버전**—Adobe Commerce 2.1.4 이상

빌드 단계가 끝날 때 정적 보기 파일을 `<magento_root>/init/` 디렉터리에 복사하는 것을 활성화하거나 비활성화합니다. `true`(으)로 설정하면 파일이 복사되지 않으며 요청 시 HTML 축소를 사용할 수 있습니다. 스테이징 및 프로덕션 환경에 배포할 때 가동 중지 시간을 줄이려면 이 값을 `true`(으)로 설정하십시오.

- **`false`** - 빌드 단계가 끝날 때 `view_preprocessed` 디렉터리를 `<magento_root>/init/` 디렉터리에 복사하고 배포 단계가 시작될 때 `<magento_root>/var` 디렉터리에 있는 디렉터리를 복원합니다.
- **`true`**—주문형 HTML 축소를 사용합니다. 빌드 단계가 끝날 때 `<magento_root>var/view_preprocessed`을(를) `<magento_root>/init/` 디렉터리에 복사하지 _않습니다_.

`SKIP_HTML_MINIFICATION` 환경 변수를 `.magento.env.yaml` 파일의 `global` 단계에 추가합니다.

```yaml
stage:
  global:
    SKIP_HTML_MINIFICATION: true
```

## `X_FRAME_CONFIGURATION`

- **기본값**—_설정되지 않음_
- **버전**—Adobe Commerce 2.1.4 이상

`X_FRAME_CONFIGURATION` 변수를 사용하여 Adobe Commerce 사이트에 대한 [`X-Frame-Options`](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/security/xframe-options.html) 헤더 구성을 변경합니다. 이 구성은 브라우저가 `<frame>`, `<iframe>` 또는 `<object>`에서 페이지를 렌더링하는 방법을 제어합니다. 다음 옵션 중 하나를 사용합니다.

- `DENY` - 프레임에 페이지를 표시할 수 없습니다.
- `SAMEORIGIN`—(기본 Adobe Commerce 설정) 페이지는 페이지 자체와 동일한 원점의 프레임에만 표시될 수 있습니다.

>[!WARNING]
>
>Adobe Commerce에서 지원하는 브라우저에서 더 이상 지원하지 않으므로 `ALLOW-FROM <uri>` 옵션은 더 이상 사용되지 않습니다. [브라우저 호환성](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options#Browser_compatibility)을 참조하세요.

`X_FRAME_CONFIGURATION` 환경 변수를 `.magento.env.yaml` 파일의 `global` 단계에 추가합니다.

```yaml
stage:
  global:
    X_FRAME_CONFIGURATION: SAMEORIGIN
```
