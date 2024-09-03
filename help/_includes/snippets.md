---
source-git-commit: 762ce3cb6268401b0f5fae5b2280a870aa9c83a5
workflow-type: tm+mt
source-wordcount: '857'
ht-degree: 0%

---
# 클라우드 스니펫

## Elasticsearch 경고 {#elasticsearch-support}

>[!WARNING]
>
>Elasticsearch 7.11 이상 버전은 클라우드 인프라의 Adobe Commerce에서 지원되지 않습니다. Adobe Commerce 버전 2.3.7-p3, 2.4.3-p2 및 2.4.4 이상 버전은 OpenSearch 서비스를 지원합니다. 온-프레미스 설치는 Elasticsearch을 계속 지원합니다.

## 향상된 통합 {#enhanced-integration-envs}

>[!NOTE]
>
>2020년 6월 5일 이전에 프로비저닝된 프로젝트에는 소규모 통합 환경이 여러 개 있었습니다. 테스트 및 개발을 위해 더 큰 통합 환경이 필요한 경우 향상된 통합 환경으로 업그레이드를 요청하십시오. 자세한 내용은 _Adobe Commerce 도움말 센터_&#x200B;의 [통합 환경 요청](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/announcements/commerce-announcements/integration-environment-enhancement-request-pro-and-starter.html) 문서를 참조하십시오.

## 병합 옵션 {#merge-options}

기본적으로 배포 프로세스는 `env.php` 파일의 모든 설정을 덮어쓰지만 모든 값을 덮어쓰지 않고 서비스 구성에 대해 하나 이상의 값을 병합하도록 선택할 수 있습니다.

`_merge` 옵션을 다음 중 하나로 설정하십시오.

- `true`—**환경 변수 값을 사용하여 구성된 서비스 값을 병합**&#x200B;합니다.
- `false`—**구성된 서비스 값을 환경 변수 값으로 덮어씁니다**.

## 개인 저장소 {#private-repository}

>[!NOTE]
>
>Adobe은 Adobe Commerce on cloud infrastructure 프로젝트에 개인 저장소를 사용하여 확장 및 중요한 구성과 같은 독점 정보 또는 개발 작업을 보호할 것을 강력히 권장합니다.

## Pro 셀프 서비스 경고 {#pro-self-service-warning}

>[!WARNING]
>
>일부 **Pro 프로젝트**&#x200B;에서는 `routes.yaml` 파일의 경로 구성과 `.magento.app.yaml` 파일의 cron 구성을 업데이트하려면 지원 티켓이 필요합니다. Adobe은 통합 환경에서 YAML 구성 파일을 업데이트하고 테스트한 다음 스테이징 환경에 변경 사항을 배포하는 것을 권장합니다. 다시 배포한 후 변경 사항이 스테이징 사이트에 적용되지 않고 로그에 관련 오류 메시지가 없는 경우 시도한 구성 변경 사항을 설명하는 **MUST** [Adobe Commerce 지원 티켓을 제출](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)합니다. 업데이트된 YAML 구성 파일을 티켓에 포함합니다.

## Pro 서비스 지원 {#pro-update-service}

>[!TIP]
>Pro 프로젝트의 경우 `Staging` 및 `Production` 환경에서만 [서비스](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/service/services-yaml.html)를 설치하거나 업데이트하려면 [Adobe Commerce 지원 티켓을 제출](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)해야 합니다.
>
>필요한 서비스 변경 사항을 표시하고 업데이트된 `.magento.app.yaml` 및 `services.yaml` 파일을 포함하고 티켓에 PHP 버전을 명시하십시오. PHP 버전, 확장, 환경 설정에 대한 셀프 서비스 변경 내용은 _응용 프로그램 구성_&#x200B;의 [PHP 설정](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/app/php-settings.html)을 참조하십시오.
>
>_live_ 프로덕션 환경(**Pro만 해당**)을 변경하는 경우 클라우드 인프라 팀이 리소스를 마샬링하고 보안 업그레이드를 수행할 수 있는 충분한 시간을 허용하기 위해 최소 48시간 알림을 제공해야 합니다. 고시 기간에는 주말이 포함되지 않습니다. 예를 들어, 월요일에 서비스 업그레이드를 수행하려면 이전 수요일까지 요청을 제출해야 합니다.

## Pro 백업 {#pro-backups}

>[!TIP]
>
>Pro 스테이징 및 프로덕션 환경에서는 티켓의 날짜, 시간 및 시간대를 나타내는 특정 백업을 검색하려면 [Adobe Commerce 지원 티켓을 제출](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)해야 합니다.
>
>Adobe이 자동 백업에서 환경을 복원하지 **않습니다**. 스테이징 또는 프로덕션 스냅숏을 복원하는 방법을 선택하는 데 도움이 필요하면 [스테이징 또는 프로덕션에서 DB 스냅숏 복원](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/restore-a-db-snapshot-from-staging-or-production.html)을 참조하십시오.

## 재배포 경고 {#redeploy-warning}

>[!WARNING]
>
>배포 프로세스는 환경의 병합, 푸시 또는 동기화를 수행하거나 수동 재배포를 트리거할 때 시작되며, 이 기간 동안 [!DNL Commerce] 응용 프로그램은 유지 관리 모드에 있습니다. Adobe 프로덕션 환경의 경우, 서비스가 중단되지 않도록 사용량이 적은 시간 동안 이 작업을 완료하는 것이 좋습니다.

## 경로 자리 표시자 {#route-placeholder}

>[!NOTE]
>
>다음 경로 구성 예제에서는 자리 표시자와 함께 경로 템플릿을 사용합니다. `{default}` 자리 표시자는 사이트에 대해 구성된 기본 도메인을 나타냅니다. 프로젝트에 여러 도메인이 있는 경우 `{all}` 자리 표시자를 사용하여 기본 도메인 및 모든 별칭에 대한 라우팅을 구성하십시오. [경로 구성](/help/cloud-guide/routes/routes-yaml.md)을 참조하세요.

## SCD 시간 {#scd-timing-warning}

>[!WARNING]
>
>사용자 지정 테마 파일이 누락되는 등 배포 후 애플리케이션에 정적 콘텐츠 파일에 문제가 있는 경우 최대 예상 실행 시간을 900초 이상으로 늘리십시오.

## 시나리오 기반 배포 {#scenarios}

>[!NOTE]
>
>[!DNL ECE-Tools] 2002.1.0 이상 버전에서는 시나리오 기반 배포 기능을 사용하여 Adobe Commerce on cloud infrastructure 프로젝트의 빌드, 배포 및 배포 후 프로세스를 사용자 지정할 수 있습니다. [시나리오 기반 배포](/help/cloud-guide/deploy/scenario-based.md)를 참조하세요.

## 서비스 지침 {#service-instruction}

`master` 분기를 포함하여 Pro 통합 환경 및 스타터 환경에서 서비스를 설정하려면 다음 지침을 사용하십시오.

>[!NOTE]
>
>Pro 프로덕션 및 스테이징 환경에서 서비스 구성을 변경하려면 [Adobe Commerce 지원 티켓을 제출](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)하십시오.

## 서비스 변경 {#service-change-tip}

>[!TIP]
>
>초기 서비스를 설정한 후 `services.yaml` 및 `.magento.app.yaml` 구성 파일을 업데이트하여 설치된 서비스의 소프트웨어 버전을 변경할 수 있습니다. 서비스 업그레이드 또는 다운그레이드에 대한 지침은 [서비스 버전 변경](/help/cloud-guide/services/services-yaml.md#change-service-version)을 참조하세요.

## 중단된 배포 팁 {#stuck-deployment-tip}

>[!TIP]
>
>중단 배포에 대한 도움말을 보려면 _Adobe Commerce 도움말 센터_&#x200B;에서 [Commerce 배포 문제 해결사](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/deployment/magento-deployment-troubleshooter.html)를 사용하십시오.

## ECE-Tools 업데이트 {#ece-tools-package}

>[!NOTE]
>
>`ece-tools` 패키지가 포함되지 않은 클라우드 인프라의 Adobe Commerce 버전을 사용하는 경우 더 이상 사용되지 않는 패키지를 제거하려면 클라우드 프로젝트에 대해 [1회 업그레이드](/help/cloud-guide/dev-tools/install-package.md)를 수행해야 합니다. 현재 `ece-tools` 패키지를 사용 중인데 업데이트해야 하는 경우 [ECE-Tools 패키지 업데이트](/help/cloud-guide/dev-tools/update-package.md)를 참조하십시오.

## 업그레이드 팁 {#upgrade-tip}

>[!TIP]
>
>업그레이드 또는 패치 프로세스를 시작하기 전에 통합 환경에서 활성 분기를 만들고 로컬 워크스테이션에 새 분기를 체크아웃합니다. 업그레이드 또는 패치 프로세스에 분기를 할당하면 진행 중인 작업에 방해가 되지 않습니다.

<!-- Fastly-related snippets begin -->

## 관리자 로그인 {#admin-login-step}

1. 책임자에 [로그인](/help/get-started/onboarding.md#access-your-admin-panel).

## 사용자 지정 VCL 코드 조각 배포 자동화 {#automate-vcl-snippet-deployment}

>[!NOTE]
>
>사용자 지정 VCL 코드 조각을 수동으로 업로드하는 대신 사용자 환경의 `$MAGENTO_CLOUD_APP_DIR/var/vcl_snippets_custom` 디렉터리에 코드 조각을 추가할 수 있습니다. Commerce 관리자에서 _VCL을 Fastly로 업로드_&#x200B;를 클릭하면 이 디렉터리의 스니펫이 자동으로 업로드됩니다. Magento 2 설명서는 Fastly CDN 모듈의 [자동화된 사용자 지정 VCL 코드 조각 배포](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/CUSTOM-VCL-SNIPPETS.md#automated-custom-vcl-snippets-deployment)를 참조하십시오.

<!-- Fastly-related snippets end -->
