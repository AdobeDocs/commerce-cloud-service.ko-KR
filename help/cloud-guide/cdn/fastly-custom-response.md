---
title: 오류 및 유지 관리 페이지 사용자 지정
description: Fastly 원본 서버에 대한 요청이 실패할 때 표시되는 기본 오류 페이지를 사용자 지정하는 방법을 알아봅니다.
feature: Cloud, Configuration, Security
exl-id: 16722821-b928-4872-8cef-7f049e600f0d
source-git-commit: 1253d8357fd2554050d1775fefbc420a2097db5f
workflow-type: tm+mt
source-wordcount: '797'
ht-degree: 0%

---

# 오류 및 유지 관리 페이지 사용자 지정

Fastly 원본 요청이 실패하면 Fastly는 기본 서식과 일반 메시징이 포함된 기본 응답 페이지를 반환하여 사용자를 혼동시킬 수 있습니다. 예를 들어 Fastly는 503 오류로 인해 Fastly 원본에 대한 요청이 실패하면 다음 기본 오류 페이지를 반환합니다.

![기본 오류 페이지](../../assets/cdn/fastly-503-example.png)

다음 예와 같이, 일부 기본 응답 페이지를 보다 친화적인 메시지와 향상된 HTML 스타일을 갖는 페이지로 대체하도록 Adobe Commerce 스토어 구성을 업데이트할 수 있습니다.

![Fastly 사용자 지정 오류 페이지](../../assets/cdn/fastly-new-error-page.png)

현재, Adobe Commerce on cloud infrastructure 프로젝트에 대해 다음 Fastly 응답 페이지를 사용자 지정할 수 있습니다.

- [서버 오류 - 내부 서버 오류, 시간 초과 또는 사이트 유지 관리 중단(오류 코드 500 이상)](#customize-the-503-error-page)
- [WAF가 의심되는 요청 트래픽을 탐지할 때 발생하는 WAF 차단 이벤트(403 금지)](#customize-the-waf-error-page)

**HTML 코딩 요구 사항:**

사용자 지정 페이지의 HTML 코드는 다음 요구 사항을 충족해야 합니다.

- 컨텐츠는 최대 65,535자를 포함할 수 있습니다.
- HTML 소스에서 모든 CSS 인라인을 지정합니다.
- Fastly가 오프라인 상태라도 표시되도록 base64를 사용하여 HTML 페이지에서 이미지를 번들로 묶습니다. css-tricks 사이트의 [데이터 URI](https://css-tricks.com/data-uris/)를 참조하세요.

## 503 오류 페이지 사용자 지정

다음과 같은 경우 기본 503 오류 페이지가 표시됩니다.

- Fastly 원본에 대한 요청이 500보다 큰 응답 상태를 반환하는 경우
- 시간 초과, 유지 관리 활동 또는 상태 문제와 같이 Fastly 출처가 작동 중지된 경우

Adobe Commerce 스토어 테마와 일치하는 스타일을 포함하고 필요에 따라 제목과 메시지를 수정하도록 다음 HTML 코드를 조정하여 기본 페이지를 사용자 지정할 수 있습니다.

```html
<!DOCTYPE html>
<html>
   <head>
      <meta charset="UTF-8">
         <title>503</title>
   </head>
   <body>
      <p>Service unavailable</p>
   </body></html>
```

수정된 소스가 브라우저에 올바르게 표시되는지 확인합니다. 그런 다음 사용자 지정된 HTML 코드를 Fastly 구성에 추가합니다.

사용자 지정 응답 페이지를 Fastly 구성에 추가하려면 다음을 수행합니다.

{{admin-login-step}}

1. **스토어** > **설정** > **구성** > **고급** > **시스템**&#x200B;을 선택합니다.

1. 오른쪽 창에서 **전체 페이지 캐시** > **빠르게 구성** > **사용자 지정 합성 페이지**&#x200B;를 확장합니다.

   ![503 오류 페이지 편집](../../assets/cdn/fastly-custom-synthetic-pages-edit-html.png)

1. **HTML 설정**&#x200B;을 선택합니다.

1. 사용자 지정 응답 페이지의 소스 코드를 복사하여 HTML 필드에 붙여넣습니다.

   ![503 오류 페이지 업데이트](../../assets/cdn/fastly-customize-503-response.png)

1. 페이지 맨 위에서 **업로드**&#x200B;를 선택하여 사용자 지정된 HTML 소스를 Fastly 서버에 업로드합니다.

1. 페이지 상단에서 **구성 저장**&#x200B;을 선택하여 업데이트된 구성 파일을 저장합니다.

1. 캐시를 새로 고칩니다.

   - 페이지 상단의 알림에서 *캐시 관리* 링크를 선택합니다.

   - 캐시 관리 페이지에서 **Magento 캐시 플러시**&#x200B;를 선택합니다.

## WAF 오류 페이지 사용자 지정

[WAF](fastly-waf-service.md) 차단 이벤트로 인해 `403 Forbidden` 오류가 발생하여 Fastly 원본 요청이 실패하면 고객에게 다음과 같은 기본 WAF 오류 페이지가 표시됩니다.

![WAF 오류 페이지](../../assets/cdn/fastly-waf-403-error.png)

다음 코드 샘플은 기본 페이지에 대한 HTML 소스를 보여 줍니다.

```html
<html>
  <head>
    <title>Magento 403 Forbidden</title>
  </head>
  <body>
    <p>The requested URL was rejected.</p>
    <p>For additional information, please contact support and provide this reference ID:</p>
    <p>"} req.http.x-request-id {"</p>
    <p><button onclick='history.back();'>Go Back</button></p>
  </body>
</html>
```

Fastly 구성 메뉴의 **사용자 지정 합성 페이지** > **WAF 페이지 편집** 옵션을 사용하여 Adobe Commerce on Cloud Infrastructure 프로젝트에 대한 기본 코드를 사용자 지정할 수 있습니다. 코드를 편집할 때 WAF 차단 이벤트에 대한 참조 ID를 제공하는 다음 줄을 유지합니다.

```html
<p>"} req.http.x-request-id {"</p>
```

>[!NOTE]
>
>WAF 편집 옵션은 Adobe Commerce on Cloud 인프라 프로젝트에 대해 Managed Cloud WAF 서비스가 활성화된 경우에만 사용할 수 있습니다.

**WAF 오류 페이지를 편집하려면**:

1. [관리자에 로그인](../../get-started/onboarding.md#access-your-admin-panel).

1. **스토어** > **설정** > **구성** > **고급** > **시스템**&#x200B;을 선택합니다.

1. 오른쪽 창에서 **전체 페이지 캐시** > **빠르게 구성** > **사용자 지정 합성 페이지**&#x200B;를 확장합니다.

   ![WAF 오류 페이지 옵션 편집](../../assets/cdn/fastly-custom-synthetic-pages-edit-waf.png)

1. **WAF 페이지 편집**&#x200B;을 선택합니다.

1. 필드를 완료하여 HTML을 업데이트합니다.

   ![WAF 오류 페이지 업데이트](../../assets/cdn/fastly-edit-waf-html.png)

   - **상태** — `403 Forbidden` 상태를 선택합니다.
   - **MIME 유형** — 유형 `text/html`.
   - **컨텐츠** — 기본 HTML 응답을 편집하여 사용자 지정 CSS를 추가하고 필요에 따라 제목과 메시지를 업데이트합니다.

1. 페이지 맨 위에서 **업로드**&#x200B;를 선택하여 사용자 지정된 HTML 소스를 Fastly 서버에 업로드합니다.

1. 페이지 상단에서 **구성 저장**&#x200B;을 선택하여 업데이트된 구성 파일을 저장합니다.

1. 캐시를 새로 고칩니다.

   - 페이지 상단의 알림에서 **캐시 관리** 링크를 선택합니다.

   - 캐시 관리 페이지에서 **Magento 캐시 플러시**&#x200B;를 선택합니다.

## 오류 보고서 번호 표시

기본적으로 Fastly는 *503 Service Unavailable* 오류 뒤에 있는 모든 Adobe Commerce 오류를 숨깁니다. 로그에서 오류 세부 정보를 찾아 검토할 수 있도록 오류 로그 보고서 번호를 표시하려면 다음 단계를 사용하여 Fastly를 생략하고 웹 사이트를 여십시오.

1. 스토어의 IP 주소 검색:

   - Pro 스테이징 및 프로덕션 환경의 경우:

     ```bash
     nslookup {your_project_id}.ent.magento.cloud
     ```

   - Pro 통합 환경 및 Starter 환경의 경우:

     ```bash
     nslookup gw.{your_region}.magentosite.cloud
     ```

1. 로컬 워크스테이션의 호스트 파일에 애플리케이션 도메인과 IP 주소를 추가합니다.

   ```text
   {server_IP} {store_domain}
   ```

1. 브라우저 캐시와 쿠키를 지웁니다(또는 시크릿 모드로 전환).

1. 스토어 웹 사이트를 다시 열어 오류 코드를 확인합니다.

1. 오류 코드를 사용하여 오류 보고서 파일에서 세부 사항을 찾습니다.

   - [SSH를 사용하여 영향을 받는 환경에 연결](../development/secure-connections.md#connect-to-a-remote-environment)

   - `./var/report/{error_number}` 파일을 찾습니다.
