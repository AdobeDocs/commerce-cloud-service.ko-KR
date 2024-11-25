---
user-guide-title: 클라우드 인프라의 Commerce 안내서
user-guide-description: 클라우드 인프라에서 Adobe Commerce 애플리케이션을 관리하는 방법에 대해 알아봅니다.
product: magento
feature: Cloud
source-git-commit: 5f00b20e599b7ba26e483a238d05d99daf1dd1b8
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 7%

---


# 클라우드 인프라의 Commerce {#user-guide}

+ [Commerce](overview.md)
+ 아키텍처 {#architecture}
   + [클라우드 인프라](architecture/cloud-architecture.md)
   + [보안](architecture/security.md)
   + [기술 스택](architecture/tech-stack.md)
   + [스타터 아키텍처](architecture/starter-architecture.md)
   + [스타터 워크플로](architecture/starter-develop-deploy-workflow.md)
   + [Pro 아키텍처](architecture/pro-architecture.md)
   + [Pro 워크플로우](architecture/pro-develop-deploy-workflow.md)
   + [확장 아키텍처](architecture/scaled-architecture.md)
   + [자동 크기 조정](architecture/autoscaling.md)
+ [시작](https://experienceleague.adobe.com/docs/commerce-cloud-service/start/overview.html)
+ 릴리스 정보 {#release-notes}
   + [클라우드 도구 세트](release-notes/cloud-tools-suite.md)
   + [ECE-Tools 패키지](release-notes/ece-tools-package.md)
   + [클라우드 패치](release-notes/cloud-patches.md)
   + [Cloud Docker 패키지](release-notes/cloud-docker.md)
   + [클라우드 구성 요소](release-notes/cloud-components.md)
   + [클라우드 패키지](release-notes/cloud-packages.md)
   + [이전 버전과 호환 불가능한 변경 사항](release-notes/backward-incompatible-changes.md)
   + [릴리스 정보 아카이브](release-notes/cloud-release-archive.md)
+ 클라우드 프로젝트 {#project}
   + [프로젝트 개요](project/overview.md)
   + [프로젝트 구조](project/file-structure.md)
   + [사용자 액세스](project/user-access.md)
   + [다단계 인증](project/multi-factor-authentication.md)
   + [활동 스트림](project/activity-stream.md)
   + [발신 이메일](project/outgoing-emails.md)
   + [SendGrid 이메일 서비스](project/sendgrid.md)
   + [콘솔 분기 관리](project/console-branches.md)
   + [지역 IP 주소](project/regional-ip-addresses.md)
+ 개발자 도구 {#dev-tools}
   + [개요](dev-tools/overview.md)
   + Cloud CLI {#cloud-cli}
      + [CLI 개요](dev-tools/cloud-cli-overview.md)
      + [CLI 참조](dev-tools/cloud-cli-reference.md)
   + [클라우드 도커](dev-tools/cloud-docker.md)
   + ECE-Tools {#ece-tools}
      + [패키지 개요](dev-tools/package-overview.md)
      + [ECE-Tools 사용을 위한 1회 업그레이드](dev-tools/install-package.md)
      + [ECE-Tools 패키지 업데이트](dev-tools/update-package.md)
      + [CLI 참조](dev-tools/ece-tools-cli-reference.md)
      + [오류 참조](dev-tools/error-reference.md)
   + 통합 {#integrations}
      + [개요](integrations/overview.md)
      + [Bitbucket](integrations/bitbucket.md)
      + [GitHub](integrations/github.md)
      + [GitLab](integrations/gitlab.md)
      + [상태 알림](integrations/health-notifications.md)
+ 개발 {#develop}
   + [개요](development/overview.md)
   + [인증 키](development/authentication-keys.md)
   + [CLI 분기 관리](development/cli-branches.md)
   + [보안 연결](development/secure-connections.md)
   + {#deploy} 배포
      + [배포 프로세스](deploy/process.md)
      + [최적화](deploy/optimization.md)
      + [우수 사례](deploy/best-practices.md)
      + [시나리오 기반 배포](deploy/scenario-based.md)
      + [다운타임 없는 배포](deploy/reduce-downtime.md)
      + [정적 콘텐츠 배포](deploy/static-content.md)
      + [스마트 마법사](deploy/smart-wizards.md)
      + [스테이징 및 프로덕션에 배포](deploy/staging-production.md)
      + [구성 요소 장애 복구](deploy/recover-failed-deployment.md)
   + {#test} 테스트
      + [테스트 지침](test/guidance.md)
      + [로그](test/log-locations.md)
      + [Xdebug](test/debug.md)
      + [샘플 데이터](test/sample-data.md)
      + [스테이징 및 프로덕션](test/staging-and-production.md)
      + [두 번째 스테이징 환경](test/second-staging.md)
   + [PrivateLink 서비스](development/privatelink-service.md)
   + [보호 블럭](development/protective-block.md)
   + [환경 복원](development/restore-environment.md)
   + 저장소 {#storage}
      + [디스크 공간 관리](storage/manage-disk-space.md)
      + [프로필 데이터베이스 쿼리](storage/profile-database-queries.md)
      + [데이터베이스 백업](storage/database-dump.md)
      + [백업 관리](storage/snapshots.md)
   + 업그레이드 및 패치 {#upgrade}
      + [우수 사례](development/best-practices.md)
      + [Commerce 버전 업그레이드](development/commerce-version.md)
      + [패치 적용](development/apply-patches.md)
+ 구성 {#configure}
   + [개요](environment/overview.md)
   + 응용 프로그램 {#app}
      + [애플리케이션 배포 구성](application/configure-app-yaml.md)
      + [PHP 설정](application/php-settings.md)
      + 속성 {#properties}
         + [애플리케이션 속성](application/properties.md)
         + [크론](application/crons-property.md)
         + [방화벽(Starter만)](application/firewall-property.md)
         + [후크](application/hooks-property.md)
         + [변수](application/variables-property.md)
         + [웹](application/web-property.md)
         + [작업자](application/workers-property.md)
      + [정적 파일에 대한 캐시 설정](application/set-cache.md)
   + 환경 {#env}
      + [환경 배포 구성](environment/configure-env-yaml.md)
      + [변수 수준 및 옵션](environment/variable-levels.md)
      + 변수 {#stage} 재정의
         + [환경 변수](environment/variables-intro.md)
         + [관리자](environment/variables-admin.md)
         + [클라우드 변수](environment/variables-cloud.md)
         + [글로벌](environment/variables-global.md)
         + [빌드](environment/variables-build.md)
         + [배포](environment/variables-deploy.md)
         + [Post-deploy](environment/variables-post-deploy.md)
      + 알림 구성 {#log}
         + [알림](environment/set-up-notifications.md)
         + [로그 핸들러](environment/log-handlers.md)
   + 경로 {#routes}
      + [경로 구성](routes/routes-yaml.md)
      + [캐싱](routes/caching.md)
      + [리디렉션](routes/redirects.md)
      + [서버측 포함](routes/server-side-includes.md)
   + 서비스 {#service}
      + [서비스 구성](services/services-yaml.md)
      + [Elasticsearch](services/elasticsearch.md)
      + [MySQL](services/mysql.md)
      + [OpenSearch](services/opensearch.md)
      + [RabbitMQ](services/rabbitmq.md)
      + [레디스](services/redis.md)
+ Fastly 서비스 {#cdn}
   + [개요](cdn/fastly.md)
   + Fastly 설정 {#setup-fastly}
      + [Fastly 서비스 구성](cdn/fastly-configuration.md)
      + [캐시 구성 사용자 정의](cdn/fastly-custom-cache-configuration.md)
      + [오류 및 유지 관리 페이지 사용자 지정](cdn/fastly-custom-response.md)
   + [웹 애플리케이션 방화벽](cdn/fastly-waf-service.md)
   + [이미지 최적화](cdn/fastly-image-optimization.md)
   + VCL {#custom-vcl-snippets}(으)로 사용자 지정
      + [시작하기](cdn/fastly-vcl-custom-snippets.md)
      + [CMS 백엔드로 요청 재라우팅](cdn/fastly-vcl-wordpress.md)
      + [참조 스팸 차단](cdn/fastly-vcl-badreferer.md)
      + [IP 허용 목록](cdn/fastly-vcl-allowlist.md)
      + [IP 차단 목록](cdn/fastly-vcl-blocking.md)
      + [Fastly 캐시 우회](cdn/fastly-vcl-bypass-to-origin.md)
   + [Fastly 문제 해결](cdn/fastly-troubleshooting.md)
+ 저장소 설정 {#configure-store}
   + [개요](store/overview.md)
   + [우수 사례](store/best-practices.md)
   + [사용자 정의 테마](store/custom-theme.md)
   + [확장](store/extensions.md)
   + [B2B 모듈](store/b2b-module.md)
   + [여러 사이트](store/multiple-sites.md)
   + [사이트 맵 및 검색 엔진 로봇](store/robots-sitemap.md)
   + [PayPal 결제 방법](store/paypal.md)
   + [구성 관리](store/store-settings.md)
+ 사이트 {#launch} 시작
   + [개요](launch/overview.md)
   + [시작 체크리스트](launch/checklist.md)
   + [시작 단계](launch/steps.md)
+ 사이트 {#monitor} 모니터링
   + [성능](monitor/performance.md)
   + New Relic 서비스 {#new-relic}
      + [New Relic 개요](monitor/new-relic-service.md)
      + [계정 및 사용자 관리](monitor/account-management.md)
      + 성능 조사 {#investigate}
         + [정책, 경고 및 워크플로](monitor/investigate-performance.md)
         + [데이터 수집](monitor/ingest-data.md)
         + [배포 추적](monitor/track-deployments.md)
      + [로그 관리](monitor/log-management.md)
