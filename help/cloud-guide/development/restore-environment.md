---
title: 환경 복원
description: 클라우드 인프라 프로젝트에서 Adobe Commerce 애플리케이션을 제거하고 환경을 안정적인 상태로 복원하는 방법에 대해 알아봅니다.
role: Developer
topic: Development
exl-id: b76bd6c3-986e-4adc-abd0-5b27db0d8a3b
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '480'
ht-degree: 0%

---

# 환경 복원

통합 환경에서 문제가 발생하고 [유효한 백업](../storage/snapshots.md)를 클릭하고 다음 방법 중 하나를 사용하여 환경을 복원해 보십시오.

- Git 분기의 코드 재설정 또는 되돌리기
- 제거 [!DNL Commerce] 애플리케이션
- 강제 재배포
- 수동으로 데이터베이스 재설정

{{stuck-deployment-tip}}

## Git 분기 재설정

Git 분기를 재설정하면 코드가 과거에 안정적인 상태로 되돌아갑니다.

**분기를 재설정하려면**:

1. 로컬 워크스테이션에서 프로젝트 디렉터리로 변경합니다.

1. Git 커밋 내역을 검토합니다. 사용 `--oneline` 한 줄에 약식 커밋을 표시하려면 다음을 수행합니다.

   ```bash
   git log --oneline
   ```

   샘플 응답:

   ```terminal
   6bf9f45 (HEAD -> master, magento/master, magento/develop, magento/HEAD, develop) Create composer.lock
   34d7434 2.4.6 upgrade
   b69803c Update composer.lock
   c1bca24 Add sample data
   ec604c3 Update magento/ece-tools
   ...
   ```

1. 마지막으로 알려진 안정적인 코드 상태를 나타내는 커밋 해시를 선택합니다.

   분기를 초기화된 원래 상태로 재설정하려면 분기를 만든 첫 번째 커밋을 찾습니다. 다음을 사용할 수 있습니다. `--reverse` 시간 순서를 반대로 표시합니다.

1. 하드 재설정 옵션을 사용하여 분기를 재설정합니다. 이 명령은 선택한 커밋 이후의 모든 변경 사항을 무시하므로 이 명령을 사용할 때 주의하십시오.

   ```bash
   git reset --hard <commit>
   ```

1. 변경 사항을 푸시하여 재배포를 트리거하고 이를 통해 Adobe Commerce이 다시 설치됩니다.

   ```bash
   git push --force <origin> <branch>
   ```

## Commerce 제거

제거 [!DNL Commerce] 응용 프로그램이 데이터베이스를 복원하고 배포 구성을 제거하고 `var/` 하위 디렉토리. 또한 이 지침은 git 분기를 이전의 안정적인 상태로 재설정합니다. 최근 백업이 없지만 SSH를 사용하여 원격 환경에 액세스할 수 있는 경우 다음 단계에 따라 환경을 복원합니다.

- 구성 관리 비활성화
- Adobe Commerce 제거
- git 분기 재설정

Adobe Commerce 소프트웨어를 제거하면 데이터베이스가 삭제 및 복원되고 배포 구성이 제거되며 `var/` 하위 디렉토리. 비활성화하는 것이 중요합니다 [구성 관리](../store/store-settings.md) 따라서 다음 배포 중에 이전 구성 설정을 자동으로 적용하지 않습니다. 다음을 확인하십시오. `app/etc/` 디렉터리에 다음 항목이 없습니다. `config.php` 파일.

**Adobe Commerce 소프트웨어를 제거하려면**:

1. 로컬 워크스테이션에서 프로젝트 디렉터리로 변경합니다.

1. SSH를 사용하여 원격 환경에 로그인합니다.

   ```bash
   magento-cloud ssh
   ```

1. 구성 파일을 제거합니다.
   - Adobe Commerce 2.2 이상:

     ```bash
     rm app/etc/config.php
     ```

   - Adobe Commerce 2.1의 경우:

     ```bash
     rm app/etc/config.local.php
     ```

1. Adobe Commerce 응용 프로그램을 제거합니다.

   ```bash
   php bin/magento setup:uninstall -n
   ```

1. Adobe Commerce이 성공적으로 제거되었는지 확인합니다.

   성공적으로 제거되었음을 확인하는 메시지가 표시됩니다.

   ```terminal
   [SUCCESS]: Magento uninstallation complete.
   ```

1. 지우기 `var/` 하위 디렉토리.

   ```bash
   rm -rf var/*
   ```

1. 로그아웃.

>[!TIP]
>
>선택적으로, 빌드 캐시를 정리하는 것이 좋습니다.
>
>```bash
>magento-cloud project:clear-build-cache
>```

## 강제 재배포

Adobe Commerce을 제거하려고 했지만 배포가 계속 실패하는 경우 수동으로 재배포를 강제 적용할 수 있습니다.

```bash
git commit --allow-empty -m "<message>" && git push <origin> <branch>
```

## 데이터베이스 재설정

Adobe Commerce을 제거하려고 했지만 명령이 실패했거나 완료되지 않은 경우 데이터베이스를 수동으로 재설정할 수 있습니다.

**데이터베이스를 재설정하려면**:

1. 로컬 워크스테이션에서 프로젝트 디렉터리로 변경합니다.

1. SSH를 사용하여 원격 환경에 로그인합니다.

   ```bash
   magento-cloud ssh
   ```

1. 데이터베이스에 연결합니다.

   ```bash
   mysql -h database.internal
   ```

1. 드롭하기 `main` 데이터베이스.

   ```shell
   drop database main;
   ```

1. 빈 항목 만들기 `main` 데이터베이스.

   ```shell
   create database main;
   ```

1. 다음 구성 파일을 삭제합니다.

   - `config.php`
   - `config.php.bak`
   - `env.php`
   - `env.php.bak`

1. 로그아웃하고 재배포를 트리거합니다.

   ```bash
   magento-cloud environment:redeploy
   ```

{{redeploy-warning}}
