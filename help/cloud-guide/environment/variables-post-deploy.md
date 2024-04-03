---
title: 사후 배포 변수
description: Adobe Commerce on cloud infrastructure 배포 후 단계에서 작업을 제어하는 환경 변수 목록을 참조하십시오.
feature: Cloud, Configuration, Cache
recommendations: noDisplay, catalog
role: Developer
exl-id: e460335f-cd2b-4c98-b1ff-32504599b33d
source-git-commit: 8b02757591c4e8f607e936de4eda74d76953d9b7
workflow-type: tm+mt
source-wordcount: '511'
ht-degree: 0%

---

# 사후 배포 변수

다음 _사후 배포_ 변수는 배포 후 단계에서 작업을 제어하고 변수에서 값을 상속하고 재정의할 수 있습니다. [전역 변수](variables-global.md). 다음 변수를에 삽입합니다. `post-deploy` 의 단계 `.magento.env.yaml` 파일:

```yaml
stage:
  post-deploy:
    POST-DEPLOY_VARIABLE_NAME: value
```

빌드 및 배포 프로세스 사용자 지정에 대한 자세한 내용은 다음을 참조하십시오.

- [배포 구성](configure-env-yaml.md)
- [배포 프로세스](../deploy/process.md)

## `TTFB_TESTED_PAGES`

- **기본값**— `[]` (빈 배열)
- **버전**—Adobe Commerce 2.1.4 이상

구성 _첫 번째 바이트까지의 시간_ (TTFB) 사이트 성능을 테스트하기 위해 지정된 페이지에 대한 테스트. 테스트가 필요한 각 페이지에 대한 절대 경로 참조 또는 프로토콜 및 호스트가 있는 URL을 지정합니다.

```yaml
stage:
  post-deploy:
    TTFB_TESTED_PAGES:
       - "index.php"
       - "index.php/customer/account/create"
       - "https://example.com/catalog/some-category"
```

변경 사항을 테스트하고 커밋할 페이지를 지정한 후 _첫 번째 바이트까지의 시간_ 테스트는 배포 후 단계 동안 실행되고 각 경로에 대한 결과를 클라우드 로그에 게시합니다.

```terminal
[2019-06-20 20:42:22] INFO: TTFB test result: 0.313s {"url":"https://staging-tkyicst-xkmwgjkwmwfuk.us-4.magentosite.cloud/customer/account/create","status":200}
[2019-06-20 20:42:22] INFO: TTFB test result: 0.408s {"url":"https://staging-tkyicst-xkmwgjkwmwfuk.us-4.magentosite.cloud/checkout/cart","status":200}
```

리디렉션된 경로의 경우, 로그는 환경 변수에 구성된 경로 대신 리디렉션 대상의 경로를 보고합니다. 잘못된 경로를 지정하면 로그에 경고 메시지가 표시됩니다.

## `WARM_UP_CONCURRENCY`

- **기본값**—_설정되지 않음_
- **버전**—Adobe Commerce 2.1.4 이상

서버 로드를 줄이기 위해 캐시 웜업 작업 중에 전송할 동시 요청의 제한을 지정합니다. 이 값은 병렬 연결 수를 제한하며 `WARM_UP_PAGES` 사후 배포 변수는 캐시 사전 로드를 위한 여러 페이지를 지정합니다.

```yaml
stage:
  post-deploy:
    WARM_UP_CONCURRENCY: 4
```

## `WARM_UP_PAGES`

- **기본값**— `index.php`
- **버전**—Adobe Commerce 2.1.4 이상

에서 캐시를 미리 로드하는 데 사용되는 페이지 목록 사용자 지정 `post_deploy` 스테이지. 사후 배포 후크를 구성해야 합니다. 다음을 참조하십시오. [후크 섹션](../application/hooks-property.md) / `.magento.app.yaml` 파일.

- **단일 페이지**- 캐시에 추가할 단일 페이지를 지정합니다. 기본 기본 기본 URL을 나타내지 않아도 됩니다. 다음 예제는 를 캐시합니다 `BASE_URL/index.php` 페이지:

  ```yaml
  stage:
    post-deploy:
      WARM_UP_PAGES:
        - "index.php"
  ```

- **여러 도메인**- 여러 URL을 나열합니다. 다음 예제는 두 도메인의 페이지를 캐시합니다.

  ```yaml
  stage:
    post-deploy:
      WARM_UP_PAGES:
        - 'http://example1.com/test'
        - 'http://example2.com/test'
  ```

- **여러 페이지**- 특정 정규 표현식 패턴에 따라 여러 페이지를 캐시하려면 다음 형식을 사용합니다.

  ```terminal
  <entity_type>:<pattern|url|product_sku>:<store_id|store_code>
  ```

   - `entity_type`: 가능한 변형 `category`, `cms-page`, `product`, `store-page`
   - `pattern|url|product_sku`: 사용 `regexp` 패턴 또는 정확한 일치 `url` url을 필터링하거나 모든 페이지에 별표(\*)를 사용합니다. 에 대한 제품 sku 사용 `product` 엔터티 유형
   - `store_id|store_code`: 저장소의 ID 또는 코드를 사용하거나 모든 저장소의 별표(\*)를 사용합니다. 여러 저장소 ID 또는 코드를 로 구분하여 전달할 수 있습니다. `|`

  다음 예제는 를 위해 캐시합니다. `category` 및 `cms-page` 다음 기준에 따른 엔티티 유형:
   - id가 있는 저장소의 모든 범주 페이지 `1`
   - 코드가 있는 저장소의 모든 범주 페이지 `store1` 및 `store2`
   - 카테고리 페이지 `cars` 코드 포함 저장소용 `store_en`
   - cms 페이지 `contact` 모든 스토어의 경우
   - cms 페이지 `contact` ID가 있는 저장소의 경우 `1` 및 `2`
   - 다음을 포함하는 모든 카테고리 페이지 `car_` 다음으로 끝남 `html` ID 2의 저장소용
   - 다음을 포함하는 모든 카테고리 페이지 `tires_` 코드 포함 저장소용 `store_gb`

     ```yaml
     stage:
       post-deploy:
         WARM_UP_PAGES:
           - "category:*:1"
           - "category:*:store1|store2"
           - "category:cars:store_en"
           - "cms-page:contact:*"
           - "cms-page:contact:1|2"
           - "category:|car_.*?\\.html$|:2"
           - "category:|tires_.*|:store_gb"
     ```

  다음 예제는 를 위해 캐시합니다. `product` 다음 기준에 따른 엔티티 유형:
   - 모든 스토어의 모든 제품(성능 문제를 방지하기 위해 스토어당 100개로 프로그래밍 방식으로 제한)
   - 스토어의 모든 제품 `store1`
   - 제품 `sku1` 모든 스토어의 경우
   - 제품 `sku1` 코드가 있는 스토어의 경우 `store1` 및 `store2`
   - 제품 `sku1`, `sku2` 및 `sku3` 코드가 있는 스토어의 경우 `store1` 및 `store2`

     ```yaml
     stage:
       post-deploy:
         WARM_UP_PAGES:
           - "product:*:*"
           - "product:*:store1"
           - "product:sku1:*"
           - "product:sku1:store1|store2"
           - "product:sku1|sku2|sku3:store1|store2"
     ```

  다음 예제는 를 위해 캐시합니다. `store-page` 다음 기준에 따른 엔티티 유형:
   - 페이지 `/contact-us` 모든 스토어의 경우
   - 페이지 `/contact-us` ID가 있는 저장소용 `1`
   - 페이지 `/contact-us` 코드가 있는 스토어의 경우 `code1` 및 `code2`

  ```yaml
        stage:
          post-deploy:
            WARM_UP_PAGES:
              - "store-page:/contact-us:*"
              - "store-page:/contact-us:1"
              - "store-page:/contact-us:code1|code2"
  ```
