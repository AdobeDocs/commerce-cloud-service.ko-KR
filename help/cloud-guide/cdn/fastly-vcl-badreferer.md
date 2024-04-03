---
title: 참조 스팸 차단
description: Fastly Edge 사전 및 사용자 지정 VCL 코드 조각을 사용하여 사이트의 참조 스팸을 차단합니다.
feature: Cloud, Configuration, Security
exl-id: 665bac93-75db-424f-be2c-531830d0e59a
source-git-commit: 7a181af2149eef7bfaed4dd4d256b8fa19ae1dda
workflow-type: tm+mt
source-wordcount: '684'
ht-degree: 0%

---

# 참조 스팸 차단

다음 예는 를 구성하는 방법을 보여 줍니다 [Fastly Edge 사전](https://docs.fastly.com/guides/edge-dictionaries/working-with-dictionaries-using-the-api) 사용자 지정 VCL 코드 조각을 사용하여 cloud infrastructure 사이트에서 Adobe Commerce의 참조 스팸을 차단합니다.

>[!NOTE]
>
>프로덕션 환경에서 실행하기 전에 사용자 지정 VCL 구성을 테스트할 수 있는 스테이징 환경에 추가하는 것이 좋습니다.

**사전 요구 사항:**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

- 사이트 로그에서 가짜 참조 URL을 검토하고 차단할 도메인 목록을 만드십시오.

## 레퍼러 차단 목록 만들기

Edge 사전은 VCL 코드 조각을 처리하는 동안 VCL 함수에 액세스할 수 있는 키-값 쌍을 생성합니다. 이 예제에서는 차단할 레퍼러 웹 사이트 목록을 제공하는 에지 사전을 만듭니다.

{{admin-login-step}}

1. 클릭 **스토어** > **설정** > **구성** > **고급** > **시스템**.

1. 확장 **전체 페이지 캐시** > **Fastly 구성** > **가장자리 사전**.

1. 사전 컨테이너를 만듭니다.

   - 클릭 **컨테이너 추가**.

   - 다음에서 *컨테이너* 페이지, 입력 **사전 이름**—`referrer_blocklist`.

   - 선택 **변경 후 활성화** 편집 중인 Fastly 서비스 구성 버전에 변경 사항을 배포합니다.

   - 클릭 **업로드** Fastly 서비스 구성에 사전을 첨부합니다.

1. 차단할 도메인 이름 목록을 `referrer_blocklist` 사전:

   - 에 대한 설정 아이콘을 클릭합니다. `referrer_blocklist` 사전.

   - 새 사전에 키-값 쌍을 추가 및 저장합니다. 이 예제의 경우 각각 **키** 는 차단할 레퍼러 URL의 도메인 이름이며, **값** 은(는) `true`.

     ![잘못된 레퍼러 사전 항목 추가](../../assets/cdn/fastly-referrer-blocklist-dictionary.png)

   - 클릭 **취소** 시스템 구성 페이지로 돌아갑니다.

1. 클릭 **구성 저장**.

1. 페이지 상단의 알림에 따라 캐시를 새로 고칩니다.

Edge 사전에 대한 자세한 내용은 [Edge 사전 만들기 및 사용](https://docs.fastly.com/guides/edge-dictionaries/working-with-dictionaries-using-the-api) 및 [사용자 지정 VCL 코드 조각](https://docs.fastly.com/guides/edge-dictionaries/working-with-dictionaries-using-the-api#custom-vcl-examples) Fastly 설명서

## 레퍼러 스팸을 차단하는 사용자 지정 VCL 코드 조각 만들기

다음 사용자 지정 VCL 코드 조각 코드(JSON 형식)는 요청을 확인하고 차단하는 논리를 보여 줍니다. VCL 코드 조각은 레퍼러 웹 사이트의 호스트를 헤더로 캡처한 다음 호스트 이름을 의 URL 목록과 비교합니다. `referrer_blocklist` 사전. 호스트 이름이 일치하면 요청이 `403 Forbidden` 오류.

```json
{
  "name": "block_bad_referrer",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "set req.http.Referer-Host = regsub(req.http.Referer, \"^https?:\/\/?([^:\/s]+).*$\", \"\\1\"); if (table.lookup(referrer_blocklist, req.http.Referer-Host)) { error 403 \"Forbidden\"; }"
}
```

이 예제를 기반으로 코드 조각을 만들기 전에 값을 검토하여 변경해야 하는지 여부를 결정합니다.

- `name` — VCL 코드 조각의 이름입니다. 이 예에서는 다음을 사용했습니다. `block_bad_referrer`.

- `dynamic` — 값 0은 [일반 코드 조각](https://docs.fastly.com/en/guides/using-regular-vcl-snippets) Fastly 구성을 위해 버전이 지정된 VCL에 업로드합니다.

- `priority` — VCL 코드 조각이 실행되는 시기를 결정합니다. 우선 순위는 `5` 기본 Magento VCL 코드 조각(`magentomodule_*`)에 50의 우선 순위가 할당되었습니다. 코드 조각을 실행할 시기에 따라 각 사용자 지정 코드 조각의 우선 순위를 50보다 높거나 낮게 설정합니다. 우선 순위가 낮은 번호가 있는 코드 조각이 먼저 실행됩니다.

- `type` — VCL 버전에 코드 조각을 삽입할 위치를 지정합니다. 이 예에서 VCL 코드 조각은 `recv` 코드 조각. 코드 조각이 VCL 버전에 삽입되면 `vcl_recv` 서브루틴, 기본 Fastly VCL 코드 아래 및 임의의 오브젝트 위.

- `content` — 줄 바꿈 없이 한 줄로 실행되는 VCL 코드 조각입니다.

환경에 대한 코드를 검토하고 업데이트한 후 다음 방법 중 하나를 사용하여 사용자 지정 VCL 코드 조각을 Fastly 서비스 구성에 추가합니다.

- [관리자의 사용자 지정 VCL 코드 조각 추가](#add-the-custom-vcl-snippet). 관리자에 액세스할 수 있는 경우 이 방법이 권장됩니다. (필수 [Fastly 버전 1.2.58](fastly-configuration.md#upgrade) 또는 나중에)

- JSON 코드 예제를 파일에 저장(예: `allowlist.json`) 및 [fastly API를 사용하여 업로드합니다](fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api). 관리자에 액세스할 수 없는 경우 이 메서드를 사용합니다.

## 사용자 지정 VCL 코드 조각 추가

{{admin-login-step}}

1. 클릭 **스토어** > 설정 > **구성** > **고급** > **시스템**.

1. 확장 **전체 페이지 캐시** > **Fastly 구성** > **사용자 지정 VCL 코드 조각**.

1. 클릭 **사용자 지정 코드 조각 만들기**.

1. VCL 코드 조각 값을 추가합니다.

   - **이름** — `block_bad_referrer`

   - **유형** — `recv`

   - **우선 순위** — `5`

   - **VCL** 코드 조각 컨텐츠 —

     ```conf
     set req.http.Referer-Host = regsub(req.http.Referer,
     "^https?://?([^:/\s]+).*$", "1");
     if (table.lookup(referrer_blocklist, req.http.Referer-Host)) {
       error 403 "Forbidden";
     }
     ```

1. 클릭 **만들기**.

   ![사용자 지정 레퍼러 블록 VCL 코드 조각 만들기](/help/assets/cdn/fastly-create-referrer-block-snippet.png)

1. 페이지를 다시 로드한 후 **VCL을 Fastly에 업로드** 다음에서 *Fastly 구성* 섹션.

1. 업로드가 완료되면 페이지 상단의 알림에 따라 캐시를 새로 고칩니다.

업로드 프로세스 중에 업데이트된 VCL 버전을 빠르게 확인합니다. 유효성 검사가 실패하면 사용자 지정 VCL 코드 조각을 편집하여 문제를 해결하십시오. 그런 다음 VCL을 다시 업로드합니다.

{{automate-vcl-snippet-deployment}}

{{$include /help/_includes/vcl-snippet-modify.md}}

{{$include /help/_includes/vcl-snippet-delete.md}}
