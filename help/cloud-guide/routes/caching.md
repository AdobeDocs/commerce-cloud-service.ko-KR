---
title: 캐싱
description: 클라우드 인프라 환경에서 Adobe Commerce에 대한 캐싱을 활성화하는 방법을 알아봅니다.
feature: Cloud, Cache, Routes
exl-id: 4856aa94-2947-4dc8-b0d1-0960869dc39c
source-git-commit: 7b9c6a4cd17069c25455195bd8f273664b8a29eb
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 0%

---

# 캐싱

클라우드 인프라 프로젝트 환경에서 캐싱을 활성화할 수 있습니다. 캐싱을 비활성화하면 Adobe Commerce이 파일을 직접 제공합니다.

{{route-placeholder}}

## 캐싱 설정

다음과 같이 `.magento/routes.yaml` 파일에서 캐시 규칙을 구성하여 응용 프로그램에 캐싱을 사용하도록 설정합니다.

```yaml
http://{default}/:
    type: upstream
    upstream: php:php
    cache:
        enabled: true
        headers: [ "Accept", "Accept-Language", "X-Language-Locale" ]
        cookies: ["*"]
        default_ttl: 60
```

## 경로 기반 캐싱

다음 예와 같이 여러 루트에 대한 캐싱 규칙을 별도로 설정하여 세분화된 캐싱을 활성화합니다.

```yaml
http://{default}/:
    type: upstream
    upstream: php:php
    cache:
        enabled: true

http://{default}/path/:
    type: upstream
    upstream: php:php
    cache:
        enabled: false

http://{default}/path/more/:
    type: upstream
    upstream: php:php
    cache:
        enabled: true
```

앞의 예제에서는 다음 경로를 캐시합니다.

- `http://{default}/`
- `http://{default}/path/more/`
- `http://{default}/path/more/etc/`

그리고 다음 경로가 캐시되었습니다. **not**:

- `http://{default}/path/`
- `http://{default}/path/etc/`

>[!NOTE]
>
>경로의 정규식이 **지원되지 않음**&#x200B;입니다.

## 캐시 기간

캐시 기간은 `Cache-Control` 응답 헤더 값에 의해 결정됩니다. 응답에 `Cache-Control` 헤더가 없으면 `default_ttl` 키가 사용됩니다.

## 캐시 키

응답을 캐시하는 방법을 결정하기 위해 Adobe Commerce은 여러 요인에 의존하는 캐시 키를 만들고 이 키와 연관된 응답을 저장합니다. 요청이 동일한 캐시 키와 함께 제공되면 응답이 재사용됩니다. 그 목적은 HTTP [`Vary` 헤더](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.44)와(과) 유사합니다.

매개 변수 `headers` 및 `cookies` 키를 사용하면 이 캐시 키를 변경할 수 있습니다.

이러한 키의 기본값은 다음과 같습니다.

```yaml
cache:
    enabled: true
    headers: ["Accept-Language", "Accept"]
    cookies: ["*"]
```

## 캐시 속성

### `enabled`

`true`(으)로 설정된 경우 이 경로에 대한 캐시를 활성화하십시오. `false`(으)로 설정된 경우 이 경로에 대한 캐시를 비활성화합니다.

### `headers`

캐시 키가 종속되어야 하는 값을 정의합니다.

예를 들어 `headers` 키가 다음과 같은 경우:

```yaml
cache:
    enabled: true
    headers: ["Accept"]
```

그러면 Adobe Commerce에서 `Accept` HTTP 헤더의 각 값에 대해 다른 응답을 캐시합니다.

### `cookies`

`cookies` 키는 캐시 키가 종속되어야 하는 값을 정의합니다.

For example:

```yaml
cache:
    enabled: true
    cookies: ["value"]
```

캐시 키는 요청의 `value` 쿠키 값에 따라 다릅니다.

`cookies` 키에 `["*"]` 값이 있는 경우 특별한 경우가 있습니다. 이 값은 쿠키가 있는 모든 요청이 캐시를 우회함을 의미합니다. 이 값은 기본값입니다.

>[!NOTE]
>
>쿠키 이름에는 와일드카드를 사용할 수 없습니다. 정확한 쿠키 이름을 사용하거나 모든 쿠키를 별표(`*`)와 일치시키십시오. 예를 들어, `SESS*` 또는 `~SESS`은(는) 현재 **not** 올바른 값입니다.

쿠키에는 다음과 같은 제한 사항이 있습니다.

- 시스템에서 최대 **50개의 쿠키**&#x200B;를 설정할 수 있습니다. 그렇지 않으면 응용 프로그램에서 `Unable to send the cookie. Maximum number of cookies would be exceeded` 예외가 발생합니다.
- 최대 쿠키 크기는 **4,096바이트**&#x200B;입니다. 그렇지 않으면 응용 프로그램에서 `Unable to send the cookie. Size of '%name' is %size bytes` 예외가 발생합니다.

### `default_ttl`

응답에 `Cache-Control` 헤더가 없으면 `default_ttl` 키를 사용하여 캐시 기간을 초 단위로 정의합니다. 기본값은 `0`입니다. 즉, 캐시된 항목이 없습니다.
