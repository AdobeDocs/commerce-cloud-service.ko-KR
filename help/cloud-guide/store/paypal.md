---
title: PayPal 결제 방법 설정
description: 클라우드 인프라에서 Adobe Commerce에 대한 PayPal 결제 방법을 설정합니다.
feature: Cloud, Checkout, Payments
exl-id: e52fd719-f936-4e8b-8222-af133389d9e2
source-git-commit: aa1a334ca1383559194ca75247679c6fb5411802
workflow-type: tm+mt
source-wordcount: '669'
ht-degree: 0%

---

# PayPal 결제 방법 설정

Adobe Commerce on cloud infrastructure는 관리자를 통해 직접 PayPal Express 체크아웃 계정을 구성할 수 있는 온보딩 도구를 제공합니다. 이 도구는 ECE 2.1.8 이상에서 사용할 수 있습니다. 라이브 진행 및 PayPal 결제 방법 테스트를 지원하기 위해 샌드박스 또는 프로덕션 계정에 대해 PayPal Express 체크아웃 계정을 활성화하고 구성할 수 있습니다.

모든 환경에서 샌드박스 또는 프로덕션 계정을 구성할 수 있습니다.

* 통합 및 스테이징 환경의 경우 샌드박스 자격 증명을 설정합니다.
* 프로덕션 환경의 경우, 초기 테스트를 위해 샌드박스 자격 증명을 설정한 다음 실행된 스토어에 대한 라이브 프로덕션 자격 증명으로 대체합니다.

## PayPal 계정

준비 및 구성된 PayPal 판매자 계정을 사용하는 것이 가장 좋지만 관리자를 통해 계정을 만들거나 개인 계정을 업그레이드할 수 있습니다.

[!DNL PayPal onboarding] 는 다음 계정과의 연결을 지원합니다.

* PayPal 비즈니스 계정
* PayPal 개인 계정, 비즈니스 계정으로 전환. 기존 개인 PayPal 계정이 있는 경우 해당 자격 증명으로 로그인하고 동기화를 완료할 때 이 계정을 비즈니스 계정으로 업그레이드할 수 있습니다.

기존 PayPal 계정이 없는 경우 계정을 만드십시오. 새 계정의 이메일 주소를 입력합니다. 일치하는 PayPal 계정을 찾을 수 없으면 메시지에 따라 PayPal 비즈니스 계정을 만듭니다. 또는 다음을 통해 직접 계정을 만들 수 있습니다. [PayPal](https://www.paypal.com/us/webapps/mpp/account-selection).

### PayPal 제한

PayPal은 다음과 같은 제한 사항을 제외하고 전 세계 국가를 대상으로 PayPal Express Checkout 연결을 지원합니다.

* 인도, 일본(향후 PayPal 업데이트를 통해 이러한 계정을 지원할 수 있음)
* 이스라엘

브라질의 경우 연결할 기존 PayPal 비즈니스 계정이 있어야 합니다. 이 프로세스 중에는 브라질의 기존 개인 PayPal 계정을 전환할 수 없습니다. 계좌가 필요하시면, [비즈니스 PayPal 계정 만들기](https://www.paypal.com/us/webapps/mpp/account-selection).

## PayPal Express 체크아웃 구성

PayPal Express 체크아웃을 구성하려면:

1. 환경의 관리자에 액세스합니다.
1. 왼쪽 탐색에서 을 선택합니다. **스토어** > **구성**&#x200B;을 선택한 다음 을 선택합니다. **판매** > **결제 방법**.
1. PayPal에서 다음을 선택합니다. **구성**. 구성 필드는 빠른 체크아웃, PayPal 크레딧 광고, 기본 및 고급 설정에 대한 확장 가능한 섹션에 표시됩니다.
1. PayPal 계정을 연결합니다. 계정이 연결될 때까지 활성화 옵션이 비활성화됩니다. 연결할 수 있는 계정 및 지원되는 계정 및 제한 사항에 대한 자세한 내용은 [PayPal 계정](#paypal-account).

   * PayPal Live 계정을 연결하려면 PayPal로 연결 을 클릭하고 안내를 따릅니다. 라이브 PayPal을 사용하여 구매하는 모든 고객은 라이브 스토어에서 고객에게 적극적으로 요금을 부과합니다.
   * 테스트를 위해 샌드박스 계정을 연결하려면 샌드박스 자격 증명 을 클릭하고 프롬프트에 따릅니다. Sandbox PayPal을 사용하여 구매한 모든 고객은 적극적으로 고객에게 비용을 청구하지 않고도 완료할 수 있습니다.

1. PayPal API를 인증하고 사용하려면 빠른 체크아웃 설정을 구성하십시오.

   * **PayPal 판매자 계정과 연결된 이메일** (선택 사항) PayPal 판매자 계정과 연결된 이메일 주소를 입력합니다. 이 이메일은 대소문자를 구분합니다.
   * **API 인증 방법** API 서명 또는 API 인증서로 사용됩니다.
   * PayPal 계정에서 캡처한 API 사용자 이름, 암호 및 서명.
   * **샌드박스 모드** 입력한 자격 증명이 샌드박스에 대한 것인지 여부를 나타내려면 예 또는 아니오를 선택합니다. 프로덕션 자격 증명을 입력한 경우 아니오를 선택합니다.
   * **API에서 프록시 사용** 시스템에서 프록시 서버를 사용하여 Adobe Commerce과 PayPal 결제 시스템 간의 연결을 설정하는 경우 예 또는 아니오를 선택합니다. [예]일 경우 프록시 호스트 및 포트를 입력합니다.

1. 계정 구성에 대한 자세한 내용 및 단계는 를 참조하십시오. [PayPal Express 체크아웃](https://docs.magento.com/user-guide/payment/paypal-express-checkout.html) 2단계부터 필요한 설정을 완료합니다.

계정이 구성 및 인증되면 필수 PayPal 설정에서 PayPal 결제 옵션을 활성화 및 비활성화할 수 있습니다.

* **이 솔루션 사용** 웹 사이트를 통해 고객에게 PayPal 결제 방법을 표시합니다.
* **컨텍스트 내 체크아웃 경험 활성화**
* **PayPal 크레딧 활성화** 고객이 추가 비용 없이 PayPal 크레딧 금융을 할 수 있도록 허용합니다. PayPal은 주문금액을 선불로 지불하며 고객과 직접 크레딧에 대한 모든 상환을 처리합니다.

## PayPal 변수

클라우드 인프라에서 Adobe Commerce과 함께 PayPal 온보딩 도구를 사용할 때 다음 변수를 `variables:env` 의 섹션 `magento.app.yaml` 파일.

```yaml
# Environment variables
variables:
  env:
    CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
```

2.1.8 이상에서 2.2로 업그레이드하는 경우에도 이 변수를 추가해야 합니다.
