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

을(를) 생성하고 작성하려는 시도 `sitemap.xml` 파일을 루트 디렉토리로 이동하면 다음 오류가 발생합니다.

```terminal
Please make sure that "/" is writable by the web-server.
```

클라우드 인프라의 Adobe Commerce을 사용하면 다음과 같은 특정 디렉터리에만 쓸 수 있습니다. `var`, `pub/media`, `pub/static`, 또는 `app/etc`. 다음을 생성하는 경우 `sitemap.xml` 관리 패널을 사용하는 파일에서는 `/media/` 경로.

다음을 생성하지 않아도 됩니다. `robots.txt` 파일을 생성합니다. `robots.txt` on-demand에 컨텐츠를 저장하고 데이터베이스에 저장합니다. 를 사용하여 브라우저에서 콘텐츠를 볼 수 있습니다. `<domain.your.project>/robots.txt` 또는 `<domain.your.project>/robots` 링크를 클릭합니다.

이를 위해서는 ECE-Tools 버전 2002.0.12 이상(업데이트 포함)이 필요합니다 `.magento.app.yaml` 파일. 에서 이러한 규칙의 예를 참조하십시오. [magento-cloud 저장소](https://github.com/magento/magento-cloud/blob/master/.magento.app.yaml#L43-L49).

**을(를) 생성하려면 `sitemap.xml` 버전 2.2 이상의 파일**:

1. 관리자에 액세스합니다.
1. 다음에서 _마케팅_ 메뉴, 클릭 **사이트 맵** 다음에서 _SEO 및 검색_ 섹션.
1. 다음에서 _사이트 맵_ 보기, 클릭 **사이트 맵 추가**.
1. 다음에서 _새 사이트 맵_ 보려면 다음 값을 입력합니다.

   - **파일 이름**:`sitemap.xml`
   - **경로**:`/media/`

1. 클릭 **저장 및 생성**. 새 사이트 맵은에서 사용할 수 있습니다. _사이트 맵_ 그리드.
1. 에서 경로를 클릭합니다. _Google 링크_ 열.

**에 컨텐츠를 추가하려면 `robots.txt` 파일**:

1. 관리자에 액세스합니다.
1. 다음에서 _콘텐츠_ 메뉴, 클릭 **구성** 다음에서 _디자인_ 섹션.
1. 다음에서 _디자인 구성_ 보기, 클릭 **편집** 웹 사이트용 _작업_ 열.
1. 다음에서 _기본 웹 사이트_ 보기, 클릭 **검색 엔진 로봇**.
1. 업데이트 **robots.txt의 사용자 정의 명령 편집** 필드.
1. 클릭 **구성 저장**.
1. 확인 `<domain.your.project>/robots.txt` 파일 또는 `<domain.your.project>/robots` 브라우저의 URL입니다.

>[!NOTE]
>
>다음과 같은 경우 `<domain.your.project>/robots.txt` 파일이 다음을 생성합니다. `404 error`, [Adobe Commerce 지원 티켓 제출](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) 에서 리디렉션을 제거하려면 `/robots.txt` 끝 `/media/robots.txt`.

## Fastly VCL 코드 조각을 사용하여 다시 작성

도메인이 다르고 별도의 사이트 맵이 필요한 경우 VCL을 만들어 적절한 사이트 맵으로 라우팅할 수 있습니다. 생성 `sitemap.xml` 위에서 설명한 대로 관리 패널에 있는 파일을 만든 다음 사용자 지정 Fastly VCL 코드 조각을 만들어 리디렉션을 관리합니다. 다음을 참조하십시오 [사용자 정의 Fastly VCL 스니펫](../cdn/fastly-vcl-custom-snippets.md).

>[!NOTE]
>
> 관리 UI에서 또는 Fastly API를 사용하여 사용자 지정 VCL 조각을 업로드할 수 있습니다. 다음을 참조하십시오 [사용자 지정 VCL 코드 조각 예 및 자습서](../cdn/fastly-vcl-custom-snippets.md#example-vcl-snippet-code).

### 리디렉션용 Fastly VCL 코드 조각 사용

사용자 지정 VCL 코드 조각을 만들어 경로 다시 작성 `sitemap.xml` 끝 `/media/sitemap.xml` 사용 `type` 및 `content` 키-값 쌍입니다.

```json
{
  "name": "sitemapxml_rewrite",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path ~ \"^/?sitemap.xml$\" ) { set req.url = \"/media/sitemap.xml\"; }"
}
```

다음 예제는 의 경로를 다시 작성하는 방법을 보여 줍니다 `robots.txt` 및 `sitemap.xml` 끝 `/media/robots.txt` 및 `/media/sitemap.xml`

```json
{
  "name": "sitemaprobots_rewrite",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path ~ \"^/?sitemap.xml$\" ) { set req.url = \"/media/sitemap.xml\"; } else if (req.url.path ~ \"^/?robots.txt$\") { set req.url = \"/media/robots.txt\";}"
}
```

**특정 도메인 리디렉션에 Fastly VCL 코드 조각을 사용하려면**:

만들기 `pub/media/domain_robots.txt` 파일. 여기서 도메인은 입니다. `domain.com`를 참조하고 다음 VCL 코드 조각을 사용하십시오.

```json
{
  "name": "domain_robots",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path == \"/robots.txt\" ) { if ( req.http.host ~ \"(domain).com$\" ) { set req.url = \"/media/\" re.group.1 \"_robots.txt\"; }}"
}
```

VCL 코드 조각 경로 `http://domain.com/robots.txt` 및 을(를) 제공합니다. `pub/media/domain_robots.txt` 파일.

다음에 대한 리디렉션을 구성하려면 `robots.txt` 및 `sitemap.xml` 단일 스니펫에서 `pub/media/domain_robots.txt` 및 `pub/media/domain_sitemap.xml` 파일. 여기서 도메인은 입니다. `domain.com` 다음 VCL 코드 조각을 사용하십시오.

```json
{
  "name": "domain_sitemaprobots",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path == \"/robots.txt\" ) { if ( req.http.host ~ \"(domain).com$\" ) { set req.url = \"/media/\" re.group.1 \"_robots.txt\"; }} else if ( req.url.path == \"/sitemap.xml\" ) { if ( req.http.host ~ \"(domain).com$\" ) {  set req.url = \"/media/\" re.group.1 \"_sitemap.xml\"; }}"
}
```

다음에서 `sitemap` 관리자 구성에서 다음을 사용하여 파일의 위치를 지정해야 합니다. `pub/media/` 보다 `/`.

### 검색 엔진별 색인화 구성

활성화하려면 `robots.txt` 사용자 지정을 사용하려면 다음을 활성화해야 합니다 **다음에 대해 검색 엔진별 색인화가 켜져 있음`<environment-name>`** 옵션을 선택할 수 있습니다.

![사용 [!DNL Cloud Console] 환경 관리](../../assets/robots-indexing-by-search-engine.png)

>[!NOTE]
>
>PWA Studio을 사용 중인데 구성된 을(를) 액세스할 수 없는 경우 `robots.txt` 파일, 추가 `robots.txt` (으)로 [전방 이름 허용 목록](https://github.com/magento/magento2-upward-connector#front-name-allowlist) 위치: **스토어** > 구성 > **일반** > **웹** > 상향 PWA 구성
