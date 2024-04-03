---
title: 속성
description: 를 구성할 때 속성 목록을 참조로 사용 [!DNL Commerce] 클라우드 인프라에 빌드 및 배포하기 위한 애플리케이션입니다.
feature: Cloud, Configuration, Build, Deploy, Roles/Permissions, Storage
exl-id: 58a86136-a9f9-4519-af27-2f8fa4018038
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '797'
ht-degree: 0%

---

# 응용 프로그램 구성에 대한 속성

다음 `.magento.app.yaml` 파일은 속성을 사용하여 [!DNL Commerce] 응용 프로그램.

| 이름 | 설명 | 기본값 | 필수 |
| ------ | --------------------------------- | ------- | -------- |
| [`access`](#access) | 사용자 역할 사용자 지정 | — | 아니요 |
| [`crons`](crons-property.md) | 사양 업데이트 및 cron 작업 예약 | — | 아니요 |
| [`dependencies`](#dependencies) | 추가 종속성 활성화 | `php:composer/composer: '2.2.4'` | 아니요 |
| [`disk`](#disk) | 영구 디스크 크기 정의 | `5120` | 예 |
| [`firewall`](firewall-property.md) | (초보자 전용) 아웃바운드 트래픽 제어 | — | 아니요 |
| [`hooks`](hooks-property.md) | 빌드, 배포 및 배포 후 단계에 대해 셸 명령 사용자 지정 | — | 아니요 |
| [`mounts`](#mounts) | 경로 설정 | 경로:<ul><li>`"var": "shared:files/var"`</li><li>`"app/etc": "shared:files/etc"`</li><li>`"pub/media": "shared:files/media"`</li><li>`"pub/static": "shared:files/static"`</li></ul> | 아니요 |
| [`name`](#name) | 응용 프로그램 이름 정의 | `mymagento` | 예 |
| [`relationships`](#relationships) | 맵 서비스 | 서비스:<ul><li>`database: "mysql:mysql"`</li><li>`redis: "redis:redis"`</li><li>`opensearch: "opensearch:opensearch"`</li></ul> | 아니요 |
| [`runtime`](#runtime) | 런타임 속성에는 [!DNL Commerce] 응용 프로그램. | 확장:<ul><li>`xsl`</li><li>`newrelic`</li><li>`sodium`</li></ul> | 예 |
| [`type`](#type-and-build) | 기본 컨테이너 이미지 설정 | `php:8.1` | 예 |
| [`variables`](variables-property.md) | 특정 상거래 버전에 대한 환경 변수 적용 | — | 아니요 |
| [`web`](web-property.md) | 외부 요청 처리 | — | 예 |
| [`workers`](workers-property.md) | 외부 요청 처리 | — | 예, 웹 속성을 사용하지 않는 경우 |

{style="table-layout:auto"}

## `name`

다음 `name` 속성은에 사용되는 응용 프로그램 이름을 제공합니다. [`routes.yaml`](../routes/routes-yaml.md) HTTP 업스트림을 정의하는 파일(기본적으로 `mymagento:http`). 예를 들어, 다음 값이 `name` 은(는) `app`, 다음을 사용해야 합니다. `app:http` 업스트림 필드.

>[!WARNING]
>
>응용 프로그램을 배포한 후에는 응용 프로그램의 이름을 변경하지 마십시오. 이렇게 하면 데이터가 손실됩니다.

## `type` 및 `build`

다음 `type`  및 `build` 속성은 프로젝트를 빌드하고 실행할 기본 컨테이너 이미지에 대한 정보를 제공합니다.

지원됨 `type` 언어는 PHP입니다. 다음과 같이 PHP 버전을 지정합니다.

```yaml
type: php:<version>
```

다음 `build` 속성은 프로젝트를 빌드할 때 기본적으로 수행되는 작업을 결정합니다. 다음 `flavor` 실행할 기본 빌드 작업 집합을 지정합니다. 다음 예제는 의 기본 구성을 보여 줍니다. `type` 및 `build` 출처: `magento-cloud/.magento.app.yaml`:

```yaml
# The toolstack used to build the application.
type: php:8.1
build:
    flavor: none

dependencies:
    php:
        composer/composer: '2.2.4'
```

### Composer 2 설치 및 사용

다음 `build: flavor:` 속성은 Composer 2.x에 사용되지 않으므로 빌드 단계에서 Composer를 수동으로 설치해야 합니다. Starter 및 Pro 프로젝트에서 Composer 2.x를 설치하고 사용하려면 를 세 번 변경해야 합니다. `.magento.app.yaml` 구성:

1. 제거 `composer` (으)로 `build: flavor:` 및 추가 `none`. 이 변경 사항으로 인해 Cloud에서 기본 1.x 버전의 Composer를 사용하여 빌드 작업을 실행할 수 없습니다.
1. 추가 `composer/composer: '^2.0'` as a `php` Composer 2.x 설치 종속성.
1. 추가 `composer` 에 작업 빌드 `build` 작성기 2.x를 사용하여 빌드 작업을 실행하도록 후크합니다.

다음 구성 조각을 직접 사용 `.magento.app.yaml` 구성:

```yaml
# 1. Change flavor to none.
build:
    flavor: none

# 2. Add Composer ^2.0 as a php dependency.
dependencies:
    php:
        composer/composer: '^2.0'

# 3. Add a build hook to run the build tasks using Composer 2.x.
hooks:
    build: |
        set -e
        composer --no-ansi --no-interaction install --no-progress --prefer-dist --optimize-autoloader
```

다음을 참조하십시오 [필수 패키지](../development/overview.md#required-packages) 작성기에 대한 자세한 정보입니다.

## `dependencies`

빌드 프로세스 중에 응용 프로그램에 필요할 수 있는 종속성을 지정합니다.

Adobe Commerce은 다음 언어에 대한 종속성을 지원합니다.

- PHP
- 루비
- Node.js

이러한 종속성은 애플리케이션의 최종 종속성과는 독립적이며 `PATH`를 설정하는 것이 좋습니다.

이러한 종속성을 다음과 같이 지정할 수 있습니다.

```yaml
ruby:
   sass: "~3.4"
nodejs:
   grunt-cli: "~0.3"
```

## `runtime`

를 사용하여 확장 활성화와 같이 런타임 시 PHP 구성을 수정합니다. 다음 확장이 필요합니다.

```yaml
runtime:
    extensions:
        - xsl
        - newrelic
        - sodium
```

다음을 참조하십시오 [PHP 설정](php-settings.md) 확장 활성화에 대한 자세한 내용

## `disk`

응용 프로그램의 영구 디스크 크기(MB)를 정의합니다.

```yaml
disk: 5120
```

권장되는 최소 디스크 크기는 256MB입니다. 오류가 표시되면 `UserError: Error building the project: Disk size may not be smaller than 128MB`, 크기를 256MB로 늘립니다.

>[!NOTE]
>
>Pro Staging 및 프로덕션 환경의 경우 [Adobe Commerce 지원 티켓 제출](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) 을(를) 업데이트하려면 `mounts` 및 `disk` 애플리케이션에 대한 구성 티켓을 제출할 때 필요한 구성 변경 사항을 표시하고 의 업데이트된 버전을 포함하십시오. `.magento.app.yaml` 파일.

## `relationships`

애플리케이션에서 서비스 매핑을 정의합니다.

관계 `name` 은 의 애플리케이션에서 사용할 수 있습니다. `MAGENTO_CLOUD_RELATIONSHIPS` 환경 변수입니다. 다음 `<service-name>:<endpoint-name>` 관계가에 정의된 이름 및 유형 값에 매핑됩니다. `.magento/services.yaml` 파일.

```yaml
relationships:
    <name>: "<service-name>:<endpoint-name>"
```

다음은 기본 관계의 예입니다.

```yaml
relationships:
    database: "mysql:mysql"
    redis: "redis:redis"
    opensearch: "opensearch:opensearch"
    rabbitmq: "rabbitmq:rabbitmq"
```

다음을 참조하십시오 [서비스](../services/services-yaml.md) 현재 지원되는 서비스 유형 및 엔드포인트의 전체 목록입니다.

## `mounts`

키가 애플리케이션 루트에 상대적인 경로인 객체입니다. 마운트는 디스크에 파일 쓰기 가능 영역입니다. 다음은에 구성된 기본 마운트 목록입니다. `magento.app.yaml` 를 사용하는 파일 `volume_id[/subpath]` 구문:

```yaml
 # The mounts that will be performed when the package is deployed.
mounts:
    "var": "shared:files/var"
    "app/etc": "shared:files/etc"
    "pub/media": "shared:files/media"
    "pub/static": "shared:files/static"
```

이 목록에 마운트를 추가하는 형식은 다음과 같습니다.

```bash
"/public/sites/default/files": "shared:files/files"
```

- `shared`—환경 내의 애플리케이션 간에 볼륨을 공유합니다.
- `disk`- 공유 볼륨에 사용할 수 있는 크기를 정의합니다.

>[!NOTE]
>
>Pro Staging 및 프로덕션 환경의 경우 [Adobe Commerce 지원 티켓 제출](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) 을(를) 업데이트하려면 `mounts` 및 `disk` 애플리케이션에 대한 구성 티켓을 제출할 때 필요한 구성 변경 사항을 표시하고 의 업데이트된 버전을 포함하십시오. `.magento.app.yaml` 파일.

마운트 웹에 추가하여 액세스할 수 있도록 할 수 있습니다. [`web`](web-property.md) 위치 블록.

>[!WARNING]
>
>사이트에 데이터가 있으면 를 변경하지 마십시오 `subpath` 마운트 이름의 일부입니다. 이 값은 의 고유 식별자입니다. `files` 영역입니다. 이 이름을 변경하면 이전 위치에 저장된 모든 사이트 데이터가 손실됩니다.

## `access`

다음 `access` 속성은 환경에 대한 SSH 액세스가 허용되는 최소 사용자 역할 수준을 나타냅니다. 사용 가능한 사용자 역할은 다음과 같습니다.

- `admin`—환경에서 설정을 변경하고 작업을 실행할 수 있습니다. _참여자_ 및 _조회자_ 권한.
- `contributor`—이 환경에 코드를 푸시하고 환경에서 분기할 수 있습니다. _조회자_ 권한.
- `viewer`- 환경만 볼 수 있습니다.

기본 사용자 역할은 `contributor`를 사용하여 를 사용하는 사용자의 SSH 액세스만 제한함 _조회자_ 권한. 사용자 역할을 다음으로 변경할 수 있습니다. `viewer` 을(를) 가진 사용자에게만 SSH 액세스를 허용하려면 _조회자_ 권한:

```yaml
access:
    ssh: viewer
```
