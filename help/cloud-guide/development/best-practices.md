---
title: 프로젝트 업그레이드 모범 사례
description: 프로젝트 파일 업그레이드에 대한 모범 사례 목록을 참조하십시오.
feature: Cloud, Best Practices, Upgrade
exl-id: 7d0a2627-e4c5-46b4-9e6c-24d20fa4f92f
source-git-commit: c61d711b1041ecf76ec6468cd225a34fd77c24b1
workflow-type: tm+mt
source-wordcount: '442'
ht-degree: 0%

---

# 프로젝트 업그레이드 모범 사례

빌드 및 배포에 대한 모범 사례를 따르고 [업그레이드 및 패치](../development/commerce-version.md) 응용 프로그램을 업그레이드하는 워크플로우입니다. 업그레이드 및 업그레이드 후 작업을 계획하려면 다음 지침을 사용하십시오.

- **프로젝트 백업**-Adobe Commerce 및 서드파티 또는 사용자 지정 확장을 업그레이드하기 전에 통합, 스테이징 및 프로덕션 환경에서 데이터베이스를 백업합니다. 다음을 참조하십시오 [데이터베이스 백업](../development/commerce-version.md#project-backup).

- **호환성 문제 확인**-

   - 모든 사용자 지정 테마가 새 Adobe Commerce 버전과 호환되는지 확인합니다

   - 타사 및 사용자 지정 확장을 업그레이드한 후 `magento-cloud local:build` 배포하기 전에 작성기 종속 항목의 유효성을 검사하는 명령입니다.

   - Adobe Commerce 릴리스 노트 및 확장 설명서 를 검토하여 업그레이드된 Adobe Commerce 버전 및 확장과 관련된 알려진 기능 문제 및 버그를 해결하는 데 필요한 해결 방법 또는 구성 변경 사항을 구현했는지 확인하십시오.

   - 설치된 서비스 버전이 새 Adobe Commerce 버전과 호환되는지 확인하고 필요에 따라 서비스를 업그레이드합니다. 다음을 참조하십시오 [서비스](../services/services-yaml.md).

   - 데이터베이스를 테스트하여 Adobe Commerce 버전 및 확장에 대한 업데이트로 인해 발생한 문제를 해결합니다.

   - 원격 환경에 배포하기 전에 환경별 설정을 필요한 대로 업데이트하십시오.

   - 검색 서비스 버전이 PHP 클라이언트 버전과 호환되는지 확인하십시오. 다음을 참조하십시오 [Elasticsearch 설정](../services/elasticsearch.md) 또는 [OpenSearch 설정](../services/opensearch.md).

- **원격 환경에서 데이터베이스 연결 및 사용 가능한 스토리지 확인**-

   - SSH를 사용하여 원격 서버에 로그인하고 MySQL 데이터베이스에 대한 연결을 확인합니다. 다음을 참조하십시오 [데이터베이스에 연결](../services/mysql.md#connect-to-the-database).

   - 원격 환경에서 사용 가능한 스토리지 확인 - `disk free` 클라우드 환경에서 사용 가능한 디스크 공간을 보고 관리하는 명령입니다. 다음을 참조하십시오 [디스크 공간 관리](../storage/manage-disk-space.md).

      - 업그레이드된 데이터베이스의 크기를 확인하고 `services.yaml` 파일에 할당된 디스크 공간이 충분합니다.

      - 디스크 공간 사용-캐시를 지우고 `/log` 및 `/tmp` 배포 전 디렉토리.

- **스테이징에 배포하기 전에 로컬 및 통합 환경에서 성공적인 업그레이드를 계획하고 수행합니다.**-업그레이드 후 배포를 테스트하고 문제를 해결합니다.

- **스테이징에 코드를 병합한 다음 프로덕션에 병합**-프로덕션 환경에 변경 사항을 푸시하기 전에 스테이징 환경에서 모든 문제를 테스트하고 해결합니다.

- **업그레이드 후 작업 완료**-

   - SSH를 사용하여 원격 서버에 로그인하고 다음을 확인합니다.

      - 인덱서 상태를 확인하고 필요에 따라 다시 인덱싱합니다. 다음을 참조하십시오 [인덱서 관리](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/manage-indexers.html) 다음에서 _구성 안내서_.

      - 다음 확인: `cron` 로그 및 `cron_schedule` cron 상태를 확인하고 필요한 경우 cron 작업을 다시 실행할 수 있도록 Adobe Commerce 데이터베이스의 테이블입니다.
다음을 참조하십시오 [로깅](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/configure-cron-jobs.html#logging) 다음에서 _구성 안내서_.

   - 스테이징 및 프로덕션 환경에서 업그레이드 후 사용자 승인 테스트 UAT를 완료하고 서드파티 및 사용자 정의 확장 업그레이드와 관련된 문제를 수정합니다.
