---
title: 로그 보기 및 관리
description: 클라우드 인프라에서 사용할 수 있는 로그 파일의 유형과 찾을 수 있는 위치를 파악합니다.
last-substantial-update: 2023-05-23T00:00:00Z
exl-id: d7f63dab-23bf-4b95-b58c-3ef9b46979d4
source-git-commit: 86af69eed16e8fe464de93bd0f33cfbfd4ed8f49
workflow-type: tm+mt
source-wordcount: '1056'
ht-degree: 0%

---

# 로그 보기 및 관리

클라우드 인프라 프로젝트의 Adobe Commerce 로그는 [후크 빌드 및 배포](../application/hooks-property.md), 클라우드 서비스 및 Adobe Commerce 애플리케이션과 관련된 문제를 해결하는 데 유용합니다.

파일 시스템, [!DNL Cloud Console] 및 `magento-cloud` CLI에서 로그를 볼 수 있습니다.

- **파일 시스템** - `/var/log` 시스템 디렉터리에 모든 환경에 대한 로그가 있습니다. `var/log/` 디렉터리에 특정 환경에 고유한 앱별 로그가 있습니다. 이러한 디렉토리는 클러스터의 노드 간에 공유되지 않습니다. Pro 프로덕션 및 스테이징 환경에서는 각 노드의 로그를 확인해야 합니다.

- **[!DNL Cloud Console]**—환경 _메시지_ 목록에서 로그 정보를 빌드, 배포 및 배포 후 확인할 수 있습니다.

- **Cloud CLI** - `magento-cloud log` 명령을 사용하여 로컬 환경 로그를 보거나 `magento-cloud ssh` 명령을 사용하여 원격 환경 로그를 볼 수 있습니다.

## 로그 위치

시스템 로그는 다음 위치에 저장됩니다.

- 통합: `/var/log/<log-name>.log`
- Pro 스테이징: `/var/log/platform/<project-ID>_stg/<log-name>.log`
- 프로프로덕션: `/var/log/platform/<project-ID>/<log-name>.log`

`<project-ID>`의 값은 프로젝트와 환경이 스테이징인지 아니면 프로덕션인지에 따라 다릅니다. 예를 들어 프로젝트 ID가 `yw1unoukjcawe`인 경우 스테이징 환경 사용자는 `yw1unoukjcawe_stg`이고 프로덕션 환경 사용자는 `yw1unoukjcawe`입니다.

이 예제를 사용하면 배포 로그는 `/var/log/platform/yw1unoukjcawe_stg/deploy.log`입니다.

### 원격 환경 로그 보기

대부분의 로그에는 원격 환경에서 발생하는 이벤트가 포함됩니다. Pro의 경우 여러 노드가 있고 각 노드에는 고유한 로그가 있습니다. 모든 호스트 목록을 보려면 다음을 사용하십시오.

```bash
magento-cloud ssh -p <project-ID> -e <environment-ID> --all
```

샘플 응답:

```terminal
1.ent-project-environment-id@ssh.region.magento.cloud
2.ent-project-environment-id@ssh.region.magento.cloud
3.ent-project-environment-id@ssh.region.magento.cloud
```

**원격 환경 로그 목록을 보려면**:

```bash
magento-cloud ssh -e <environment-ID> "ls var/log"
```

Pro의 경우:

```bash
ssh 1.ent-project-environment-id@ssh.region.magento.cloud "ls var/log | grep error"
```

**원격 로그를 보려면**:

```bash
magento-cloud ssh -e <environment-ID> "cat var/log/cron.log"
```

Pro의 경우:

```bash
ssh 1.ent-project-environment-id@ssh.region.magento.cloud "cat var/log/cron.log"
```

>[!TIP]
>
>Pro 스태이징 및 프로덕션 환경의 경우, 고정된 파일 이름의 로그 파일에 대해 자동 로그 회전, 압축 및 제거를 사용할 수 있습니다. 각 로그 파일 유형에는 회전 패턴과 수명이 있습니다. 스타터 환경에는 로그 회전이 없습니다. 환경의 로그 회전 및 압축된 로그 수명에 대한 전체 세부 정보는 `/etc/logrotate.conf` 및 `/etc/logrotate.d/<various>`에서 찾을 수 있습니다. Pro 통합 환경에서는 로그 순환을 구성할 수 없습니다. Pro 통합의 경우 사용자 지정 솔루션/스크립트를 구현하고 필요에 따라 스크립트를 실행하도록 [cron을 구성](../application/crons-property.md)해야 합니다.

## 로그 작성 및 배포

변경 내용을 환경에 푸시한 후 `var/log/cloud.log` 파일의 각 후크에서 로깅을 검토할 수 있습니다. 로그에는 각 후크에 대한 시작 및 중지 메시지가 포함되어 있습니다. 다음 예제에서는 메시지가 &quot;`Starting post-deploy.`&quot; 및 &quot;`Post-deploy is complete.`&quot;입니다.

로그 항목에 대한 타임스탬프를 확인하고 특정 배포에 대한 로그를 확인합니다. 다음은 문제 해결에 사용할 수 있는 로그 출력의 압축된 예입니다.

```terminal
Re-deploying environment project-integration-ID
  Executing post deploy hook for service `mymagento`
    [2019-01-03 19:44:11] NOTICE: Starting post-deploy.
    [2019-01-03 19:44:11] INFO: Validating configuration
    [2019-01-03 19:44:11] INFO: End of validation
    [2019-01-03 19:44:11] INFO: Enable cron
    [2019-01-03 19:44:11] INFO: Create backup of important files.
    [2019-01-03 19:44:11] INFO: Backup /app/app/etc/env.php.bak for /app/app/etc/env.php was created.
    [2019-01-03 19:44:11] INFO: Backup /app/app/etc/config.php.bak for /app/app/etc/config.php was created.
    [2019-01-03 19:44:11] INFO: php ./bin/magento cache:flush --ansi --no-interaction
    [2019-01-03 19:44:32] INFO: Warming up failed: http://integration-id-project.us.magentosite.cloud/
    [2019-01-03 19:44:32] NOTICE: Post-deploy is complete.
```

>[!TIP]
>
>클라우드 환경을 구성할 때 빌드 및 배포 작업에 대해 [로그 기반 Slack 및 전자 메일 알림](../environment/set-up-notifications.md)을 설정할 수 있습니다.

다음 로그에는 모든 클라우드 프로젝트에 대한 공통 위치가 있습니다.

- **배포 로그**: `var/log/cloud.log`
- **마지막 배포 오류 로그**: `var/log/cloud.error.log`
- **디버그 로그**: `var/log/debug.log`
- **예외 로그**: `var/log/exception.log`
- **시스템 로그**: `var/log/system.log`
- **지원 로그**: `var/log/support_report.log`
- **보고서**: `var/report/`

`cloud.log` 파일에 배포 프로세스의 각 단계에서 받은 피드백이 포함되어 있지만 배포 후크에서 만든 로그는 각 환경에 대해 고유합니다. 환경별 배포 로그는 다음 디렉토리에 있습니다.

- **Starter 및 Pro 통합**: `/var/log/deploy.log`
- **Pro 스테이징**: `/var/log/platform/<project-ID>_stg/deploy.log`
- **Pro 프로덕션**: `/var/log/platform/<project-ID>/deploy.log`

### 로그 배포

각 배포에 대한 로그는 특정 `deploy.log` 파일에 연결됩니다. 다음 예제에서는 터미널에 현재 환경의 배포 로그를 인쇄합니다.

```bash
magento-cloud log -e <environment-ID> deploy
```

샘플 응답:

```terminal
Reading log file projectID-branchname-ID--mymagento@ssh.zone.magento.cloud:/var/log/'deploy.log'

[2023-04-24 18:58:03.080678] Launching command 'b'php ./vendor/bin/ece-tools run scenario/deploy.xml\n''.

[2023-04-24T18:58:04.129888+00:00] INFO: Starting scenario(s): scenario/deploy.xml (magento/ece-tools version: 2002.1.14, magento/magento2-base version: 2.4.6)
[2023-04-24T18:58:04.364714+00:00] NOTICE: Starting pre-deploy.
...
```

{{scd-timing-warning}}

### 오류 로그

배포 프로세스 중에 생성된 오류 및 경고 메시지는 `var/log/cloud.log` 및 `var/log/cloud.error.log` 파일에 모두 기록됩니다. 클라우드 오류 로그 파일에는 최신 배포의 오류와 경고만 포함되어 있습니다. 빈 파일은 오류 없이 성공적으로 배포했음을 나타냅니다.

[Cloud CLI SSH](#view-remote-environment-logs)를 사용하여 로그 파일을 보거나 ECE-Tools를 사용하여 오류를 표시하며 다음과 같은 제안을 할 수 있습니다.

```bash
magento-cloud ssh -e <environment-ID> "./vendor/bin/ece-tools error:show"
```

샘플 응답:

```terminal
errorCode: 1001
stage: build
step: validate-config
suggestion: Please run the following commands:
1. bin/magento module:enable --all
2. git add -f app/etc/config.php
3. git commit -m 'Adding config.php'
4. git push
title: File app/etc/config.php does not exist
type: warning
---------------

errorCode: 1006
stage: build
step: validate-config
suggestion: Your application does not have the "post_deploy" hook enabled.
  In order to minimize downtime, add the following to ".magento.app.yaml":
  hooks:
      post_deploy: |
          php ./vendor/bin/ece-tools run scenario/post-deploy.xml
title: The configured state is not ideal
type: warning
```

대부분의 오류 메시지에는 설명 및 제안 작업이 포함되어 있습니다. 추가 지침을 위해 오류 코드를 조회하려면 [ECE-Tools용 오류 메시지 참조](../dev-tools/error-reference.md)를 사용하십시오. 자세한 지침은 [Adobe Commerce 배포 문제 해결사](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/deployment/magento-deployment-troubleshooter.html)를 사용하십시오.

## 애플리케이션 로그

배포 로그와 마찬가지로 애플리케이션 로그는 각 환경에 대해 고유합니다.

| 로그 파일 | Starter 및 Pro 통합 | 설명 |
| ------------------- | --------------------------- | ------------------------------------------------- |
| **로그 배포** | `/var/log/deploy.log` | [배포 후크](../application/hooks-property.md)의 활동. |
| **Post 배포 로그** | `/var/log/post_deploy.log` | [사후 배포 후크](../application/hooks-property.md)의 활동. |
| **크론 로그** | `/var/log/cron.log` | cron 작업의 출력. |
| **Nginx 액세스 로그** | `/var/log/access.log` | Nginx 시작 시 누락된 디렉터리 및 제외된 파일 유형에 대한 HTTP 오류가 발생합니다. |
| **Nginx 오류 로그** | `/var/log/error.log` | Nginx와 관련된 구성 오류를 디버깅하는 데 유용한 시작 메시지입니다. |
| **PHP 액세스 로그** | `/var/log/php.access.log` | PHP 서비스에 대한 요청입니다. |
| **PHP FPM 로그** | `/var/log/app.log` | |

Pro 스테이징 및 프로덕션 환경의 경우 배포, Post-배포 및 Cron 로그는 클러스터의 첫 번째 노드에서만 사용할 수 있습니다.

| 로그 파일 | Pro 스테이징 | 프로프로덕션 |
| ------------------- | --------------------------------------------------- | ----------------------------------------------- |
| **로그 배포** | 첫 번째 노드만:<br>`/var/log/platform/<project-ID>_stg/deploy.log` | 첫 번째 노드만:<br>`/var/log/platform/<project-ID>/deploy.log` |
| **Post 배포 로그** | 첫 번째 노드만:<br>`/var/log/platform/<project-ID>_stg/post_deploy.log` | 첫 번째 노드만:<br>`/var/log/platform/<project-ID>/post_deploy.log` |
| **크론 로그** | 첫 번째 노드만:<br>`/var/log/platform/<project-ID>_stg/cron.log` | 첫 번째 노드만:<br>`/var/log/platform/<project-ID>/cron.log` |
| **Nginx 액세스 로그** | `/var/log/platform/<project-ID>_stg/access.log` | `/var/log/platform/<project-ID>/access.log` |
| **Nginx 오류 로그** | `/var/log/platform/<project-ID>_stg/error.log` | `/var/log/platform/<project-ID>/error.log` |
| **PHP 액세스 로그** | `/var/log/platform/<project-ID>_stg/php.access.log` | `/var/log/platform/<project-ID>/php.access.log` |
| **PHP FPM 로그** | `/var/log/platform/<project-ID>_stg/php5-fpm.log` | `/var/log/platform/<project-ID>/php5-fpm.log` |

### 보관된 로그 파일

애플리케이션 로그는 하루에 한 번 압축되고 아카이빙되며 1년 동안 보관됩니다. `Number of Days Ago + 1`에 해당하는 고유 ID를 사용하여 압축 로그 이름을 지정합니다. 예를 들어 Pro 프로덕션 환경에서는 과거 21일 동안의 PHP 액세스 로그가 다음과 같이 저장되고 이름이 지정됩니다.

```terminal
/var/log/platform/<project-ID>/php.access.log.22.gz
```

아카이브된 로그 파일은 항상 압축 전에 원래 파일이 있던 디렉토리에 저장됩니다.

>[!NOTE]
>
>**Deploy** 및 **Post-deploy** 로그 파일이 회전되고 보관되지 않습니다. 전체 배포 기록은 이러한 로그 파일 내에 기록됩니다.

## 서비스 로그

각 서비스는 별도의 컨테이너에서 실행되므로 통합 환경에서 서비스 로그를 사용할 수 없습니다. Adobe Commerce on cloud infrastructure는 통합 환경에서만 웹 서버 컨테이너에 액세스할 수 있습니다. 다음 서비스 로그 위치는 Pro 프로덕션 및 스테이징 환경용입니다.

- **Redis 로그**: `/var/log/platform/<project-ID>_stg/redis-server-<project-ID>_stg.log`
- **Elasticsearch 로그**: `/var/log/elasticsearch/elasticsearch.log`
- **Java 가비지 수집 로그**: `/var/log/elasticsearch/gc.log`
- **메일 로그**: `/var/log/mail.log`
- **MySQL 오류 로그**: `/var/log/mysql/mysql-error.log`
- **MySQL 느린 로그**: `/var/log/mysql/mysql-slow.log`
- **RabbitMQ 로그**: `/var/log/rabbitmq/rabbit@host1.log`

서비스 로그는 로그 유형에 따라 서로 다른 기간 동안 보관되고 저장됩니다. 예를 들어 MySQL 로그는 7일 후에 제거되어 수명이 가장 짧습니다.

>[!TIP]
>
>크기 조정된 아키텍처의 로그 파일 위치는 노드 유형에 따라 다릅니다. [조정된 아키텍처의 로그 위치](../architecture/scaled-architecture.md#log-locations) 항목을 참조하십시오.

## Pro 프로덕션 및 스테이징에 대한 로그 데이터

Pro 프로덕션 및 스테이징 환경에서 프로젝트와 통합된 [New Relic 로그 관리](../monitor/log-management.md)를 사용하여 클라우드 인프라 프로젝트에서 Adobe Commerce과 연결된 모든 로그의 집계된 로그 데이터를 관리합니다.

New Relic 로그 애플리케이션은 클라우드 인프라 프로덕션 및 스테이징 환경에서 Adobe Commerce의 문제를 해결하고 모니터링하는 중앙 집중식 로그 관리 대시보드를 제공합니다. 또한 대시보드는 Fastly CDN, 이미지 최적화 및 WAF(Web Application Firewall) 서비스의 로그 데이터에 대한 액세스를 제공합니다. [New Relic 서비스](../monitor/new-relic-service.md)를 참조하세요.
