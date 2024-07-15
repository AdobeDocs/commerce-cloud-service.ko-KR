---
title: 기술 스택
description: 클라우드 인프라에서 Commerce을 구성하는 기술 스택을 참조하십시오.
feature: Cloud, Iaas, Paas
exl-id: e456db25-c44b-4053-b96d-517d3d1606d0
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '374'
ht-degree: 0%

---

# 기술 스택

아래와 같이 클라우드 인프라의 Adobe Commerce을 5개의 기능 계층으로 생각해 보십시오.

![클라우드 스택](../../assets/CloudStack.svg)

1. [**클라우드 인프라**](pro-architecture.md): Adobe Commerce on cloud infrastructure Pro 프로젝트를 위한 IaaS(Infrastructure as a Service) 기반으로 Amazon Web Services(AWS) 또는 Microsoft Azure를 선택합니다.

   Adobe은 정기적으로 가상 컴퓨팅 리소스(vCPU) 사용을 분석하고 리소스를 자동으로 할당하여 장기 사용을 최적화하고 최대 연간 vCPU 일일 허용 시간을 초과할 위험을 완화합니다. 특정 기간 동안 사이트 트래픽이 증가할 것으로 예상되면 [임시 업사이징을 요청](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/how-to-request-temporary-magento-upsize.html)할 지원 티켓을 계속 열어야 합니다.

1. [**Platform as a Service**](cloud-architecture.md): 클라우드 인프라 프로젝트의 각 Adobe Commerce은 서비스 개발, 테스트 및 통합을 위한 PaaS(Platform as a Service) 통합 환경을 제공합니다.
1. [**Adobe Commerce**](../project/overview.md): 클라우드 인프라의 Adobe Commerce은 PHP, MySQL(MariaDB), Redis, [!DNL RabbitMQ] 및 지원되는 검색 엔진 기술을 포함하는 사전 제공된 인프라를 제공합니다.
1. [**성능 도구**](../monitor/new-relic-service.md): New Relic 성능 도구를 사용하면 Adobe Commerce에서 클라우드 인프라 프로젝트를 통해 데이터를 수집, 분석 및 표시하여 응용 프로그램과 인프라를 디버그, 모니터링 및 관리할 수 있습니다.
1. [**CDN(콘텐츠 전송 네트워크), 웹 응용 프로그램 방화벽([!DNL WAF]) 및 이미지 최적화(IO)**](../cdn/fastly.md):

   * [Fastly CDN](../cdn/fastly.md#ddos-protection) - [!DNL Ping of Death], [!DNL Smurf] 공격 및 기타 ICMP(Internet Control Message Protocol) 기반 플러드 공격과 같은 DDoS(Distributed Denial of Service) 공격으로부터 안전하게 보호하는 CDN 서비스를 제공합니다.
   * [WAF(Web Application Firewall)](../cdn/fastly-waf-service.md)—WAF 서비스는 프로덕션 환경의 Adobe Commerce 상점 및 WAF 정책에 대한 PCI 규정 준수를 보장하여 주입 공격, 악의적인 입력, 교차 사이트 스크립팅, 데이터 내보내기, HTTP 프로토콜 위반 및 기타 [[!DNL OWASP] 10대 보안 위협](https://owasp.org/www-project-top-ten/)으로부터 Adobe Commerce 웹 애플리케이션을 보호합니다.
   * [이미지 최적화(IO)](../cdn/fastly-image-optimization.md) - 실시간 이미지 조작 및 최적화를 제공하여 이미지 전달 속도를 높이고 응답형 웹 애플리케이션의 이미지 소스 세트 유지 관리를 간소화합니다. 신속한 IO는 이미지 처리 및 크기 조정 부하를 분산시켜 서버가 주문 및 전환을 효율적으로 처리할 수 있도록 합니다.

모노리식 응용 프로그램은 리소스 집약적이고 확장과 신속한 서비스가 어렵습니다. Commerce 고객은 클라우드 인프라를 통해 풍부하고 지능적이며 성능이 뛰어난 SaaS 기반 마이크로서비스에 매우 쉽게 액세스할 수 있습니다. [지원되는 소프트웨어 및 서비스](cloud-architecture.md#supported-software-and-services)를 참조하십시오.

[Commerce 시작 안내서](../../get-started/overview.md)를 사용하여 새 클라우드 프로그램을 설정하고 클라우드 기반 환경에서 [!DNL Commerce] 응용 프로그램을 관리할 수 있습니다.
