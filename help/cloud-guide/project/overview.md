---
title: 클라우드 인프라 프로젝트
description: 클라우드 인프라의 Adobe Commerce에 대한 개요 [!DNL Cloud Console] 를 읽고 계정 설정에 액세스하는 방법을 알아보십시오.
last-substantial-update: 2024-02-06T00:00:00Z
exl-id: ae862898-9b4d-45ed-b370-e82cc6f99017
source-git-commit: abe9aa36b907be8bdfdf42e6f28f1e1eac68fecf
workflow-type: tm+mt
source-wordcount: '987'
ht-degree: 0%

---

# 클라우드 인프라 프로젝트

클라우드 인프라 프로젝트의 Adobe Commerce에는 Git 분기의 모든 코드, 관련 환경 및 [!DNL Commerce] 응용 프로그램을 배포하는 스크립트가 포함되어 있습니다. 환경에는 데이터베이스, 웹 서버 및 캐싱 서버를 포함하여 [!DNL Commerce] 응용 프로그램을 지원하는 서비스가 포함되어 있습니다.

Adobe은 프로젝트의 모든 측면을 완전히 관리할 수 있는 [!DNL Cloud Console] 및 개발자 도구를 제공합니다. 계정 소유자는 모든 환경에 대한 전체 액세스 권한을 갖습니다.

## [!DNL Cloud Console]

[!DNL Cloud Console]은(는) 사용자에게 친숙한 형식으로 Commerce 코드를 빌드하고 관리하고 배포하는 대화형 방법을 제공합니다. 프로젝트 목록을 보려면 [로그인 [!DNL Cloud Console]](https://console.adobecommerce.com)하세요. 관리자로 또는 특정 환경 유형에 대해 액세스할 수 있는 권한이 있는 프로젝트만 표시됩니다. Adobe 솔루션 파트너인 경우 지원하는 클라이언트에 대한 여러 프로젝트가 표시될 수 있습니다.

>[!TIP]
>
>프로젝트가 표시되지 않으면 프로젝트와 연결된 [계정 소유자 또는 프로젝트 관리자](../project/user-access.md)에게 연락하여 액세스 권한을 요청해야 합니다. 처음 사용하는 사용자의 경우 _시작_ 안내서에서 [온보딩 항목](../../get-started/onboarding.md#cloud-console)을 참조하세요.

_모든 프로젝트_ 보기에는 액세스 권한이 있는 모든 프로젝트가 나열됩니다. **[!UICONTROL Show filters]**&#x200B;을(를) 클릭하고 프로젝트 목록을 유형, 지역 또는 계획별로 필터링할 수 있습니다.

![프로젝트 목록](../../assets/ui-allprojects-list.png)

### 프로젝트 개요

_모든 프로젝트_ 목록에서 프로젝트를 선택하면 프로젝트 개요가 열립니다. 프로젝트 개요에는 항상 환경 선택기와 구성 버튼이 포함된 프로젝트 탐색 모음이 표시됩니다.

![프로젝트 탐색](../../assets/project-nav.png)

환경을 선택하지 않은 경우 프로젝트 개요는 미리보기 영역에 프로젝트 세부 정보의 요약을 표시합니다.

- 프로젝트 이름
- 지역, 프로젝트 ID
- 계획, 할당된 스토리지, 환경, 사용자
- **[!UICONTROL Set a custom domain]** 단추가 있는 Storefront URL

기본 프로젝트 개요에서는

- 환경 보기는 ![활성 분기](../../assets/icon-active.png){width="32"} (active) and ![inactive branch](../../assets/icon-inactive.png){width="32"}(비활성) 환경의 목록 또는 트리 보기를 표시합니다.
- [활동 스트림](activity-stream.md)에 프로젝트에 대한 실행 중, 보류 중 및 최근 활동이 표시됩니다.
<!-- - Apps & Services—Shows a topology of service containers -->

**Starter** 프로젝트의 경우 `master`(프로덕션)부터 시작되는 분기 계층이 있습니다. 만든 모든 분기는 `master` 분기의 하위 분기로 표시됩니다. Adobe은 `staging` 분기를 만든 다음 개발을 위해 `integration` 분기를 만들 것을 권장합니다. [Starter 아키텍처](../architecture/starter-architecture.md)를 참조하세요.

**Pro**&#x200B;의 경우 `production`부터 `staging`까지 `integration`까지 분기의 계층 구조가 있습니다. ![전용 아이콘](../../assets/icon-dedicated.png){width="32"} 아이콘은 분기가 전용 환경에 배포됨을 나타냅니다. 만든 모든 분기는 `integration` 분기의 하위 분기로 표시됩니다. [Pro 아키텍처](../architecture/pro-architecture.md)를 참조하십시오.

![Pro 환경 목록](../../assets/pro-environments.png)

### 환경 개요

프로젝트 탐색 모음에서 환경을 선택하면 개요 및 탐색 막대가 선택한 환경에 중점을 두도록 변경됩니다. 탐색 모음에는 분기 컨트롤(분기, 병합 및 동기화)과 구성 버튼이 포함되어 있습니다.

![환경 선택됨](../../assets/environment-selected.png)

환경 개요는 미리보기 영역에 환경 세부 정보 요약을 표시합니다.

- 환경 이름, 유형
- 지역, 프로젝트 ID
- 백업을 포함한 마지막 작업의 날짜 및 시간
- HTTP 액세스 및 검색 엔진 상태
- 환경에 할당된 컴퓨터 이름
- 환경 상태(활성 또는 비활성)
- **[!UICONTROL Set a custom domain]** 단추가 있는 Storefront URL

기본 환경 개요에서는 다음을 수행합니다.

- [활동 스트림](activity-stream.md)은(는) 기본 환경 개요를 구성하고 선택한 환경에 대해 실행 중, 보류 중 및 최근 활동을 표시합니다.
<!-- - Services tab shows and Apps & Services menu, including overview and configuration tabs for each service. -->
- [백업 탭](../storage/snapshots.md#create-a-manual-backup)은(는) 저장된 백업 목록, 백업 작업 내역 및 백업 단추를 제공합니다.

### 상점 첫 화면

각 활성 환경에는 상점 환경이 있습니다. 위쪽 탐색에서 환경을 선택하고 환경 개요에서 URL을 클릭합니다. 또한 활동 목록 오른쪽에는 **[!UICONTROL URLs]** 목록이 있습니다.

웹 액세스 URL에는 다음이 포함될 수 있습니다.

```terminal
https://<branch>-<unique-ID>-<project-ID>.<region>.magentosite.cloud/
```

- **고유 ID** = 임의의 영숫자 문자 7개
- **프로젝트 ID** = 13자 프로젝트 ID
- **지역** = AWS 또는 Azure 지역 이름입니다. [지역 IP 주소](regional-ip-addresses.md)를 참조하세요.

Pro 프로덕션 및 스테이징 환경에는 다음 링크를 사용하여 액세스할 수 있는 세 개의 노드가 포함됩니다.

- 로드 밸런서 URL:

   - `http[s]://<your-domain>.c.<project-ID>.ent.magento.cloud`
   - `http[s]://<your-staging-domain>.c.<project-ID>.ent.magento.cloud`

- 세 개의 중복 서버 중 하나에 직접 액세스:

   - `http[s]://<your-domain>.{1|2|3}.<project-ID>.ent.magento.cloud`
   - `http[s]://<your-staging-domain>.{1|2|3}.<project-ID>.ent.magento.cloud`

  프로덕션 URL은 CDN(콘텐츠 전달 네트워크)에서 사용됩니다.

## 설정

프로젝트 탐색 오른쪽의 ![프로젝트 구성 아이콘](../../assets/icon-configure.png){width="36"}(구성) 아이콘을 클릭하여 _설정_ 패널을 엽니다.

### 프로젝트 설정

**[!UICONTROL Project Settings]**&#x200B;은(는) 프로젝트 수준 컨트롤 메뉴를 확장하여 사용자, 변수 등을 관리합니다.

| 옵션 | 설명 |
|--------------|-------------------------------------------------------------------------------------------------------------------------------|
| 일반 | 백업 또는 유지 관리를 예약하는 데 사용할 시간대를 관리합니다. |
| 액세스 | 프로젝트 및 환경 유형에 대한 [사용자 액세스](user-access.md)를 관리합니다. |
| 인증서 | 프로젝트와 연결된 SSL 인증서 목록을 봅니다. |
| 배포 키 | 프로젝트 코드 저장소에 공개 키를 추가하고 봅니다. |
| 도메인 | 프로젝트에 도메인 이름을 추가합니다. [도메인 관리](../cdn/fastly-custom-cache-configuration.md#manage-domains)를 참조하십시오. |
| 통합 | 상태 알림 및 웹후크와 같은 [통합](../integrations/overview.md)을(를) 추가하고 관리합니다. |
| 변수 | 모든 환경의 빌드 및 런타임에서 사용할 수 있는 [프로젝트 수준 변수](../environment/variable-levels.md)를 추가합니다. |

{style="table-layout:auto"}

### 환경 설정

사이트 설정, 환경 변수 등을 관리하려면 **[!UICONTROL Environments]**&#x200B;을(를) 클릭하고 컨트롤 목록에서 특정 환경을 선택하십시오.

| 옵션 | 설명 |
| --------- | -------------------------------------------------------------------------------------------------------------------------------- |
| 일반 | 디스플레이 이름, 환경 유형 및 상위 환경을 구성합니다.<br>다른 환경 설정 전환: |
|           | **발신 전자 메일 사용**: SMTP 프로토콜을 사용하여 환경에서 [발신 전자 메일 보내기](outgoing-emails.md). |
|           | **검색 엔진에서 숨기기**: 사이트에서 검색 엔진 인덱서와 크롤러를 차단합니다. |
|           | **HTTP 액세스 제어**: 로그인 및 IP 주소 액세스 제어를 사용하여 [!DNL Cloud Console]에 대한 보안 구성을 사용하도록 설정합니다. |
|           | 상태는 `active` 또는 `inactive`입니다. 대부분의 작업이 활성 환경에 있습니다. 환경을 비활성화하거나 삭제할 수 있습니다. |
| 변수 | 런타임에 사용할 수 있는 [환경 수준 변수](../environment/variable-levels.md)를 보고 만들고 관리합니다. |
| 도메인 | [구성된 경로](../routes/routes-yaml.md) 목록을 봅니다. |

{style="table-layout:auto"}

>[!WARNING]
>
>**HTTP 액세스 제어 메서드를 사용하여 Pro 스테이징 및 프로덕션 환경을 보호하지 마십시오**. 이는 Fastly 캐싱을 중단합니다. 대신 Adobe Commerce용 Fastly CDN에서 사용할 수 있는 [차단](../cdn/fastly-vcl-blocking.md) 기능을 사용하십시오.

## Fastly 및 New Relic 자격 증명

프로젝트에 [Fastly](../cdn/fastly.md) 및 [New Relic](../monitor/new-relic-service.md)이(가) 포함되어 있습니다. 프로젝트 세부 정보에는 프로젝트 계획에 대한 정보와 이러한 통합을 위한 중요한 라이선스 및 토큰이 표시됩니다. 라이선스 소유자만 자격 증명 및 서비스에 대한 초기 액세스 권한을 갖습니다. 필요에 따라 기술 및 개발자 리소스에 이러한 자격 증명을 제공합니다.

- [Fastly](https://www.fastly.com/)은(는) 클라우드 인프라 프로젝트에서 Adobe Commerce에 CDN(콘텐츠 전송), 이미지 최적화 및 DDoS 및 WAF(보안 서비스)를 제공합니다. [Fastly 자격 증명 가져오기](../cdn/fastly-configuration.md#get-fastly-credentials)를 참조하십시오.

- [New Relic](../monitor/new-relic-service.md)은(는) 스테이징 및 프로덕션 환경에 대한 응용 프로그램 메트릭 및 성능 정보를 제공합니다.

[Cloud CLI](../dev-tools/cloud-cli-overview.md)를 사용하여 통합 토큰, ID 등을 검토하십시오.

```bash
magento-cloud subscription:info services
```
