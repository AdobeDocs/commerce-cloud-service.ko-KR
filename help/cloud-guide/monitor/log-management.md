---
title: New Relic 로그 관리
description: New Relic 로그 사용 방법 알아보기
feature: Cloud, Logs, Observability
exl-id: d8afb5c0-9727-4123-8944-81cd6ad7cbb7
source-git-commit: ebe1746ee9fd08480da5ad07d7f1f8299ad9af7e
workflow-type: tm+mt
source-wordcount: '343'
ht-degree: 0%

---

# New Relic 로그 관리

모든 클라우드 인프라 프로젝트는 다음과 같습니다. [New Relic 로그 관리](https://docs.newrelic.com/docs/logs/get-started/get-started-log-management/). 이 서비스는 스테이징 및 프로덕션 환경에서 모든 로그 데이터를 집계하고 중앙 집중식 로그 관리 대시보드에 표시하도록 사전 구성되어 있습니다.

집계된 데이터에는 다음 로그의 정보가 포함됩니다.

- 모두 `ece-tools` 및 의 애플리케이션 로그 `~/var/log` 디렉터리
- 에서 클라우드 서비스에 대한 로그 `var/log/platform/<project-ID>` 디렉터리
- Fastly CDN 및 WAF

프로젝트가 New Relic에 연결되면 New Relic 로그 서비스를 사용하여 다음과 같은 작업을 완료할 수 있습니다.

- New Relic 쿼리를 사용하여 집계된 로그 데이터 검색
- New Relic 로그 애플리케이션을 통해 로그 데이터 시각화
- 사용자 정의 차트, 대시보드 및 경고 만들기
- 단일 대시보드의 성능 문제 해결

## 로그 데이터 보기 및 분석

New Relic 로그 애플리케이션을 사용하여 집계된 로그 데이터를 검색하고 애플리케이션, 인프라, CDN 및 WAF 오류를 해결합니다. New Relic APM 및 인프라 서비스에서 수집된 로그 데이터를 사용하여 차트, 대시보드 및 경고를 만들 수 있습니다.

**New Relic 로그 애플리케이션을 사용하려면**:

1. 에 로그인 [New Relic 계정](https://login.newrelic.com/login).

1. 선택 **로그** Explorer 탐색 메뉴에서 엽니다.

1. 계정 이(가) 의 맨 위에서 선택되었는지 확인합니다. _모든 로그_ 보기.

1. 로그 쿼리의 시간 범위를 선택합니다.

1. 클라우드 서비스에 대한 인프라 로그 데이터를 검토하려면(의 로그) `~/var/log/`), 쿼리 문자열을 입력합니다 `has: "filePath"` 다음에서 _로그 검색_ 필드. 그런 다음 을 클릭합니다. **[!UICONTROL Query logs]**.

   로그 파일의 이름은 `filePath` 열(로그 파일의 전체 경로 포함)

   ![클라우드 프로젝트 New Relic 서비스 로그 데이터](../../assets/new-relic/var-log-query.png)

1. Fastly 로그 데이터를 검토하려면 쿼리 문자열을 입력합니다 `has: "client_ip"` 다음에서 _로그 검색_ 필드. 그런 다음 을 클릭합니다. **[!UICONTROL Query logs]**.

1. 국가 코드별로 Fastly 로그 결과를 필터링하려면 **[!UICONTROL Add column]**&#x200B;을 선택한 다음 을 선택합니다. **[!UICONTROL geo_country_code]**.

   ![클라우드 프로젝트 New Relic CDN 로그 특성 필터](../../assets/new-relic/fastly-countrycode-filter.png)

>[!TIP]
>
>쿼리 보기를 저장할 수 있습니다. _저장된 보기_ 드롭다운입니다. 클릭 **[!UICONTROL Create new]**, 이름을 입력하고 옵션을 선택한 다음 **[!UICONTROL Save view]**.
>
>다음을 참조하십시오 [로그 관리 시작](https://docs.newrelic.com/docs/logs/get-started/get-started-log-management/) 및 [New Relic 쿼리 언어 소개](https://docs.newrelic.com/docs/query-your-data/nrql-new-relic-query-language/get-started/introduction-nrql-new-relics-query-language/) 다음에 있음 _New Relic 문서_ 사이트.
