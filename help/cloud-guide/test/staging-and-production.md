---
title: 스테이징 및 프로덕션 테스트
description: 스테이징 및 프로덕션 환경에서 테스트하는 방법을 알아봅니다.
exl-id: 5b762d59-04c5-4e89-a637-719141759158
source-git-commit: 1253d8357fd2554050d1775fefbc420a2097db5f
workflow-type: tm+mt
source-wordcount: '1329'
ht-degree: 0%

---

# 스테이징 및 프로덕션 테스트

코드, 파일 및 데이터를 스테이징 또는 프로덕션으로 성공적으로 마이그레이션한 후 환경 URL을 사용하여 사이트 및 스토어를 테스트합니다. 다음은 로그 확인, Fastly 구성 테스트, UAT(사용자 수락 테스트) 등에 대한 정보를 제공합니다.

## 로그 파일

테스트할 때 배포 시 오류 또는 기타 문제가 발생하는 경우 로그 파일을 확인하십시오. 로그 파일은 `var/log` 디렉토리.

배포 로그가 로그인되었습니다. `/var/log/platform/<prodject-ID>/deploy.log`. 값: `<project-ID>` 프로젝트 ID와 환경이 스테이징인지 아니면 프로덕션인지에 따라 다릅니다. 예를 들어 의 프로젝트 ID를 사용하는 경우 `yw1unoukjcawe`, 스테이징 사용자는 `yw1unoukjcawe_stg` 및 프로덕션 사용자는 `yw1unoukjcawe`.

프로덕션 또는 스테이징 환경에서 로그에 액세스할 때 SSH를 사용하여 세 노드에 각각 로그인하여 로그를 찾습니다. 또는 다음을 사용할 수 있습니다 [New Relic 로그 관리](../monitor/log-management.md) 모든 노드에서 집계된 로그 데이터를 보고 쿼리합니다. 다음을 참조하십시오 [로그 보기](log-locations.md#application-logs).

## 코드 베이스 확인

코드 베이스가 스테이징 및 프로덕션 환경에 올바르게 배포되었는지 확인합니다. 환경에는 동일한 코드 기반이 있어야 합니다.

## 구성 설정 확인

기본 URL, 기본 관리 URL, 다중 사이트 설정 등이 포함된 구성 설정은 관리 패널을 통해 확인하십시오. 추가로 변경해야 하는 경우 로컬 Git 분기에서 편집을 완료하고 로 푸시합니다. `master` 통합, 스테이징 및 프로덕션의 지점.

## Fastly 캐싱 확인

[Fastly 구성](../cdn/fastly-configuration.md) 올바른 Fastly 서비스 ID 및 Fastly API 토큰 자격 증명을 사용하고, Fastly VCL 코드를 업로드하고, DNS 구성을 업데이트하고, 환경에 SSL/TLS 인증서를 적용하는 등 세부 사항에 주의해야 합니다. 이러한 설정 작업을 완료한 후 스테이징 및 프로덕션 환경에서 Fastly 캐싱을 확인할 수 있습니다.

**Fastly 서비스 구성을 확인하려면**:

1. 다음 URL을 사용하여 스테이징 및 프로덕션용 관리자에 로그인합니다. `/admin`또는 [업데이트된 관리자 URL](../environment/variables-admin.md#admin-url).

1. 다음으로 이동 **스토어** > **설정** > **구성** > **고급** > **시스템**. 스크롤하고 클릭 **전체 페이지 캐시**.

1. 다음을 확인합니다. **캐싱 응용 프로그램** 값이 (으)로 설정됨 _Fastly CDN_ .

1. Fastly 자격 증명을 테스트합니다.

   - 클릭 **Fastly 구성**.

   - Fastly 서비스 ID 및 Fastly API 토큰 자격 증명의 값을 확인합니다. 다음을 참조하십시오 [Fastly 자격 증명 가져오기](/help/cloud-guide/cdn/fastly-configuration.md#get-fastly-credentials).

   - 클릭 **자격 증명 테스트**.

   >[!WARNING]
   >
   >스테이징 및 프로덕션 환경에서 올바른 Fastly 서비스 ID 및 API 토큰을 입력했는지 확인하십시오. Fastly 자격 증명은 서비스 환경별로 생성 및 매핑됩니다. 프로덕션 환경에서 스테이징 자격 증명을 입력할 경우 VCL 조각을 업로드할 수 없고 캐싱이 제대로 작동하지 않으며 캐싱 구성이 잘못된 서버 및 스토어를 가리킵니다.

**Fastly 캐싱 동작을 확인하려면**:

1. 다음을 사용하여 헤더 확인 `dig` 명령줄 유틸리티를 사용하여 사이트 구성에 대한 정보를 얻을 수 있습니다.

   를 사용하는 모든 URL을 사용할 수 있습니다 `dig` 명령입니다. 다음 예제에서는 Pro URL을 사용합니다.

   - 스테이징: `dig https://mcstaging.<your-domain>.com`
   - 프로덕션: `dig https://mcprod.<your-domain>.com`

   추가 `dig` 테스트, Fastly 참조 [DNS 변경 전 테스트](https://docs.fastly.com/en/guides/working-with-domains).

1. 사용 `cURL` 응답 헤더 정보를 확인합니다.

   ```bash
   curl https://mcstaging.<your-domain>.com -H "host: mcstaging.<your-domain.com>" -k -vo /dev/null -H Fastly-Debug:1
   ```

   다음을 참조하십시오 [응답 헤더 확인](../cdn/fastly-troubleshooting.md#check-cache-hit-and-miss-response-headers) 헤더 확인에 대한 자세한 내용.

1. 라이브 상태가 되면 다음을 사용합니다. `cURL` 을 클릭하여 라이브 사이트를 확인합니다.

   ```bash
   curl https://<your-domain> -k -vo /dev/null -H Fastly-Debug:1
   ```

## 전체 UAT 테스트

스테이징 및 프로덕션에 대한 UAT(사용자 승인 테스트)를 완료합니다. 다음 테스트는 판매자와 고객으로서 테스트할 수 있는 작업 및 영역의 빠른 목록입니다. 목록이 더 길어질 수 있으며 사용자 지정 모듈, 확장 및 타사 통합에 대한 추가 테스트가 포함될 수 있습니다. 테스트할 때는 데스크탑, 노트북 및 모바일 장치를 사용합니다.

문제가 발생하면 재생 단계, 오류 메시지, 이상한 화면 캡처 및 링크를 저장하십시오. 이 정보를 사용하여 통합 환경 코드 및 구성 또는 환경 설정의 문제를 조사하고 해결할 수 있습니다.

<table>
<tr>
<td style="width:150px">사용자 관리</td>
<td>
<ul>
<li>고객 계정 만들기 및 편집, 이메일 확인</li>
<li>판매자에 대한 관리자 역할 만들기</li>
<li>특정 역할을 가진 판매자 계정 만들기</li>
<li>역할당 판매자 계정 액세스 테스트</li>
</ul>
</td>
</tr>
<tr>
<td>카탈로그 및 제품</td>
<td>
<ul>
<li>관련 제품을 사용하여 카탈로그 만들기</li>
<li>모든 제품 유형(단순, 구성 가능, 번들)을 포함하여 상점 내 제품을 만드십시오.</li>
<li>제품 이미지, 견본, 비디오 및 기타 미디어 옵션 추가</li>
<li>가격, 할인, 가격책정 규칙 구성 </li>
<li>가격 범위, 주요 제품, 사용 가능 날짜 등 고급 기능 구성</li>
<li>재고 수정 및 정확한 값 표시 및 증가 및 완료된 구매당 변경 확인</li>
</ul>
</td>
</tr>
<tr>
<td>장바구니 및 체크아웃</td>
<td>
<ul>
<li>제품 검색 및 필터링 옵션 선택</li>
<li>검색 결과, 카테고리 페이지, 제품 페이지에서 제품을 장바구니에 추가</li>
<li>모든 제품 유형 테스트</li>
<li>장바구니를 보고 금액을 제거하거나 변경하여 콘텐츠를 수정합니다. </li>
<li>체크아웃을 통해 장바구니 및 제품 정보에 대한 주문 금액을 확인합니다</li>
<li>장바구니에 대해 세금이 올바르게 계산되는지 확인</li>
<li>다른 옵션으로 구매 완료: 쿠폰 추가, 배송 선택, 배송 및 청구 정보 입력, 결제 정보</li>
<li>체크아웃 중 결제 게이트웨이 및 옵션 확인</li>
<li>화면 알림, 고객 계정에 나열된 주문 및 이메일 알림을 확인합니다.</li>
<li>게스트 및 고객 체크아웃 테스트</li>
</ul>
</td>
</tr>
<tr>
<td>Order Management</td>
<td>
<ul>
<li>고객에 대한 주문 생성</li>
<li>주문 검색 및 보기</li>
<li>제품 추가 및 제거, 금액 변경, 배송 및 청구 정보 수정을 통해 주문 수정</li>
<li>환불 처리</li>
<li>주문 취소</li>
<li>쿠폰 코드 및 할인 적용</li>
</ul>
</td>
</tr>
<tr>
<td>사이트 콘텐츠</td>
<td>
<ul>
<li>모든 테마 및 에셋이 올바르게 로드되는지 확인</li>
<li>응답형 미디어 크기를 포함하여 CSS가 올바르게 표시되는지 확인</li>
<li>약관, 환불 정책 및 기타 정책 정보 확인</li>
<li>회사에 대한 연락처 정보, 링크 등을 확인하십시오.</li>
<li>제품 및 컨텐츠를 검색하고 결과 필터링을 확인합니다.</li>
<li>바닥글 블록 및 위쪽 탐색 블록 확인</li>
<li>404 및 유지 관리 페이지 테스트</li>
</ul>
</td>
</tr>
<tr>
<td>확장</td>
<td>
<ul>
<li>모든 확장 설정을 확인합니다. 특히 과세, 운송 및 결제 모듈에 대해 확인합니다(예: 창고 및 재무 관리 시스템으로 보낸 주문).</li>
<li>모든 사용자 지정된 모듈 및 설치된 확장 상호 작용 테스트</li>
<li>완료해야 하는 모든 상호 작용(결제, 주문, 이메일 알림)에 대한 데이터를 확인합니다.</li>
<li>확장에 대한 환경별 구성 확인</li>
<li>모듈 및 확장 간의 종속성 작동 확인</li>
<li>판매자와 고객으로서의 모든 조치 확인</li>
</ul>
</td>
</tr>
<tr>
<td>타사 통합</td>
<td>
<ul>
<li>데이터가 Adobe Commerce에 올바르게 저장되고 서드파티 서비스에서 내보내기, 푸시 또는 액세스할 수 있는지 확인합니다(예: 주문은 서드파티 주문 관리 시스템에 표시됨).</li>
<li>통합당 구성 및 상호 작용 확인</li>
<li>Adobe Commerce 및 서드파티 서비스에서 시작한 왕복 테스트를 수행합니다</li>
<li>인증 완료 확인</li>
<li>제어판에서 코드 통합 또는 오류 메시지를 업데이트하려면 기록된 문제를 확인하십시오</li>
</ul>
</td>
</tr>
<tr>
<td>백엔드 테스트</td>
<td>
<ul>
<li>캐시 테스트 및 지우기 </li>
<li>리인덱싱 수행 및 결과 확인</li>
<li>cron 작업을 확인하고 cron_schedule 오류가 있는지 확인합니다.</li>
<li>셸 스크립트 문제 확인 및 확인</li>
<li>응용 프로그램 로그, PHP 로그, MySQL 로그, 이메일 로그 등 기록된 문제가 있는지 확인합니다.</li>
</ul>
</td>
</tr>
</table>

## 부하 및 부하 테스트

시작하기 전에 스테이징 및 프로덕션 환경에서 광범위한 트래픽 및 성능 테스트를 수행하는 것이 가장 좋습니다. 프론트엔드 및 백엔드 프로세스에 대한 성능 테스트를 고려하십시오.

테스트를 시작하기 전에 테스트 중인 환경, 사용 중인 도구 및 기간에 대한 지원을 제공하는 티켓을 입력합니다. 성과 추적을 위해 결과와 정보로 티켓을 업데이트합니다. 테스트를 완료하면 업데이트된 결과를 추가하고 티켓 테스트가 완료되고 날짜 및 타임스탬프가 포함된 메모를 보내십시오.

리뷰 [성능 툴킷](https://github.com/magento/magento2/tree/2.4/setup/performance-toolkit) 사전 실행 준비 프로세스의 일부인 옵션입니다.

최상의 결과를 얻으려면 다음 도구를 사용하십시오.

- [응용 프로그램 성능 테스트](../environment/variables-post-deploy.md#ttfb_tested_pages)—다음을 구성하여 애플리케이션 성능 테스트 `TTFB_TESTED_PAGES` 사이트 응답 시간을 테스트하기 위한 환경 변수입니다.
- [공성전](https://www.joedog.org/siege-home/)—트래픽 셰이핑 및 테스트 소프트웨어를 통해 스토어를 최대 한도로 늘릴 수 있습니다. 구성 가능한 수의 시뮬레이션된 클라이언트를 사용하여 사이트를 테스트합니다. Siege는 기본 인증, 쿠키, HTTP, HTTPS 및 FTP 프로토콜을 지원합니다.
- [Jmeter](https://jmeter.apache.org)—플래시 판매와 같은 급격한 트래픽에 대한 성능을 측정하는 데 도움이 되는 탁월한 부하 테스트입니다. 사이트에 대해 실행할 사용자 지정 테스트를 만듭니다.
- [New Relic](../monitor/new-relic-service.md) (제공)—데이터, 쿼리, Redis 등의 전송과 같은 작업당 추적된 시간을 통해 성능이 저하되는 사이트의 프로세스 및 영역을 찾는 데 도움이 됩니다.
- [웹 페이지 테스트](https://www.webpagetest.org) 및 [핑돔](https://www.pingdom.com)- 서로 다른 원본 위치를 사용하여 사이트 페이지 로드 시간을 실시간으로 분석합니다. 핑돔(Pingdom)은 추가 요금이 부과될 수 있습니다. WebPageTest는 무료 도구입니다.

## 기능 테스트

MFTF(Magento 기능 테스트 프레임워크)를 사용하여 Cloud Docker 환경에서 Adobe Commerce에 대한 기능 테스트를 완료할 수 있습니다. 다음을 참조하십시오 [애플리케이션 테스트](https://developer.adobe.com/commerce/cloud-tools/docker/test/application-testing/) 다음에서 _Commerce용 Cloud Docker 안내서_.

## 보안 검색 도구 설정

귀하의 사이트에 대 한 무료 보안 검색 도구가 있습니다. 사이트를 추가하고 도구를 실행하려면 를 참조하십시오. [보안 검색 도구](../launch/overview.md#set-up-the-security-scan-tool).
