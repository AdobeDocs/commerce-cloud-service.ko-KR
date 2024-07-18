---
title: 구성 요소 장애 복구
description: 구성 요소가 클라우드 인프라의 Adobe Commerce에서 제대로 배포되지 않는 경우 복구하는 방법을 알아봅니다.
feature: Cloud, Deploy
exl-id: 4855be0c-6883-4ab1-a364-316d10e97250
source-git-commit: b49a51aba56f79b5253eeacb1adf473f42bb8959
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 0%

---

# 구성 요소 장애 복구

이 항목에서는 구성 요소가 제대로 배포되지 않을 경우 복구하는 방법을 설명합니다. 일반적인 예에는 호환되지 않는 PHP 버전과 같이 원격 환경에서 충족되지 않는 종속성이 있는 구성 요소가 포함됩니다.

다음 방법 중 하나로 실패한 배포에서 복구할 수 있습니다.

- [백업 복원](../storage/snapshots.md#restore-a-snapshot)
- 이전 변경 사항에서 프로젝트 및 코드 정리 및 재배포

## 정리, 제거 및 재배포

이전 배포에서 정리하려면 추가되거나 업데이트된 구성 요소를 식별한 다음 제거합니다. 먼저 원격 환경에 로그인하고 `var` 디렉터리의 내용을 수동으로 지웁니다. 그런 다음 `composer.json` 파일에서 구성 요소를 제거하고 환경을 다시 배포합니다.

**`var` 디렉터리를 정리하려면**:

1. 로컬 워크스테이션에서 프로젝트 디렉터리로 변경합니다.

1. SSH를 사용하여 원격 환경에 로그인합니다.

   ```bash
   magento-cloud ssh
   ```

1. `var` 디렉터리를 지웁니다.

   ```shell
   rm -rf var/*
   ```

1. 로그아웃.

**구성 요소를 제거하려면**:

1. 로컬 워크스테이션에서 프로젝트 디렉터리로 변경합니다.

1. 캐시를 지웁니다.

   ```bash
   composer clear-cache
   ```

1. `composer.json` 파일에서 구성 요소를 제거합니다.

   ```bash
   composer remove <component-name>:<version>
   ```

   다음 메시지가 표시되면 더 이상 작업을 수행하지 않아도 됩니다.

   ```
   Package "<name>:<version>" listed for update is not installed. Ignoring.
   ```

1. 종속성이 업데이트되는 동안 잠시 기다려 주십시오.

1. 코드 변경 사항을 추가, 커밋 및 푸시합니다.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "<message>"
   ```

   ```bash
   git push origin <environment-ID>
   ```

{{redeploy-warning}}

[환경 복원](../development/restore-environment.md)에서 백업 없이 환경을 복원하는 방법에 대해 자세히 알아보세요.

{{stuck-deployment-tip}}
