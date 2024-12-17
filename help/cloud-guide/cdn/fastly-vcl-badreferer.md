---
title: 참조 스팸 차단
description: Fastly Edge 사전 및 사용자 지정 VCL 코드 조각을 사용하여 사이트의 레퍼러 스팸을 차단합니다.
feature: Cloud, Configuration, Security
exl-id: 665bac93-75db-424f-be2c-531830d0e59a
source-git-commit: a06e3f98b8b581213de1e0fd87ea4c2241ccaa62
workflow-type: tm+mt
source-wordcount: '684'
ht-degree: 0%

---

# 참조 스팸 차단

다음 예제에서는 사용자 지정 VCL 코드 조각으로 [Fastly Edge 사전](https://docs.fastly.com/guides/edge-dictionaries/working-with-dictionaries-using-the-api)을 구성하여 클라우드 인프라 사이트의 Adobe Commerce에서 조회 스팸을 차단하는 방법을 보여 줍니다.

>[!NOTE]
>
>프로덕션 환경에서 실행하기 전에 사용자 지정 VCL 구성을 테스트할 수 있는 스테이징 환경에 추가하는 것이 좋습니다.

**필수 구성 요소:**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

- 사이트 로그에서 가짜 참조 URL을 검토하고 차단할 도메인 목록을 만드십시오.

## 레퍼러 차단 목록 만들기

Edge 사전은 VCL 코드 조각을 처리하는 동안 VCL 함수에 액세스할 수 있는 키-값 쌍을 만듭니다. 이 예제에서는 차단할 레퍼러 웹 사이트 목록을 제공하는 에지 사전을 만듭니다.

{{admin-login-step}}

1. **스토어** > **설정** > **구성** > **고급** > **시스템**&#x200B;을 클릭합니다.

1. **전체 페이지 캐시** > **가장 빠른 구성** > **Edge 사전**&#x200B;을 확장합니다.

1. 사전 컨테이너를 만듭니다.

   - **컨테이너 추가**&#x200B;를 클릭합니다.

   - *컨테이너* 페이지에서 **사전 이름**—`referrer_blocklist`을 입력하십시오.

   - 편집 중인 Fastly 서비스 구성 버전에 변경 내용을 배포하려면 **변경 후 활성화**&#x200B;를 선택하십시오.

   - 사전을 Fastly 서비스 구성에 첨부하려면 **업로드**&#x200B;를 클릭하십시오.

1. `referrer_blocklist` 사전에 차단할 도메인 이름 목록을 추가하십시오.

   - `referrer_blocklist` 사전에 대한 설정 아이콘을 클릭합니다.

   - 새 사전에 키-값 쌍을 추가 및 저장합니다. 이 예제에서 각 **Key**&#x200B;은(는) 차단할 레퍼러 URL의 도메인 이름이고 **Value**&#x200B;은(는) `true`입니다.

     ![잘못된 레퍼러 사전 항목 추가](../../assets/cdn/fastly-referrer-blocklist-dictionary.png)

   - 시스템 구성 페이지로 돌아가려면 **취소**&#x200B;를 클릭하십시오.

1. **구성 저장**&#x200B;을 클릭합니다.

1. 페이지 상단의 알림에 따라 캐시를 새로 고칩니다.

Edge 사전에 대한 자세한 내용은 Fastly 설명서에서 [Edge 사전 만들기 및 사용](https://docs.fastly.com/guides/edge-dictionaries/working-with-dictionaries-using-the-api) 및 [사용자 지정 VCL 코드 조각](https://docs.fastly.com/guides/edge-dictionaries/working-with-dictionaries-using-the-api#custom-vcl-examples)을 참조하십시오.

## 레퍼러 스팸을 차단하는 사용자 지정 VCL 코드 조각 만들기

다음 사용자 지정 VCL 코드 조각 코드(JSON 형식)는 요청을 확인하고 차단하는 논리를 보여 줍니다. VCL 코드 조각은 레퍼러 웹 사이트의 호스트를 헤더로 캡처한 다음 호스트 이름을 `referrer_blocklist` 사전의 URL 목록과 비교합니다. 호스트 이름이 일치하면 `403 Forbidden` 오류가 발생하여 요청이 차단됩니다.

```json
{
  "name": "block_bad_referrer",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if (req.http.Referer ~ \"^(.*:)//([A-Za-z0-9\-\.]+)(:[0-9]+)?(.*)$\") {set req.http.Referer-Host = re.group.2;}if (table.lookup(referrer_blocklist, req.http.Referer-Host)) {error 403 \"Forbidden\";}"
}
```

이 예제를 기반으로 코드 조각을 만들기 전에 값을 검토하여 변경해야 하는지 여부를 결정합니다.

- `name` — VCL 코드 조각의 이름입니다. 이 예제에서는 `block_bad_referrer`을(를) 사용했습니다.

- `dynamic` — 값 0은 Fastly 구성을 위해 버전이 지정된 VCL에 업로드할 [일반 코드 조각](https://docs.fastly.com/en/guides/using-regular-vcl-snippets)을 나타냅니다.

- `priority` — VCL 코드 조각이 실행되는 시기를 결정합니다. 기본 Magento VCL 코드 조각(`magentomodule_*`)에 우선 순위 50이 할당되기 전에 이 코드 조각 코드를 실행하는 우선 순위는 `5`입니다. 코드 조각을 실행할 시기에 따라 각 사용자 지정 코드 조각의 우선 순위를 50보다 높거나 낮게 설정합니다. 우선 순위가 낮은 번호가 있는 코드 조각이 먼저 실행됩니다.

- `type` — VCL 버전에 코드 조각을 삽입할 위치를 지정합니다. 이 예에서 VCL 코드 조각은 `recv` 코드 조각입니다. 코드 조각이 VCL 버전에 삽입되면 기본 Fastly VCL 코드 아래 및 개체 위의 `vcl_recv` 서브루틴에 추가됩니다.

- `content` - 한 줄에서 실행되는 줄 바꿈 없이 VCL 코드 조각입니다.

환경에 대한 코드를 검토하고 업데이트한 후 다음 방법 중 하나를 사용하여 사용자 지정 VCL 코드 조각을 Fastly 서비스 구성에 추가합니다.

- [관리자로부터 사용자 지정 VCL 코드 조각을 추가](#add-the-custom-vcl-snippet)합니다. 관리자에 액세스할 수 있는 경우 이 방법이 권장됩니다. ([Fastly 버전 1.2.58](fastly-configuration.md#upgrade) 이상이 필요합니다.)

- JSON 코드 예제를 파일(예: `allowlist.json`)에 저장하고 [Fastly API를 사용하여 업로드](fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api)합니다. 관리자에 액세스할 수 없는 경우 이 메서드를 사용합니다.

## 사용자 지정 VCL 코드 조각 추가

{{admin-login-step}}

1. **스토어** > 설정 > **구성** > **고급** > **시스템**&#x200B;을 클릭합니다.

1. **전체 페이지 캐시** > **빠른 구성** > **사용자 지정 VCL 조각**&#x200B;을 확장합니다.

1. **사용자 지정 코드 조각 만들기**&#x200B;를 클릭합니다.

1. VCL 코드 조각 값을 추가합니다.

   - **이름** — `block_bad_referrer`

   - **유형** — `recv`

   - **우선 순위** — `5`

   - **VCL** 코드 조각 콘텐츠 —

     ```conf
     if (req.http.Referer ~ "^(.*:)//([A-Za-z0-9\-\.]+)(:[0-9]+)?(.*)$") {
       set req.http.Referer-Host = re.group.2;  
     }
     if (table.lookup(referrer_blocklist, req.http.Referer-Host)) {
       error 403 "Forbidden";
     }
     ```

1. **만들기**&#x200B;를 클릭합니다.

   ![사용자 지정 레퍼러 블록 VCL 코드 조각 만들기](/help/assets/cdn/fastly-create-referrer-block-snippet.png)

1. 페이지가 다시 로드되면 *Fastly 구성* 섹션에서 **Fastly에 VCL 업로드**&#x200B;를 클릭합니다.

1. 업로드가 완료되면 페이지 상단의 알림에 따라 캐시를 새로 고칩니다.

업로드 프로세스 중에 업데이트된 VCL 버전을 빠르게 확인합니다. 유효성 검사가 실패하면 사용자 지정 VCL 코드 조각을 편집하여 문제를 해결하십시오. 그런 다음 VCL을 다시 업로드합니다.

{{automate-vcl-snippet-deployment}}

{{$include /help/_includes/vcl-snippet-modify.md}}

{{$include /help/_includes/vcl-snippet-delete.md}}
