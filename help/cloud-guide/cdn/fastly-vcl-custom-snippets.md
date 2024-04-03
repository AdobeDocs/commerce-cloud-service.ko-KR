---
title: 사용자 지정 VCL 코드 조각 시작
description: Varnish Control Language 코드 조각을 사용하여 Adobe Commerce에 대한 Fastly 서비스 구성을 맞춤화하는 방법에 대해 알아봅니다.
feature: Cloud, Configuration, Services
exl-id: df0f0906-8ffa-41a1-a31c-d36deb5a6a31
source-git-commit: 89c57a486545d6165e0407f913f4fa4cf6c95abe
workflow-type: tm+mt
source-wordcount: '1947'
ht-degree: 0%

---

# 사용자 정의 VCL 시작하기

Fastly는 사용자 요구 사항에 맞게 Fastly 서비스 구성을 맞춤화하기 위해 사용자 지정된 버전의 Vcl(Varnish Configuration Language)을 지원합니다.

사용자 지정 VCL 코드 조각은 Adobe Commerce 사이트에 업로드된 활성 VCL 버전에 추가된 VCL 로직의 블록입니다. 사용자 지정 VCL 코드 조각은 Fastly 캐싱 서비스가 요청 트래픽에 응답하는 방식을 수정합니다. 예를 들어 사용자 지정 VCL 코드 조각을 추가하여 지정된 클라이언트 IP 주소의 요청 트래픽만 허용할 수 있습니다. 또는 Adobe Commerce 사이트로 참조 스팸을 보내는 것으로 알려진 웹 사이트의 트래픽을 차단하는 코드 조각을 만듭니다.

사용자 정의 VCL 조각(생성, 컴파일 및 모든 Fastly 캐시에 전송)은 서버 다운타임 없이 로드 및 활성화됩니다.

>[!NOTE]
>
>사용자 지정 VCL 코드, Edge 사전 및 ACL을 Fastly 모듈 구성에 추가하기 전에 Fastly 캐싱 서비스가 기본 구성에서 작동하는지 확인하십시오. 다음을 참조하십시오 [Fastly 서비스 구성](fastly-configuration.md).

Fastly는 두 가지 유형의 사용자 지정 VCL 코드 조각을 지원합니다.

- [일반 코드 조각](https://docs.fastly.com/en/guides/about-vcl-snippets)- 사용자 정의 일반 VCL 조각은 특정 VCL 버전에 대해 코딩됩니다. Admin 또는 Fastly API에서 일반 VCL 조각을 생성, 수정 및 배포할 수 있습니다.

- [동적 스니펫](https://docs.fastly.com/en/guides/using-dynamic-vcl-snippets)- Fastly API를 사용하여 생성된 VCL 코드 조각. 서비스에 대한 Fastly VCL 버전을 업데이트하지 않고도 동적 스니펫을 수정하고 배포할 수 있습니다.

Edge 사전 및 ACL(액세스 제어 목록)과 함께 사용자 지정 VCL 코드 조각을 사용하여 사용자 지정 코드에 사용되는 데이터를 저장하는 것이 좋습니다.

- [**가장자리 사전**](https://docs.fastly.com/guides/edge-dictionaries/about-edge-dictionaries)- 사용자 지정 VCL 코드 조각에서 참조할 수 있는 사전 컨테이너에 키-값 쌍으로 데이터를 저장합니다

- [**Edge ACL**](https://docs.fastly.com/guides/access-control-lists/about-acls)- 사용자 지정 VCL 코드 조각을 사용하여 구현된 블록 또는 허용 규칙에 대한 액세스 제어 목록을 정의하는 클라이언트 IP 주소 데이터를 저장합니다

딕셔너리 및 ACL 데이터는 네트워크 지역에서 액세스할 수 있는 Fastly Edge 노드에 배포됩니다. 또한 스테이징 또는 프로덕션 환경에 맞게 VCL 코드를 다시 배포하지 않고도 네트워크를 통해 데이터를 동적으로 업데이트할 수 있습니다.

>[!NOTE]
>
>다음을 보유한 경우에만 사용자 지정 VCL 코드 조각을 스테이징 또는 프로덕션 환경에 추가할 수 있습니다. [구성된 Fastly 서비스](fastly-configuration.md) 해당 환경을 위해

## 튜토리얼

이 자습서와 예제는 Edge 사전 및 Edge ACL과 함께 일반 사용자 지정 VCL 조각을 사용하여 Adobe Commerce에 대한 Fastly 서비스 구성을 사용자 지정하는 방법을 보여줍니다. 자세한 내용은 Fastly 설명서를 참조하십시오.

- [Fastly VCL 가이드](https://docs.fastly.com/guides/vcl/guide-to-vcl)- Fastly Varnish 구현, Fastly VCL 확장 및 Varnish 및 VCL에 대한 자세한 내용을 알아보기 위한 리소스에 대한 정보입니다.
- [Fastly VCL 참조](https://docs.fastly.com/guides/vcl/)- Fastly 사용자 지정 VCL 및 사용자 지정 VCL 코드 조각을 개발하고 문제를 해결하기 위한 자세한 프로그래밍 참조입니다.

Adobe Commerce 관리자 또는 Fastly API를 사용하여 사용자 지정 VCL 코드 조각을 만들고 관리할 수 있습니다.

- [Adobe Commerce 관리자](#manage-custom-vcl-from-admin)—VCL 변경 사항을 Fastly 서비스 구성에 확인, 업로드 및 적용하는 프로세스를 자동화하므로 Adobe Commerce 관리를 사용하여 사용자 지정 VCL 조각을 관리하는 것이 좋습니다. 또한 관리자에서 Fastly 서비스 구성에 추가된 사용자 지정 VCL 조각을 보고 편집할 수 있습니다.

- [Fastly API](#manage-vcl-using-the-api)- 관리자에 액세스할 수 없는 경우 Fastly API를 사용하여 사용자 지정 VCL 코드 조각을 관리합니다. 예를 들어 API를 사용하여 사이트가 다운될 때 Fastly 서비스 구성 문제를 해결하거나 사용자 지정 VCL 코드 조각을 추가합니다. 또한 일부 작업은 API를 사용해서만 완료할 수 있습니다. 예를 들어 이전 VCL 버전을 다시 활성화하거나 지정된 VCL 버전에 포함된 모든 VCL 조각을 보려면 API를 사용해야 합니다. 다음을 참조하십시오 [VCL 코드 조각에 대한 API 빠른 참조](#api-quick-reference-for-vcl-snippets).

### 예제 VCL 코드 조각

다음 예는 클라이언트 IP 주소별로 트래픽을 필터링하는 사용자 지정 VCL 코드 조각(JSON 형식)을 보여 줍니다.

```json
{
  "service_id": "FASTLY_SERVICE_ID",
  "version": "{Editable Version #}",
  "name": "apply_acl",
  "priority": "100",
  "dynamic": "1",
  "type": "hit",
  "content": "if ((client.ip ~ {ACLNAME}) && !req.http.Fastly-FF){ error 403; }"
}
```

>[!WARNING]
>
>이 예에서 VCL 코드는 파일에 저장하고 Fastly API 요청에 제출할 수 있는 JSON 페이로드로 포맷됩니다. API 요청에 대해 JSON으로 코드 조각을 전송할 때 JSON 유효성 검사 오류를 방지하려면 백슬래시를 사용하여 코드의 특수 문자를 이스케이프 처리합니다. 다음을 참조하십시오 [동적 VCL 코드 조각 사용](https://docs.fastly.com/vcl/vcl-snippets/) Fastly VCL 설명서에서 Admin에서 VCL 코드 조각을 제출하는 경우 특수 문자를 이스케이프 처리할 필요가 없습니다.

의 VCL 논리 `content` 필드는 다음 작업을 수행합니다.

- 수신 IP 주소 확인, `client.ip` 각 요청에 대해

- 에 포함된 IP 주소가 있는 모든 요청을 차단합니다. *ACLNAME* edge ACL, 반환 `403 Forbidden` 오류

다음 표는 사용자 지정 VCL 조각의 주요 데이터에 대한 세부 정보를 제공합니다. 자세한 참조는 [VCL 코드 조각](https://docs.fastly.com/api/config#api-section-snippet) Fastly 설명서에서 를 참조하십시오.

| 값 | 설명 |
|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `API_KEY` | Fastly 계정에 액세스하기 위한 API 키. 다음을 참조하십시오 [자격 증명 가져오기](fastly-configuration.md). |
| `active` | 코드 조각 또는 버전의 활성 상태입니다. 반환 `true` 또는 `false`. true인 경우 코드 조각 또는 버전이 사용 중입니다. 버전 번호를 사용하여 활성 코드 조각을 복제합니다. |
| `content` | 실행할 VCL 코드 조각입니다. Fastly는 모든 VCL 언어 기능을 지원하지 않습니다. 또한 Fastly는 사용자 지정 기능을 갖춘 확장을 제공합니다. 지원되는 기능에 대한 자세한 내용은 [Fastly VCL 프로그래밍 참조](https://docs.fastly.com/vcl/reference/). |
| `dynamic` | 코드 조각의 동적 상태. 반환 `false` 대상 [일반 코드 조각](https://docs.fastly.com/en/guides/about-vcl-snippets) Fastly 서비스 구성을 위한 버전 관리 VCL에 포함됩니다. 반환 `true` 용 [동적 코드 조각](https://docs.fastly.com/vcl/vcl-snippets/using-dynamic-vcl-snippets/) 새 VCL 버전 없이 수정 및 배포할 수 있습니다. |
| `number` | 코드 조각이 포함된 VCL 버전 번호입니다. Fastly 사용 *편집 가능한 버전 번호* 예제에 포함되었습니다. API에서 사용자 지정 코드 조각을 추가하는 경우 API 요청에 버전 번호를 포함하십시오. 관리자에서 사용자 정의 VCL을 추가하는 경우 버전이 제공됩니다. |
| `priority` | 숫자 값 `1` 끝 `100` 사용자 지정 VCL 코드 조각 코드가 실행되는 시점을 지정합니다. 우선 순위 값이 낮은 코드 조각이 먼저 실행됩니다. 지정하지 않으면 `priority` 기본값은 입니다. `100`.<p>우선 순위 값이 인 모든 사용자 지정 VCL 코드 조각 `5` 즉시 실행되며, 이는 요청 라우팅(블록 및 허용 목록과 리디렉션)을 구현하는 VCL 코드에 가장 적합합니다. 우선 순위 `100` 는 기본 VCL 코드 조각을 재정의하는 데 가장 적합합니다.<p>모두 [기본 VCL 코드 조각](fastly-configuration.md#upload-vcl-snippets) Magento-Fastly 모듈에 포함된 `priority=50`.<ul><li>다음과 같이 높은 우선 순위 할당 `100` 다른 모든 VCL 함수 다음에 사용자 지정 VCL 코드를 실행하고 기본 VCL 코드를 재정의합니다.</li></ul> |
| `service_id` | 특정 스테이징 또는 프로덕션 환경에 대한 Fastly 서비스 ID입니다. 이 ID는 프로젝트가 클라우드 인프라의 Adobe Commerce에 추가될 때 할당됩니다 [Fastly 서비스 계정](fastly.md#fastly-service-account-and-credentials). |
| `type` | 생성된 코드 조각을 삽입할 위치를 지정합니다. 예: `init` (하위 루틴 위) 및 `recv` (하위 루틴 내). 자세한 내용은 Fastly 를 참조하십시오. [VCL 코드 조각](https://docs.fastly.com/api/config#api-section-snippet) 참조. |

## 관리자로부터 사용자 지정 VCL 관리

다음을 수행할 수 있습니다. [사용자 지정 VCL 코드 조각 추가](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/CUSTOM-VCL-SNIPPETS.md) 다음에서 *Fastly 구성* > *사용자 지정 VCL 코드 조각* 섹션에 있는 마지막 항목이 될 필요가 없습니다.

![사용자 지정 VCL 코드 조각 관리](../../assets/cdn/fastly-edit-snippets.png)

다음 *사용자 지정 VCL 코드 조각* 보기는 관리자를 통해 추가된 스니펫만 표시합니다. Fastly API를 사용하여 스니펫을 추가하는 경우 API를 사용하여 다음을 수행합니다 [관리](#manage-vcl-using-the-api).

다음 예에서는 관리자로부터 사용자 지정 VCL 조각을 생성 및 관리하고 Fastly Edge 모듈 및 Edge 사전을 사용하는 방법을 보여 줍니다.

- [CMS 백엔드로 요청 재라우팅](fastly-vcl-wordpress.md)
- [참조 스팸 차단](fastly-vcl-badreferer.md)
- [참조 스팸 차단](fastly-vcl-badreferer.md)
- [허용 목록에 추가하다 IP vcl에 대한 사용자 정의 VCL](fastly-vcl-allowlist.md)
- [차단 목록에 추가하다 IP vcl에 대한 사용자 정의 VCL](fastly-vcl-blocking.md)
- [Fastly 캐시 우회](fastly-vcl-bypass-to-origin.md)

## API를 사용하여 VCL 관리

다음 연습에서는 일반 VCL 코드 조각 파일을 만들고 Fastly API를 사용하여 Fastly 서비스 구성에 추가하는 방법을 보여줍니다. 에서 코드 조각을 만들고 관리할 수 있습니다. *터미널* 응용 프로그램. 특정 환경에 SSH 연결이 필요하지 않습니다.

**사전 요구 사항:**

- Fastly 서비스를 위해 클라우드 인프라 환경에서 Adobe Commerce을 구성합니다. 다음을 참조하십시오 [Fastly 설정](fastly-configuration.md).

- [Fastly API 자격 증명 가져오기](fastly-configuration.md) Fastly API에 대한 요청을 인증합니다. 올바른 환경(스테이징 또는 프로덕션)에 대한 자격 증명을 가져왔는지 확인합니다.

- Fastly 서비스 자격 증명을 cURL 명령에 사용할 수 있는 기본 환경 변수로 저장합니다.

  ```bash
  export FASTLY_SERVICE_ID=<Service-ID>
  ```

  ```bash
  export FASTLY_API_TOKEN=<API-Token>
  ```

  내보낸 환경 변수는 현재 기본 세션에서만 사용할 수 있으며 터미널을 닫으면 손실됩니다. 새 값을 내보내어 변수를 재정의할 수 있습니다. Fastly와 관련된 내보낸 변수 목록을 보려면 다음과 같이 하십시오.

  ```bash
  export | grep FASTLY
  ```

## VCL 코드 조각 추가

이 자습서에서는 Fastly API를 사용하여 사용자 지정 스니펫을 추가하는 기본 단계를 제공합니다.

>[!NOTE]
>
>Adobe Commerce 관리자로부터 사용자 지정 VCL 코드 조각을 관리하는 방법에 대해 알아보려면 다음을 참조하십시오. [Adobe Commerce 관리자의 VCL 관리](#manage-custom-vcl-from-admin).


**전제 조건**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

### 1단계: 활성 VCL 버전 찾기

Fastly API 사용 [버전 가져오기](https://docs.fastly.com/api/config#version_dfde9093f4eb0aa2497bbfd1d9415987) 활성 VCL 버전 번호를 가져오는 작업:

```bash
curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/active
```

JSON 응답에서 반환된 활성 VCL 버전 번호를 확인합니다. `number` 키, 예 `"number": 99`. 편집할 VCL을 복제할 때 버전 번호가 필요합니다.

```json
{
  "testing": false,
  "locked": true,
  "number": 99,
  "active": true,
  "service_id": "872zhjyxhto5SIRb3GAE0",
  "staging": false,
  "created_at": "2019-01-29T22:38:53Z",
  "deleted_at": null,
  "comment": "Magento Module uploaded VCL",
  "updated_at": "2019-01-29T22:39:06Z",
  "deployed": false
}
```

후속 API 요청에 사용할 기본 환경 변수에 활성 버전 번호를 저장합니다.

```bash
export FASTLY_VERSION_ACTIVE=<Version>
```

### 2단계: 활성 VCL 버전 및 모든 코드 조각 복제

사용자 지정 VCL 조각을 추가하거나 수정하려면 먼저 편집할 활성 VCL 버전의 복사본을 생성해야 합니다. Fastly API 사용 [복제](https://docs.fastly.com/api/config#version_7f4937d0663a27fbb765820d4c76c709) 작업:

```bash
curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_VERSION_ACTIVE/clone -X PUT
```

JSON 응답에서 버전 번호가 증가하며 *활성* 키 값: `false`. 새 비활성 VCL 버전을 로컬로 수정할 수 있습니다.

```json
{
  "testing": false,
  "locked": false,
  "number": 100,
  "active": false,
  "service_id": "vW2bLFWhhto5SIRb3GAE0",
  "staging": false,
  "created_at": "2019-01-29T22:38:53Z",
  "deleted_at": null,
  "comment": "Magento Module uploaded VCL",
  "updated_at": "2019-01-29T22:39:06Z",
  "deployed": false
}
```

후속 명령에 사용할 수 있도록 기본 환경 변수에 새 버전 번호를 저장합니다.

```bash
export FASTLY_EDIT_VERSION=<Version>
```

### 3단계: 사용자 지정 VCL 코드 조각 만들기

다음 콘텐츠 및 형식을 사용하여 JSON 파일에 사용자 지정 VCL 코드를 만들고 저장합니다.

```json
{
  "name": "<name>",
  "dynamic": "0",
  "type": "<type>",
  "priority": "100",
  "content": "<code all in one line>"
}
```

값에는 다음이 포함됩니다.

- `name`- VCL 코드 조각의 이름입니다.

- `dynamic`- 이 항목이 [일반 코드 조각](https://docs.fastly.com/en/guides/about-vcl-snippets) 또는 [동적 코드 조각](https://docs.fastly.com/guides/vcl-snippets/using-dynamic-vcl-snippets).

- `type`- 다음과 같이 생성된 코드 조각을 삽입할 위치를 지정합니다. `init` (하위 루틴 위) 및 `recv` (하위 루틴 내). 다음을 참조하십시오 [Fastly VCL 코드 조각 개체 값](https://docs.fastly.com/api/config#snippet) 을 참조하십시오.

- `priority`- 값 `1` 끝 `100` 사용자 지정 VCL 코드 조각 코드가 실행되는 시기를 결정합니다. 값이 낮은 사용자 지정 VCL 코드 조각이 먼저 실행됩니다.

  Fastly VCL 모듈의 모든 기본 VCL 코드에는 `priority` / `50`. 작업을 마지막으로 수행하거나 기본 VCL 코드를 재정의하려면 다음과 같이 높은 숫자를 사용합니다. `100`. 사용자 지정 VCL 코드 조각을 즉시 실행하려면 우선 순위를 낮은 값으로 설정합니다. 예: `5`.

- `content`- 줄 바꿈 없이 한 줄로 실행되는 VCL 코드 조각입니다. 다음을 참조하십시오 [사용자 지정 VCL 코드 조각 예](#example-vcl-snippet-code).

### 4단계: Fastly 구성에 VCL 코드 조각 추가

Fastly API 사용 [코드 조각 만들기](https://docs.fastly.com/api/config#snippet_41e0e11c662d4d56adada215e707f30d) 사용자 지정 VCL 코드 조각을 VCL 버전에 추가하는 작업입니다.

```bash
curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_EDIT_VERSION/snippet -H 'Content-Type: application/json' -X POST --data @<filename.json>
```

다음 `<filename.json>` 는 이전 단계에서 준비한 파일의 이름입니다. 각 VCL 스니펫에 대해 이 명령을 반복합니다.

받은 경우 `500 Internal Server Error` fastly 서비스의 응답에서 JSON 파일 구문을 확인하여 유효한 파일을 업로드하고 있는지 확인하십시오.

### 5단계: 사용자 지정 VCL 코드 조각의 유효성 검사 및 활성화

사용자 지정 VCL 코드 조각을 추가한 후 는 편집 중인 VCL 버전에 코드 조각을 삽입합니다. 변경 사항을 적용하려면 다음 단계를 완료하여 VCL 코드 조각의 유효성을 검사하고 VCL 버전을 활성화합니다.

1. Fastly API 사용 [vcl 버전 유효성 검사](https://docs.fastly.com/api/config#version_97f8cf7bfd5dc2e5ea1933d94dc5a9a6) 업데이트된 VCL 코드를 확인하는 작업입니다.

   ```bash
   curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_EDIT_VERSION/validate
   ```

   Fastly API가 오류를 반환하는 경우 문제를 수정하고 업데이트된 VCL 버전을 다시 확인합니다.

1. Fastly API 사용 [활성화](https://docs.fastly.com/api/config#version_0b79ae1ba6aee61d64cc4d43fed1e0d5) 새 VCL 버전 활성화 작업

   ```bash
   curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_EDIT_VERSION/activate -X PUT
   ```

## VCL 코드 조각에 대한 API 빠른 참조

이러한 API 요청 예는 내보낸 환경 변수를 사용하여 Fastly로 인증할 자격 증명을 제공합니다. 이러한 명령에 대한 자세한 내용은 [Fastly API 참조](https://docs.fastly.com/api/config#vcl).

>[!NOTE]
>
>Fastly API를 사용하여 추가한 코드 조각을 관리하려면 다음 명령을 사용합니다. 책임자로부터 스니펫을 추가한 경우 [관리자를 사용하여 VCL 조각 관리](#manage-vcl-using-the-api).

- **활성 VCL 버전 번호 가져오기**

  ```bash
  curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/active
  ```

- **서비스에 연결된 모든 일반 VCL 코드 조각 나열**

  ```bash
  curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_VERSION/snippet
  ```

- **개별 코드 조각 검토**

  ```bash
  curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_VERSION/snippet/<snippet_name>
  ```

  다음 `<snippet_name>` 는 다음과 같은 코드 조각의 이름입니다. `my_regular_snippet`.

- **코드 조각 업데이트**

  수정 [준비된 JSON 파일](#step-3-create-a-custom-vcl-snippet) 및 다음 요청을 보냅니다.

  ```bash
  curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_VERSION/snippet/<snippet_name> -H 'Content-Type: application/json' -X PUT --data @<filename.json>
  ```

- **개별 VCL 코드 조각 삭제**

  코드 조각 목록 가져오기 및 사용 `curl` 삭제할 특정 코드 조각 이름을 가진 명령:

  ```bash
  curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_VERSION/snippet/<snippet_name> -X DELETE
  ```

- **에서 값 재정의 [기본 Fastly VCL 코드](https://github.com/fastly/fastly-magento2/tree/master/etc/vcl_snippets)**

  업데이트된 값으로 코드 조각을 만들고 우선 순위 할당 `100`.
