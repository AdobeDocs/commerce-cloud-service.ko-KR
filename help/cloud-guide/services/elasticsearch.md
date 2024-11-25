---
title: Elasticsearch 서비스 설정
description: 클라우드 인프라에서 Adobe Commerce에 대한 Elasticsearch 서비스를 활성화하는 방법을 알아봅니다.
feature: Cloud, Search, Services
exl-id: ac559cbb-342a-4756-ade5-49eba4827965
source-git-commit: 38c29e3a2cee1658bb73922f0f56fdfa84af5a6f
workflow-type: tm+mt
source-wordcount: '788'
ht-degree: 0%

---

# Elasticsearch 서비스 설정

[Elasticsearch](https://www.elastic.co)은(는) 모든 원본, 모든 형식에서 데이터를 가져와 실시간으로 검색하고 시각화할 수 있는 오픈 소스 제품입니다.

{{elasticsearch-support}}

Adobe Commerce 버전 2.4.4 이상은 [OpenSearch 서비스 설정](opensearch.md)을 참조하십시오.

- Elasticsearch은 제품 카탈로그의 제품에 대해 빠른 검색 및 고급 검색을 수행합니다
- Elasticsearch 분석기는 여러 언어를 지원합니다
- 정지어 및 동의어 지원
- 색인 재지정 작업이 완료될 때까지 색인화는 고객에게 영향을 주지 않습니다

>[!TIP]
>
>Adobe은 Adobe Commerce 애플리케이션에 대한 서드파티 Elasticsearch을 구성하려는 경우에도 항상 Adobe Commerce on cloud infrastructure 프로젝트에 대한 검색을 설정할 것을 권장합니다. Elasticsearch 설정은 서드파티 검색 도구가 실패할 경우에 대비하기 위한 대체 옵션을 제공합니다.

{{service-instruction}}

**Elasticsearch을 사용하려면**:

1. 시작 프로젝트의 경우 Elasticsearch 버전과 할당된 디스크 공간(MB)을 사용하여 `elasticsearch` 서비스를 `.magento/services.yaml` 파일에 추가하십시오.

   ```yaml
   elasticsearch:
       type: elasticsearch:<version>
       disk: 1024
   ```

   Pro 프로젝트의 경우 Adobe Commerce 지원 티켓을 제출하여 스테이징 및 프로덕션 환경에서 Elasticsearch 버전을 변경해야 합니다.

1. `.magento.app.yaml` 파일에서 `relationships` 속성을 설정합니다.

   ```yaml
   relationships:
       elasticsearch: "elasticsearch:elasticsearch"
   ```

1. 코드 변경 사항을 추가, 커밋 및 푸시합니다.

   ```bash
   git add .magento/services.yaml .magento.app.yaml && git commit -m "Enable Elasticsearch" && git push origin <branch-name>
   ```

   이러한 변경 내용이 환경에 미치는 영향에 대한 자세한 내용은 [서비스](services-yaml.md)를 참조하세요.

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

## Elasticsearch 소프트웨어 호환성

클라우드 인프라 프로젝트에서 Adobe Commerce을 설치하거나 업그레이드할 때 항상 Elasticsearch 서비스 버전과 Adobe Commerce용 [Elasticsearch PHP](https://github.com/elastic/elasticsearch-php) 클라이언트 간의 호환성을 확인하십시오.

- **처음 설치**-`services.yaml` 파일에 지정된 Elasticsearch 버전이 Adobe Commerce에 대해 구성된 Elasticsearch PHP 클라이언트와 호환되는지 확인하십시오.

- **프로젝트 업그레이드**-새 응용 프로그램 버전의 Elasticsearch PHP 클라이언트가 클라우드 인프라에 설치된 Elasticsearch 서비스 버전과 호환되는지 확인하십시오.

클라우드 인프라에서 Adobe Commerce에 대한 서비스 버전 및 호환성 지원은 클라우드 인프라에 배포된 버전에 따라 결정되며 Adobe Commerce 온프레미스 배포에서 지원하는 버전과 다른 경우가 있습니다. [서비스 버전](services-yaml.md#service-versions)을 참조하세요.

**Elasticsearch 소프트웨어 호환성을 확인하려면**:

1. 로컬 워크스테이션에서 프로젝트 디렉터리로 변경합니다.

1. 활성 환경에 대한 Elasticsearch 세부 정보를 표시합니다.

   ```bash
   magento-cloud relationships --property=elasticsearch
   ```

1. 또는 SSH를 사용하여 원격 환경에 로그인할 수 있습니다.

   ```bash
   magento-cloud ssh
   ```

1. `elasticsearch/elasticsearch`의 작성기 패키지 버전을 확인하세요.

   ```bash
   composer show elasticsearch/elasticsearch
   ```

   응답에서 `versions` 속성에 설치된 버전을 확인합니다.

   ```
   name     : elasticsearch/elasticsearch
   descrip. : PHP Client for Elasticsearch
   keywords : client, elasticsearch, search
   versions : * v7.17.1
   type     : library
   license  : Apache License 2.0 (Apache-2.0) (OSI approved) https://spdx.org/licenses/Apache-2.0.html#licenseText
   license  : GNU Lesser General Public License v2.1 only (LGPL-2.1-only) (OSI approved) https://spdx.org/licenses/LGPL-2.1-only.html#licenseText
   homepage :
   source   : [git] git@github.com:elastic/elasticsearch-php.git f1b8918f411b837ce5f6325e829a73518fd50367
   dist     : [zip] https://api.github.com/repos/elastic/elasticsearch-php/zipball/f1b8918f411b837ce5f6325e829a73518fd50367 f1b8918f411b837ce5f6325e829a73518fd50367
   path     : ~/vendor/elasticsearch/elasticsearch
   names    : elasticsearch/elasticsearch
   ```

   또한 환경 루트 디렉터리의 `composer.lock` 파일에서 Elasticsearch PHP 클라이언트 버전을 찾을 수 있습니다.

1. 명령줄에서 Elasticsearch 서비스 연결 세부 정보를 검색합니다.

   ```bash
   vendor/bin/ece-tools env:config:show services
   ```

   응답에서 Elasticsearch 서비스 끝점의 IP 주소를 찾습니다.

   ```
   | elasticsearch:                                                                                                  |
   +------------------------------------------+----------------------------------------------------------------------+
   | username                                 | null                                                                 |
   | scheme                                   | http                                                                 |
   | service                                  | elasticsearch                                                        |
   | fragment                                 | null                                                                 |
   | ip                                       | 169.254.220.11                                                       |
   | hostname                                 | dzggu33f75wi3sd24lgwtoupxm.elasticsearch.service._.magentosite.cloud |
   | public                                   | false                                                                |
   | cluster                                  | fo3qdoxtla4j4-master-7rqtwti                                         |
   | host                                     | elasticsearch.internal                                               |
   | rel                                      | elasticsearch                                                        |
   | query                                    |                                                                      |
   | path                                     | null                                                                 |
   | password                                 | null                                                                 |
   | type                                     | elasticsearch:6.5                                                    |
   | port                                     | 9200                                                                 |
   +------------------------------------------+----------------------------------------------------------------------+
   ```

1. 서비스 끝점에서 설치된 Elasticsearch 서비스 `version:number`을(를) 검색합니다.

   ```bash
   curl -XGET <elasticsearch-service-endpoint-ip-address>:9200/
   ```

   ```json
   {
      "name" : "-AqGi9D",
      "cluster_name" : "elasticsearch",
      "cluster_uuid" : "_yze6-ywSEW1MaAF8ZPWyQ",
      "version" : {
        "number" : "6.5.4",
        "build_flavor" : "default",
        "build_type" : "deb",
        "build_hash" : "82a8aa7",
        "build_date" : "2019-01-23T12:07:18.760675Z",
        "build_snapshot" : false,
        "lucene_version" : "7.5.0",
        "minimum_wire_compatibility_version" : "5.6.0",
        "minimum_index_compatibility_version" : "5.0.0"
   },
   "  tagline" : "You Know, for Search"
   }
   ```

1. Elasticsearch 서비스와 PHP 클라이언트 간의 버전 호환성을 확인합니다.

   버전이 호환되지 않는 경우 환경 구성을 다음 중 하나로 업데이트합니다.

   - Elasticsearch PHP 클라이언트를 Elasticsearch 서비스 버전과 호환되는 버전으로 변경합니다.

     ```bash
     composer require "elasticsearch/elasticsearch:~<version>"
     ```

   - `services.yaml` 파일의 Elasticsearch 서비스 버전을 Elasticsearch PHP 클라이언트와 호환되는 버전으로 변경하십시오.

     {{pro-update-service}}

## Elasticsearch 서비스 다시 시작

[Elasticsearch](https://www.elastic.co) 서비스를 다시 시작해야 하는 경우 Adobe Commerce 지원 센터에 문의해야 합니다.

## 추가 검색 구성

- 기본적으로 클라우드 환경에 대한 검색 구성은 배포할 때마다 다시 생성됩니다. `SEARCH_CONFIGURATION` 배포 변수를 사용하여 배포 간에 사용자 지정 검색 설정을 유지할 수 있습니다. [변수 배포](../environment/variables-deploy.md#search_configuration)를 참조하세요.

- 프로젝트에 대한 Elasticsearch 서비스를 설정한 후 관리 UI를 사용하여 Elasticsearch 연결을 테스트하고 Adobe Commerce에 대한 Elasticsearch 설정을 사용자 지정합니다.

### Elasticsearch을 위한 플러그인 추가

필요한 경우 `.magento/services.yaml` 파일의 Elasticsearch 서비스에 `configuration:plugins` 섹션을 추가하여 Elasticsearch에 대한 플러그인을 추가할 수 있습니다. 예를 들어 다음 코드는 ICU 분석 및 음성 분석 플러그인을 활성화합니다.

```yaml
elasticsearch:
    type: elasticsearch:<service-version>
    disk: 1024
    configuration:
        plugins:
            - analysis-icu
            - analysis-phonetic
```

Elastic Suite 타사 플러그인을 사용하는 경우 [패키지 `ece-tools`을(를) 버전 2002.0.19 이상으로 업데이트](../dev-tools/update-package.md)해야 합니다.
Elastic Suite를 설정할 때 `ELASTICSUITE_CONFIGURATION` 배포 변수에 구성 설정을 추가합니다. 이 구성은 배포 전반에 걸쳐 설정을 저장합니다.

### Elasticsearch에 대한 플러그인 제거

`.magento/services.yaml`의 `elasticsearch:`에서 플러그 인 항목을 제거해도 예상대로 제거되거나 비활성화되지 않습니다. Elasticsearch 데이터를 다시 색인화해야 합니다. 이 동작은 이러한 플러그인에 의존하는 데이터의 가능한 손실이나 손상을 방지하기 위한 것입니다.

**Elasticsearch 플러그인을 제거하려면**:

1. `.magento/services.yaml` 파일에서 Elasticsearch 플러그 인 항목을 제거하십시오.
1. 코드 변경 사항을 추가, 커밋 및 푸시합니다.

   ```bash
   git add .magento/services.yaml
   ```

   ```bash
   git commit -m "Remove Elasticsearch plugin"
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

>[!TIP]
>
>Adobe Commerce에서 Elastic Suite 플러그인을 사용하거나 문제를 해결하는 방법에 대한 자세한 내용은 [Elastic Suite 설명서](https://github.com/Smile-SA/elasticsuite)를 참조하십시오.

## 문제 해결

Elasticsearch 문제 해결에 대한 도움말은 다음 Adobe Commerce 지원 문서를 참조하십시오.

- [Elasticsearch 5가 구성되어 있지만 검색 페이지가 &quot;필드 데이터를 사용할 수 없습니다...&quot; 오류](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/elasticsearch/elasticsearch-5-is-configured-but-search-page-does-not-load-with-fielddata-is-disabled...-error.html)
- Adobe Commerce 문제 해결사의 [Elasticsearch](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/elasticsearch/elasticsearch-in-magento-troubleshooter.html)
- [Elasticsearch 인덱스 상태가 `yellow` 또는 `red`](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/elasticsearch/elasticsearch-index-status-is-yellow-or-red.html)입니다.
