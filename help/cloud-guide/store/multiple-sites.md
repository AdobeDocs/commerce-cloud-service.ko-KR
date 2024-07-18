---
title: 여러 웹 사이트 또는 스토어 설정
description: 클라우드 인프라에서 Adobe Commerce에 대한 여러 웹 사이트 또는 스토어를 구성하는 방법에 대해 알아봅니다.
feature: Cloud, Configuration, Routes, Site Navigation
exl-id: 16e932ef-f083-44d7-977f-0c78899e151a
source-git-commit: b49a51aba56f79b5253eeacb1adf473f42bb8959
workflow-type: tm+mt
source-wordcount: '1013'
ht-degree: 0%

---

# 여러 웹 사이트 또는 스토어 설정

영어 스토어, 프랑스 스토어 및 독일 스토어와 같은 여러 웹 사이트나 스토어가 있도록 Adobe Commerce을 구성할 수 있습니다. [웹 사이트, 스토어 및 스토어 조회수 이해](best-practices.md#store-views)를 참조하세요.

>[!WARNING]
>
>웹 사이트 및 스토어 수를 늘리면 카탈로그 데이터가 확장됩니다. 프로젝트 아키텍처에 따라 추가 저장소로 인해 색인 지정 프로세스가 길어지고 캐시되지 않은 카탈로그 페이지에 대한 응답 시간이 느려질 수 있습니다. Adobe은 사이트 성능을 면밀히 모니터링할 것을 권장합니다.

여러 스토어를 설정하는 프로세스는 고유한 도메인을 사용할지 또는 공유 도메인을 사용할지 여부에 따라 다릅니다.

고유 도메인이 있는 여러 저장소:

```
https://first.store.com/
https://second.store.com/
```

동일한 도메인을 사용하는 여러 저장소:

```
https://store.com/first/
https://store.com/second/
```

>[!TIP]
>
>사이트 기본 URL에 저장소 보기를 추가하려면 여러 디렉터리를 만들 필요가 없습니다. _구성 가이드_&#x200B;에서 [기본 URL에 스토어 코드 추가](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/multi-sites/ms-admin.html)를 참조하십시오.

## 도메인 추가

사용자 정의 도메인은 Pro Staging 및 모든 프로덕션 환경에 추가할 수 있으며, 통합 환경에 추가할 수 없습니다.

도메인을 추가하는 프로세스는 클라우드 계정 유형에 따라 다릅니다.

- Pro Staging 및 프로덕션용

  새 도메인을 Fastly에 추가하거나 [도메인 관리](../cdn/fastly-custom-cache-configuration.md#manage-domains)를 참조하거나 지원 티켓을 열어 지원을 요청하세요. 또한 클러스터에 추가할 새 도메인을 요청하려면 [Adobe Commerce 지원 티켓을 제출](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)해야 합니다.

- 스타터 프로덕션용

  새 도메인을 Fastly에 추가하거나 [도메인 관리](../cdn/fastly-custom-cache-configuration.md#manage-domains) 또는 [Adobe Commerce 지원 티켓 제출](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)을 참조하여 지원을 요청하세요. 또한 [!DNL Cloud Console]의 **도메인** 탭에 새 도메인을 추가해야 합니다. `https://<zone>.magento.cloud/projects/<project-ID>/edit`

## 로컬 설치 구성

여러 스토어를 사용하도록 로컬 설치를 구성하려면 _구성 가이드_&#x200B;에서 [여러 웹 사이트 또는 스토어][config-multiweb]를 참조하십시오.

여러 스토어를 사용하도록 로컬 설치를 성공적으로 만들고 테스트한 후 통합 환경을 준비해야 합니다.

1. **경로 또는 위치 구성**—Adobe Commerce에서 수신 URL을 처리하는 방법을 지정합니다.

   - [별도의 도메인에 대한 경로](#configure-routes-for-separate-domains)
   - [공유 도메인의 위치](#configure-locations-for-shared-domains)

1. **웹 사이트, 스토어 및 스토어 보기 설정**—Adobe Commerce 관리 UI를 사용하여 구성
1. **변수 수정** - `magento-vars.php` 파일에서 `MAGE_RUN_TYPE` 및 `MAGE_RUN_CODE` 변수의 값을 지정합니다.
1. **환경 배포 및 테스트**—`integration` 분기 배포 및 테스트

>[!TIP]
>
>로컬 환경을 사용하여 여러 웹 사이트나 스토어를 설정할 수 있습니다. [여러 웹 사이트 또는 스토어를 설정](https://developer.adobe.com/commerce/cloud-tools/docker/configure/multiple-sites/)하는 Cloud Docker 지침을 참조하십시오.

### Pro 환경에 대한 구성 업데이트

{{pro-self-service-warning}}

### 별도의 도메인에 대한 경로 구성

경로는 들어오는 URL을 처리하는 방법을 정의합니다. 고유 도메인이 있는 여러 저장소를 사용하려면 `routes.yaml` 파일에서 각 도메인을 정의해야 합니다. 경로를 구성하는 방법은 사이트를 운영하는 방식에 따라 다릅니다.

**통합 환경에서 경로를 구성하려면**:

1. 로컬 워크스테이션에서 텍스트 편집기에서 `.magento/routes.yaml` 파일을 엽니다.

1. 도메인과 하위 도메인을 정의합니다. `mymagento` 업스트림 값은 `.magento.app.yaml` 파일의 이름 속성과 같은 값입니다.

   ```yaml
   "http://{default}/":
       type: upstream
       upstream: "mymagento:http"
   
   "http://<second-site>.{default}/":
       type: upstream
       upstream: "mymagento:http"
   ```

1. 변경 내용을 `routes.yaml` 파일에 저장합니다.

1. [웹 사이트, 스토어 및 스토어 보기 설정](#set-up-websites-stores-and-store-views)을 계속합니다.

### 공유 도메인의 위치 구성

경로 구성이 URL의 처리 방법을 정의하는 경우 `.magento.app.yaml` 파일의 `web` 속성은 응용 프로그램이 웹에 노출되는 방법을 정의합니다. 웹 _위치_&#x200B;에서 수신 요청을 더 세부적으로 지정할 수 있습니다. 예를 들어 도메인이 `store.com`인 경우 도메인을 공유하는 두 개의 다른 저장소에 대한 요청에 `/first`(기본 사이트) 및 `/second`을(를) 사용할 수 있습니다.

**새 웹 위치를 구성하려면**:

1. 루트(`/`)에 대한 별칭을 만듭니다. 이 예제에서 별칭은 3행의 `&app`입니다.

   ```yaml
   web:
       locations:
           "/": &app
               # The public directory of the app, relative to its root.
               root: "pub"
               passthru: "/index.php"
               index:
               - index.php
               ...
   ```

1. 웹 사이트(`/website`)의 통과 경로를 만들고 이전 단계의 별칭을 사용하여 루트를 참조합니다.

   별칭을 사용하면 `website`이(가) 루트 위치의 값에 액세스할 수 있습니다. 이 예제에서는 `passthru` 웹 사이트가 21행에 있습니다.

   ```yaml
   web:
       locations:
           "/": &app
               # The public directory of the app, relative to its root.
               root: "pub"
               passthru: "/index.php"
               index:
               - index.php
               ...
           "/media":
               root: "pub/media"
               ...
           "/static":
               root: "pub/static"
               allow: true
               scripts: false
               passthru: "/front-static.php"
               rules:
                   ^/static/version\d+/(?<resource>.*)$:
                       passthru: "/static/$resource"
           "/<website>": *app
             ...
   ```

**다른 디렉터리로 위치를 구성하려면**:

1. 루트(`/`) 및 정적(`/static`) 위치에 대한 별칭을 만듭니다.

   ```yaml
   web:
       locations:
           "/": &root
               # The public directory of the app, relative to its root.
               root: "pub"
               passthru: "/index.php"
               index:
               - index.php
               ...
           "/static": &static
               root: "pub/static"
   ```

1. `pub` 디렉터리 아래에 웹 사이트의 하위 디렉터리를 만듭니다. `pub/<website>`

1. `pub/index.php` 파일을 `pub/<website>` 디렉터리에 복사하고 `bootstrap` 경로(`/../../app/bootstrap.php`)를 업데이트합니다.

   ```
   try {
       require __DIR__ . '/../../app/bootstrap.php';
   } catch (\Exception $e) { 
   ```

1. `index.php` 파일에 대한 통과 만들기

   ```yaml
   web:
       locations:
           "/": &root
               # The public directory of the app, relative to its root.
               root: "pub"
               passthru: "/index.php"
               index:
               - index.php
               ...
           "/media":
               root: "pub/media"
               ...
           "/static": &static
               root: "pub/static"
               allow: true
               scripts: false
               passthru: "/front-static.php"
               rules:
                   ^/static/version\d+/(?<resource>.*)$:
                       passthru: "/static/$resource"
           "/<website>":
               <<: *root
               passthru: "<website>/index.php"
           "/<website>/static": *static
             ...
   ```

1. 변경된 파일을 커밋하고 푸시합니다.

   - `pub/<website>/index.php`(이 파일이 `.gitignore`에 있는 경우 푸시에 강제 옵션이 필요할 수 있습니다.)
   - `.magento.app.yaml`

### 웹 사이트, 스토어 및 스토어 조회수 설정

_관리 UI_&#x200B;에서 Adobe Commerce **웹 사이트**, **스토어** 및 **스토어 보기**&#x200B;를 설정합니다. _구성 가이드_&#x200B;의 [Admin에서 여러 웹 사이트, 스토어 및 스토어 보기 설정](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/multi-sites/ms-admin.html)을 참조하십시오.

로컬 설치를 설정할 때 관리자의 웹 사이트, 스토어 및 스토어 조회수와 동일한 이름과 코드를 사용하는 것이 중요합니다. `magento-vars.php` 파일을 업데이트할 때 이러한 값이 필요합니다.

### 변수 수정

NGINX 가상 호스트를 구성하는 대신 프로젝트 루트 디렉터리의 `magento-vars.php` 파일을 사용하여 `MAGE_RUN_CODE` 및 `MAGE_RUN_TYPE` 변수를 전달합니다.

**`magento-vars.php` 파일을 사용하여 변수를 전달하려면**:

1. 텍스트 편집기에서 `magento-vars.php` 파일을 엽니다.

   [기본 `magento-vars.php` 파일](https://github.com/magento/magento-cloud/blob/master/magento-vars.php)은(는) 다음과 같아야 합니다.

   ```php
   <?php
   // enable, adjust and copy this code for each store you run
   // Store #0, default one
   //if (isHttpHost("example.com")) {
   //    $_SERVER["MAGE_RUN_CODE"] = "default";
   //    $_SERVER["MAGE_RUN_TYPE"] = "store";
   //}
   function isHttpHost($host)
   {
       if (!isset($_SERVER['HTTP_HOST'])) {
           return false;
       }
       return $_SERVER['HTTP_HOST'] === $host;
   }
   ```

1. `function` 블록의 _after_&#x200B;가 되고 더 이상 주석을 달지 않도록 주석 처리된 `if` 블록을 이동하십시오.

   ```php
   <?php
   // enable, adjust and copy this code for each store you run
   // Store #0, default one
   
   function isHttpHost($host)
   {
       if (!isset($_SERVER['HTTP_HOST'])) {
           return false;
       }
       return $_SERVER['HTTP_HOST'] ===  $host;
   }
   
   if (isHttpHost("example.com")) {
       $_SERVER["MAGE_RUN_CODE"] = "default";
       $_SERVER["MAGE_RUN_TYPE"] = "store";
   }
   ```

1. `if (isHttpHost("example.com"))` 블록에서 다음 값을 바꿉니다.
   - `example.com`—_웹 사이트_&#x200B;의 기본 URL 사용
   - `default`—_웹 사이트_ 또는 _스토어 보기_&#x200B;에 대한 고유 코드 사용
   - `store`—다음 값 중 하나를 사용합니다.
      - `website`—상점 첫 화면에서 _웹 사이트_ 로드
      - `store`—상점 전면에서 _스토어 보기_&#x200B;를 로드합니다.

   고유 도메인을 사용하는 여러 사이트의 경우:

   ```php
   <?php
   function isHttpHost($host)
   {
       if (!isset($_SERVER['HTTP_HOST'])) {
       return false;
       }
       return $_SERVER['HTTP_HOST'] ===  $host;
   }
   
   if (isHttpHost("second.store.com")) {
       $_SERVER["MAGE_RUN_CODE"] = "<second-site>";
       $_SERVER["MAGE_RUN_TYPE"] = "website";
   }elseif (isHttpHost("store.com")){
       $_SERVER["MAGE_RUN_CODE"] = "base";
       $_SERVER["MAGE_RUN_TYPE"] = "website";
   }
   ```

   도메인이 같은 여러 사이트의 경우 _host_ 및 _URI_&#x200B;을(를) 확인해야 합니다.

   ```php
   <?php
   function isHttpHost($host)
   {
       if (!isset($_SERVER['HTTP_HOST'])) {
       return false;
       }
       return $_SERVER['HTTP_HOST'] ===  $host;
   }
   
   if (isHttpHost("store.com")) {
      $code = "base";
      $type = "website";
   
      $uri = explode('/', $_SERVER['REQUEST_URI']);
      if (isset($uri[1]) && $uri[1] == 'second') {
        $code = '<second-site>';
        $type = 'website';
      }
      $_SERVER["MAGE_RUN_CODE"] = $code;
      $_SERVER["MAGE_RUN_TYPE"] = $type;
   }
   ```

1. 변경 내용을 `magento-vars.php` 파일에 저장합니다.

### 통합 서버에서 배포 및 테스트

클라우드 인프라 통합 환경의 Adobe Commerce에 변경 사항을 푸시하고 사이트를 테스트합니다.

1. 원격 분기에 코드 변경 사항을 추가, 커밋 및 푸시합니다.

   ```bash
   git add -A && git commit -m "Implement multiple sites" && git push origin <branch-name>
   ```

1. 배포가 완료될 때까지 기다립니다.

1. 배포 후 웹 브라우저에서 스토어 URL을 엽니다.

   고유한 도메인을 사용하는 경우 `http://<magento-run-code>.<site-URL>` 형식을 사용하십시오.

   For example, `http://french.master-name-projectID.us.magentosite.cloud/`

   공유 도메인에 `http://<site-URL>/<magento-run-code>` 형식을 사용합니다.

   For example, `http://master-name-projectID.us.magentosite.cloud/french/`

1. 사이트를 철저히 테스트하고 코드를 `integration` 분기에 병합하여 추가 배포합니다.

## 스테이징 및 프로덕션에 배포

[스테이징 및 프로덕션에 배포](../deploy/staging-production.md)에 대한 배포 프로세스를 따르십시오. Starter 및 Pro 환경의 경우 [!DNL Cloud Console]을(를) 사용하여 환경 간에 코드를 푸시합니다.

Adobe은 프로덕션 환경으로 푸시하기 전에 스테이징 환경에서 완전히 테스트하는 것을 권장합니다. 통합 환경에서 코드를 변경하고 환경 간에 다시 배포하는 프로세스를 시작합니다.

<!-- link definitions -->

[config-multiweb]: https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/multi-sites/ms-overview.html
