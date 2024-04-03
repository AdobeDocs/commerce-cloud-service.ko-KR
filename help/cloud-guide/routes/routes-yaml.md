---
title: 경로 구성
description: 클라우드 인프라 환경에서 Adobe Commerce에 대한 수신 HTTPS 요청의 경로를 정의하는 방법을 알아봅니다.
feature: Cloud, Configuration, Routes
exl-id: a33797e5-14cc-45eb-a048-96180b872a4a
source-git-commit: 1253d8357fd2554050d1775fefbc420a2097db5f
workflow-type: tm+mt
source-wordcount: '886'
ht-degree: 0%

---

# 경로 구성

다음 `routes.yaml` 파일 위치: `.magento/routes.yaml` directory는 클라우드 인프라 통합, 스테이징 및 프로덕션 환경에서 Adobe Commerce의 경로를 정의합니다. 경로는 애플리케이션이 수신 HTTP 및 HTTPS 요청을 처리하는 방법을 결정합니다.

기본값 `routes.yaml` 파일은 단일 기본 도메인이 있는 프로젝트와 여러 도메인에 대해 구성된 프로젝트에서 HTTP 요청을 HTTPS로 처리하기 위한 경로 템플릿을 지정합니다.

```yaml
"http://{default}/":
    type: upstream
    upstream: "mymagento:http"
"http://{all}/":
    type: upstream
    upstream: "mymagento:http"
```

사용 `magento-cloud` 구성된 경로 목록을 보기 위한 CLI:

```bash
magento-cloud environment:routes

+-------------------+----------+---------------+
| Route             | Type     | To            |
+-------------------+----------+---------------+
| http://{default}/ | upstream | mymagento     |
+-------------------+----------+---------------+
```

## Pro 환경에 대한 구성 업데이트

{{pro-self-service-warning}}

## 루트 템플릿

다음 `routes.yaml` 파일은 템플릿으로 만들어진 경로와 해당 구성 목록입니다. 경로 템플릿에서 다음 자리 표시자를 사용할 수 있습니다.

- 다음 `{default}` placeholder는 프로젝트의 기본값으로 구성된 적격 도메인 이름을 나타냅니다.

  예를 들어 기본 도메인이 있는 프로젝트 `example.com` 및 다음 경로 템플릿:

  ```text
  https://www.{default}/
  https://{default}/blog
  ```

  이러한 템플릿은 프로덕션 환경에서 다음 URL로 확인됩니다.

  ```text
  https://www.example.com/
  https://example.com/blog
  ```

- 다음 `{all}` placeholder는 프로젝트에 대해 구성된 모든 도메인 이름을 나타냅니다.

  (예: 다음과 같은 프로젝트) `example.com` 및 `example1.com` 다음과 같은 경로 템플릿을 사용하는 도메인:

  ```text
  https://www.{all}/
  
  https://{all}/blog
  ```

  이러한 템플릿은 프로젝트의 모든 도메인에 대해 다음 경로로 확인됩니다.

  ```text
  https://www.example.com/
  
  https://www.example1.com/
  
  https://example.com/blog
  
  https://example1.com/blog
  ```

  다음 `{all}` 자리 표시자는 여러 도메인에 대해 구성된 프로젝트에 유용합니다. 비프로덕션 분기에서 `{all}` 는 각 도메인의 프로젝트 ID 및 환경 ID로 대체됩니다.

  프로젝트에 개발 중에 일반적으로 사용되는 구성된 도메인이 없는 경우 `{all}` 자리 표시자는 와 같은 방식으로 동작합니다. `{default}` 자리 표시자.

또한 Adobe Commerce은 모든 활성 통합 환경에 대한 경로를 생성합니다. 통합 환경의 경우 `{default}` 자리 표시자가 다음 도메인 이름으로 대체됩니다.

```text
[branch]-[per-environment-random-string]-[project-id].[region].magentosite.cloud
```

예를 들어 `refactorcss` 에 대한 분기 `mswy7hzcuhcjw` 에서 호스팅되는 프로젝트 `us` 클러스터에는 다음 도메인이 있습니다.

```text
https://refactorcss-oy3m2pq-mswy7hzcuhcjw.us.magentosite.cloud/
```

>[!NOTE]
>
>클라우드 프로젝트가 여러 스토어를 지원하는 경우 다음의 경로 구성 지침을 따르십시오. [여러 웹 사이트 또는 스토어](../store/multiple-sites.md).

### 후행 슬래시

경로 정의에는 폴더 또는 디렉토리를 나타내는 후행 슬래시가 포함되어 있습니다. 그러나 동일한 컨텐츠에 후행 슬래시를 사용하거나 사용하지 않을 수 있습니다. 다음 URL은 동일하게 확인되지만 다음과 같이 해석될 수 있습니다. _두 가지 다름_ URL:

```text
https://www.example.com/blog/

https://www.example.com/blog
```

>[!TIP]
>
>디렉토리에 후행 슬래시를 사용하는 것이 가장 좋지만, 선택하는 방법에 관계없이 **일관성 유지** 두 개의 위치를 생성하지 않도록 합니다.

## 경로 프로토콜

모든 환경에서는 자동으로 HTTP와 HTTPS를 모두 지원합니다.

- 구성에서 HTTP 경로만 지정하는 경우 HTTPS 경로가 자동으로 생성되므로 리디렉션 없이 HTTP와 HTTPS 모두에서 사이트를 제공할 수 있습니다.

  예를 들어 기본 도메인이 있는 프로젝트 `example.com` 및 다음 경로 템플릿:

  ```text
  http://{default}/
  ```

  이 템플릿은 다음 URL로 확인됩니다.

  ```text
  http://example.com/
  
  https://example.com/
  ```

- 구성에서 HTTPS 경로만 지정하는 경우 모든 HTTP 요청은 HTTPS로 리디렉션됩니다.

  예를 들어 기본 도메인이 있는 프로젝트 `example.com` (다음 경로 템플릿 사용)

  ```text
  https://{default}/
  ```

  이 템플릿은 다음 URL로 확인됩니다.

  ```text
  https://example.com/
  ```

  또한 다음 리디렉션을 처리합니다.

  `http://example.com/` ==> `https://example.com/`

모든 페이지를 TLS에서 제공합니다. 이 구성의 경우 다음 방법 중 하나를 사용하여 암호화되지 않은 모든 요청에서 TLS에 해당하는 리디렉션을 구성해야 합니다.

- 에서 프로토콜을 HTTPS로 변경 `routes.yaml` 파일.

  ```yaml
  "https://{default}/":
      type: upstream
      upstream: "mymagento:http"
  "https://{all}/":
      type: upstream
      upstream: "mymagento:http"
  ```

- 스테이징 및 프로덕션 환경의 경우 [TLS를 Fastly로 강제 실행](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/redirect-http-to-https-for-all-pages-on-cloud-force-tls.html) 관리자 UI의 옵션입니다. 이 옵션을 사용하면 Fastly가 HTTPS로의 리디렉션을 처리하므로 을(를) 업데이트할 필요가 없습니다 `routes.yaml` 구성.

## 경로 옵션

다음 속성을 사용하여 각 경로를 별도로 구성합니다.

| 속성 | 설명 |
| ---------------- | ----------- |
| `type: upstream` | 애플리케이션 제공. 또한 `upstream` 응용 프로그램의 이름을 지정하는 속성입니다( `.magento.app.yaml`) 다음에 오는 `:http` 엔드포인트. |
| `type: redirect` | 다른 경로로 리디렉션합니다. 그 뒤에 가 옵니다. `to` 속성 : 템플릿으로 식별되는 다른 경로로의 HTTP 리디렉션입니다. |
| `cache:` | 컨트롤 [경로 캐싱](caching.md). |
| `redirects:` | 컨트롤 [리디렉션 규칙](redirects.md). |
| `ssi:` | 의 활성화 제어 [서버측 포함](server-side-includes.md). |

## 단순 경로

다음 예에서 경로 구성은 apex 도메인과 `www` 에 대한 하위 도메인 `mymagento` 응용 프로그램. 이 경로는 HTTPS 요청을 리디렉션하지 않습니다.

**예제 1:**

```yaml
"http://{default}/":
    type: upstream
    upstream: "mymagento:http"

"http://www.{default}/":
    type: redirect
    to: "http://{default}/"
```

이 예에서 요청 라우팅은 다음 규칙을 따릅니다.

- 서버는 다음 URL 패턴으로 요청에 직접 응답합니다.

  ```text
  http://example.com/path
  ```

- 서버에서 _301 리디렉션_ 다음 URL 패턴이 있는 요청의 경우:

  ```text
  http://www.example.com/mypath
  ```

  이러한 요청은 Apex 도메인으로 리디렉션됩니다. 예:

  ```text
  http://example.com/mypath
  ```

다음 예제에서는 라우트 구성이 www 도메인에서 apex 도메인으로 URL을 리디렉션하지 않습니다. 대신 요청은 www와 apex 도메인 모두에서 제공됩니다.

**예제 2:**

```yaml
"http://{default}/":
    type: upstream
    upstream: "mymagento:http"

"http://www.{default}/":
    type: upstream
    upstream: "mymagento:http"
```

## 와일드카드 경로

클라우드 인프라의 Adobe Commerce은 와일드카드 경로를 지원하므로 여러 하위 도메인을 동일한 애플리케이션에 매핑할 수 있습니다. 리디렉션 및 업스트림 경로에 대해 작동합니다. 경로 접두사로 별표(\*)를 사용합니다. 예를 들어, 다음 경로는 동일한 애플리케이션입니다.

- `*.example.com`
- `www.example.com`
- `blog.example.com`
- `us.example.com`

라이브 환경에서 다목적 캐치 도메인으로서 작동합니다.

### 매핑되지 않은 도메인 라우팅

점( )을 사용하여 도메인에 매핑되지 않은 시스템으로 라우팅할 수 있습니다.`.`)을 클릭하여 하위 도메인을 분리합니다.

**예:**

이(가) 있는 프로젝트 `add-theme` 분기는 다음 URL로 라우팅됩니다.

```text
http://add-theme-projectID.us.magento.com/
```

다음 경로 템플리트를 정의하는 경우

```text
http://www.{default}/
```

경로는 다음 URL로 확인됩니다.

```text
http://www.add-theme-projectID.us.magento.com/
```

점과 경로가 확인되기 전에 하위 도메인을 삽입할 수 있습니다.

**예:**

다음 경로 템플릿을 정의합니다.

```text
http://*.{default}/
```

이 템플릿은 다음 URL을 모두 확인합니다.

```text
http://foo.add-theme-projectID.us.magentosite.cloud/
http://bar.add-theme-projectID.us.magentosite.cloud/
```

환경에 대한 SSH 연결을 설정하고 를 사용하여 매핑되지 않은 도메인에 대한 경로 패턴을 볼 수 있습니다. `magento-cloud` 경로를 나열하는 CLI:

```terminal
web@mymagento.0:~$ vendor/bin/ece-tools env:config:show routes

Magento Cloud Routes:
+------------------------------------------+--------------------------------------------------------------+
| Route configuration                      | Value                                                        |
+------------------------------------------+--------------------------------------------------------------+
| http://www.add-theme-projectID.us.magento.com/:                                                         |
+------------------------------------------+--------------------------------------------------------------+
| primary                                  | false                                                        |
| id                                       | null                                                         |
| attributes                               |                                                              |
| type                                     | upstream                                                     |
| to                                       | mymagento                                                    |
| original_url                             | https:/{default}/                                            |
+------------------------------------------+--------------------------------------------------------------+
| https://*.add-theme-projectID.us.magentosite.cloud/:                                                    |
+------------------------------------------+--------------------------------------------------------------+
| primary                                  | false                                                        |
| id                                       | null                                                         |
| attributes                               |                                                              |
| type                                     | upstream                                                     |
| to                                       | mymagento                                                    |
| original_url                             | https://*.{default}/                                         |
+------------------------------------------+--------------------------------------------------------------+
```

## 리디렉션 및 캐싱

에서 더 자세히 설명했듯이 [리디렉션](redirects.md)과 같은 복잡한 리디렉션 규칙을 관리할 수 있습니다. _부분 리디렉션_&#x200B;및 경로 기반 규칙 지정 [캐싱](caching.md):

```yaml
https://www.{default}/:
    type: redirect
    to: https://{default}/
https://{default}/:
    cache:
        cookies: [""]
        default_ttl: 0
        enabled: true
        headers:
            - Accept
            - Accept-Language
    ssi:
        enabled: false
    type: upstream
    upstream: mymagento:http
```
