---
title: 속성
description: 클라우드 인프라에 빌드 및 배포하기 위해  [!DNL Commerce] 응용 프로그램을 구성할 때 속성 목록을 참조로 사용하십시오.
feature: Cloud, Configuration, Build, Deploy, Roles/Permissions, Storage
exl-id: 58a86136-a9f9-4519-af27-2f8fa4018038
source-git-commit: 1d671d7d2b9ef8742f50b23aa56020d82c701fa4
workflow-type: tm+mt
source-wordcount: '816'
ht-degree: 0%

---

# 응용 프로그램 구성에 대한 속성

`.magento.app.yaml` 파일은 속성을 사용하여 [!DNL Commerce] 응용 프로그램에 대한 환경 지원을 관리합니다.

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
| [`runtime`](#runtime) | 런타임 속성에는 [!DNL Commerce] 응용 프로그램에 필요한 확장이 포함되어 있습니다. | 확장:<ul><li>`xsl`</li><li>`newrelic`</li><li>`sodium`</li></ul> | 예 |
| [`type`](#type-and-build) | 기본 컨테이너 이미지 설정 | `php:8.3` | 예 |
| [`variables`](variables-property.md) | 특정 Commerce 버전에 대한 환경 변수 적용 | — | 아니요 |
| [`web`](web-property.md) | 외부 요청 처리 | — | 예 |
| [`workers`](workers-property.md) | 외부 요청 처리 | — | 예, 웹 속성을 사용하지 않는 경우 |

{style="table-layout:auto"}

## `name`

`name` 속성은 [`routes.yaml`](../routes/routes-yaml.md) 파일에서 HTTP 업스트림을 정의하는 데 사용되는 응용 프로그램 이름을 제공합니다(기본적으로 `mymagento:http`). 예를 들어 `name`의 값이 `app`인 경우 업스트림 필드에서 `app:http`을(를) 사용해야 합니다.

>[!WARNING]
>
>응용 프로그램을 배포한 후에는 응용 프로그램의 이름을 변경하지 마십시오. 이렇게 하면 데이터가 손실됩니다.

## `type` 및 `build`

`type` 및 `build` 속성은 프로젝트를 빌드하고 실행할 기본 컨테이너 이미지에 대한 정보를 제공합니다.

지원되는 `type` 언어는 PHP입니다. 다음과 같이 PHP 버전을 지정합니다.

```yaml
type: php:<version>
```

`build` 속성은 프로젝트를 빌드할 때 기본적으로 수행되는 작업을 결정합니다. `flavor`은(는) 실행할 기본 빌드 작업 집합을 지정합니다. 다음 예제에서는 `magento-cloud/.magento.app.yaml`의 `type` 및 `build`에 대한 기본 구성을 보여 줍니다.

```yaml
# The toolstack used to build the application.
type: php:8.3
build:
    flavor: none

dependencies:
    php:
        composer/composer: '2.7.2'
```

### Composer 2 설치 및 사용

`build: flavor:` 속성은 Composer 2.x에 사용되지 않으므로 빌드 단계에서 Composer를 수동으로 설치해야 합니다. Starter 및 Pro 프로젝트에서 Composer 2.x를 설치하고 사용하려면 `.magento.app.yaml` 구성을 세 번 변경해야 합니다.

1. `composer`을(를) `build: flavor:`(으)로 제거하고 `none`을(를) 추가합니다. 이 변경 사항으로 인해 Cloud에서 기본 1.x 버전의 Composer를 사용하여 빌드 작업을 실행할 수 없습니다.
1. Composer 2.x를 설치하기 위해 `composer/composer: '^2.0'`을(를) `php` 종속성으로 추가합니다.
1. `composer` 빌드 작업을 `build` 후크에 추가하여 Composer 2.x를 사용하여 빌드 작업을 실행합니다.

자신의 `.magento.app.yaml` 구성에서 다음 구성 조각을 사용합니다.

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

작성기에 대한 자세한 내용은 [필수 패키지](../development/overview.md#required-packages)를 참조하십시오.

## `dependencies`

빌드 프로세스 중에 응용 프로그램에 필요할 수 있는 종속성을 지정합니다.

Adobe Commerce은 다음 언어에 대한 종속성을 지원합니다.

- PHP
- 루비
- Node.js

이러한 종속성은 응용 프로그램의 최종 종속성에 독립적이며 빌드 프로세스 중 및 응용 프로그램의 런타임 환경에서 `PATH`에서 사용할 수 있습니다.

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

확장을 사용하는 방법에 대한 자세한 내용은 [PHP 설정](php-settings.md)을 참조하십시오.

## `disk`

응용 프로그램의 영구 디스크 크기(MB)를 정의합니다.

```yaml
disk: 5120
```

권장되는 최소 디스크 크기는 256MB입니다. 오류 `UserError: Error building the project: Disk size may not be smaller than 128MB`이(가) 표시되면 크기를 256MB로 늘리십시오.

>[!NOTE]
>
>Pro 스테이징 및 프로덕션 환경의 경우 응용 프로그램에 대한 `mounts` 및 `disk` 구성을 업데이트하려면 [Adobe Commerce 지원 티켓을 제출](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)해야 합니다. 티켓을 제출할 때 필요한 구성 변경 사항을 표시하고 업데이트된 `.magento.app.yaml` 파일 버전을 포함하십시오.
>
>스태이징 또는 프로덕션의 디스크 저장소를 일시적으로 늘릴 수는 없으며, 이 프로세스는 되돌릴 수 없습니다.

## `relationships`

애플리케이션에서 서비스 매핑을 정의합니다.

`name` 관계는 `MAGENTO_CLOUD_RELATIONSHIPS` 환경 변수의 응용 프로그램에서 사용할 수 있습니다. `<service-name>:<endpoint-name>` 관계는 `.magento/services.yaml` 파일에 정의된 이름 및 형식 값에 매핑됩니다.

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

현재 지원되는 서비스 종류 및 끝점의 전체 목록은 [서비스](../services/services-yaml.md)를 참조하십시오.

## `mounts`

키가 애플리케이션 루트에 상대적인 경로인 객체입니다. 마운트는 디스크에 파일 쓰기 가능 영역입니다. 다음은 `volume_id[/subpath]` 구문을 사용하여 `magento.app.yaml` 파일에 구성된 기본 탑재 목록입니다.

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

- `shared` - 환경 내의 응용 프로그램 간에 볼륨을 공유합니다.
- `disk`—공유 볼륨에 사용할 수 있는 크기를 정의합니다.

>[!NOTE]
>
>Pro 스테이징 및 프로덕션 환경의 경우 응용 프로그램에 대한 `mounts` 및 `disk` 구성을 업데이트하려면 [Adobe Commerce 지원 티켓을 제출](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)해야 합니다. 티켓을 제출할 때 필요한 구성 변경 사항을 표시하고 업데이트된 `.magento.app.yaml` 파일 버전을 포함하십시오.

마운트 웹을 [`web`](web-property.md) 위치 블록에 추가하여 액세스할 수 있도록 할 수 있습니다.

>[!WARNING]
>
>사이트에 데이터가 있으면 마운트 이름의 `subpath` 부분을 변경하지 마십시오. 이 값은 `files` 영역의 고유 식별자입니다. 이 이름을 변경하면 이전 위치에 저장된 모든 사이트 데이터가 손실됩니다.

## `access`

`access` 속성은 환경에 대한 SSH 액세스가 허용되는 최소 사용자 역할 수준을 나타냅니다. 사용 가능한 사용자 역할은 다음과 같습니다.

- `admin`—환경에서 설정을 변경하고 작업을 실행할 수 있습니다. _기여자_ 및 _뷰어_ 권한이 있습니다.
- `contributor`—이 환경에 코드를 푸시하고 환경에서 분기할 수 있습니다. _뷰어_ 권한이 있습니다.
- `viewer`—환경만 볼 수 있습니다.

기본 사용자 역할은 `contributor`이며, _뷰어_ 권한만 있는 사용자의 SSH 액세스를 제한합니다. _뷰어_ 권한만 있는 사용자에 대해 SSH 액세스를 허용하도록 사용자 역할을 `viewer`(으)로 변경할 수 있습니다.

```yaml
access:
    ssh: viewer
```
