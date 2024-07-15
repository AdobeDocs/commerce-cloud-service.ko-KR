---
title: 데이터베이스 백업
description: ECE-tools를 사용하여 Adobe Commerce on cloud infrastructure 프로젝트에 사용할 데이터베이스 백업을 만드는 방법에 대해 알아봅니다.
feature: Cloud, Iaas, Storage
exl-id: 8a96effe-a587-4edf-b0c7-e73ca8d3b56c
source-git-commit: 4d790bff2ba5d02ef10de5c36a2f0d140e31a407
workflow-type: tm+mt
source-wordcount: '339'
ht-degree: 0%

---

# 데이터베이스 백업

서비스 및 마운트에서 모든 환경 데이터를 캡처하지 않고 `ece-tools db-dump` 명령을 사용하여 데이터베이스의 복사본을 만들 수 있습니다. 기본적으로 이 명령은 환경 구성에 지정된 모든 데이터베이스 연결에 대해 `/app/var/dump-main` 디렉터리에 백업을 만듭니다. DB 덤프 작업은 응용 프로그램을 유지 관리 모드로 전환하고 소비자 큐 프로세스를 중지하며 덤프가 시작되기 전에 cron 작업을 비활성화합니다.

DB 덤프에 대한 다음 지침을 고려하십시오.

- Adobe 프로덕션 환경의 경우, 사이트가 유지 관리 모드에 있을 때 발생하는 서비스 중단을 최소화하기 위해 사용량이 적은 시간 동안 데이터베이스 덤프 작업을 완료하는 것이 좋습니다.
- 덤프 작업 중에 오류가 발생하면 이 명령은 디스크 공간을 절약하기 위해 덤프 파일을 삭제합니다. 자세한 내용은 로그를 검토하십시오(`var/log/cloud.log`).
- Pro 프로덕션 환경의 경우 이 명령은 세 개의 고가용성 노드 중 _one_&#x200B;에서만 덤프하므로 덤프 중에 다른 노드에 기록된 프로덕션 데이터는 복사되지 않을 수 있습니다. 명령이 둘 이상의 노드에서 실행되지 않도록 하기 위해 명령이 `var/dbdump.lock` 파일을 생성합니다.
- Adobe 모든 환경 서비스를 백업하려면 [백업](snapshots.md)을(를) 만드는 것이 좋습니다.

데이터베이스 이름을 명령에 추가하여 여러 데이터베이스를 백업하도록 선택할 수 있습니다. 다음 예제에서는 두 개의 데이터베이스를 백업합니다. `main` 및 `sales`:

```bash
php vendor/bin/ece-tools db-dump main sales
```

추가 옵션을 보려면 `php vendor/bin/ece-tools db-dump --help` 명령을 사용하십시오.

- `--dump-directory=<dir>`—데이터베이스 덤프의 대상 디렉터리를 선택하십시오.
- `--remove-definers`—데이터베이스 덤프에서 DEFINER 문 제거

**스테이징 또는 프로덕션 환경에서 데이터베이스 덤프를 만들려면**:

1. [SSH를 사용하여 로그인하거나 터널을 만들어 원격 환경에 연결합니다](../development/secure-connections.md) 복사할 데이터베이스가 들어 있습니다.

1. 환경 관계를 나열하고 데이터베이스 로그인 정보를 확인합니다.

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   또는

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"]))->database);'
   ```

1. 데이터베이스의 백업을 만듭니다. DB 덤프의 대상 디렉터리를 선택하려면 `--dump-directory` 옵션을 사용합니다.

   ```bash
   php vendor/bin/ece-tools db-dump -- main
   ```

   샘플 응답:

   ```terminal
   The db-dump operation switches the site to maintenance mode, stops all active cron jobs and consumer queue processes, and disables cron jobs before starting the dump process.
   Your site will not receive any traffic until the operation completes.
   Do you wish to proceed with this process? (y/N)? y
   2020-01-28 16:38:08] INFO: Starting backup.
   [2020-01-28 16:38:08] NOTICE: Enabling Maintenance mode
   [2020-01-28 16:38:10] INFO: Trying to kill running cron jobs and consumers processes
   [2020-01-28 16:38:10] INFO: Running Magento cron and consumers processes were not found.
   [2020-01-28 16:38:10] INFO: Waiting for lock on db dump.
   [2020-01-28 16:38:10] INFO: Start creation DB dump for main database...
   [2020-01-28 16:38:10] INFO: Finished DB dump for main database, it can be found here: /tmp/qxmtlseakof6y/dump-main-1580229490.sql.gz
   [2020-01-28 16:38:10] INFO: Backup completed.
   [2020-01-28 16:38:11] NOTICE: Maintenance mode is disabled.
   ```

1. `db-dump` 명령은 원격 프로젝트 디렉터리에 `dump-<timestamp>.sql.gz` 보관 파일을 만듭니다.

>[!TIP]
>
>이 데이터를 특정 환경에 푸시하려면 [데이터 및 정적 파일 마이그레이션](../deploy/staging-production.md#migrate-static-files)을 참조하십시오.
