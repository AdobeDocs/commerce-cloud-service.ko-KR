---
title: 디스크 공간 관리
description: 명령줄 인터페이스를 사용하여 디스크 공간을 관리하는 방법에 대해 알아봅니다.
feature: Cloud, Storage
exl-id: 480cb33b-ac83-441d-946e-5b4de09ad84e
source-git-commit: 8b40397796ee865aefbf8a7948cc9a3aceb1d35c
workflow-type: tm+mt
source-wordcount: '714'
ht-degree: 0%

---

# 디스크 공간 관리

Adobe Commerce on cloud infrastructure 계약 및 를 통해 클라우드 프로젝트에 대한 총 스토리지 용량을 찾을 수 있습니다. [계정 페이지](https://accounts.magento.cloud/user). 계정의 각 프로젝트 카드에 _환경_, _스토리지_ 용량(GB) 및 _사용자_. 또는 다음 클라우드 명령을 사용할 수 있습니다.

```bash
magento-cloud subscription:info | grep storage
```

샘플 응답:

```terminal
| storage              | 51200
```

Pro 프로덕션 또는 스테이징 환경이 스토리지 용량의 95%에 도달하거나 초과할 경우 클라우드 인프라 모니터링 도구는 스토리지 용량의 자동 증가를 알리는 지원 경고를 트리거합니다.

알림 예:

>[!BEGINSHADEBOX]

_&quot;모니터링에서 클러스터(project-id-environment)의 파일 저장소가 거의 꽉 찼음을 감지했습니다. 디스크 사용량이 현재 1GiB 미만의 중요한 사용 수준에 있습니다. 공유 스토리지 볼륨을 현재 60GiB에서 70GiB로 업그레이드하여 서비스를 계속 실행할 수 있습니다. 프로덕션 및 스테이징 파일 사용량을 살펴보고 일부 공간을 정리할 수 있는지 확인하십시오.&quot;_

>[!ENDSHADEBOX]

>[!TIP]
>
>이러한 자동 증가를 피하기 위해 저장 용량을 정기적으로 모니터링하고 90% 미만으로 유지하는 것이 좋습니다. 할당되면 Pro 스테이징 및 프로덕션에 대한 스토리지 증가는 되돌릴 수 없습니다.

## 통합 환경 확인

다음을 사용하여 통합 환경에 대한 디스크 공간 사용을 확인할 수 있습니다. `magento-cloud` CLI.

**대략적인 디스크 공간 사용 확인**:

```bash
magento-cloud db:size
```

샘플 응답:

```terminal
Checking database service mysql...

+----------------+-----------------+--------+
| Allocated disk | Estimated usage | % used |
+----------------+-----------------+--------+
| 2.0 GiB        | 193.3 MiB       | ~ 9%   |
+----------------+-----------------+--------+
```

모든 마운트는 디스크를 공유합니다. 를 사용하여 마운트에 대한 디스크 공간 사용을 확인할 수 있습니다. `magento-cloud` CLI.

**마운트에 대한 대략적인 디스크 공간 사용 확인**:

```bash
magento-cloud mount:size
```

샘플 응답:

```terminal
Checking disk usage for all mounts on <project>-<environment>-mymagento@ssh.us.magento.cloud...

+------------+-----------+---------+-----------+-----------+--------+
| Mount(s)   | Size(s)   | Disk    | Used      | Available | % Used |
+------------+-----------+---------+-----------+-----------+--------+
| app/etc    | 184 KiB   | 1.9 GiB | 481.3 MiB | 1.4 GiB   | 24.7%  |
| pub/media  | 128 KiB   |         |           |           |        |
| pub/static | 158.2 MiB |         |           |           |        |
| var        | 316.7 MiB |         |           |           |        |
+------------+-----------+---------+-----------+-----------+--------+
```

## 전용 클러스터 확인

Pro Staging 및 Production 환경의 경우 `disk free` 파일 시스템에서 사용하는 디스크 공간을 보고하는 명령입니다. 원격 환경에 로그인하려면 SSH를 사용해야 합니다.

```bash
df -h
```

다음 `-h` 옵션은 사람이 읽을 수 있는 형식(KB, MB 또는 GB)을 사용하여 보고서를 표시합니다.

다음 샘플 응답에서 `/mnt/shared` 마운트에는 미디어 및 의 디스크 공간이 표시됩니다. `/data/mysql/` 마운트하면 데이터베이스의 디스크 공간이 표시됩니다.

```terminal
Filesystem                                    Size  Used Avail Use% Mounted on
udev                                           16G     0   16G   0% /dev
tmpfs                                         3.2G  9.1M  3.2G   1% /run
/dev/xvda1                                     59G  8.9G   48G  16% /
tmpfs                                          16G   36K   16G   1% /dev/shm
tmpfs                                         5.0M     0  5.0M   0% /run/lock
tmpfs                                          16G     0   16G   0% /sys/fs/cgroup
/dev/xvdj                                     9.8G  2.3G  7.6G  23% /data/mysql
/dev/xvdi                                     9.8G  491M  9.3G   5% /data/exports
192.168.5.5:/shared                           9.8G  591M  9.3G   6% /mnt/shared
/dev/loop0                                     91M   91M     0 100% /app/project
192.168.5.5:/shared/project/var         9.8G  591M  9.3G   6% /app/project/var
192.168.5.5:/shared/project/app/etc     9.8G  591M  9.3G   6% /app/project/app/etc
192.168.5.5:/shared/project/pub/media   9.8G  591M  9.3G   6% /app/project/pub/media
192.168.5.5:/shared/project/pub/static  9.8G  591M  9.3G   6% /app/project/pub/static
```

디렉터리를 지정하여 응답을 제한할 수 있습니다. For example:

```bash
df -h var/
```

샘플 응답:

```terminal
Filesystem                                    Size  Used Avail Use% Mounted on
192.168.5.5:/shared/project/var         9.8G  591M  9.3G   6% /app/project/var
```

## 디스크 공간 할당

2 [구성 파일](../environment/overview.md) 클라우드 환경에서 디스크 공간 할당 제어: `.magento.app.yaml` 파일 및 `.magento/services.yaml` 파일. 각 파일에는 `disk` 속성은 각 구성의 디스크 크기 값(MB)을 정의합니다. Pro 통합 및 Starter 환경에서만 디스크 공간 할당을 변경할 수 있습니다.

>[!IMPORTANT]
>
>Pro 프로덕션 및 스테이징 환경의 경우 [Adobe Commerce 지원 티켓 제출](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) 디스크 공간 할당을 변경합니다. Pro 프로덕션 및 스테이징 환경의 크기 증가는 특정 간격으로만 이루어질 수 있으므로 현재 디스크 공간 사용량에 따라 디스크 공간 할당을 최소 10GB 늘리는 것이 좋습니다. 할당되면 Pro 스테이징 및 프로덕션에 대한 스토리지 증가는 되돌릴 수 없습니다. 리소스 간에 스토리지를 재할당하거나 재배포할 수 없습니다. 파일 저장 공간을 더 추가하려면 MySQL에 할당된 디스크 공간을 줄이십시오.

### 응용 프로그램 디스크 공간

다음 `.magento.app.yaml` 파일은 [영구 디스크 공간](../application/properties.md#disk) 애플리케이션에서 사용할 수 있습니다.

**응용 프로그램의 디스크 공간을 늘리려면**:

1. 로컬 개발 환경에서 `.magento.app.yaml` 구성 파일입니다.

1. 에 대한 새 값 설정 `disk` 속성(MB).

   ```yaml
   disk: <value-mb>
   ```

1. 파일에 변경 사항을 저장합니다.

1. 코드 변경 사항을 추가, 커밋 및 푸시합니다.

   ```bash
   git add .magento.app.yaml && git commit -m "Increase disk space for application" && git push origin <branch-name>
   ```

   업데이트된 YAML 파일을 원격 환경에 푸시하면 변경 사항이 적용됩니다.

### 서비스 디스크 공간

다음 `.magento/services.yaml` 파일은 MySQL 및 Redis와 같은 각 서비스에서 사용할 수 있는 디스크 공간을 제어합니다.

**서비스의 디스크 공간을 늘리려면**:

1. 로컬 개발 환경에서 `.magento/services.yaml` 구성 파일입니다.

1. 파일에서 서비스를 추가하거나 찾습니다. 다음을 참조하십시오 [서비스 구성에 대한 자세한 정보](../services/services-yaml.md).

1. 디스크 속성에 대한 새 값(MB)을 설정합니다.

   ```yaml
   <name>:
       type: <service-name>:<service-version>
       disk: <value-mb>
   ```

1. 파일에 변경 사항을 저장합니다.

1. 코드 변경 사항을 추가, 커밋 및 푸시합니다.

   ```bash
   git add .magento/services.yaml && git commit -m "Increase disk space for service" && git push origin <branch-name>
   ```

   업데이트된 YAML 파일을 원격 환경에 푸시하면 변경 사항이 적용됩니다.

## 디스크 공간 모니터링

Pro 프로덕션 환경에서는 New Relic에 대한 Adobe Commerce 경고 관리 정책을 사용하여 디스크 공간 및 기타 성능 지표를 모니터링할 수 있습니다. 자세한 내용은 [관리 경고로 성능 모니터링](../monitor/investigate-performance.md#monitor-performance-with-managed-alerts). 자세한 지침은 다음을 참조하십시오. [데이터베이스 성능 문제 해결 모범 사례](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/maintenance/resolve-database-performance-issues.html).

## 남은 공간 없음

빌드 캐시는 시간이 지남에 따라 늘어날 수 있습니다. 다음 내용이 포함된 경고가 표시되는 경우 `No space left on device`를 클릭하여 빌드 캐시를 지우고 다시 배포해 보십시오.

```bash
magento-cloud project:clear-build-cache
```
