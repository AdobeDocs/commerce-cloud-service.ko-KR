---
title: 리디렉션
description: Adobe Commerce on cloud infrastructure 프로젝트에 대한 리디렉션 규칙을 관리하는 방법을 알아봅니다.
feature: Cloud, Routes
exl-id: 7089a790-6341-4443-990a-df42091f0680
source-git-commit: 649c11b111aa9c9105e54908bf9c6f48741f10e4
workflow-type: tm+mt
source-wordcount: '646'
ht-degree: 0%

---

# 리디렉션

리디렉션 규칙 관리는 웹 애플리케이션의 일반적인 요구 사항이며, 특히 시간이 지남에 따라 변경되거나 제거된 수신 링크를 잃지 않으려는 경우에 유용합니다.

다음은 를 사용하여 클라우드 인프라 프로젝트에서 Adobe Commerce의 리디렉션 규칙을 관리하는 방법을 보여 줍니다. `routes.yaml` 구성 파일입니다. 이 항목에서 설명한 리디렉션 메서드가 작동하지 않는 경우 캐싱 헤더를 사용하여 동일한 작업을 수행할 수 있습니다.

{{route-placeholder}}

## Pro 환경 업데이트

{{pro-self-service-warning}}

>[!WARNING]
>
>클라우드 인프라 프로젝트의 Adobe Commerce에 대해 는에서 regex가 아닌 수많은 리디렉션 및 재작성을 구성합니다. `routes.yaml` 파일이 성능 문제를 일으킬 수 있습니다. 다음의 경우 `routes.yaml` 파일은 32KB 이상이며, regex가 아닌 리디렉션 및 재작성을 Fastly로 오프로드합니다. 다음을 참조하십시오 [Nginx(경로) 대신 Fastly로 리디렉션되지 않는 리디렉션 오프로드](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/offload-non-regex-redirects-to-fastly-instead-of-nginx-routes.html) 다음에서 _Adobe Commerce 도움말 센터_.

## 전체 경로 리디렉션

전체 경로 리디렉션을 사용하면 다음을 사용하여 간단한 경로를 정의할 수 있습니다. `routes.yaml` 파일. 예를 들어 apex 도메인에서 `www` 하위 도메인은 다음과 같습니다.

```yaml
http://{default}/:
    type: redirect
    to: http://www.{default}/
```

## 부분 경로 리디렉션

다음에서 `.magento/routes.yaml` 파일에서 패턴 일치를 기반으로 기존 경로에 부분 리디렉션 규칙을 추가할 수 있습니다.

```yaml
http://{default}/:
    redirects:
        expires: 1d
        paths:
          "/from": { to: "http://example.com/" }
          "/regexp/(.*)/matching": { to: "http://example.com/$1", regexp: true }
```

부분 리디렉션은 애플리케이션에서 직접 제공하는 경로를 포함하여 모든 유형의 경로에서 작동합니다.

아래에 두 개의 키가 있습니다. `redirects`:

- **만료**- 선택 사항이며, 브라우저에서 리디렉션을 캐시하는 시간을 지정합니다. 유효한 값의 예는 다음과 같습니다. `3600s`, `1d`, `2w`, `3m`.

- **경로**- 부분 경로 리디렉션 규칙에 대한 구성을 지정하는 하나 이상의 키-값 쌍입니다.

  각 리디렉션 규칙의 경우 키는 리디렉션을 위한 요청 경로를 필터링하는 표현식입니다. 값은 리디렉션의 대상 및 리디렉션 처리 옵션을 지정하는 개체입니다.

  값 개체에는 다음 속성이 있습니다.

  | 속성 | 설명 |
  | ---------- | ----------- |
  | `to` | 필수, 부분 절대 경로, 프로토콜 및 호스트가 있는 URL 또는 리디렉션 규칙의 대상 을 지정하는 패턴입니다. |
  | `regexp` | 선택 사항이며 기본값은 입니다. `false`. 경로 키를 PCRE 정규 표현식으로 해석할지 여부를 지정합니다. |
  | `prefix` | 리디렉션이 경로 및 모든 하위 항목에 모두 적용되는지 또는 경로 자체에만 적용되는지 여부를 지정합니다. 기본값은 입니다. `true`. 이 값은 다음과 같은 경우 지원되지 않습니다. `regexp` 은(는) `true`. |
  | `append_suffix` | 접미사가 리디렉션과 함께 전달되는지 여부를 결정합니다. 기본값은 입니다. `true`. 다음 경우에는 이 값이 지원되지 않습니다. `regexp` 키: `true` 또는*인 경우 `prefix` 키: `false`. |
  | `code` | HTTP 상태 코드를 지정합니다. 유효한 상태 코드는 다음과 같습니다. [`301` (영구적으로 이동됨)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.3.2), [`302`](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.3.3), [`307`](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.3.8), 및 [`308`](https://www.rfc-editor.org/rfc/rfc7238). 기본값은 입니다. `302`. |
  | `expires` | 선택 사항, 브라우저에서 리디렉션을 캐시하는 시간을 지정합니다. 기본값은 입니다 `expires` 아래에 직접 정의된 값 `redirects` 키. 그러나 이 수준에서 개별 부분 리디렉션에 대한 캐시 만료를 미세 조정할 수 있습니다. |

## 부분 경로 리디렉션의 예

다음 예에서는 에서 부분 경로 리디렉션을 지정하는 방법을 보여 줍니다. `routes.yaml` 다양한 파일 사용 `paths` 구성 옵션을 참조하십시오.

### 정규 표현식 패턴 일치

다음 형식을 사용하여 정규 표현식에 따라 리디렉션 요청을 구성합니다.

```yaml
http://{default}/:
    type: upstream
    redirects:
    paths:
        "/regexp/(.*)/match": { to: "http://example.com/$1", regexp: true }
```

이 구성은 정규 표현식에 대해 요청 경로를 필터링하고 일치하는 요청을 로 리디렉션합니다. `https://example.com`. 예를 들어 다음에 대한 요청 `https://example.com/regexp/a/b/c/match` (으)로 리디렉션 `https://example.com/a/b/c`.

### 접두사 패턴 일치

지정된 접두사 패턴으로 시작하는 경로에 대해 리디렉션 요청을 구성하려면 다음 형식을 사용하십시오.

```yaml
http://{default}/:
    type: upstream
    redirects:
    paths:
        "/from": { to: "https://{default}/to", prefix: true }
```

이 구성은 다음과 같이 작동합니다.

- 패턴과 일치하는 요청 리디렉션 `/from` 경로로 `http://{default}/to`.

- 패턴과 일치하는 요청 리디렉션 `/from/another/path` 끝 `https://{default}/to/another/path`.

- 을(를) 변경하면 `prefix` 다음으로 속성: `false`, 와 일치하는 요청 `/from` 패턴이 리디렉션을 트리거하지만 `/from/another/path` 패턴은 해당되지 않습니다.

### 접미사 패턴 일치

다음 형식을 사용하여 요청의 경로 접미사를 대상 대상에 추가하는 리디렉션 요청을 구성합니다.

```yaml
http://{default}/:
    type: upstream
    redirects:
    paths: "/from": { to: "https://{default}/to", append_suffix: false }
```

이 구성은 다음과 같이 작동합니다.

- 패턴과 일치하는 요청 리디렉션 `/from/path/suffix` 경로로 `https://{default}/to`.

- 을(를) 변경하면 `append_suffix` 다음으로 속성: `true`, 그런 다음 일치하는 요청 `/from/path/suffix`  경로로 리디렉션 `https://{default}/to/path/suffix`.

### 경로별 캐시 구성

특정 경로에서 리디렉션을 캐시하는 시간을 사용자 지정하려면 다음 형식을 사용하십시오.

```yaml
http://{default}/:
    type: upstream
    redirects:
    expires: 1d
    paths:
        "/from": { to: "https://example.com/" }
        "/here": { to: "https://example.com/there", expires: "2w" }
```

이 구성은 다음과 같이 작동합니다.

- 첫 번째 경로에서 리디렉션(`/from`)은 1일 동안 캐시됩니다.

- 두 번째 경로에서 리디렉션(`/here`)은 2주 동안 캐시됩니다.
