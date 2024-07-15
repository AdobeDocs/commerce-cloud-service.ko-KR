---
title: Fastly 캐시를 무시하는 사용자 지정 VCL
description: Fastly 캐시를 무시하는 사용자 지정 VCL 코드 조각을 만들어 원본 서버로의 요청 트래픽 문제를 해결합니다.
feature: Cloud, Configuration, Cache
exl-id: a2e9dc57-9b5e-4716-9965-a4324442ad00
source-git-commit: 7a181af2149eef7bfaed4dd4d256b8fa19ae1dda
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 0%

---

# Fastly 캐시를 무시하는 사용자 지정 VCL

원본 서버에 대한 요청 트래픽 문제를 해결할 수 있도록 Fastly 캐시를 무시하는 사용자 지정 VCL 코드 조각을 만들 수 있습니다. 예를 들어 사이트 문제가 캐싱으로 인해 발생하는지 또는 헤더 문제를 해결하기 위해 코드 조각을 만들 수 있습니다.

특정 IP 주소 또는 URL의 요청에 대해 Fastly 캐싱을 무시하도록 코드 조각을 구성할 수 있습니다.

>[!NOTE]
>
>사용자 지정 VCL 구성을 프로덕션 환경에 병합하기 전에 스테이징 환경에서 코드를 테스트해야 합니다.

**필수 구성 요소:**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

**IP 주소 또는 URL을 기반으로 Fastly 캐시를 무시하려면**:

{{admin-login-step}}

1. **스토어** > 설정 > **구성** > **고급** > **시스템**&#x200B;을 클릭합니다.

1. **전체 페이지 캐시** > **빠른 구성** > **사용자 지정 VCL 조각**&#x200B;을 확장합니다.

1. **사용자 지정 코드 조각 만들기**&#x200B;를 클릭합니다.

1. VCL 코드 조각 값을 추가합니다.

   - **이름** — `bypass_fastly`

   - **유형** — `recv`

   - **우선 순위** — `5`

   - **VCL** 코드 조각 콘텐츠 —

     다음 예제에서는 특정 IP 주소에 대해 Fastly 를 우회합니다.

     ```conf
     if (client.ip == "<Your IPv4 IP address>" || client.ip == "<Your IPv6 IP address>") {
       return(pass);
     }
     ```

     다음 예제는 특정 URL 패턴에 대해 Fastly를 우회합니다.

     ```conf
     if (req.url ~ "/media/feeds/GoogleShoppingHiVisNew.xml") {  return (pass);}
     ```

     정확한 URL을 일치시키려면 `~` 연산자 대신 `==` 연산자를 사용하십시오. 자세한 내용은 [Fastly VCL 참조]를 참조하십시오.

1. **만들기**&#x200B;를 클릭합니다.

   ![VCL 코드 조각을 빠르게 우회](/help/assets/cdn/fastly-create-bypass-snippet.png)

1. 페이지가 다시 로드되면 *Fastly 구성* 섹션에서 **Fastly에 VCL 업로드**&#x200B;를 클릭합니다.

1. 업로드가 완료되면 페이지 상단의 알림에 따라 캐시를 새로 고칩니다.

   업로드 프로세스 중에 업데이트된 VCL 버전을 빠르게 확인합니다. 유효성 검사가 실패하면 사용자 지정 VCL 코드 조각을 편집하여 문제를 해결하십시오. 그런 다음 VCL을 다시 업로드합니다.

VCL 코드 조각을 추가한 후 다음 예와 같이 cURL 명령을 사용하여 지정된 IP 주소 또는 URL에서 원천 서버로 요청을 제출할 수 있습니다.

```bash
curl -svo /dev/null www.example.com/index.html
```

그런 다음 응답을 검사하여 캐시되지 않은 콘텐츠 문제를 해결합니다.

{{automate-vcl-snippet-deployment}}

{{$include /help/_includes/vcl-snippet-modify.md}}

{{$include /help/_includes/vcl-snippet-delete.md}}

<!--External link definitions-->

[Fastly VCL 참조]: https://docs.fastly.com/vcl/
