---
title: 정적 콘텐츠 배포
description: 클라우드 인프라 프로젝트에서 Adobe Commerce에 이미지, 스크립트 및 CSS와 같은 정적 콘텐츠를 배포하는 전략에 대해 알아봅니다.
feature: Cloud, Build, Deploy, SCD
exl-id: e128d0d5-1326-44e5-a822-009e11ba105f
source-git-commit: 1253d8357fd2554050d1775fefbc420a2097db5f
workflow-type: tm+mt
source-wordcount: '707'
ht-degree: 0%

---

# 정적 콘텐츠 배포 전략

정적 콘텐츠 배포(SCD)는 이미지, 스크립트, CSS, 비디오, 테마, 로케일 및 웹 페이지와 같은 생성할 콘텐츠의 양과 콘텐츠 생성 시기에 따라 저장소 배포 프로세스에 상당한 영향을 줍니다. 예를 들어 기본 전략은 [배포 단계](process.md#deploy-phase-deploy-phase) 사이트가 유지 관리 모드에 있는 경우, 그러나 이 배포 전략은 마운트된 디바이스에 콘텐츠를 직접 작성하는 데 시간이 소요됩니다 `pub/static` 디렉토리. 필요에 따라 배포 시간을 개선하는 데 도움이 되는 몇 가지 옵션 또는 전략이 있습니다.

## JavaScript 및 HTML 컨텐츠 최적화

번들 및 축소를 사용하여 정적 콘텐츠 배포 중에 최적화된 JavaScript 및 HTML 콘텐츠를 작성할 수 있습니다.

### 콘텐츠 축소

에서 정적 보기 파일 복사를 건너뛸 경우 배포 프로세스 중에 SCD 로드 시간을 향상시킬 수 있습니다. `var/view_preprocessed` 디렉토리 및 생성 _축소됨_ HTML 요청 시. 다음을 설정하여 활성화할 수 있습니다. [SKIP_HTML_축소](../environment/variables-global.md#skiphtmlminification) 전역 환경 변수 대상 `true` 다음에서 `.magento.env.yaml` 파일.

>[!NOTE]
>
>다음으로 시작 `ece-tools` 패키지 버전 2002.0.13에서는 SKIP_VARIABLES_MINIFICATION HTML의 기본값이 로 설정됩니다. `true`.

저장할 수 있습니다 **기타** 불필요한 테마 파일 수를 줄여 배포 시간과 디스크 공간을 줄입니다. 예를 들어 `magento/backend` 테마(영어) 및 사용자 지정 테마(다른 언어)를 참조하십시오. 다음을 사용하여 이러한 테마 설정을 구성할 수 있습니다. [SCD_ 매트릭스](../environment/variables-deploy.md#scdmatrix) 환경 변수입니다.

## 배포 전략 선택

배포 전략은 다음 작업 중에 정적 콘텐츠를 생성할지 여부에 따라 다릅니다. _빌드_ 단계, _배포_ 단계 또는 _온디맨드_. 다음 차트에서 볼 수 있듯이 배포 단계 동안 정적 콘텐츠를 생성하는 것이 가장 낮은 최적의 선택입니다. 축소된 HTML을 사용하더라도 각 콘텐츠 파일을 마운트된 파일에 복사해야 합니다 `~/pub/static` 디렉터리입니다. 시간이 오래 걸릴 수 있습니다. 주문형 정적 콘텐츠를 생성하는 것이 최적의 선택인 것 같습니다. 그러나 콘텐츠 파일이 요청 시 캐시에 없으면 콘텐츠 파일이 요청되어 사용자 경험에 로드 시간이 추가됩니다. 따라서 빌드 단계 중에 정적 콘텐츠를 생성하는 것이 가장 최적입니다.

![SCD 로드 비교](../../assets/scd-load-times.png)

### 빌드에서 SCD 설정

HTML이 축소된 빌드 단계 동안 정적 콘텐츠를 생성하는 것이 최적의 구성 [**가동 중지 시간 없음** 배포](reduce-downtime.md)라고도 함 **이상 상태**. 마운트된 드라이브에 파일을 복사하는 대신 `./init/pub/static` 디렉토리.

정적 콘텐츠를 생성하려면 테마 및 로케일에 액세스해야 합니다. Adobe Commerce은 빌드 단계에서 액세스할 수 있는 파일 시스템에 테마를 저장하지만 Adobe Commerce은 로케일을 데이터베이스에 저장합니다. 데이터베이스는 _아님_ 빌드 단계에서 사용할 수 있습니다. 빌드 단계에서 정적 콘텐츠를 생성하려면 다음을 사용해야 합니다. `config:dump` 의 명령 `ece-tools` 파일 시스템으로 로케일을 이동하는 패키지 로케일을 읽고 로케일을 `app/etc/config.php` 파일.

**빌드에서 SCD를 생성하도록 프로젝트를 구성하려면**:

1. 로컬 워크스테이션에서 프로젝트 디렉터리로 변경합니다.
1. SSH를 사용하여 원격 환경에 로그인합니다.

   ```bash
   magento-cloud ssh
   ```

1. 로케일을 파일 시스템으로 이동한 다음 [`config.php` 파일](../development/commerce-version.md#create-a-configphp-file).

1. 다음 `.magento.env.yaml` 구성 파일에는 다음 값이 포함되어야 합니다.

   - [SKIP_HTML_축소](../environment/variables-global.md#skip_html_minification) 은(는) `true`
   - [SKIP_SCD](../environment/variables-build.md#skip_scd) 빌드 단계: `false`
   - [SCD_STRATEGY](../environment/variables-build.md#scd_strategy) 은(는) `compact`

1. 구성 확인 [Post-deploy 후크](../application/hooks-property.md) 다음에서 `.magento.app.yaml` 파일.

1. 를 실행하여 설정을 확인합니다. [이상적인 상태를 위한 스마트 마법사](smart-wizards.md).

   ```bash
   php ./vendor/bin/ece-tools wizard:ideal-state
   ```

### SCD on demand 설정

주문형 SCD를 생성하는 것은 통합 환경의 개발 워크플로우에 최적입니다. 배포 시간을 줄여 구현을 빠르게 검토하고 통합 테스트를 실행할 수 있습니다. 활성화 [SCD_ON_DEMAND](../environment/variables-global.md#scdondemand) 의 전역 단계에 있는 환경 변수 `.magento.env.yaml` 파일. SCD_ON_DEMAND 변수는 SCD와 관련된 다른 모든 구성을 무시하고 `~/pub/static` 디렉토리.

SCD 온디맨드 전략을 사용할 때, 이 전략은 홈 페이지와 같이 요청할 것으로 예상되는 페이지로 캐시를 미리 로드하는 데 도움이 됩니다. 에서 예상 페이지 목록 추가 [WARM_UP_PAGES](../environment/variables-post-deploy.md#warmuppages) 의 배포 후 단계에 있는 환경 변수 `.magento.env.yaml` 파일.

>[!WARNING]
>
>프로덕션 환경에서는 SCD 온디맨드 전략을 사용하지 마십시오.

### SCD 건너뛰기

경우에 따라 정적 콘텐츠 생성을 완전히 건너뛰도록 선택할 수 있습니다. 다음을 설정할 수 있습니다. [SKIP_SCD](../environment/variables-build.md#skipscd) scd와 관련된 다른 구성을 무시하도록 전역 단계에 있는 환경 변수입니다. 이 작업은 의 기존 콘텐츠에 영향을 주지 않습니다. `~/pub/static` 디렉토리.
