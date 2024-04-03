---
title: 저장소 구성 관리
description: 클라우드 인프라 환경의 모든 Adobe Commerce에서 스토어 구성 설정을 관리하고 동기화하는 방법에 대해 알아봅니다.
feature: Cloud, Configuration, SCD
exl-id: f2dd876d-24ee-4d47-b9ac-44fcf77b61b5
source-git-commit: 13e76d3e9829155995acbb72d947be3041579298
workflow-type: tm+mt
source-wordcount: '1439'
ht-degree: 0%

---

# 저장소 구성 관리

저장소에 대한 기본 구성은 `config.xml` 를 참조하십시오. Commerce Admin 또는 CLI에서 설정을 변경할 때 `bin/magento config:set` 명령은 변경 내용이 코어 데이터베이스에 반영되며 특히 `core_config_data` 테이블. 이러한 설정은 의 기본 구성을 덮어씁니다. `config.xml` 파일.

저장소 설정 - 관리자의 구성을 참조합니다. **스토어** > **설정** > **구성** 섹션은 구성 유형에 따라 배포 구성 파일에 저장됩니다.

- `app/etc/config.php`—저장소, 웹 사이트, 모듈 또는 확장, 정적 파일 최적화 및 정적 컨텐츠 배포와 관련된 시스템 값에 대한 구성 설정. 다음을 참조하십시오. [config.php 참조](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-configphp.html) 다음에서 _구성 안내서_.
- `app/etc/env.php`- 다음과 같은 시스템별 재정의 및 중요한 설정에 대한 값 _아님_ 소스 제어에 저장됩니다. 다음을 참조하십시오. [env.php 참조](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-envphp.html) 다음에서 _구성 안내서_.

>[!NOTE]
>
>클라우드 인프라의 Adobe Commerce은 프로덕션 및 유지 관리 모드만 지원하므로 **고급** > **개발자** 관리자 권한에서는 섹션에 액세스할 수 없습니다. 다음을 수행해야 합니다. [환경 관리자 권한](../project/user-access.md) 구성 관리 작업을 완료합니다. 다음을 사용하여 추가 설정을 구성할 수 있습니다. [환경 변수](../environment/configure-env-yaml.md).

구성 관리는 파이프라인 배포를 사용하여 가동 중단을 최소화하면서 환경 전체에 일관된 저장소 설정을 배포하는 방법을 제공합니다. Adobe Commerce on cloud infrastructure 프로젝트에는 빌드 서버, 빌드 및 배포 스크립트, [파이프라인 배포 전략](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/deployment/technical-details.html) 명심해.

## 구성 재정의 스키마

모든 시스템 구성은 다음 재정의 체계에 따라 빌드 및 배포 단계 동안 설정됩니다.

1. 환경 변수가 있는 경우 사용자 지정 구성을 사용하고 기본 구성을 무시합니다.
1. 환경 변수가 없는 경우 `MAGENTO_CLOUD_RELATIONSHIPS` 의 이름-값 쌍 [`.magento.app.yaml` 파일](../application/configure-app-yaml.md). 기본 구성을 무시합니다.
1. 환경 변수가 없는 경우 `MAGENTO_CLOUD_RELATIONSHIPS` 이름-값 쌍을 포함하지 않습니다. 사용자 지정된 구성을 모두 제거하고 기본 구성의 값을 사용하십시오.

요약하면 환경 변수는 다른 모든 값을 재정의합니다.

>[!TIP]
>
>다음을 참조하십시오 [구성 관리](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/deployment/technical-details.html) 다음에서 _구성 안내서_ 파이프라인 배포의 재정의 체계에 대한 자세한 정보.

동일한 설정이 여러 위치에 구성된 경우 애플리케이션은 다음 구성 계층 구조를 사용하여 환경에 적용할 값을 결정합니다.

| 우선 순위 | 구성<br>방법 | 설명 |
| -------- | ------------------------ | ----------- |
| 1 | [!DNL Cloud Console]<br>환경 변수 | 값에서 추가된 값 _변수_ 에서 환경 구성 탭 [!DNL Cloud Console]. 중요한 구성 또는 환경별 구성에 대한 값을 여기에 지정하십시오. 여기에서 지정된 설정은 책임자로부터 편집할 수 없습니다. 다음을 참조하십시오 [환경 구성 변수](../project/overview.md#configure-environment). |
| 2 | `.magento.app.yaml` | 에 추가된 값 `variables` 의 섹션 `.magento.app.yaml` 파일. 모든 환경에서 일관된 구성을 위해 여기에 값을 지정하십시오. **에서 중요한 값을 지정하지 마십시오. `.magento.app.yaml` 파일.** 다음을 참조하십시오 [응용 프로그램 설정](../application/configure-app-yaml.md). |
| 3 | `app/etc/env.php` | 여기에 저장된 환경별 구성 값은 `app:config:dump` 명령입니다. 환경 변수 또는 CLI를 사용하여 시스템별로 중요 값을 설정합니다. 다음을 참조하십시오 [중요 데이터](#sensitive-data). 다음 `env.php` 파일: **아님** 소스 제어에 포함됩니다. |
| 4 | `app/etc/config.php` | 여기에 저장된 값은 `app:config:dump` 명령입니다. 공유 구성 값이에 추가됩니다. `config.php`. 관리자로부터 또는 CLI를 사용하여 공유 구성을 설정합니다. 다음 `config.php` 파일은 소스 제어에 포함됩니다. |
| 5 | 데이터베이스 | 여기에 저장된 값은 관리자에서 구성을 설정하여 추가됩니다. 위의 방법 중 하나를 사용하여 설정된 구성은 잠겨(회색으로 표시됨) 관리자에서 편집할 수 없습니다. |
| 6 | `config.xml` | 많은 구성의 기본값이 `config.xml` 모듈용 파일입니다. Adobe Commerce이 이전 메서드에서 설정한 값을 찾을 수 없는 경우 기본값이 됩니다(설정된 경우). |

{style="table-layout:auto"}

## 구성 덤프

다음을 사용할 수 있습니다 `ece-tools` 생성 명령 `config.php` 현재 저장소 구성이 모두 포함된 파일:

```bash
./vendor/bin/ece-tools config:dump
```

데이터가 로 &quot;덤프됨&quot; `app/etc/config.php` 파일이 _잠김_&#x200B;즉, 상거래 관리자의 해당 필드가 다음과 같이 됩니다 **읽기 전용**. 다음 `config.php` 파일에는 사용자가 구성하는 설정만 포함됩니다. 기본값이 잠기지 않습니다. 업데이트하는 값만 잠그면 스테이징 및 프로덕션 환경에서 사용되는 모든 확장이 읽기 전용 구성, 특히 Fastly로 인해 중단되지 않습니다.

>[!WARNING]
>
>다음 `ece-tools config:dump` 명령은 B2B와 같은 모듈에 대한 자세한 구성을 검색하지 않습니다. 포괄적인 구성 덤프가 필요한 경우 `app:config:dump` 명령이지만 이 명령은 읽기 전용 상태에서 구성 값을 잠급니다.

### 중요 데이터

중요한 구성을으로 내보내기 `app/etc/env.php` 파일 사용 시 `bin/magento app:config:dump` 명령입니다. CLI 명령을 사용하여 중요한 값을 설정할 수 있습니다. `bin/magento config:sensitive:set`. 다음을 참조하십시오  [중요 및 환경별 설정](https://developer.adobe.com/commerce/php/development/configuration/sensitive-environment-settings/) 다음에서 _상거래 PHP 확장_ 구성 설정을 중요하거나 시스템별로 지정하는 방법에 대해 알아봅니다.

목록 보기 [중요 또는 시스템별 설정](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/paths/config-reference-sens.html) 다음에서 _구성 안내서_.

### SCD 성능

저장소 크기에 따라 배포할 정적 콘텐츠 파일이 많을 수 있습니다. 일반적으로 정적 콘텐츠는 애플리케이션이 유지 관리 모드에 있는 경우 배포 단계 동안 배포됩니다. 가장 최적의 구성은 빌드 단계 중에 정적 콘텐츠를 생성하는 것입니다. 다음을 참조하십시오 [배포 전략 선택](../deploy/static-content.md).

구성을 덤프한 후 구성 관리를 활성화한 경우 배포 단계의 SCD_* 변수를 빌드 단계로 이동하여 빌드 단계 동안 정적 콘텐츠 생성을 올바르게 활성화해야 합니다. 다음을 참조하십시오 [환경 변수](../environment/configure-env-yaml.md#environment-variables).

**구성 관리 전**:

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

**구성 관리 활성화 후**:

SCD_* 변수를 빌드 단계로 이동합니다.

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

>[!NOTE]
>
>정적 파일을 배포하기 전에 빌드 및 배포 단계에서 GZIP을 사용하여 정적 콘텐츠를 압축합니다. 정적 파일을 압축하면 서버 로드가 줄어들고 사이트 성능이 향상됩니다. 다음을 참조하십시오 [빌드 옵션](../environment/variables-build.md) 파일 압축 사용자 지정 또는 비활성화에 대해 알아봅니다.

## 설정 관리 절차

다음은 이 프로세스에 대한 높은 수준의 개요를 보여 줍니다.

![Starter 구성 관리 개요](../../assets/starter/configuration-management-flow.png)

**저장소를 구성하고 구성 파일을 생성하려면 다음을 수행하십시오**:

1. 환경 중 하나의 경우 관리자의 저장소에 대한 모든 구성을 완료합니다.

   - 스타터: 활성 개발 분기
   - Pro: 통합 환경의 활성 분기

   이 환경에서 스테이징 및 프로덕션 환경으로 데이터베이스를 덤프하려는 경우가 아니면 이러한 구성에 실제 제품이 포함되지 않습니다. 일반적으로 개발 데이터베이스에는 전체 저장소 데이터가 포함되지 않습니다.

1. 로컬 워크스테이션에서 프로젝트 디렉터리로 변경합니다.

1. 원격 데이터베이스의 로컬 덤프를 만듭니다.

   ```bash
   magento-cloud db:dump
   ```

1. 코드 변경 사항을 추가, 커밋 및 푸시하여 원격 환경을 업데이트합니다.

   ```bash
   git add app/etc/config.php
   ```

   ```bash
   git commit -m "Add system-specific configuration"
   ```

   ```bash
   git push origin <branch-name>
   ```

배포가 완료되면 업데이트된 환경의 관리자에 로그인하여 설정을 확인합니다. 필요에 따라 추가 구성을 스테이징 및 프로덕션 환경에 계속 병합합니다.

### 구성 업데이트

관리자를 통해 환경을 수정하고 명령을 다시 실행하면 새 구성이 의 코드에 추가됩니다. `config.php` 파일.

>[!WARNING]
>
>을(를) 수동으로 편집할 수 있는 동안 `config.php` 스테이징 및 프로덕션 환경의 파일인 경우 **아님** 추천. 이 파일은 모든 환경에서 모든 구성을 일관되게 유지하는 데 도움이 됩니다. 삭제 안 함 `config.php` 파일을 다시 빌드합니다. 파일을 삭제하면 빌드 및 배포 프로세스에 필요한 특정 구성 및 설정이 제거될 수 있습니다.

### 구성 파일 복원

원본 사본 `app/etc/env.php` 및 `app/etc/config.php` 배포 프로세스 중에 파일이 만들어지고 동일한 폴더에 저장됩니다. 다음은 BAK(백업 파일)와 PHP(원본 파일)를 동일하게 보여줍니다 `app/etc` 폴더:

```terminal
...
config.php.bak
di.xml
env.php.bak
vendor_path.php
config.php
db_schema.xml
env.php
...
```

이전 구성이 `app/etc/config.local.php` 파일. 다음을 참조하십시오 [이전 구성 마이그레이션](#migrate-older-configurations).

**구성 파일을 복원하려면**:

1. 로컬 워크스테이션에서 SSH를 사용하여 원격 프로젝트 및 환경에 로그인합니다.

   ```bash
   magento-cloud ssh
   ```

1. 백업 파일의 위치 및 가용성을 확인합니다.

   ```bash
   ./vendor/bin/ece-tools backup:list
   ```

   샘플 응답:

   ```terminal
   The list of backup files:
   app/etc/env.php
   app/etc/config.php
   ```

1. 백업 파일 복원

   ```bash
   ./vendor/bin/ece-tools backup:restore
   ```

### 이전 구성 마이그레이션

Cloud Infrastructure 2.2 이상에서 Adobe Commerce으로 업그레이드하는 경우 다음에서 설정을 마이그레이션할 수 있습니다. `config.local.php` 파일을 새 파일에 저장 `config.php` 파일. 관리자의 구성 설정이 파일의 내용과 일치하면 지침에 따라 을 생성하고 추가합니다. `config.php` 파일.

서로 다른 경우 `config.local.php` 파일을 새 파일에 저장 `config.php` 파일:

1. 지침에 따라 다음을 생성합니다. `config.php` 파일.

1. 를 엽니다. `config.php` 파일을 만들고 마지막 줄을 삭제합니다.

1. 를 엽니다. `config.local.php` 파일 및 복사

1. 콘텐츠를 다음에 붙여넣기 `config.php` git에 추가하고, 저장하고, 완료합니다.

1. 환경 전체에 배포합니다.

이 마이그레이션은 한 번만 완료합니다. 마이그레이션 후 다음을 사용하십시오. `config.php` 파일.

### 로케일 변경

복잡한 구성 가져오기 및 내보내기 프로세스를 따르지 않고 저장소 로케일을 변경할 수 있습니다. _if_ 다음을 보유한 경우: [SCD_ON_DEMAND](../environment/variables-global.md#scd_on_demand) 활성화되었습니다. 관리자를 사용하여 로케일을 업데이트할 수 있습니다.

를 활성화하여 스테이징 또는 프로덕션 환경에 다른 로케일을 추가할 수 있습니다 `SCD_ON_DEMAND` 통합 분기에서 업데이트된 을 생성합니다. `config.php` 파일을 새 로케일 정보로 복제하고 구성 파일을 대상 환경에 복사합니다.

>[!WARNING]
>
>이 프로세스 **덮어쓰기** 저장소 구성. 환경에 동일한 저장소가 포함된 경우에만 다음을 수행합니다.

1. 통합 환경에서 `SCD_ON_DEMAND` 변수를 사용하는 중 [`.magento.env.yaml` 파일](../environment/configure-env-yaml.md).

1. 관리자를 사용하여 필요한 로케일을 추가합니다.

1. SSH를 사용하여 원격 환경에 로그인하고 `/app/etc/config.php` 모든 로케일이 들어 있는 파일입니다.

   ```bash
   ssh <SSH-URL> "./vendor/bin/ece-tools config:dump"
   ```

1. 원격 통합 환경에서 로컬 환경 디렉토리로 새 구성 파일을 복사합니다.

   ```bash
   rsync <SSH-URL>:app/etc/config.php ./app/etc/config.php
   ```

1. 코드 변경 사항을 추가, 커밋 및 푸시하여 원격 환경을 업데이트합니다.
