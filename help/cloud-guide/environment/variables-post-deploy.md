---
title: 사후 배포 변수
description: Adobe Commerce on cloud infrastructure 배포 후 단계에서 작업을 제어하는 환경 변수 목록을 참조하십시오.
feature: Cloud, Configuration, Cache
recommendations: noDisplay, catalog
role: Developer
exl-id: e460335f-cd2b-4c98-b1ff-32504599b33d
source-git-commit: b49a51aba56f79b5253eeacb1adf473f42bb8959
workflow-type: tm+mt
source-wordcount: '511'
ht-degree: 0%

---

# 사후 배포 변수

다음 _사후 배포_ 변수는 사후 배포 단계에서 작업을 제어하며 [전역 변수](variables-global.md)에서 값을 상속하고 재정의할 수 있습니다. `.magento.env.yaml` 파일의 `post-deploy` 단계에 다음 변수를 삽입합니다.

```yaml
stage:
  post-deploy:
    POST-DEPLOY_VARIABLE_NAME: value
```

빌드 및 배포 프로세스 사용자 지정에 대한 자세한 내용은 다음을 참조하십시오.

- [배포 구성](configure-env-yaml.md)
- [배포 프로세스](../deploy/process.md)

## `TTFB_TESTED_PAGES`

- **기본값**— `[]`(빈 배열)
- **버전**—Adobe Commerce 2.1.4 이상

지정된 페이지에 대해 _첫 번째 바이트까지의 시간_(TTFB) 테스트를 구성하여 사이트 성능을 테스트합니다. 테스트가 필요한 각 페이지에 대한 절대 경로 참조 또는 프로토콜 및 호스트가 있는 URL을 지정합니다.

```yaml
stage:
  post-deploy:
    TTFB_TESTED_PAGES:
       - "index.php"
       - "index.php/customer/account/create"
       - "https://example.com/catalog/some-category"
```

변경 사항을 테스트하고 커밋할 페이지를 지정한 후 배포 후 단계 동안 _첫 바이트에 대한 시간_ 테스트가 실행되고 각 경로의 결과를 클라우드 로그에 게시합니다.

```
[2019-06-20 20:42:22] INFO: TTFB test result: 0.313s {"url":"https://staging-tkyicst-xkmwgjkwmwfuk.us-4.magentosite.cloud/customer/account/create","status":200}
[2019-06-20 20:42:22] INFO: TTFB test result: 0.408s {"url":"https://staging-tkyicst-xkmwgjkwmwfuk.us-4.magentosite.cloud/checkout/cart","status":200}
```

리디렉션된 경로의 경우, 로그는 환경 변수에 구성된 경로 대신 리디렉션 대상의 경로를 보고합니다. 잘못된 경로를 지정하면 로그에 경고 메시지가 표시됩니다.

## `WARM_UP_CONCURRENCY`

- **기본값**—_설정되지 않음_
- **버전**—Adobe Commerce 2.1.4 이상

서버 로드를 줄이기 위해 캐시 웜업 작업 중에 전송할 동시 요청의 제한을 지정합니다. 이 값은 병렬 연결 수를 제한하며 `WARM_UP_PAGES` 사후 배포 변수가 캐시 미리 로드를 위한 여러 페이지를 지정하는 환경 구성에 유용합니다.

```yaml
stage:
  post-deploy:
    WARM_UP_CONCURRENCY: 4
```

## `WARM_UP_PAGES`

- **기본값**— `index.php`
- **버전**—Adobe Commerce 2.1.4 이상

`post_deploy` 단계에서 캐시를 미리 로드하는 데 사용되는 페이지 목록을 사용자 지정합니다. 사후 배포 후크를 구성해야 합니다. `.magento.app.yaml` 파일의 [후크 섹션](../application/hooks-property.md)을(를) 참조하세요.

- **단일 페이지**—캐시에 추가할 단일 페이지를 지정합니다. 기본 기본 기본 URL을 나타내지 않아도 됩니다. 다음 예제에서는 `BASE_URL/index.php` 페이지를 캐시합니다.

  ```yaml
  stage:
    post-deploy:
      WARM_UP_PAGES:
        - "index.php"
  ```

- **여러 도메인** - 여러 URL을 나열합니다. 다음 예제는 두 도메인의 페이지를 캐시합니다.

  ```yaml
  stage:
    post-deploy:
      WARM_UP_PAGES:
        - 'http://example1.com/test'
        - 'http://example2.com/test'
  ```

- **여러 페이지** - 특정 정규 표현식 패턴에 따라 여러 페이지를 캐시하려면 다음 형식을 사용하십시오.

  ```
  <entity_type>:<pattern|url|product_sku>:<store_id|store_code>
  ```

   - `entity_type`: 가능한 변형 `category`, `cms-page`, `product`, `store-page`
   - `pattern|url|product_sku`: `regexp` 패턴 또는 정확히 일치하는 `url`을(를) 사용하여 URL을 필터링하거나 모든 페이지에 별표(\*)를 사용하십시오. `product` 엔터티 형식에 제품 sku 사용
   - `store_id|store_code`: 저장소의 ID 또는 코드를 사용하거나 모든 저장소의 별표(\*)를 사용합니다. `|`(으)로 구분된 여러 저장소 ID 또는 코드를 전달할 수 있습니다

  다음 예제에서는 이러한 조건을 기반으로 `category` 및 `cms-page` 엔터티 형식을 캐시합니다.
   - ID가 `1`인 저장소의 모든 범주 페이지
   - 코드가 `store1` 및 `store2`인 저장소의 모든 범주 페이지
   - 코드가 `store_en`인 스토어의 범주 페이지 `cars`
   - 모든 스토어의 cms 페이지 `contact`
   - ID가 `1` 및 `2`인 저장소의 cms 페이지 `contact`
   - `car_`이(가) 포함되어 있고 ID 2의 저장소에 대해 `html`(으)로 끝나는 모든 범주 페이지
   - 코드가 `store_gb`인 스토어의 `tires_`이(가) 포함된 모든 범주 페이지

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

  다음 예제에서는 이러한 조건을 기반으로 `product` 엔터티 형식에 대해 캐싱합니다.
   - 모든 스토어의 모든 제품(성능 문제를 방지하기 위해 스토어당 100개로 프로그래밍 방식으로 제한)
   - `store1` 스토어의 모든 제품
   - 모든 스토어에 대해 `sku1`을(를) 사용하는 제품
   - 코드가 `store1` 및 `store2`인 스토어에 대해 `sku1`이(가) 있는 제품
   - 코드가 `store1` 및 `store2`인 스토어의 경우 `sku1`, `sku2` 및 `sku3`인 제품

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

  다음 예제에서는 이러한 조건을 기반으로 `store-page` 엔터티 형식에 대해 캐싱합니다.
   - 모든 스토어의 `/contact-us` 페이지
   - ID가 `1`인 저장소의 `/contact-us` 페이지
   - 코드가 `code1` 및 `code2`인 스토어의 `/contact-us` 페이지

  ```yaml
        stage:
          post-deploy:
            WARM_UP_PAGES:
              - "store-page:/contact-us:*"
              - "store-page:/contact-us:1"
              - "store-page:/contact-us:code1|code2"
  ```
