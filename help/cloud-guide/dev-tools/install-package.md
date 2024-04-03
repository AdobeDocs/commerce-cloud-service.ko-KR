---
title: ECE-Tools를 사용하도록 프로젝트 업그레이드
description: ECE-Tools 패키지를 사용하고 최신 수정 사항 및 기능을 활용하기 위해 Adobe Commerce on cloud infrastructure 프로젝트를 업그레이드하는 방법에 대해 알아봅니다.
feature: Cloud, Install
exl-id: 820cca84-2817-4881-829f-ebb78400d8c7
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '365'
ht-degree: 0%

---

# ECE-Tools 패키지를 사용하도록 프로젝트 업그레이드

Adobe 사용 안 함 `magento/magento-cloud-configuration` 및 `magento/ece-patches` 다음을 위한 패키지 `ece-tools` 여러 클라우드 프로세스를 단순화하는 패키지 이전 Adobe Commerce on cloud infrastructure 프로젝트를 사용하는 경우 _아님_ 다음을 포함: `ece-tools` 패키지를 선택한 다음 한 번의 수동 작업을 수행해야 합니다. _업그레이드_ 을 프로젝트에 추가합니다.

>[!WARNING]
>
>프로젝트에 `ece-tools` 패키지: 다음 업그레이드를 건너뛸 수 있습니다. 확인하려면 [!DNL Commerce] 버전 사용 `php vendor/bin/ece-tools -V` 로컬 프로젝트 루트 디렉터리의 명령입니다.

이 프로젝트 업그레이드 프로세스를 수행하려면 다음을 업데이트해야 합니다. `magento/magento-cloud-metapackage` 의 버전 제한 `composer.json` 루트 디렉토리에 있는 파일입니다. 이 제한 사항을 사용하면 현재 Adobe Commerce 버전을 업그레이드하지 않고도 더 이상 사용되지 않는 패키지를 제거하는 등 Adobe Commerce에서 클라우드 인프라 메타패키지를 업데이트할 수 있습니다.

{{upgrade-tip}}

## 더 이상 사용되지 않는 패키지 제거

업그레이드를 수행하여 를 사용하기 전에 `ece-tools` 패키지, 확인 `composer.lock` 더 이상 사용되지 않는 다음 패키지에 대한 파일:

- `magento/magento-cloud-configuration`
- `magento/ece-patches`

## 메타패키지 업데이트

각 Adobe Commerce 버전에는 다음에 따라 다른 제한 사항이 필요합니다.

```terminal
>=current_version <next_version
```

- 대상 `current_version`를 클릭하고 설치할 Adobe Commerce 버전을 지정합니다.
- 대상 `next_version`에 지정된 값 뒤에 있는 다음 패치 버전을 지정합니다. `current_version`.

Adobe Commerce을 설치하려면 `2.3.5-p2`, 설정됨 `current_version` 끝 `2.3.5` 및 `next_version` 끝 `2.3.6`. 제한 `">=2.3.5 <2.3.6"` 2.3.5에 사용 가능한 최신 패키지를 설치합니다.

항상 다음에서 최신 메타패키지 제약 조건을 찾을 수 있습니다 [`magento-cloud` 템플릿](https://github.com/magento/magento-cloud/blob/master/composer.json).

다음 예제에서는 Adobe Commerce on cloud infrastructure 메타패키지에 대해 현재 버전 2.4.5보다 크거나 같고 다음 버전 2.4.6보다 작은 버전을 제한합니다.

```json
"require": {
    "magento/magento-cloud-metapackage": ">=2.4.5 <2.4.6"
},
```

## 프로젝트 업그레이드

프로젝트를 업그레이드하여 `ece-tools` 패키지, 메타패키지 및 `.magento.app.yaml` 속성을 후크하고 Composer 업데이트를 수행합니다.

**ece-tools를 사용하도록 프로젝트를 업그레이드하려면**:

1. 업데이트 `magento/magento-cloud-metapackage` 의 버전 제한 `composer.json` 파일.

   ```bash
   composer require "magento/magento-cloud-metapackage":">=2.4.5 <2.4.6" --no-update
   ```

1. 메타패키지를 업데이트합니다.

   ```bash
   composer update magento/magento-cloud-metapackage
   ```

1. 에서 후크 명령을 수정합니다. `magento.app.yaml` 파일.

   ```yaml
   hooks:
       # We run build hooks before your application has been packaged.
       build: |
           set -e
           php ./vendor/bin/ece-tools run scenario/build/generate.xml
           php ./vendor/bin/ece-tools run scenario/build/transfer.xml
       # We run deploy hook after your application has been deployed and started.
       deploy: |
           php ./vendor/bin/ece-tools run scenario/deploy.xml
       # We run post deploy hook to clean and warm the cache. Available with ECE-Tools 2002.0.10.
       post_deploy: |
           php ./vendor/bin/ece-tools run scenario/post-deploy.xml
   ```

1. 확인 후 제거 [더 이상 사용되지 않는 패키지](#remove-deprecated-packages). 더 이상 사용되지 않는 패키지로 인해 업그레이드가 성공적으로 수행되지 않을 수 있습니다.

   ```bash
   composer remove magento/magento-cloud-configuration
   ```

   ```bash
   composer remove magento/ece-patches
   ```

1. 를 업데이트해야 할 수 있습니다. `ece-tools` 패키지.

   ```bash
   composer update magento/ece-tools
   ```

1. 코드 변경 사항을 추가하고 커밋합니다. 이 예에서는 다음 파일이 업데이트되었습니다.

   ```terminal
   .magento.app.yaml
   composer.json
   composer.lock
   ```

1. 코드 변경 내용을 원격 서버에 푸시하고 이 분기를 `integration` 분기입니다.

   ```bash
   git push origin <branch-name>
   ```
