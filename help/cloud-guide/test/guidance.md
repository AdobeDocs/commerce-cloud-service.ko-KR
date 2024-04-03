---
title: 테스트 지침
description: 클라우드 인프라에서 Adobe Commerce을 시작하기 위한 테스트 유형 및 모범 사례에 대해 알아보십시오.
exl-id: 1d7897a2-af97-46ab-89c0-2ec54284190e
source-git-commit: aae285d85188bfadd73d5b4e1fea16afa5beb117
workflow-type: tm+mt
source-wordcount: '348'
ht-degree: 0%

---

# 테스트 지침

클라우드 인프라 프로젝트에서 Adobe Commerce을 구성하고 맞춤화한 후 스토어 웹 사이트를 시작하기 전에 애플리케이션을 철저히 테스트하는 것이 좋습니다. 테스트는 클러스터 규모에 대한 기대치를 적절하게 관리하고 향후 비즈니스 요구에 맞게 적절히 확장할 수 있도록 도와줍니다.

## 기능 테스트

개발 중에 Adobe Commerce on cloud infrastructure 프로젝트에서 엔드 투 엔드 기능 테스트를 수행하는 것이 중요합니다. Docker 환경에서 기능 테스트를 수행하려면 다음 지침을 참조하십시오.

- **애플리케이션 테스트**- [Magento 기능 테스트 프레임워크(MTF)](https://developer.adobe.com/commerce/cloud-tools/docker/test/application-testing/) Cloud Docker 환경의 애플리케이션 테스트용.

- **코드 테스트**- [PHP용 codeception testing framework](https://developer.adobe.com/commerce/cloud-tools/docker/test/code-testing/) 클라우드 패키지 저장소에 기여하기 위한 코드 유효성 검사.

## 시작 전 우수 사례

사이트 시작 전에 수행하는 우수 사례로 다음 테스트 유형을 고려하십시오.

- **부하 테스트**- 부하 테스트를 수행하여 예상 부하에서 시스템의 동작을 이해합니다. 예를 들어, 애플리케이션에서 활성 사용자의 동시 수를 테스트하여 각 사용자가 설정된 기간 내에 특정 수의 트랜잭션을 수행하도록 합니다. 이 테스트에서는 데이터베이스 또는 애플리케이션 서버 비헤이비어와 같은 중요한 비즈니스 크리티컬 트랜잭션의 응답 시간을 보여 줍니다. 부하 테스트는 병목 현상을 식별하는 데 도움이 될 수 있습니다.

- **부하 시험**- 시스템 내의 용량 상한에 도전하여 현재 부하가 예상 최대값을 훨씬 초과할 때 시스템이 충분히 작동하는지 확인합니다.

- **보안 검사**- Adobe에서 무료 제공 [보안 검색 도구](../launch/overview.md#set-up-the-security-scan-tool) 사이트에 사용됩니다.

- **침투 시험**—시스템 보안을 평가하기 위해 고안된 컴퓨터 시스템에 대한 공인 모의 사이버 공격입니다. 침투 테스트는 권한이 없는 당사자가 시스템 기능 및 데이터에 액세스할 수 있는 가능성을 포함하여 취약점이나 취약점을 식별하는 데 도움이 됩니다.

>[!WARNING]
>
>고객은 AWS 인프라 또는 AWS 서비스 자체에 대한 보안 평가를 수행할 수 없습니다. 보안 평가에서 관찰된 AWS 서비스 내에서 보안 문제가 발견되면 [AWS 보안 문의](mailto:aws-security@amazon.com) 즉시. 다음을 참조하십시오 [침투 테스트를 위한 AWS 고객 정책](https://aws.amazon.com/security/penetration-testing/).
