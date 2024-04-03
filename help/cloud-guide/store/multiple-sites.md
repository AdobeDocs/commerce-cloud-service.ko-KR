---
title: 여러 웹 사이트 또는 스토어 설정
description: 클라우드 인프라에서 Adobe Commerce에 대한 여러 웹 사이트 또는 스토어를 구성하는 방법에 대해 알아봅니다.
feature: Cloud, Configuration, Routes, Site Navigation
exl-id: 16e932ef-f083-44d7-977f-0c78899e151a
source-git-commit: 85aa54af10e7ea44adde5403b69ff03d4a0c622f
workflow-type: tm+mt
source-wordcount: '1013'
ht-degree: 0%

---

# 여러 웹 사이트 또는 스토어 설정

영어 스토어, 프랑스 스토어 및 독일 스토어와 같은 여러 웹 사이트나 스토어가 있도록 Adobe Commerce을 구성할 수 있습니다. 다음을 참조하십시오 [웹 사이트, 스토어 및 스토어 조회수 이해](best-practices.md#store-views).

>[!WARNING]
>
>웹 사이트 및 스토어 수를 늘리면 카탈로그 데이터가 확장됩니다. 프로젝트 아키텍처에 따라 추가 저장소로 인해 색인 지정 프로세스가 길어지고 캐시되지 않은 카탈로그 페이지에 대한 응답 시간이 느려질 수 있습니다. Adobe은 사이트 성능을 면밀히 모니터링할 것을 권장합니다.

여러 스토어를 설정하는 프로세스는 고유한 도메인을 사용할지 또는 공유 도메인을 사용할지 여부에 따라 다릅니다.

고유 도메인이 있는 여러 저장소:

```terminal
https://first.store.com/
https://second.store.com/
```

동일한 도메인을 사용하는 여러 저장소:

```terminal
https://store.com/first/
https://store.com/second/
```

>[!TIP]
>
>사이트 기본 URL에 저장소 보기를 추가하려면 여러 디렉터리를 만들 필요가 없습니다. 다음을 참조하십시오 [기본 URL에 스토어 코드 추가](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/multi-sites/ms-admin.html) 다음에서 _구성 안내서_.

## 도메인 추가

사용자 정의 도메인은 Pro Staging 및 모든 프로덕션 환경에 추가할 수 있으며, 통합 환경에 추가할 수 없습니다.

도메인을 추가하는 프로세스는 클라우드 계정 유형에 따라 다릅니다.

- Pro Staging 및 프로덕션용

  새 도메인을 Fastly에 추가합니다. 다음을 참조하십시오. [도메인 관리](../cdn/fastly-custom-cache-configuration.md#manage-domains)또는 지원 티켓을 열어 지원을 요청하십시오. 또한 [Adobe Commerce 지원 티켓 제출](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) 클러스터에 추가할 새 도메인을 요청합니다.

- 스타터 프로덕션용

  새 도메인을 Fastly에 추가합니다. 다음을 참조하십시오. [도메인 관리](../cdn/fastly-custom-cache-configuration.md#manage-domains), 또는 [Adobe Commerce 지원 티켓 제출](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) 도움을 요청하려면요 또한 새 도메인을 **도메인** 의 탭 [!DNL Cloud Console]: `https://<zone>.magento.cloud/projects/<project-ID>/edit`

## 로컬 설치 구성

여러 스토어를 사용하도록 로컬 설치를 구성하려면 다음을 참조하십시오. [여러 웹 사이트 또는 스토어][config-multiweb] 다음에서 _구성 안내서_.

여러 스토어를 사용하도록 로컬 설치를 성공적으로 만들고 테스트한 후 통합 환경을 준비해야 합니다.

1. **경로 또는 위치 구성**—Adobe Commerce에서 수신 URL을 처리하는 방법을 지정합니다.

   - [별도의 도메인에 대한 경로](#configure-routes-for-separate-domains)
   - [공유 도메인의 위치](#configure-locations-for-shared-domains)

1. **웹 사이트, 스토어 및 스토어 조회수 설정**—Adobe Commerce 관리 UI를 사용하여 구성
1. **변수 수정**- 값 지정 `MAGE_RUN_TYPE` 및 `MAGE_RUN_CODE` 의 변수 `magento-vars.php` 파일
1. **환경 배포 및 테스트**—배포 및 테스트 `integration` 분기

>[!TIP]
>
>로컬 환경을 사용하여 여러 웹 사이트나 스토어를 설정할 수 있습니다. 다음 Cloud Docker 지침을 참조하십시오. [여러 웹 사이트 또는 스토어 설정](https://developer.adobe.com/commerce/cloud-tools/docker/configure/multiple-sites/).

### Pro 환경에 대한 구성 업데이트

{{pro-self-service-warning}}

### 별도의 도메인에 대한 경로 구성

경로는 들어오는 URL을 처리하는 방법을 정의합니다. 고유 도메인이 있는 여러 저장소를 사용하려면 `routes.yaml` 파일. 경로를 구성하는 방법은 사이트를 운영하는 방식에 따라 다릅니다.

**통합 환경에서 경로를 구성하려면 다음을 수행하십시오**:

1. 로컬 워크스테이션에서 `.magento/routes.yaml` 파일을 텍스트 편집기에 넣습니다.

1. 도메인과 하위 도메인을 정의합니다. 다음 `mymagento` 업스트림 값은 의 이름 속성과 동일한 값입니다. `.magento.app.yaml` 파일.

   ```yaml
   "http://{default}/":
       type: upstream
       upstream: "mymagento:http"
   
   "http://<second-site>.{default}/":
       type: upstream
       upstream: "mymagento:http"
   ```

1. 변경 내용을 `routes.yaml` 파일.

1. 계속 [웹 사이트, 스토어 및 스토어 조회수 설정](#set-up-websites-stores-and-store-views).

### 공유 도메인의 위치 구성

경로 구성이 URL의 처리 방법을 정의하는 경우 `web` 의 속성 `.magento.app.yaml` 파일은 응용 프로그램이 웹에 노출되는 방식을 정의합니다. 웹 _위치_ 수신 요청을 더 세부적으로 허용합니다. 예를 들어 도메인이 입니다. `store.com`, 다음을 사용할 수 있습니다 `/first` (기본 사이트) 및 `/second` 도메인을 공유하는 두 개의 서로 다른 스토어에 대한 요청.

**새 웹 위치를 구성하려면**:

1. 루트에 대한 별칭 만들기(`/`). 이 예제에서 별칭은 `&app` 3호선에

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

1. 웹 사이트에 대한 패스스루 만들기(`/website`) 이전 단계의 별칭을 사용하여 루트를 참조합니다.

   별칭을 사용하면 `website` 루트 위치에서 값에 액세스합니다. 이 예에서는 웹 사이트입니다 `passthru` 21호선에 있습니다

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

**다른 디렉토리로 위치 구성하기**:

1. 루트에 대한 별칭 만들기(`/`) 및 정적(`/static`) 위치.

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

1. 웹 사이트에 대한 하위 디렉토리를 `pub` 디렉터리: `pub/<website>`

1. 다음을 복사합니다. `pub/index.php` 파일을 `pub/<website>` 디렉터리 및 업데이트 `bootstrap` 경로 (`/../../app/bootstrap.php`).

   ```
   try {
       require __DIR__ . '/../../app/bootstrap.php';
   } catch (\Exception $e) { 
   ```

1. 에 대한 패스스루 만들기 `index.php` 파일.

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

   - `pub/<website>/index.php` (이 파일이 있는 경우) `.gitignore`, 푸시는 force 옵션이 필요할 수 있습니다.)
   - `.magento.app.yaml`

### 웹 사이트, 스토어 및 스토어 조회수 설정

다음에서 _관리자 UI_, Adobe Commerce 설정 **웹 사이트**, **스토어**, 및 **보기 저장**. 다음을 참조하십시오 [관리자에서 여러 웹 사이트, 스토어 및 스토어 보기 설정](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/multi-sites/ms-admin.html) 다음에서 _구성 안내서_.

로컬 설치를 설정할 때 관리자의 웹 사이트, 스토어 및 스토어 조회수와 동일한 이름과 코드를 사용하는 것이 중요합니다. 다음을 업데이트할 때 이러한 값이 필요합니다. `magento-vars.php` 파일.

### 변수 수정

NGINX 가상 호스트를 구성하는 대신 `MAGE_RUN_CODE` 및 `MAGE_RUN_TYPE` 변수를 사용하는 경우 `magento-vars.php` 프로젝트 루트 디렉토리에 있는 파일입니다.

**를 사용하여 변수를 전달하려면 `magento-vars.php` 파일**:

1. 를 엽니다. `magento-vars.php` 파일을 텍스트 편집기에 넣습니다.

   다음 [기본값 `magento-vars.php` 파일](https://github.com/magento/magento-cloud/blob/master/magento-vars.php) 은 다음과 같아야 합니다.

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

1. 주석을 이동 `if` 다음을 차단합니다. _이후_ 다음 `function` 더 이상 주석을 남기지 않고 차단합니다.

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

1. 에서 다음 값을 바꿉니다. `if (isHttpHost("example.com"))` 차단:
   - `example.com`—의 기본 URL 사용 _웹 사이트_
   - `default`—의 고유한 코드 사용 _웹 사이트_ 또는 _스토어 뷰_
   - `store`- 다음 값 중 하나를 사용합니다.
      - `website`- 를 로드합니다. _웹 사이트_ 상점 앞에서
      - `store`- a 로드 _스토어 뷰_ 상점 앞에서

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

   도메인이 동일한 여러 사이트의 경우 다음을 확인해야 합니다. _호스트_ 및 _URI_:

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

1. 변경 내용을 `magento-vars.php` 파일.

### 통합 서버에서 배포 및 테스트

클라우드 인프라 통합 환경의 Adobe Commerce에 변경 사항을 푸시하고 사이트를 테스트합니다.

1. 원격 분기에 코드 변경 사항을 추가, 커밋 및 푸시합니다.

   ```bash
   git add -A && git commit -m "Implement multiple sites" && git push origin <branch-name>
   ```

1. 배포가 완료될 때까지 기다립니다.

1. 배포 후 웹 브라우저에서 스토어 URL을 엽니다.

   고유한 도메인을 사용하려면 다음 형식을 사용하십시오. `http://<magento-run-code>.<site-URL>`

   For example, `http://french.master-name-projectID.us.magentosite.cloud/`

   공유 도메인에서 다음 형식을 사용합니다. `http://<site-URL>/<magento-run-code>`

   For example, `http://master-name-projectID.us.magentosite.cloud/french/`

1. 사이트를 완전히 테스트하고 코드를 `integration` 추가 배포를 위한 분기.

## 스테이징 및 프로덕션에 배포

다음에 대한 배포 프로세스를 따릅니다. [스테이징 및 프로덕션에 배포](../deploy/staging-production.md). Starter 및 Pro 환경의 경우 [!DNL Cloud Console] 을 클릭하여 코드를 여러 환경에 푸시할 수 있습니다.

Adobe은 프로덕션 환경으로 푸시하기 전에 스테이징 환경에서 완전히 테스트하는 것을 권장합니다. 통합 환경에서 코드를 변경하고 환경 간에 다시 배포하는 프로세스를 시작합니다.

<!-- link definitions -->

[config-multiweb]: https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/multi-sites/ms-overview.html
