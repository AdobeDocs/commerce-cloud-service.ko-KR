---
title: 사이트 맵 및 검색 엔진 로봇 추가
description: 클라우드 인프라의 Adobe Commerce에 사이트 맵 및 검색 엔진 로봇을 추가하는 방법을 알아봅니다.
feature: Cloud, Configuration, Search, Site Navigation
exl-id: b98f43fa-1878-466d-8ea0-1e7207af8b60
source-git-commit: ee1db75c73c086e0ea54e1a7591ca7f2b4d2b36d
workflow-type: tm+mt
source-wordcount: '517'
ht-degree: 0%

---

# 사이트 맵 및 검색 엔진 로봇 추가

`sitemap.xml` 파일을 생성하여 루트 디렉터리에 쓰려고 하면 다음 오류가 발생합니다.

```terminal
Please make sure that "/" is writable by the web-server.
```

클라우드 인프라의 Adobe Commerce을 사용하면 `var`, `pub/media`, `pub/static` 또는 `app/etc`과 같은 특정 디렉터리에만 쓸 수 있습니다. 관리 패널을 사용하여 `sitemap.xml` 파일을 생성할 때 `/media/` 경로를 지정해야 합니다.

`robots.txt` 콘텐츠를 요청 시 생성하여 데이터베이스에 저장하므로 `robots.txt` 파일을 생성할 필요가 없습니다. `<domain.your.project>/robots.txt` 또는 `<domain.your.project>/robots` 링크를 사용하여 브라우저에서 콘텐츠를 볼 수 있습니다.

이렇게 하려면 업데이트된 `.magento.app.yaml` 파일이 있는 ECE-Tools 버전 2002.0.12 이상이 필요합니다. [magento-cloud 저장소](https://github.com/magento/magento-cloud/blob/master/.magento.app.yaml#L43-L49)에서 이러한 규칙의 예제를 참조하십시오.

**버전 2.2 이상에서 `sitemap.xml` 파일을 생성하려면**:

1. 관리자에 액세스합니다.
1. _마케팅_ 메뉴의 _SEO 및 검색_ 섹션에서 **사이트 맵**&#x200B;을 클릭합니다.
1. _사이트 맵_ 보기에서 **사이트 맵 추가**&#x200B;를 클릭합니다.
1. _새 사이트 맵_ 보기에서 다음 값을 입력하십시오.

   - **파일 이름**:`sitemap.xml`
   - **경로**:`/media/`

1. **저장 및 생성**&#x200B;을 클릭합니다. 새 사이트 맵은 _사이트 맵_ 그리드에서 사용할 수 있습니다.
1. _Google 링크_ 열의 경로를 클릭합니다.

**콘텐츠를 `robots.txt` 파일에 추가하려면**:

1. 관리자에 액세스합니다.
1. _콘텐츠_ 메뉴의 _디자인_ 섹션에서 **구성**&#x200B;을 클릭합니다.
1. _디자인 구성_ 보기에서 _작업_ 열의 웹 사이트에 대해 **편집**&#x200B;을 클릭합니다.
1. _기본 웹 사이트_ 보기에서 **검색 엔진 로봇**&#x200B;을 클릭합니다.
1. **robots.txt의 사용자 지정 명령 편집** 필드를 업데이트합니다.
1. **구성 저장**&#x200B;을 클릭합니다.
1. 브라우저에서 `<domain.your.project>/robots.txt` 파일 또는 `<domain.your.project>/robots` URL을 확인하십시오.

>[!NOTE]
>
>`<domain.your.project>/robots.txt` 파일에서 `404 error`이(가) 생성되면 [Adobe Commerce 지원 티켓을 제출](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)하여 `/robots.txt`에서 `/media/robots.txt`(으)로 리디렉션을 제거합니다.

## Fastly VCL 코드 조각을 사용하여 다시 작성

도메인이 다르고 별도의 사이트 맵이 필요한 경우 VCL을 만들어 적절한 사이트 맵으로 라우팅할 수 있습니다. 위에서 설명한 대로 관리 패널에서 `sitemap.xml` 파일을 생성한 다음 사용자 지정 Fastly VCL 코드 조각을 만들어 리디렉션을 관리합니다. [사용자 지정 Fastly VCL 코드 조각](../cdn/fastly-vcl-custom-snippets.md)을 참조하세요.

>[!NOTE]
>
> 관리 UI에서 또는 Fastly API를 사용하여 사용자 지정 VCL 조각을 업로드할 수 있습니다. [사용자 지정 VCL 코드 조각 예제와 튜토리얼](../cdn/fastly-vcl-custom-snippets.md#example-vcl-snippet-code)을 참조하세요.

### 리디렉션용 Fastly VCL 코드 조각 사용

`type` 및 `content` 키-값 쌍을 사용하여 `sitemap.xml`에서 `/media/sitemap.xml`로의 경로를 다시 쓰는 사용자 지정 VCL 코드 조각을 만드십시오.

```json
{
  "name": "sitemapxml_rewrite",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path ~ \"^/?sitemap.xml$\" ) { set req.url = \"/media/sitemap.xml\"; }"
}
```

다음 예제에서는 `robots.txt` 및 `sitemap.xml`의 경로를 `/media/robots.txt` 및 `/media/sitemap.xml`에 다시 쓰는 방법을 보여 줍니다

```json
{
  "name": "sitemaprobots_rewrite",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path ~ \"^/?sitemap.xml$\" ) { set req.url = \"/media/sitemap.xml\"; } else if (req.url.path ~ \"^/?robots.txt$\") { set req.url = \"/media/robots.txt\";}"
}
```

**특정 도메인 리디렉션에 대해 Fastly VCL 코드 조각을 사용하려면**:

도메인이 `domain.com`인 `pub/media/domain_robots.txt` 파일을 만들고 다음 VCL 코드 조각을 사용합니다.

```json
{
  "name": "domain_robots",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path == \"/robots.txt\" ) { if ( req.http.host ~ \"(domain).com$\" ) { set req.url = \"/media/\" re.group.1 \"_robots.txt\"; }}"
}
```

VCL 코드 조각은 `http://domain.com/robots.txt`을(를) 라우팅하고 `pub/media/domain_robots.txt` 파일을 제공합니다.

단일 코드 조각에서 `robots.txt` 및 `sitemap.xml`에 대한 리디렉션을 구성하려면 `pub/media/domain_robots.txt` 및 `pub/media/domain_sitemap.xml` 파일을 만드십시오. 여기서 도메인은 `domain.com`이고 다음 VCL 코드 조각을 사용하십시오.

```json
{
  "name": "domain_sitemaprobots",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path == \"/robots.txt\" ) { if ( req.http.host ~ \"(domain).com$\" ) { set req.url = \"/media/\" re.group.1 \"_robots.txt\"; }} else if ( req.url.path == \"/sitemap.xml\" ) { if ( req.http.host ~ \"(domain).com$\" ) {  set req.url = \"/media/\" re.group.1 \"_sitemap.xml\"; }}"
}
```

`sitemap` 관리자 구성에서 `/`이(가) 아닌 `pub/media/`을(를) 사용하여 파일의 위치를 지정해야 합니다.

### 검색 엔진별 색인화 구성

`robots.txt` 사용자 지정을 활성화하려면 프로젝트 설정에서 `<environment-name>`**에 대해 검색 엔진별**&#x200B;색인화가 켜져 있음 옵션을 활성화해야 합니다.

![환경을 관리하려면 [!DNL Cloud Console]을(를) 사용](../../assets/robots-indexing-by-search-engine.png)

>[!NOTE]
>
>PWA Studio을 사용하고 있는데 구성된 `robots.txt` 파일에 액세스할 수 없는 경우 **스토어** > 구성 > **일반** > **웹** > 상향 PWA 구성에서 [전면 허용 목록 이름](https://github.com/magento/magento2-upward-connector#front-name-allowlist)에 `robots.txt`을(를) 추가하십시오.
