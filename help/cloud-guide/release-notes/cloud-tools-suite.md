---
title: Cloud Tools 제품군 릴리스 정보
description: Adobe Commerce용 Cloud Tools 제품군의 최신 개선 사항에 대해 알아봅니다.
feature: Cloud, Release Notes
exl-id: 6a652e93-46a2-4590-97fc-fb5d114ece9a
source-git-commit: e04a9de8f0e31098f0cc2e47112f206c11a0e23b
workflow-type: tm+mt
source-wordcount: '195'
ht-degree: 1%

---

# Commerce Cloud 도구 세트에 대한 릴리스 노트

이 릴리스 정보는 Commerce 플랫폼에서의 Adobe Commerce 설치 및 업그레이드를 배포하고 관리하도록 설계된 Cloud Tools Suite for Cloud Platform 패키지에 대한 최신 개선 사항을 자세히 설명합니다.

| 릴리스 정보 | 버전 | 설명 | Source |
| ----------------- |-----------| ---------------------------------------- | --------------------------- |
| [`ece-tools` 패키지](ece-tools-package.md) | 2002.1.19 | 클라우드 프로젝트를 관리 및 배포하도록 설계된 스크립트 및 도구 세트 | [`magento/ece-tools`](https://github.com/magento/ece-tools/tree/2002.1) |
| Commerce용 [클라우드 패치](cloud-patches.md) | 1.0.27 | 모든 Adobe Commerce 버전과 클라우드 환경의 통합을 개선하는 패치 세트입니다. 이 패키지에는 `ece-tools`을(를) 사용하여 배포할 때 적용되는 Adobe Commerce 패치와 사용 가능한 핫픽스가 포함되어 있습니다. | [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches/tree/1.0.1) |
| Commerce용 [Cloud Docker](cloud-docker.md) | 1.3.7 | Adobe Commerce을 로컬 클라우드 환경에 배포하기 위한 도커 이미지용 기능 및 구성 파일 | [`magento/magento-cloud-docker`](https://github.com/magento/magento-cloud-docker/tree/1.0) |
| [Commerce의 클라우드 구성 요소](cloud-components.md) | 1.0.14 | 클라우드 인프라에 배포된 사이트를 위한 확장된 Adobe Commerce 핵심 기능 | [`magento/magento-cloud-components`](https://github.com/magento/magento-cloud-components/tree/1.0.2) |

ECE-Tools 2002.1.0 이상으로 업데이트하면 `ece-tools` 패키지에 종속된 다른 패키지의 최신 버전으로 자동 업데이트됩니다. 종속성 목록은 [클라우드 메타패키지](../development/overview.md#cloud-metapackage)를 참조하세요.
