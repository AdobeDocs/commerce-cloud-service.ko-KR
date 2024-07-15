---
title: 웹 속성
description: ' [!DNL Commerce] 응용 프로그램 구성 파일에서 웹 속성을 구성하는 방법에 대한 예를 참조하십시오.'
feature: Cloud, Configuration
exl-id: 2ca94908-6fe1-42fd-bc3b-ef2dd473f1bb
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '410'
ht-degree: 0%

---

# 웹 속성

`web` 속성은 응용 프로그램이 웹에 노출되는 방법(HTTP)을 정의하고, 웹 응용 프로그램이 콘텐츠를 제공하는 방법을 결정하며, 각 위치 _block_&#x200B;에서 규칙을 설정하여 응용 프로그램 컨테이너가 들어오는 요청에 응답하는 방법을 제어합니다. 블록은 슬래시(`/`)로 이어지는 절대 경로를 나타냅니다.

```yaml
web:
    locations:
        "/":
            # The public directory of the app, relative to its root.
```

각 `locations` 블록에 대해 다음 키 값을 사용하여 `locations` 구성을 미세 조정할 수 있습니다.

| 속성 | 설명 |
| :--- | :--- |
| `allow` | &quot;규칙&quot;과 일치하지 않는 파일을 제공합니다. 기본값 = `true` |
| `expires` | 브라우저에서 콘텐츠를 캐시할 시간(초)을 설정합니다. 이 키를 사용하면 정적 콘텐츠에 대해 `cache-control` 및 `expires` 헤더를 사용할 수 있습니다. 이 값을 설정하지 않으면 정적 콘텐츠 파일을 제공할 때 `expires` 지시문 및 결과 헤더가 포함되지 않습니다. 음수 1(`-1`) 값은 캐싱을 생성하지 않으며 기본값입니다. 시간 값을 `ms`(밀리초), `s`(초), `m`(분), `h`(시간), `d`(일), `w`(주), `M`(월, 30d) 또는 `y`(년, 365d) 단위로 표현할 수 있습니다. |
| `headers` | 이 위치에서 제공되는 정적 콘텐츠에 대해 `X-Frame-Options` 같은 사용자 지정 헤더를 설정합니다. |
| `index` | 응용 프로그램을 제공할 정적 파일(예: `index.html` 파일)을 나열합니다. 이 키에는 컬렉션이 필요합니다. 이 기능은 이 위치에 대한 `allow` 또는 `rules` 키가 파일에 대한 액세스를 &quot;허용&quot;한 경우에만 작동합니다. |
| `rules` | 위치에 대한 재정의를 지정합니다. 정규 표현식을 사용하여 요청을 일치시키십시오. 들어오는 요청이 규칙과 일치하는 경우, 규칙에서 사용되는 키에 의해 요청의 일반 처리가 무시됩니다. |
| `passthru` | 정적 파일 또는 PHP 파일을 찾을 수 없는 경우에 사용할 URL을 설정합니다. 일반적으로 이 URL은 `/index.php` 또는 `/app.php`과(와) 같은 응용 프로그램의 전면 컨트롤러입니다. |
| `root` | 웹에 노출되는 애플리케이션의 루트를 기준으로 상대 경로를 설정합니다. 클라우드 프로젝트의 공용 디렉터리(위치 &quot;/&quot;)는 기본적으로 &quot;pub&quot;로 설정됩니다. |
| `scripts` | 이 위치에서 스크립트를 로드할 수 있습니다. 스크립트를 허용하려면 값을 `true`(으)로 설정하십시오. |

기본 구성에서는 다음 작업을 수행할 수 있습니다.

- 루트(`/`) 경로에서 웹 및 미디어에만 액세스할 수 있습니다.
- `~/pub/static` 및 `~/pub/media` 경로에서 모든 파일에 액세스할 수 있습니다.

다음 예제에서는 [`mounts` 속성의 항목](properties.md#mounts)과(와) 연결된 웹 액세스 가능 위치 집합에 대한 `.magento.app.yaml` 파일의 기본 구성을 보여 줍니다.

```yaml
 # The configuration of app when it is exposed to the web.
web:
    locations:
        "/":
            # The public directory of the app, relative to its root.
            root: "pub"
            # The front-controller script to send non-static requests to.
            passthru: "/index.php"
            index:
                - index.php
            expires: -1
            scripts: true
            allow: false
            rules:
                \.(css|js|map|hbs|gif|jpe?g|png|tiff|wbmp|ico|jng|bmp|svgz|midi?|mp?ga|mp2|mp3|m4a|ra|weba|3gpp?|mp4|mpe?g|mpe|ogv|mov|webm|flv|mng|asx|asf|wmv|avi|ogx|swf|jar|ttf|eot|woff|otf|html?)$:
                    allow: true
                ^/sitemap(.*)\.xml$:
                    passthru: "/media/sitemap$1.xml"
        "/media":
            root: "pub/media"
            allow: true
            scripts: false
            expires: 1y
            passthru: "/get.php"
        "/static":
            root: "pub/static"
            allow: true
            scripts: false
            expires: 1y
            passthru: "/front-static.php"
            rules:
                ^/static/version\d+/(?<resource>.*)$:
                    passthru: "/static/$resource"
```

>[!NOTE]
>
>이 예제에서는 단일 도메인을 지원하도록 구성된 클라우드 프로젝트에 대한 기본 웹 구성을 보여줍니다. 여러 웹 사이트 또는 스토어에 대한 지원이 필요한 프로젝트의 경우 공유 도메인을 지원하도록 `web` 구성을 설정해야 합니다. [공유 도메인의 위치 구성](../store/multiple-sites.md#configure-locations-for-shared-domains)을 참조하십시오.
