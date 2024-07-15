---
title: OpenSearch 서비스 설정
description: 클라우드 인프라에서 Adobe Commerce용 OpenSearch 서비스를 활성화하는 방법을 알아봅니다.
feature: Cloud, Search, Services
exl-id: 10dc6367-3f90-4ab6-a84e-15e8c3b32a38
source-git-commit: d4c36b084094846cfad69adc2bffd567a58fab26
workflow-type: tm+mt
source-wordcount: '651'
ht-degree: 0%

---

# OpenSearch 서비스 설정

[OpenSearch](https://www.opensearch.org) 서비스는 Elasticsearch에 대한 라이선스 변경 후 Elasticsearch 7.10.2의 오픈 소스 포크입니다. GitHub에서 [OpenSource 프로젝트](https://github.com/opensearch-project)를 참조하십시오.

{{elasticsearch-support}}

OpenSearch를 사용하면 모든 소스, 모든 형식에서 데이터를 가져와 실시간으로 검색하고 시각화할 수 있습니다.

- 제품 카탈로그의 제품에 대한 빠른 검색 및 고급 검색
- OpenSearch Analyzer는 여러 언어를 지원합니다
- 정지어 및 동의어 지원
- 색인 재지정 작업이 완료될 때까지 색인화는 고객에게 영향을 주지 않습니다

{{service-instruction}}

>[!TIP]
>
>Adobe은 Adobe Commerce 애플리케이션에 대한 서드파티 검색 도구를 구성하려는 경우에도 항상 Adobe Commerce on cloud infrastructure 프로젝트에 대해 OpenSearch를 설정할 것을 권장합니다. OpenSearch 설정은 타사 검색 도구가 실패할 경우 대체 옵션을 제공합니다.

**OpenSearch를 사용하려면**:

1. Starter 및 Pro 통합 환경의 경우 적절한 버전과 할당된 디스크 공간(MB)을 사용하여 `opensearch` 서비스를 `.magento/services.yaml` 파일에 추가하십시오. 이 경우 버전 2가 적절합니다. 클라우드 인프라에서 최신 버전의 OpenSearch를 사용하기 때문에 부 버전은 필요하지 않습니다.

   ```yaml
   opensearch:
       type: opensearch:2
       disk: 1024
   ```

   Pro 프로젝트의 경우 스테이징 및 프로덕션 환경에서 OpenSearch 버전을 변경하려면 [Adobe Commerce 지원 티켓을 제출](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)해야 합니다.

1. `.magento.app.yaml` 파일에서 `relationships` 속성을 설정하거나 확인하십시오.

   ```yaml
   relationships:
       opensearch: "opensearch:opensearch"
   ```

1. 코드 변경 사항을 추가, 커밋 및 푸시합니다.

   ```bash
   git add .magento/services.yaml .magento.app.yaml
   ```

   ```bash
   git commit -m "Enable OpenSearch"
   ```

   ```bash
   git push origin <branch-name>
   ```

   이러한 변경 내용이 환경에 미치는 영향에 대한 자세한 내용은 [서비스 구성](services-yaml.md)을 참조하십시오.

1. 배포 프로세스가 완료되면 SSH를 사용하여 원격 환경에 로그인합니다.

   ```bash
   magento-cloud ssh
   ```

1. 카탈로그 검색 색인을 다시 색인화합니다.

   ```bash
   bin/magento indexer:reindex catalogsearch_fulltext
   ```

1. 캐시를 정리합니다.

   ```bash
   bin/magento cache:clean
   ```

{{service-change-tip}}

## OpenSearch 소프트웨어 호환성

클라우드 인프라 프로젝트에서 Adobe Commerce을 설치하거나 업그레이드할 때 항상 OpenSearch 서비스 버전과 Adobe Commerce용 [OpenSearch PHP](https://github.com/opensearch-project/opensearch-php) 클라이언트 간의 호환성을 확인하십시오.

- **처음 설치**-`services.yaml` 파일에 지정된 OpenSearch 버전이 Adobe Commerce용으로 구성된 OpenSearch PHP 클라이언트와 호환되는지 확인합니다.

- **프로젝트 업그레이드**-새 응용 프로그램 버전의 OpenSearch PHP 클라이언트가 클라우드 인프라에 설치된 OpenSearch 서비스 버전과 호환되는지 확인하십시오.

서비스 버전 및 호환성 지원은 Cloud 인프라에서 테스트하고 배포한 버전에 따라 결정되며 Adobe Commerce 온프레미스 배포에서 지원하는 버전과 다른 경우가 있습니다. 지원되는 버전 목록은 _설치 가이드_&#x200B;의 [시스템 요구 사항](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html)을 참조하십시오.

**OpenSearch 소프트웨어 호환성을 확인하려면**:

1. 로컬 워크스테이션에서 프로젝트 디렉터리로 변경합니다.

1. 활성 환경에 대한 OpenSearch 세부 정보를 표시합니다.

   ```bash
   magento-cloud relationships --property=opensearch
   ```

1. 또는 SSH를 사용하여 원격 환경에 로그인할 수 있습니다.

   ```bash
   magento-cloud ssh
   ```

1. OpenSearch 서비스 연결 세부 정보를 검색합니다.

   ```bash
   vendor/bin/ece-tools env:config:show services
   ```

   응답에서 OpenSearch 서비스 끝점의 IP 주소와 포트를 찾습니다.

   ```terminal
   +------------------------------------------+--------------------------------------------------------+
   | opensearch:                                                                                       |
   +------------------------------------------+--------------------------------------------------------+
   | username                                 | null                                                   |
   | scheme                                   | http                                                   |
   | service                                  | opensearch                                             |
   | fragment                                 | null                                                   |
   | ip                                       | 169.254.220.11                                         |
   | hostname                                 | hostf75wi3sd24l.opensearch.service._.magentosite.cloud |
   | port                                     | 9200                                                   |
   | cluster                                  | projectID-develop-4ranwui                              |
   | host                                     | opensearch.internal                                    |
   | rel                                      | opensearch                                             |
   | path                                     | null                                                   |
   | query                                    |                                                        |
   | password                                 | null                                                   |
   | type                                     | opensearch:2                                           |
   | public                                   | false                                                  |
   | host_mapped                              | false                                                  |
   ```

1. 서비스 끝점에서 설치된 OpenSearch 서비스 `version:number`을(를) 검색합니다.

   ```bash
   curl -XGET <opensearch-service-endpoint-ip-address>:9200
   ```

   ```terminal
   {
      "name" : "opensearch.0",
      "cluster_name" : "opensearch",
      "cluster_uuid" : "_yzaae6-ywSEW1MaAF8ZPWyQ",
      "version" : {
        "distribution" : "opensearch",
        "number" : "2.5.0",
        "build_type" : "deb",
        "build_hash" : "aaaaaaa",
        "build_date" : "2023-01-23T12:07:18.760675Z",
        "build_snapshot" : false,
        "lucene_version" : "9.4.2",
        "minimum_wire_compatibility_version" : "7.10.0",
        "minimum_index_compatibility_version" : "7.0.0"
   },
   "tagline" : "The OpenSearch Project: https://opensearch.org/"
   }
   ```

{{pro-update-service}}

## OpenSearch 서비스 다시 시작

OpenSearch 서비스를 다시 시작해야 하는 경우에는 Adobe Commerce 지원 센터에 문의해야 합니다.

## 추가 검색 구성

- 기본적으로 클라우드 환경에 대한 검색 구성은 배포할 때마다 다시 생성됩니다. `SEARCH_CONFIGURATION` 배포 변수를 사용하여 배포 간에 사용자 지정 검색 설정을 유지할 수 있습니다. [변수 배포](../environment/variables-deploy.md#search_configuration)를 참조하세요.

- 프로젝트에 대한 OpenSearch 서비스를 설정한 후 관리 UI를 사용하여 OpenSearch 연결을 테스트하고 Adobe Commerce에 대한 OpenSearch 설정을 사용자 지정합니다.

### OpenSearch에 대한 플러그인 추가

필요한 경우 `.magento/services.yaml` 파일의 OpenSearch 서비스에 `configuration:plugins` 섹션을 추가하여 OpenSearch에 대한 플러그인을 추가할 수 있습니다. 예를 들어 다음 코드는 ICU 분석 및 음성 분석 플러그인을 활성화합니다.

```yaml
opensearch:
    type: opensearch:2
    disk: 1024
    configuration:
        plugins:
            - analysis-icu
            - analysis-phonetic
```

플러그인에 대한 자세한 내용은 [OpenSearch 프로젝트](https://github.com/opensearch-project)를 참조하십시오.

### OpenSearch에 대한 플러그인 제거

`.magento/services.yaml` 파일의 `opensearch:` 섹션에서 플러그 인 항목을 제거하면 서비스를 제거하거나 사용하지 않도록 설정할 수 **없습니다**. 서비스를 완전히 사용하지 않도록 설정하려면 `.magento/services.yaml` 파일에서 플러그인을 제거한 후 OpenSearch 데이터를 다시 인덱싱해야 합니다. 이 설계는 이러한 플러그인에 의존하는 데이터의 가능한 손실 또는 손상을 방지합니다.

**OpenSearch 플러그인을 제거하려면**:

1. `.magento/services.yaml` 파일에서 OpenSearch 플러그인 항목을 제거합니다.
1. 코드 변경 사항을 추가, 커밋 및 푸시합니다.

   ```bash
   git add .magento/services.yaml
   ```

   ```bash
   git commit -m "Remove OpenSearch plugin"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. 클라우드 저장소에 `.magento/services.yaml` 변경 내용을 커밋합니다.
1. 카탈로그 검색 색인을 다시 색인화합니다.

   ```bash
   bin/magento indexer:reindex catalogsearch_fulltext
   ```

1. 캐시를 정리합니다.

   ```bash
   bin/magento cache:clean
   ```
