---
title: Elasticsearch 서비스 설정
description: 클라우드 인프라에서 Adobe Commerce에 대한 Elasticsearch 서비스를 활성화하는 방법을 알아봅니다.
feature: Cloud, Search, Services
exl-id: ac559cbb-342a-4756-ade5-49eba4827965
source-git-commit: 8147b43b26370d9305c3c7dc47865ddcbae1904d
workflow-type: tm+mt
source-wordcount: '798'
ht-degree: 0%

---

# Elasticsearch 서비스 설정

[Elasticsearch](https://www.elastic.co) 는 모든 소스, 모든 형식에서 데이터를 가져와 실시간으로 검색하고 시각화할 수 있는 오픈 소스 제품입니다.

{{elasticsearch-support}}

Adobe Commerce 버전 2.4.4 이상의 경우 [OpenSearch 서비스 설정](opensearch.md).

- Elasticsearch은 제품 카탈로그의 제품에 대해 빠른 검색 및 고급 검색을 수행합니다
- Elasticsearch 분석기는 여러 언어를 지원합니다
- 정지어 및 동의어 지원
- 색인 재지정 작업이 완료될 때까지 색인화는 고객에게 영향을 주지 않습니다

>[!TIP]
>
>Adobe은 Adobe Commerce 애플리케이션에 대한 서드파티 Elasticsearch을 구성하려는 경우에도 항상 Adobe Commerce on cloud infrastructure 프로젝트에 대한 검색을 설정할 것을 권장합니다. Elasticsearch 설정은 서드파티 검색 도구가 실패할 경우에 대비하기 위한 대체 옵션을 제공합니다.

{{service-instruction}}

**Elasticsearch을 활성화하려면**:

1. 스타터 프로젝트의 경우 `elasticsearch` 에 대한 서비스 `.magento/services.yaml` Elasticsearch 버전 및 할당된 디스크 공간(MB)이 있는 파일입니다.

   ```yaml
   elasticsearch:
       type: elasticsearch:<version>
       disk: 1024
   ```

   Pro 프로젝트의 경우 Adobe Commerce 지원 티켓을 제출하여 스테이징 및 프로덕션 환경에서 Elasticsearch 버전을 변경해야 합니다.

1. 설정 `relationships` 의 속성 `.magento.app.yaml` 파일.

   ```yaml
   relationships:
       elasticsearch: "elasticsearch:elasticsearch"
   ```

1. 코드 변경 사항을 추가, 커밋 및 푸시합니다.

   ```bash
   git add .magento/services.yaml .magento.app.yaml && git commit -m "Enable Elasticsearch" && git push origin <branch-name>
   ```

   이러한 변경 사항이 환경에 미치는 영향에 대한 자세한 내용은 [서비스](services-yaml.md).

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

Adobe Commerce on cloud infrastructure 프로젝트를 설치하거나 업그레이드할 때 항상 Elasticsearch 서비스 버전과 [ELASTICSEARCH PHP](https://github.com/elastic/elasticsearch-php) Adobe Commerce용 클라이언트.

- **최초 설정**-에 지정된 Elasticsearch 버전이 있는지 확인합니다. `services.yaml` 파일은 Adobe Commerce용으로 구성된 Elasticsearch PHP 클라이언트와 호환됩니다.

- **프로젝트 업그레이드**-새 애플리케이션 버전의 Elasticsearch PHP 클라이언트가 클라우드 인프라에 설치된 Elasticsearch 서비스 버전과 호환되는지 확인합니다.

클라우드 인프라에서 Adobe Commerce에 대한 서비스 버전 및 호환성 지원은 클라우드 인프라에 배포된 버전에 따라 결정되며 Adobe Commerce 온프레미스 배포에서 지원하는 버전과 다른 경우가 있습니다. 다음을 참조하십시오 [서비스 버전](services-yaml.md#service-versions).

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

1. 다음에 대한 Composer 패키지 버전을 확인합니다. `elasticsearch/elasticsearch`.

   ```bash
   composer show elasticsearch/elasticsearch
   ```

   응답에서 의 설치된 버전을 확인합니다. `versions` 속성.

   ```terminal
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

   또한 Elasticsearch PHP 클라이언트 버전은  `composer.lock` 환경 루트 디렉토리에 있는 파일입니다.

1. 명령줄에서 Elasticsearch 서비스 연결 세부 정보를 검색합니다.

   ```bash
   vendor/bin/ece-tools env:config:show services
   ```

   응답에서 Elasticsearch 서비스 끝점의 IP 주소를 찾습니다.

   ```terminal
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

1. 설치된 Elasticsearch 서비스 검색 `version:number` 서비스 끝점에서 가져왔습니다.

   ```bash
   curl -XGET <elasticsearch-service-endpoint-ip-address>:9200/
   ```

   ```terminal
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

   - 에서 Elasticsearch 서비스 버전 변경 `services.yaml` Elasticsearch PHP 클라이언트와 호환되는 버전으로 파일을 복사합니다.

     {{pro-update-service}}

## Elasticsearch 서비스 다시 시작

을(를) 다시 시작해야 하는 경우 [Elasticsearch](https://www.elastic.co) 서비스에서는 Adobe Commerce 지원에 문의해야 합니다.

## 추가 검색 구성

- 기본적으로 클라우드 환경에 대한 검색 구성은 배포할 때마다 다시 생성됩니다. 다음을 사용할 수 있습니다. `SEARCH_CONFIGURATION` 변수를 배포하여 배포 간 사용자 지정 검색 설정을 유지합니다. 다음을 참조하십시오 [변수 배포](../environment/variables-deploy.md#search_configuration).

- 프로젝트에 대한 Elasticsearch 서비스를 설정한 후 관리 UI를 사용하여 Elasticsearch 연결을 테스트하고 Adobe Commerce에 대한 Elasticsearch 설정을 사용자 지정합니다.

### Elasticsearch을 위한 플러그인 추가

필요한 경우 를 추가하여 Elasticsearch에 대한 플러그인을 추가할 수 있습니다. `configuration:plugins` Elasticsearch 섹션 을 참조하십시오. `.magento/services.yaml` 파일. 예를 들어 다음 코드는 ICU 분석 및 음성 분석 플러그인을 활성화합니다.

```yaml
elasticsearch:
    type: elasticsearch:<service-version>
    disk: 1024
    configuration:
        plugins:
            - analysis-icu
            - analysis-phonetic
```

Elastic Suite 타사 플러그인을 사용하는 경우 다음을 수행해야 합니다. [업데이트 `ece-tools` 패키지](../dev-tools/update-package.md) 버전 2002.0.19 이상
Elastic Suite를 설정할 때 구성 설정을 `ELASTICSUITE_CONFIGURATION` 변수를 배포합니다. 이 구성은 배포 전반에 걸쳐 설정을 저장합니다.

### Elasticsearch에 대한 플러그인 제거

에서 플러그인 항목 제거 `elasticsearch:` 위치: `.magento/services.yaml` 은 예상대로 제거하거나 비활성화하지 않습니다. Elasticsearch 데이터를 다시 색인화해야 합니다. 이 동작은 이러한 플러그인에 의존하는 데이터의 가능한 손실이나 손상을 방지하기 위한 것입니다.

**Elasticsearch 플러그인을 제거하려면**:

1. 에서 Elasticsearch 플러그인 항목을 제거합니다. `.magento/services.yaml` 파일.
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

1. 커밋: `.magento/services.yaml` 클라우드 저장소 변경.
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
>Adobe Commerce에서 Elastic Suite 플러그인 사용 또는 문제 해결에 대한 자세한 내용은 다음을 참조하십시오. [Elastic Suite 설명서](https://github.com/Smile-SA/elasticsuite).

## 문제 해결

Elasticsearch 문제 해결에 대한 도움말은 다음 Adobe Commerce 지원 문서를 참조하십시오.

- [Elasticsearch 5가 구성되었지만 검색 페이지가 &quot;Fielddata가 비활성화되었습니다...&quot; 오류로 로드되지 않음](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/elasticsearch/elasticsearch-5-is-configured-but-search-page-does-not-load-with-fielddata-is-disabled...-error.html)
- [Elasticsearch 6.x를 사용하는 경우 카탈로그 페이지 매김이 작동하지 않습니다](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/catalog-pagination-doesn-t-work-when-elasticsearch-6.x-is-used.html)
- [Adobe Commerce 문제 해결사의 Elasticsearch](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/elasticsearch/elasticsearch-in-magento-troubleshooter.html)
- [Elasticsearch 색인 상태: `yellow` 또는 `red`](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/elasticsearch/elasticsearch-index-status-is-yellow-or-red.html)
