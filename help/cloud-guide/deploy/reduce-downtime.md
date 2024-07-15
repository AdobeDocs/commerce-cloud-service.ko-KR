---
title: 다운타임 없는 배포
description: 클라우드 인프라 프로젝트에서 Adobe Commerce을 배포할 때 전반적인 가동 중지 시간을 줄이는 방법을 알아봅니다.
feature: Cloud, Deploy, SCD, Themes
exl-id: ff89d2e1-dfc8-4f6d-bd98-947559af13f0
source-git-commit: 225fba1acfd8b3ce4d7ce989c7851e7b0b218680
workflow-type: tm+mt
source-wordcount: '448'
ht-degree: 0%

---

# 다운타임 없는 배포

클라우드 인프라의 Adobe Commerce은 배포 단계 동안 [_유지 관리_ 모드](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/setup/application-modes.html#production-mode)로 응용 프로그램을 실행하며, 배포가 완료될 때까지 사이트를 오프라인으로 전환합니다. 프로덕션 사이트가 유지 관리 모드에 있는 시간은 사이트 크기, 배포 중 적용된 변경 사항 수 및 정적 콘텐츠 배포에 대한 구성에 따라 다릅니다. 프로젝트가 **제로** 중단 시간 효과로 배포되도록 구성할 수 있습니다.

배포 프로세스 중에 모든 연결은 활성 세션 및 보류 중인 작업(예: 장바구니에 추가 또는 체크아웃)을 보존하기 위해 최대 5분 동안 대기합니다. 배포 후 큐가 해제되고 연결이 중단 없이 계속됩니다. 이 _연결 유지_&#x200B;를 사용하여 배포를 _제로_ 다운타임으로 줄이려면 가장 효율적인 배포 전략을 사용하도록 프로젝트를 구성해야 합니다.

다음 단계를 사용하여 스토어에서 프로덕션에 업데이트를 배포하는 데 걸리는 시간을 줄일 수 있습니다.

1. [`ece-tools` 패키지로 업그레이드](../dev-tools/install-package.md) 또는 [`ece-tools` 버전 업데이트](../dev-tools/update-package.md)
최적의 배포를 구성하는 데 사용할 수 있는 도구를 사용할 수 있도록 Adobe Commerce on cloud infrastructure 프로젝트에 최신 `ece-tools` 패키지가 있어야 합니다. 최신 `ece-tools`이(가) 있는 경우 다음 단계를 계속합니다.

   >[!NOTE]
   >
   >최신 `ece-tools` 패키지를 사용하는 것이 모범 사례이지만 가동 중지 시간 없는 배포 메서드는 `ece-tools` [버전 2002.0.13](../release-notes/cloud-release-archive.md#v2002013) 이상에서 작동합니다.

1. [정적 콘텐츠 배포 구성](static-content.md)
배포 단계에서 정적 콘텐츠 배포가 실패하면 사이트가 유지 관리 모드에서 중단됩니다. 빌드 단계 중에 오류가 발생하면 프로세스가 배포 단계를 시작하지 않으므로 다운타임을 방지합니다. HTML이 축소된 빌드 단계 동안 정적 콘텐츠 생성[이상적인 상태라고도 하는 ](static-content.md#setting-the-scd-on-build)은(는) 가동 중지 시간이 없는 배포에 대한 최적의 구성이며 오류가 발생할 경우 _가동 중지 시간을 방지_&#x200B;합니다.

1. [배포 후 후크 구성](../application/hooks-property.md)
캐시를 정리하고 예열하도록 사후 배포 후크를 구성해야 합니다. 기본적으로 캐시 정리는 사이트가 다운된 배포 단계 동안 발생합니다. 캐시를 배치 후 단계로 이동하면 배치 단계가 완료될 때까지 캐시가 활성 상태로 유지되므로 캐시를 안전하게 정리할 수 있습니다.

   [WARM_UP_PAGES 환경 변수](../environment/variables-post-deploy.md#warmuppages)를 사용하여 캐시를 미리 로드하는 데 사용되는 페이지 목록을 사용자 지정합니다.

1. [테마 파일 줄이기](../environment/variables-deploy.md#scdmatrix)
SCD\_MATRIX 환경 변수를 구성하여 불필요한 테마 파일의 수를 줄일 수 있습니다.

1. [정적 콘텐츠 배포 속도 향상](../environment/variables-deploy.md#scdthreads)
SCD\_THREADS 환경 변수를 업데이트하여 정적 콘텐츠 배포의 스레드 수를 늘려 배포 프로세스 속도를 높일 수 있습니다.

>[!NOTE]
>
>[이상적인 상태 마법사를 실행](smart-wizards.md#verifying-an-ideal-configuration)하여 최적의 배포를 위해 프로젝트 구성의 유효성을 검사할 수 있습니다.
