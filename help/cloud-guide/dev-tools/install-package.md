---
title: ECE-Tools를 사용하도록 프로젝트 업그레이드
description: ECE-Tools 패키지를 사용하고 최신 수정 사항 및 기능을 활용하기 위해 Adobe Commerce on cloud infrastructure 프로젝트를 업그레이드하는 방법에 대해 알아봅니다.
feature: Cloud, Install
exl-id: 820cca84-2817-4881-829f-ebb78400d8c7
source-git-commit: b49a51aba56f79b5253eeacb1adf473f42bb8959
workflow-type: tm+mt
source-wordcount: '365'
ht-degree: 0%

---

# ECE-Tools 패키지를 사용하도록 프로젝트 업그레이드

Adobe은 `ece-tools` 패키지를 위해 `magento/magento-cloud-configuration` 및 `magento/ece-patches` 패키지를 더 이상 사용하지 않으므로 많은 클라우드 프로세스를 단순화합니다. `ece-tools` 패키지가 _포함되지_&#x200B;않은 클라우드 인프라 프로젝트에서 이전 Adobe Commerce을 사용하는 경우 프로젝트에 대해 일회성, 수동 _업그레이드_ 프로세스를 수행해야 합니다.

>[!WARNING]
>
>프로젝트에 `ece-tools` 패키지가 포함된 경우 다음 업그레이드를 건너뛸 수 있습니다. 확인하려면 로컬 프로젝트 루트 디렉터리에서 `php vendor/bin/ece-tools -V` 명령을 사용하여 [!DNL Commerce] 버전을 검색하십시오.

이 프로젝트를 업그레이드하려면 루트 디렉터리의 `composer.json` 파일에서 `magento/magento-cloud-metapackage` 버전 제약 조건을 업데이트해야 합니다. 이 제한 사항을 사용하면 현재 Adobe Commerce 버전을 업그레이드하지 않고도 더 이상 사용되지 않는 패키지를 제거하는 등 Adobe Commerce에서 클라우드 인프라 메타패키지를 업데이트할 수 있습니다.

{{upgrade-tip}}

## 더 이상 사용되지 않는 패키지 제거

`ece-tools` 패키지를 사용하도록 업그레이드를 수행하기 전에 더 이상 사용되지 않는 다음 패키지에 대한 `composer.lock` 파일을 확인하십시오.

- `magento/magento-cloud-configuration`
- `magento/ece-patches`

## 메타패키지 업데이트

각 Adobe Commerce 버전에는 다음에 따라 다른 제한 사항이 필요합니다.

```
>=current_version <next_version
```

- `current_version`의 경우 설치할 Adobe Commerce 버전을 지정하십시오.
- `next_version`의 경우 `current_version`에 지정된 값 뒤에 다음 패치 버전을 지정하십시오.

Adobe Commerce `2.3.5-p2`을(를) 설치하려면 `current_version`을(를) `2.3.5`(으)로 설정하고 `next_version`을(를) `2.3.6`(으)로 설정합니다. 제약 조건 `">=2.3.5 <2.3.6"`은(는) 2.3.5에 사용 가능한 최신 패키지를 설치합니다.

[`magento-cloud` 템플릿](https://github.com/magento/magento-cloud/blob/master/composer.json)에서 항상 최신 메타패키지 제약 조건을 찾을 수 있습니다.

다음 예제에서는 Adobe Commerce on cloud infrastructure 메타패키지에 대해 현재 버전 2.4.7보다 크거나 같고 다음 버전 2.4.8보다 작은 버전을 제한합니다.

```json
"require": {
    "magento/magento-cloud-metapackage": ">=2.4.7 <2.4.8"
},
```

## 프로젝트 업그레이드

`ece-tools` 패키지를 사용하도록 프로젝트를 업그레이드하려면 메타패키지 및 `.magento.app.yaml` 후크 속성을 업데이트하고 작성기 업데이트를 수행해야 합니다.

**ece 도구를 사용하도록 프로젝트를 업그레이드하려면**:

1. `composer.json` 파일에서 `magento/magento-cloud-metapackage` 버전 제약 조건을 업데이트합니다.

   ```bash
   composer require "magento/magento-cloud-metapackage":">=2.4.7 <2.4.8" --no-update
   ```

1. 메타패키지를 업데이트합니다.

   ```bash
   composer update magento/magento-cloud-metapackage
   ```

1. `magento.app.yaml` 파일에서 후크 명령을 수정합니다.

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

1. [더 이상 사용되지 않는 패키지](#remove-deprecated-packages)를 확인하고 제거하십시오. 더 이상 사용되지 않는 패키지로 인해 업그레이드가 성공적으로 수행되지 않을 수 있습니다.

   ```bash
   composer remove magento/magento-cloud-configuration
   ```

   ```bash
   composer remove magento/ece-patches
   ```

1. `ece-tools` 패키지를 업데이트해야 할 수 있습니다.

   ```bash
   composer update magento/ece-tools
   ```

1. 코드 변경 사항을 추가하고 커밋합니다. 이 예에서는 다음 파일이 업데이트되었습니다.

   ```
   .magento.app.yaml
   composer.json
   composer.lock
   ```

1. 코드 변경 내용을 원격 서버에 푸시하고 이 분기를 `integration` 분기와 병합합니다.

   ```bash
   git push origin <branch-name>
   ```
