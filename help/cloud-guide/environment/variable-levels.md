---
title: 변수 수준 및 옵션
description: 클라우드 인프라 프로젝트 런타임 환경에서 Adobe Commerce을 사용자 지정하는 데 사용되는 다양한 변수 수준 및 옵션에 대해 알아봅니다.
feature: Cloud, Configuration, Security
exl-id: 11aa0862-94c0-47fb-946a-0148f75cc24c
source-git-commit: 13e76d3e9829155995acbb72d947be3041579298
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 0%

---

# 변수 수준

프로젝트 변수는 프로젝트 내의 모든 환경에 적용됩니다. 환경 변수는 특정 환경 또는 분기에 적용됩니다. 환경 _상속_ 상위 환경의 변수 정의입니다.

환경에 특별히 변수를 정의하여 상속된 값을 재정의할 수 있습니다. 예를 들어 개발에 사용할 변수를 설정하려면 `.magento.env.yaml` 통합 환경에 있는 파일입니다. 통합 환경에서 분기하는 모든 환경은 해당 값을 상속합니다. 다음을 참조하십시오 [배포 구성](configure-env-yaml.md) 를 사용하여 환경을 구성하는 방법에 대한 자세한 내용 `.magento.env.yaml` 파일.

>[!BEGINTABS]

>[!TAB CLI]

**Cloud CLI를 사용하여 변수를 설정하려면**:

- **프로젝트별 변수**- 동일한 값을 설정합니다. _모두_ 프로젝트의 환경. 이러한 변수는 모든 환경의 빌드 및 런타임에 사용할 수 있습니다.

  ```bash
  magento-cloud variable:create --level project --name <variable-name> --value <variable-value>
  ```

- **환경별 변수**- 의 고유한 값을 설정합니다. _특정_ 환경. 이러한 변수는 런타임 시 사용할 수 있으며 하위 환경에 상속됩니다. 를 사용하여 명령에 환경을 지정합니다 `-e` 옵션을 선택합니다.

  ```bash
  magento-cloud variable:create --level environment --name <variable-name> --value <variable-value>
  ```

프로젝트별 변수를 설정한 후에는 변경 사항을 적용하려면 원격 환경을 수동으로 다시 배포해야 합니다. 재배포를 트리거하기 위해 새 커밋을 푸시합니다.

>[!TAB 콘솔]

**를 사용하여 변수를 설정하려면[!DNL Cloud Console]**:

1. 다음에서 _[!DNL Cloud Console]_프로젝트 탐색 오른쪽의 구성 아이콘을 클릭합니다.

   ![프로젝트 구성](../../assets/icon-configure.png){width="36"}

1. 프로젝트 수준 변수를 설정하려면 _프로젝트 설정_ 클릭 **변수**.

   ![프로젝트 변수](../../assets/ui-project-variables.png)

1. 환경 수준 변수를 설정하려면 _환경_ 목록을 만들고 환경을 선택한 다음 **[!UICONTROL Variables]** 탭.

   ![환경 변수 탭](../../assets/ui-environment-variables.png)

1. 클릭 **[!UICONTROL Create variable]**.

1. 변수의 이름과 값을 입력합니다. 다음 옵션 중에서 선택합니다.

   - 런타임 중에 사용 가능
   - 작성 시간 동안 사용 가능
   - JSON 값
   - 중요한 변수(콘솔 및 CLI 응답에서 숨겨진 값)
   - 상속 가능(하위 환경이 환경 수준 변수를 상속할 수 있음)

1. 클릭 **[!UICONTROL Create variable]**.

>[!CAUTION]
>
>에서 환경별 변수 설정 [!DNL Cloud Console] 환경을 자동으로 다시 배포합니다.

>[!ENDTABS]

## 가시성

를 사용하여 빌드 또는 런타임 중에 변수의 가시성을 제한할 수 있습니다. `--visible-<build|runtime>` 명령입니다. 상속과 민감도를 설정하는 옵션도 있습니다.

변수가 보이거나 상속되지 않도록 하려면 다음 옵션을 사용하십시오.

- `--inheritable false`- 하위 환경의 상속을 비활성화합니다. 이 기능은 `master` 분기 및 다른 모든 환경에서 동일한 이름의 프로젝트 수준 변수를 사용할 수 있도록 허용
- `--sensitive true`- 변수를 로 표시합니다. _읽을 수 없음_ 다음에서 [!DNL Cloud Console]. 사용자 인터페이스에서는 변수를 볼 수 없지만 다른 변수와 마찬가지로 응용 프로그램 컨테이너에서는 변수를 볼 수 있습니다.

다음은 변수가 보이거나 상속되지 않도록 하는 특정 사례를 보여 줍니다. CLI에서는 이러한 옵션만 지정할 수 있습니다. 이 경우 사용 가능한 모든 환경 변수와 관련이 없습니다.

```bash
magento-cloud variable:create --name <variable-name> --value <variable-value> --inheritable false --sensitive true
```

## 변수 수준 및 값 확인

Cloud CLI를 사용하여 기존 변수 목록을 볼 수 있습니다.

```bash
magento-cloud variables
```

```terminal
Variables on the project Project-Name (<project-id>), environment <environment-name>:
+----------------------------+-------------+-------------------------------------------+
| Name                       | Level       | Value                                     |
+----------------------------+-------------+-------------------------------------------+
| env:COMPOSER_AUTH          | project     | {                                         |
|                            |             |    "http-basic": {                        |
|                            |             |       "repo.magento.com": {               |
|                            |             |       "username":                         |
|                            |             | "<public-key>",                           |
|                            |             |       "password":                         |
|                            |             | "<private-key>"                           |
|                            |             |     }                                     |
|                            |             |   }                                       |
|                            |             | }                                         |
| ADMIN_EMAIL                | project     | admin@123.com                             |
| ADMIN_EMAIL                | environment | admin@123.com                             |
| ADMIN_PASSWORD             | environment | password                                  |
| ADMIN_URL                  | environment | admin123                                  |
| ADMIN_USERNAME             | environment | admin                                     |
| php:newrelic.license       | environment | xxxx71fb030366182117f955a22e4baf8exxxxxx  |
+----------------------------+-------------+-------------------------------------------+
```
