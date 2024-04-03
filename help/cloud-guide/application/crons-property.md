---
title: Crons 속성
description: 에서 'crons' 속성을 구성하는 방법에 대한 예를 참조하십시오. [!DNL Commerce] 응용 프로그램 구성 파일입니다.
feature: Cloud, Configuration
exl-id: 67d592c1-2933-4cdf-b4f6-d73cd44b9f59
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '1063'
ht-degree: 0%

---

# Crons 속성

Adobe Commerce은 `crons` 속성을 사용하여 반복 활동을 스케줄링합니다. 특정 작업이 하루 중 특정 시간에 실행되도록 예약하는 데 이상적입니다. 읽기 전용 환경의 특성으로 인해 클라우드 인프라 프로젝트의 Adobe Commerce에 대한 웹 인스턴스에서는 한 번에 하나의 cron 작업만 실행할 수 있습니다. 장기 실행 작업을 대기열에 추가된 더 작은 작업으로 분류하는 것이 좋습니다. 또는 다음을 빌드할 수 있습니다 [작업자 인스턴스](workers-property.md).

Adobe은 다음을 실행할 것을 권장합니다. `crons` (으)로 [파일 시스템 소유자](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html). 실행 _아님_ 실행 `crons` 다음으로: `root` 또는 를 웹 서버 사용자로 추가할 수 있습니다.

이 구성은 여러 기본 cron 작업이 있는 Adobe Commerce의 온-프레미스 배포와 다릅니다. 다음을 참조하십시오 [cron 작업 구성](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/configure-cron-jobs.html) 다음에서 _구성 안내서_.

## cron 작업 설정

다음 `crons` 속성은 일정에서 트리거되는 프로세스를 설명합니다. 각 작업에는 이름과 다음 옵션이 필요합니다.

- `spec`- 예약에 사용되는 cron 표현식입니다.
- `cmd`- 실행할 명령 `start` 및 `stop`.
- `shutdown_timeout`—(_선택 사항_) cron 작업이 취소되면 작업 또는 프로세스를 중지하기 위해 SIGKILL 신호가 전송된 시간(초)입니다. 기본값은 10초입니다.
- `timeout`—(_선택 사항_) 시간 초과 전에 cron 작업을 실행할 수 있는 최대 시간입니다. 기본값은 86400초(24시간)의 최대 허용 값으로 설정되어 있습니다.

기본적으로 모든 Commerce 클라우드 프로젝트에는 다음 기본값이 있습니다 `crons` 에서 구성 `.magento.app.yaml` 파일:

```yaml
crons:
    cronrun:
        spec: "* * * * *"
        cmd: "php bin/magento cron:run"
```

프로젝트에 사용자 지정 cron 작업이 필요한 경우 이를 기본값에 추가할 수 있습니다 `crons` 구성. 다음을 참조하십시오 [cron 작업 작성](#build-a-cron-job).

### `crontab`

Adobe Commerce은 셀프서비스를 지원하기 위해 Pro 프로젝트에만 자동 크론 구성 옵션을 추가했습니다 `crons` 스테이징 및 프로덕션 환경에서 구성합니다. 이 옵션이 활성화되면 `crontab` cron 구성을 검토하십시오. 이 은(는) _아님_ 스타터 프로젝트에서 사용할 수 있습니다.

다음을 사용할 수 있지만 `crontab` Pro 프로젝트에 대한 구성을 검토하기 위해 Adobe Commerce에서는 `crontab` 클라우드 인프라에 배포된 사이트에 대해 cron 작업을 실행합니다.

**Pro 환경에서 cron 구성을 검토하려면**:

1. 사용 [SSH](../development/secure-connections.md#use-an-ssh-command) 원격 환경에 로그인합니다.

1. 예약된 cron 프로세스를 나열합니다.

   ```shell
   crontab -l
   ```

   >[!NOTE]
   >
   >다음과 같은 경우 `crontab -l` 명령이 `Command not found` 오류, 다음을 수행해야 합니다. [Adobe Commerce 지원 티켓 제출](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) pro 프로젝트에서 auto-crons 셀프 서비스 구성 옵션을 활성화하려면 다음을 수행하십시오.

다음 예제는 `crontab` 기본값만 있는 환경용 출력 `crons` 구성:

```terminal
username@hostname:~$ crontab -l
# Crontab is managed by the system, attempts to edit it directly will fail.
SHELL=/etc/platform/6fck2obu3244c/cron-run
MAILTO=""

# m h  dom mon dow  job_name

* * * * *           cronrun
```

## cron 작업 작성

cron 작업에는 일정 및 시간 지정과 예약된 시간에 실행할 명령이 포함됩니다. Starter 환경 및 Pro용 `integration` 환경에서는 최소 간격이 5분당 한 번입니다. Pro 스테이징 및 프로덕션 환경의 경우 최소 간격은 분당 1회입니다. 클라우드 인프라의 Adobe Commerce에서에 사용자 지정 cron 작업을 추가합니다. `.magento.app.yaml` 파일 위치: `crons` 섹션. 일반적인 형식은 다음과 같습니다. `spec` 일정 및 `cmd` 실행할 명령 또는 사용자 지정 스크립트를 지정합니다.

### 사양

Adobe Commerce은 다음에 대해 5값 표현식을 사용합니다. `crons` 사양(spec): `* * * * *`

1. 분 (0~59) 모든 Starter 및 Pro 환경에서 cron 작업에 지원되는 최소 빈도는 5분입니다. 관리자에서 설정을 구성해야 할 수도 있습니다.
2. 시간(0~23)
3. 날짜 (1~31)
4. 개월(1~12)
5. 요일 (0~6) (일부 시스템의 경우 일요일~토요일; 7)

몇 가지 예:

- `00 */3 * * *` 첫 번째 분(오전 12:00, 오전 3:00, 오전 6:00)에 3시간마다 실행됩니다.
- `20 */8 * * *` 매 8시간마다 20분(오전 12:20, 오전 8:20, 오후 4:20)에 실행
- `00 00 * * *` 는 자정에 하루에 한 번 실행됩니다.
- `00 * * * 1` 는 매주 월요일 자정에 한 번 실행됩니다.

>[!NOTE]
>
>다음 `crons` 에 지정된 시간 `.magento.app.yaml` 파일은 데이터베이스의 저장소 구성 값에 지정된 시간대가 아니라 서버 시간대를 기반으로 합니다.

일정을 결정할 때 작업을 완료하는 데 걸리는 시간을 고려합니다. 예를 들어 3시간마다 작업을 실행하고 작업을 완료하는 데 40분이 걸리는 경우 예약된 시간을 변경하는 것이 좋습니다.

### 명령

다음 `cmd` 실행할 명령 또는 사용자 지정 스크립트를 지정합니다. 명령 스크립트 형식에는 다음이 포함될 수 있습니다.

```text
<path-to-php-binary> <project-dir>/<script-command>
```

For example:

```yaml
crons:
    spec: "00 */8 * * *"
    cmd: "/usr/bin/php /app/abc123edf890/bin/magento export:start catalog_category_product"
```

이 예에서는 `<path-to-php-binary>` 은(는) `/usr/bin/php`. 프로젝트 ID가 포함된 설치 디렉터리는 입니다. `/app/abc123edf890/bin/magento`, 스크립트 작업: `export:start catalog_category_product`.

### 프로젝트에 사용자 정의 크론 작업 추가

클라우드 인프라 플랫폼의 Adobe Commerce에서 `crons` 의 섹션 [`.magento.app.yaml`](../application/configure-app-yaml.md) 파일.

>[!NOTE]
>
>Starter 환경 및 Pro용 `integration` 환경에서는 최소 간격이 5분당 한 번입니다. Pro 스테이징 및 프로덕션 환경의 경우 최소 간격은 분당 1회입니다. 기본 최소 간격보다 더 빈번한 간격을 구성할 수는 없습니다.

Adobe Commerce Pro 프로젝트에서 [auto-crons 기능](#set-up-cron-jobs) 을(를) 사용하여 사용자 지정 cron 작업을 스테이징 및 프로덕션 환경에 추가하려면 먼저 프로젝트에서 활성화해야 합니다. `.magento.app.yaml` 파일. 이 기능이 활성화되지 않으면 [Adobe Commerce 지원 티켓 제출](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) 자동 크론을 활성화하려면 다음을 수행하십시오.

**사용자 정의 cron 작업을 추가하려면**:

1. 로컬 개발 환경에서 `.magento.app.yaml` Adobe Commerce의 파일 `/app` 디렉토리.

1. 다음에서 `crons` 섹션에서 다음 형식을 사용하여 사용자 지정을 추가합니다.

   ```yaml
   crons:
       <cron_name_1>:
           spec: "<schedule_time>"
           cmd: "<schedule_command>"
       <cron_name_2>:
           spec: "<schedule_time>"
           cmd: "<schedule_command>"
   ```

   다음 예제에서는 `productcatalog` 작업은 매 8시간마다 한 시간 후 20분마다 제품 카탈로그를 내보냅니다.

   ```yaml
   crons:
       magento:
           spec: '* * * * *'
           cmd: 'php bin/magento cron:run'
       productcatalog:
           spec: '20 */8 * * *'
           cmd: 'bin/magento export:start catalog_product_category'
   ```

1. 코드 변경 사항을 추가, 커밋 및 푸시합니다.

   ```bash
   git add .magento.app.yaml && git commit -m "cron config updates" && git push origin <branch-name>
   ```

### cron 작업 업데이트

사용자 정의된 작업을 추가, 제거 또는 업데이트하려면 `crons` 의 섹션 `.magento.app.yaml` 파일. 그런 다음 리모컨에서 업데이트를 테스트합니다 `integration` 스테이징 및 프로덕션 환경에 변경 사항을 푸시하기 전에 환경을 업데이트했습니다.

## cron 작업 비활성화

성능 문제를 방지하기 위해 리인덱싱 또는 캐시 정리와 같은 유지 관리 작업을 완료하기 전에 cron 작업을 수동으로 비활성화할 수 있습니다. 다음을 사용할 수 있습니다. `ece-tools` CLI 명령 `cron:disable` 모든 cron 작업을 비활성화하고 활성 cron 프로세스를 중지합니다.

**cron 작업을 비활성화하려면**:

1. 로컬 워크스테이션에서 프로젝트 디렉터리로 변경합니다.

1. SSH를 사용하여 원격 환경에 로그인합니다.

   ```bash
   magento-cloud ssh
   ```

1. cron 작업을 비활성화하고 활성 cron 프로세스를 중지합니다.

   ```shell
   ./vendor/bin/ece-tools cron:disable
   ```

1. 필요한 유지 관리 작업을 완료한 후 cron 작업을 다시 활성화해야 합니다.

   ```shell
   ./vendor/bin/ece-tools cron:enable
   ```

## 크론 작업 문제 해결

Adobe이 Adobe Commerce on cloud infrastructure platform의 cron 처리를 최적화하고 cron 관련 문제를 수정하도록 Adobe Commerce on cloud infrastructure 패키지를 업데이트했습니다. cron 처리 문제가 발생하면 프로젝트가 최신 버전의 `ece-tools` 패키지. 다음을 참조하십시오 [ECE-Tools 업데이트](../dev-tools/update-package.md).

각 환경의 애플리케이션 수준 로그 파일에서 cron 처리 정보를 검토할 수 있습니다. 다음을 참조하십시오 [애플리케이션 로그](../test/log-locations.md#application-logs).

cron 관련 문제를 해결하는 데 도움이 필요하면 다음 Adobe Commerce 지원 문서를 참조하십시오.

- [Cron 작업은 다른 그룹의 작업을 잠급니다.](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/cron-tasks-lock-tasks-from-other-groups.html)

- [클라우드에서 수동으로 중단된 크론 작업 재설정](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/reset-stuck-magento-cron-jobs-manually-on-cloud.html)
