---
title: 시나리오 기반 배포
description: 사용자 지정 구성 파일을 사용하여 클라우드 인프라 배포에서 Adobe Commerce을 사용자 지정하는 방법을 알아봅니다.
feature: Cloud, Configuration, Deploy, Build
exl-id: df243068-c7a3-46dd-abe9-9e05198fa343
source-git-commit: 9b3772cf640ebc56063434e1aa8acb1ec51dc63c
workflow-type: tm+mt
source-wordcount: '808'
ht-degree: 0%

---

# 시나리오 기반 배포

`ece-tools` 2002.1.0 이상에서는 시나리오 기반 배포 기능을 사용하여 기본 배포 동작을 사용자 지정할 수 있습니다.
이 기능은 구성에서 **시나리오** 및 **단계**&#x200B;를 사용합니다.

- **시나리오 구성**-각 배포 후크는 배포 작업을 완료하기 위한 시퀀스 및 구성 매개 변수를 설명하는 XML 구성 파일인 *시나리오*&#x200B;입니다. `.magento.app.yaml` 파일의 `hooks` 섹션에서 시나리오를 구성합니다.

- **단계 구성**-각 시나리오에서는 배포 작업을 완료하는 데 필요한 작업을 프로그래밍 방식으로 설명하는 *단계* 시퀀스를 사용합니다. XML 기반 시나리오 구성 파일에서 단계를 구성합니다.

클라우드 인프라의 Adobe Commerce은 `ece-tools` 패키지에서 [기본 시나리오](https://github.com/magento/ece-tools/tree/2002.1/scenario) 및 [기본 단계](https://github.com/magento/ece-tools/tree/2002.1/src/Step) 집합을 제공합니다. 사용자 지정 XML 구성 파일을 만들어 기본 구성을 재정의하거나 사용자 지정하여 배포 동작을 사용자 지정할 수 있습니다. 시나리오와 단계를 사용하여 사용자 지정 모듈에서 코드를 실행할 수도 있습니다.

## 빌드 및 배포 후크를 사용하여 시나리오 추가

Adobe Commerce을 빌드하고 배포하는 시나리오를 `.magento.app.yaml` 파일의 `hooks` 섹션에 추가합니다. 각 후크는 각 단계 동안 실행할 시나리오를 지정합니다. 다음 예에서는 기본 시나리오 구성을 보여 줍니다.

> `magento.app.yaml`개 후크

```yaml
hooks:
    build: |
        set -e
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    deploy: |
        php ./vendor/bin/ece-tools run scenario/deploy.xml
    post_deploy: |
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
```

>[!NOTE]
>
>`ece-tools` 2002.1.x 릴리스에는 새로운 [후크 구성](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/app/properties/hooks-property.html) 형식이 있습니다. `ece-tools` 2002.0.x 릴리스의 레거시 형식은 계속 지원됩니다. 그러나 시나리오 기반 배포 기능을 사용하려면 새 형식으로 업데이트해야 합니다.

## 시나리오 단계 검토

후크 구성에서 각 시나리오는 빌드, 배포 또는 배포 후 작업을 실행하는 단계가 포함된 XML 파일입니다. 예를 들어 `scenario/transfer` 파일에는 `compress-static-content`, `clear-init-directory`, `backup-data`의 세 단계가 포함되어 있습니다

> `scenario/transfer.xml`

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
    <step name="compress-static-content" type="Magento\MagentoCloud\Step\Build\CompressStaticContent" priority="100"/>
    <step name="clear-init-directory" type="Magento\MagentoCloud\Step\Build\ClearInitDirectory" priority="200"/>
    <step name="backup-data" type="Magento\MagentoCloud\Step\Build\BackupData" priority="300">
        <arguments>
            <argument name="logger" xsi:type="object">Psr\Log\LoggerInterface</argument>
            <argument name="steps" xsi:type="array">
                <item name="static-content" xsi:type="object" priority="100">Magento\MagentoCloud\Step\Build\BackupData\StaticContent</item>
                <item name="writable-dirs" xsi:type="object"  priority="200">Magento\MagentoCloud\Step\Build\BackupData\WritableDirectories</item>
            </argument>
        </arguments>
    </step>
</scenario>
```

## 기본 시나리오 확장

다음 예제에서는 후크 구성에 추가 배포 구성 파일을 추가하여 기본 배포 시나리오를 확장합니다.

```yaml
hooks:
  deploy: |
    php ./vendor/bin/ece-tools run scenario/deploy.xml vendor/<vendor-name>/<module-name>/deploy.xml vendor/<vendor-name>/<module-name>/deploy2.xml
```

배포 중에 사용자 지정 시나리오는 다음 규칙에 따라 기본 시나리오와 병합됩니다.

- 시나리오는 후크 정의의 시퀀스에 따라 우선순위가 지정되며 마지막 시나리오가 가장 높은 우선순위를 가지고 나열됩니다.

  이 예제에서 시나리오에는 다음 우선순위가 있습니다.

   1. `vendor/vendor-name/module-name/deploy2.xml`
   1. `vendor/vendor-name/module-name/deploy.xml`
   1. `scenario/deploy.xml`(기본 또는 기준 시나리오)

- 우선 순위가 가장 높은 시나리오의 단계는 다른 시나리오에서 동일한 이름으로 단계를 재정의합니다. 새 단계가 구성에 추가됩니다. 동일한 규칙이 두 개 이상의 시나리오에 적용되며 각 시나리오는 오른쪽에서 왼쪽으로 우선 순위가 지정됩니다(예: (C → B → A).

### 기본 단계 제거

`skip` 매개 변수를 사용하여 기본 시나리오에서 단계를 제거합니다.

예를 들어 기본 배포 시나리오에서 `enable-maintenance-mode` 및 `set-production-mode` 단계를 건너뛰려면 다음 구성을 포함하는 구성 파일을 만듭니다.

> `vendor/vendor-name/module-name/deploy-custom-mode-config.xml`

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
    <step name="enable-maintenance-mode" skip="true" />
    <step name="set-production-mode"  skip="true" />
</scenario>
```

사용자 지정 구성 파일을 사용하려면 기본 `.magento.app.yaml` 파일을 업데이트하십시오.

> 사용자 지정 배포 시나리오가 있는 `.magento.app.yaml`

```yaml
hooks:
    build: |
        set -e
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    deploy: |
        php ./vendor/bin/ece-tools run scenario/deploy.xml vendor/vendor-name/module-name/deploy-custom-mode-config.xml
    post_deploy: |
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
```

### 기본 단계 바꾸기

사용자 지정 시나리오는 사용자 지정 구현을 제공하는 기본 단계를 대체할 수 있습니다. 이렇게 하려면 기본 단계 이름을 사용자 지정 단계의 이름으로 사용합니다.

예를 들어 [기본 배포 시나리오]에서 `enable-maintenance-mode` 단계는 기본 [EnableMaintenanceMode PHP 스크립트]를 실행합니다.

```xml
<step name="enable-maintenance-mode" type="Magento\MagentoCloud\Step\EnableMaintenanceMode" priority="300"/>
```

이 단계를 재정의하려면 `enable-maintenance-mode` 단계가 실행될 때 다른 스크립트를 실행할 사용자 지정 시나리오 구성 파일을 만드십시오.

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
<scenario>
    <step name="enable-maintenance-mode" type="VendorName\VendorModule\Step\CustomEnableMaintenanceMode" priority="300"/>
</scenario>
```

### 단계 우선 순위 변경

사용자 지정 시나리오는 기본 단계의 우선 순위를 변경할 수 있습니다. 다음 단계에서는 `enable-maintenance-mode` 단계의 우선 순위를 `300`에서 `10`(으)로 변경하여 배포 시나리오에서 이전 단계를 실행합니다.

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
<scenario>
    <step name="enable-maintenance-mode" type="Magento\MagentoCloud\Step\EnableMaintenanceMode" priority="10"/>
</scenario>
```

이 예제에서 `enable-maintenance-mode` 단계는 기본 배포 시나리오의 다른 모든 단계보다 우선 순위가 낮으므로 시나리오의 시작으로 이동합니다.

### 예: 배포 시나리오 확장

다음 예제에서는 다음과 같은 변경 내용으로 [기본 배포 시나리오]를 사용자 지정합니다.

- `remove-deploy-failed-flag` 단계를 사용자 지정 단계로 바꾸기
- 배포 전 단계에서 `clean-redis-cache` 하위 단계를 건너뜁니다.
- `unlock-cron-jobs` 단계 건너뛰기
- 중요한 유효성 검사기를 비활성화하려면 `validate-config` 단계를 건너뜁니다.
- 새 배포 전 단계 추가

> `vendor/vendor-name/module-name/deploy-extended.xml`

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
    <!-- Replace "remove-deploy-failed-flag" step with custom step -->
    <step name="remove-deploy-failed-flag" type="Vendor\ModuleName\Step\Deploy\RemoveDeployFailedFlag" priority="100"/>

    <!-- Skip "clean-redis-cache" sub-step in pre-deploy step -->
    <step name="pre-deploy" type="Magento\MagentoCloud\Step\Deploy\PreDeploy" priority="200">
        <arguments>
            <argument name="logger" xsi:type="object">Psr\Log\LoggerInterface</argument>
            <argument name="steps" xsi:type="array">
                <item name="clean-redis-cache" xsi:type="object" skip="true"/>
            </argument>
        </arguments>
    </step>

    <!-- Skip step "unlock-cron-jobs" -->
    <step name="unlock-cron-jobs" skip="true"/>

    <!-- Skip critical validators -->
    <step name="validate-config" type="Magento\MagentoCloud\Step\ValidateConfiguration" priority="300">
        <arguments>
            <argument name="logger" xsi:type="object">Psr\Log\LoggerInterface</argument>
            <argument name="validators" xsi:type="array">
                <item name="critical" xsi:type="array">
                    <item name="database-configuration" xsi:type="object" skip="true"/>
                    <item name="search-configuration" xsi:type="object" skip="true"/>
                </item>
            </argument>
        </arguments>
    </step>

    <!-- Add new step into the beginning of the deploy scenario -->
    <step name="new-pre-deploy-step" type="Vendor\ModuleName\Step\Deploy\PreDeploy" priority="10"/>
</scenario>
```

프로젝트에서 이 스크립트를 사용하려면 Adobe Commerce on cloud infrastructure 프로젝트의 `.magento.app.yaml` 파일에 다음 구성을 추가하십시오.

```yaml
hooks:
    build: |
        set -e
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    deploy: |
        php ./vendor/bin/ece-tools run scenario/deploy.xml vendor/vendor-name/module-name/deploy-extended.xml
    post_deploy: |
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
```

>[!TIP]
>
>`ece-tools` GitHub 리포지토리에서 [기본 시나리오](https://github.com/magento/ece-tools/tree/2002.1/scenario) 및 [기본 단계 구성](https://github.com/magento/ece-tools/tree/2002.1/src/Step)을 검토하여 프로젝트 빌드, 배포 및 배포 후 작업에 대해 사용자 지정할 시나리오 및 단계를 결정할 수 있습니다.

## `ece-tools`을(를) 확장할 사용자 지정 모듈 추가

`ece-tools` 패키지는 의미 체계 버전 표준을 따르는 기본 API 인터페이스를 제공합니다. 모든 API 인터페이스가 **@api** 주석으로 표시됩니다. 사용자 지정 모듈을 만들고 필요에 따라 기본 코드를 수정하여 기본 API 구현을 자신의 구현으로 바꿀 수 있습니다.

클라우드 인프라에서 Adobe Commerce과 함께 사용자 지정 모듈을 사용하려면 `ece-tools` 패키지의 확장 목록에 모듈을 등록해야 합니다. 등록 프로세스는 Adobe Commerce에서 모듈을 등록하는 데 사용하는 프로세스와 유사합니다.

**모듈을 `ece-tools` 패키지에 등록하려면**:

1. 모듈 루트에서 `registration.php` 파일을 만들거나 확장합니다.

   ```php?start_inline=1
   \Magento\MagentoCloud\ExtensionRegistrar::register('module-name', __DIR__);
   ```

1. 모듈 구성 파일의 `autoload` 섹션을 업데이트하여 `composer.json`에서 모듈 파일을 자동으로 로드하도록 `registration.php` 파일을 포함하십시오.

   ```json
   {
     "name": "vendor/ece-tools-extend",
     "description": "Extension for ece-tools",
     "type": "magento2-component",
     "version": "1.0.0",
     "license": "OSL-3.0",
     "autoload": {
       "files": [ "registration.php" ],
       "psr-4": {
         "Vendor\\EceToolExtend\\": "src/"
       }
     }
   }
   ```

1. `config/services.xml` 파일을 모듈에 추가합니다. 이 구성은 `ece-tools` 패키지에서 `config/services.xml`에 걸쳐 병합됩니다.

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <container xmlns="http://symfony.com/schema/dic/services"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xsi:schemaLocation="http://symfony.com/schema/dic/services https://symfony.com/schema/dic/services/services-1.0.xsd">
       <services>
           <defaults autowire="true" autoconfigure="true" public="true"/>
   
           <prototype namespace="Vendor\EceToolExtend\" resource="../src/*" exclude="../src/{Test}"/>
   
           <!-- Use your own implementation of EnvironmentDataInterface -->
           <service id="Magento\MagentoCloud\Config\EnvironmentDataInterface" alias="Vendor\EceToolExtend\Config\CustomEnvironmentData" />
       </services>
   </container>
   ```

종속성 삽입에 대한 자세한 내용은 [동종 종속성 삽입](https://symfony.com/doc/current/components/dependency_injection.html)을 참조하세요.

<!-- link definitions -->

[기본 배포 시나리오]: https://github.com/magento/ece-tools/blob/develop/scenario/deploy.xml
[EnableMaintenanceMode PHP 스크립트]: https://github.com/magento/ece-tools/blob/develop/src/Step/EnableMaintenanceMode.php
