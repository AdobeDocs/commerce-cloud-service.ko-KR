---
title: 요청을 허용하기 위한 사용자 지정 VCL
description: Fastly Edge ACL 목록 및 사용자 지정 VCL 코드 조각을 사용하여 수신 요청을 필터링하고 Adobe Commerce 사이트에 대한 IP 주소별 액세스를 허용합니다.
feature: Cloud, Configuration, Security
exl-id: a6ee958a-c3d3-47be-b2df-510707f551fc
source-git-commit: 13e76d3e9829155995acbb72d947be3041579298
workflow-type: tm+mt
source-wordcount: '848'
ht-degree: 0%

---

# 요청을 허용하기 위한 사용자 지정 VCL

사용자 지정 VCL 코드 조각과 함께 Fastly Edge ACL 목록을 사용하여 들어오는 요청을 필터링하고 IP 주소별 액세스를 허용할 수 있습니다. ACL 목록은 허용할 IP 주소를 지정합니다.

내부 개발자 및 승인된 외부 서비스만 요청하도록 스테이징 환경에 대한 액세스를 제한하기 위한 허용 목록에 추가하다를 생성합니다. 스테이징 및 프로덕션 환경에서 관리자에 대한 보안 액세스를 위해 허용 목록에 추가하다를 만들 수도 있습니다.

다음 예제에서는 사용자 지정 VCL 코드 조각을 [ACL(Fastly Access Control List)](https://docs.fastly.com/guides/access-control-lists/about-acls)과(와) 함께 사용하여 클라우드 인프라 프로젝트 환경의 Adobe Commerce에 대한 관리자 액세스를 보호하는 방법을 보여 줍니다. 사용자 지정 VCL 코드 조각을 클라우드 환경에 추가하면 Fastly에서는 ACL에 포함된 IP 주소의 요청만 허용합니다.

>[!TIP]
>
>공개적으로 액세스할 수 없는 스테이징 및 통합 환경의 경우 [[!DNL Cloud Console]](../project/overview.md#access-the-project-web-interface)에서 사용할 수 있는 HTTP 액세스 제어 옵션을 사용하여 IP 주소별로 전체 사이트에 대한 액세스를 관리하십시오.

**필수 구성 요소:**


{{$include /help/_includes/vcl-snippet-prerequisites.md}}

- 허용 목록에 추가하다에 포함할 클라이언트 IP 주소 목록

## 클라이언트 IP 주소를 허용하기 위한 Edge ACL 만들기

Edge ACL은 사이트에 대한 액세스를 관리하기 위한 IP 주소 목록을 만듭니다. 이 예에서는 Edge ACL을 만들고 프로젝트 환경의 관리자에 액세스할 수 있는 클라이언트 IP 주소 목록을 추가합니다.

{{admin-login-step}}

1. **스토어** > 설정 > **구성** > **고급** > **시스템**&#x200B;을 클릭합니다.

1. **전체 페이지 캐시** > **빠른 구성** > **Edge ACL**&#x200B;을 확장합니다.

1. ACL 컨테이너를 만듭니다.

   - **ACL 추가**&#x200B;를 클릭합니다.

   - *ACL 컨테이너* 페이지에서 **ACL 이름**—`allowlist`을 입력하십시오.

   - 편집 중인 Fastly 서비스 구성 버전에 변경 내용을 배포하려면 **변경 후 활성화**&#x200B;를 선택하십시오.

   - ACL을 Fastly 서비스 구성에 연결하려면 **업로드**&#x200B;를 클릭하십시오.

1. 관리자 액세스가 허용되는 IP 주소 목록 추가:

   - `allowlist` ACL에 대한 설정 아이콘을 클릭합니다.

   - 각 클라이언트 IP 주소에 대해 *IP 값*&#x200B;을(를) 추가하고 저장합니다.

   - 시스템 구성 페이지로 돌아가려면 **취소**&#x200B;를 클릭하십시오.

1. **구성 저장**&#x200B;을 클릭합니다.

1. 페이지 상단의 알림에 따라 캐시를 새로 고칩니다.

## 관리자 액세스를 보호하기 위해 사용자 지정 VCL 코드 조각 만들기

다음 사용자 지정 VCL 코드 조각 코드(JSON 형식)는 관리자에 대한 요청을 필터링하고 클라이언트 IP 주소가 `allowlist` ACL의 주소와 일치하는 경우 액세스를 허용하는 논리를 보여 줍니다.

```json
{
  "name": "allowlist",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if ((req.url ~ \"^/admin\") && !(client.ip ~ allowlist) && !req.http.Fastly-FF) { error 403 \"Forbidden\"; }"
}
```

이 예제에서 [사용자 지정 코드 조각을 만들기](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/cdn/custom-vcl-snippets/fastly-vcl-allowlist.html#add-the-custom-vcl-snippet)하기 전에 값을 검토하여 변경해야 하는지 확인하십시오. 그런 다음 각 값을 각 필드에 입력합니다(예: `type`은(는) 유형 필드에, `content`은(는) 콘텐츠 필드에).

- `name` — VCL 코드 조각의 이름입니다. 이 예제의 경우 `allowlist`입니다.

- `priority` — VCL 코드 조각이 실행되는 시기를 결정합니다. 관리자 요청이 허용된 IP 주소에서 오는지 여부를 즉시 실행하고 확인하는 우선 순위는 `5`입니다. 코드 조각은 기본 Magento VCL 코드 조각(`magentomodule_*`)에 우선 순위 50이 할당되기 전에 실행됩니다. 코드 조각을 실행할 시기에 따라 각 사용자 지정 코드 조각의 우선 순위를 50보다 높거나 낮게 설정합니다. 우선 순위가 낮은 번호가 있는 코드 조각이 먼저 실행됩니다.

- `type` — 버전이 지정된 VCL 코드에 코드 조각을 삽입할 위치를 지정합니다. 이 VCL은 기본 Fastly VCL 코드 아래 및 개체 위의 `vcl_recv` 서브루틴에 코드 조각 코드를 추가하는 `recv` 코드 조각 유형입니다.

- `content` — 실행할 VCL 코드 조각입니다. 이 예에서 코드는 관리자에게 요청을 필터링하고 클라이언트 IP 주소가 `allowlist` ACL의 주소와 일치하는 경우 액세스를 허용합니다. 주소가 일치하지 않으면 `403 Forbidden` 오류가 발생하여 요청이 차단됩니다.

  관리자의 URL이 변경된 경우 샘플 값 `/admin`을(를) 환경의 URL로 바꾸십시오. 예: `/company-admin`.

코드 샘플에서는 [원본 차폐](fastly-custom-cache-configuration.md#configure-back-ends-and-origin-shielding)를 사용할 때 `!req.http.Fastly-FF` 조건이 중요합니다. 이 코드를 제거하거나 편집하지 마십시오.

환경에 대한 코드를 검토하고 업데이트한 후 다음 방법 중 하나를 사용하여 사용자 지정 VCL 코드 조각을 Fastly 서비스 구성에 추가합니다.

- [관리자로부터 사용자 지정 VCL 코드 조각을 추가](#add-the-custom-vcl-snippet)합니다. 관리자에 액세스할 수 있는 경우 이 방법이 권장됩니다. (Magento 2 버전 1.2.58](fastly-configuration.md#upgrade) 이상에 [Fastly CDN 모듈이 필요합니다.)

- JSON 코드 예제를 파일(예: `allowlist.json`)에 저장하고 [Fastly API를 사용하여 업로드](fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api)합니다. 관리자에 액세스할 수 없는 경우 이 메서드를 사용합니다.

## 사용자 지정 VCL 코드 조각 추가

{{admin-login-step}}

1. **스토어** > 설정 > **구성** > **고급** > **시스템**&#x200B;을 클릭합니다.

1. **전체 페이지 캐시** > **빠른 구성** > **사용자 지정 VCL 조각**&#x200B;을 확장합니다.

1. **사용자 지정 코드 조각 만들기**&#x200B;를 클릭합니다.

1. VCL 코드 조각 값을 추가합니다.

   - **이름** — `allowlist`

   - **유형** — `recv`

   - **우선 순위** — `5`

   - **VCL** 코드 조각 콘텐츠 추가:

     ```conf
     if ((req.url ~ "^/admin") && !(client.ip ~ allowlist) && !req.http.Fastly-FF) { error 403 "Forbidden";}
     ```

1. **만들기**&#x200B;를 클릭하여 이름 패턴이 `type_priority_name.vcl`인 VCL 코드 조각 파일을 생성합니다(예: `recv_5_allowlist.vcl`).

1. 페이지가 다시 로드되면 *Fastly 구성* 섹션에서 **Fastly에 VCL 업로드**&#x200B;를 클릭하여 Fastly 서비스 구성에 파일을 추가하십시오.

1. 업로드가 완료되면 페이지 상단의 알림에 따라 캐시를 새로 고칩니다.

업로드 프로세스 중에 업데이트된 VCL 코드 버전을 빠르게 확인합니다. 유효성 검사가 실패하면 사용자 지정 VCL 코드 조각을 편집하여 문제를 해결하십시오. 그런 다음 VCL을 다시 업로드합니다.

{{$include /help/_includes/vcl-snippet-modify.md}}

{{$include /help/_includes/vcl-snippet-delete.md}}
