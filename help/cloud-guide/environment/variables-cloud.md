---
title: 클라우드별 변수
description: 클라우드 인프라의 Adobe Commerce과 관련된 환경 변수 목록을 참조하십시오.
feature: Cloud, Configuration
recommendations: noDisplay, catalog
role: Developer
exl-id: 84b7c0fc-f0b0-4ff5-9f33-9d17180a9306
source-git-commit: b49a51aba56f79b5253eeacb1adf473f42bb8959
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 0%

---

# 클라우드별 변수

클라우드 인프라의 Adobe Commerce에 고유한 환경 변수는 `MAGENTO_CLOUD_*` 접두사를 사용합니다.

| 변수 | 설명 |
| -------- | --------------- |
| `MAGENTO_CLOUD_APP_DIR` | 응용 프로그램 디렉터리의 절대 경로입니다. |
| `MAGENTO_CLOUD_APPLICATION` | 애플리케이션을 설명하는 base64 인코딩 JSON 개체입니다. `.magento.app.yaml` 파일 콘텐츠에 매핑되고 하위 키가 있습니다. |
| `MAGENTO_CLOUD_APPLICATION_NAME` | `.magento.app.yaml` 파일에 구성된 응용 프로그램의 이름입니다. |
| `MAGENTO_CLOUD_DOCUMENT_ROOT` | 웹 문서 루트에 대한 절대 경로(해당하는 경우). |
| `MAGENTO_CLOUD_ENVIRONMENT` | 환경 분기의 이름입니다. |
| `MAGENTO_CLOUD_PROJECT` | 프로젝트 ID입니다. |
| `MAGENTO_CLOUD_RELATIONSHIPS` | 키(관계 이름) 및 값(관계 쌍 배열) 끝점 정의를 나타내는 base64로 인코딩된 JSON 개체입니다. 각 관계 끝점 정의는 분해된 URL 형식입니다. 여기에는 `scheme`, `host`, `port` 및 _선택적으로_, `username`, `password`, `path` 및 `query`에 일부 추가 정보가 있습니다. |
| `MAGENTO_CLOUD_ROUTES` | 환경 `.magento/routes.yaml` 파일에 정의된 경로를 설명합니다. |
| `MAGENTO_CLOUD_TREE_ID` | Git에 있는 트리의 SHA에 해당하는 애플리케이션용 트리 ID입니다. |
| `MAGENTO_CLOUD_VARIABLES` | 키-값 쌍이 있는 base64 인코딩 JSON 개체(예: `"key":"value"`). |
| `MAGENTO_CLOUD_LOCKS_DIR` | 클라우드 인프라의 잠금 제공자에 대한 마운트 지점 경로를 제공합니다. 잠금 공급자는 중복 크론 작업 및 크론 그룹의 시작을 방지합니다. |

>[!WARNING]
>
>[[!DNL Cloud Console]](../project/overview.md)을(를) 사용하여 [구성 설정 재정의](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/paths/override-config-settings.html)에 환경 변수를 추가하려면 다음 예제와 같이 변수 이름 앞에 `env:`을(를) 추가해야 합니다.
>
>![환경 변수 예](../../assets/set-env-variable-ui.png)

값은 시간이 지남에 따라 변경될 수 있으므로 런타임 시 변수를 검사하고 이 변수를 사용하여 애플리케이션을 구성하는 것이 가장 좋습니다. 예를 들어 `MAGENTO_CLOUD_RELATIONSHIPS` 변수를 사용하여 다음과 같이 환경 관련 관계를 검색합니다.

```php
<?php
/**
  * Get relationships information from cloud environment variable.
  *
  * @return mixed
  */
    protected function getRelationships()
    {
        return json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"]), true);
    }
```

## 환경 변수 보기

[`ece-tools` 패키지](../dev-tools/package-overview.md)의 `env:config:show` 명령을 사용하여 현재 환경에 대한 변수 목록을 표시할 수 있습니다.

```bash
php ./vendor/bin/ece-tools env:config:show variables
```

`variables` 옵션에 대한 샘플 출력:

```
Magento Cloud Environment Variables:
+-----------------------------------+----------------------------------+
| Variable name                     | Value                            |
+-----------------------------------+----------------------------------+
| ADMIN_EMAIL                       | commerceadmin@company.com        |
| ADMIN_PASSWORD                    | 123123q                          |
+-----------------------------------+----------------------------------+
```
