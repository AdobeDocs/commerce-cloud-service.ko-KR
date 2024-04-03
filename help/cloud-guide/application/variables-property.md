---
title: 변수 속성
description: variables 속성을 사용하여 저장소 구성 옵션을 사용자 지정합니다. [!DNL Commerce] 응용 프로그램.
feature: Cloud, Configuration
exl-id: 5cd92fbb-8bff-48b1-9658-500140591344
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '72'
ht-degree: 0%

---

# 변수 속성

애플리케이션 기반 환경 변수를 사용하여 저장소 구성을 사용자 지정할 수 있습니다. 이러한 변수는 특정 구문을 사용합니다. 다음을 참조하십시오 [구성 설정 재정의](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/paths/override-config-settings.html) 다음에서 _구성 안내서_.

에 포함된 다음 환경 변수 `.magento.app.yaml` 특정 버전의 [!DNL Commerce] 응용 프로그램.

Adobe Commerce 2.2.x - 2.3.x에 필요:

```yaml
variables:
    env:
        CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
        CONFIG__STORES__DEFAULT__PAYMENT__BRAINTREE__CHANNEL: 'Magento_Enterprise_Cloud_BT'
        CONFIG__STORES__DEFAULT__PAYPAL__NOTATION_CODE: 'Magento_Enterprise_Cloud'
```

Adobe Commerce 2.4.x의 경우 다음 변수를 설정합니다.

```yaml
variables:
    env:
        CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
        CONFIG__STORES__DEFAULT__PAYPAL__NOTATION_CODE: 'Magento_Enterprise_Cloud'
```
