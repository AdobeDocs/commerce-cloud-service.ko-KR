---
title: RabbitMQ 서비스 설정
description: RabbitMQ 서비스를 사용하여 클라우드 인프라에서 Adobe Commerce에 대한 메시지 대기열을 관리하는 방법을 알아봅니다.
feature: Cloud, Services
exl-id: 85794b8f-2260-4a6e-b5a6-a1b4c356594e
source-git-commit: d4c36b084094846cfad69adc2bffd567a58fab26
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 0%

---

# 설정 [!DNL RabbitMQ] 서비스

다음 [MQF(메시지 큐 프레임워크)](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/message-queues/message-queue-framework.html) 는 을 허용하는 Adobe Commerce 내의 시스템입니다. [모듈](https://glossary.magento.com/module) 메시지를 대기열에 게시합니다. 또한 비동기적으로 메시지를 수신하는 소비자도 정의합니다.

MQF는 [RabbitMQ](https://www.rabbitmq.com/) 는 메시지를 보내고 받는 확장 가능한 플랫폼을 제공하는 메시징 브로커입니다. 게재되지 않은 메시지를 저장하는 메커니즘도 포함됩니다. [!DNL RabbitMQ] 는 AMQP(고급 메시지 대기열 프로토콜) 0.9.1 사양을 기반으로 합니다.

>[!WARNING]
>
>다음과 같이 기존 AMQP 기반 서비스를 사용하는 것이 좋습니다. [!DNL RabbitMQ], Adobe Commerce on cloud infrastructure를 사용하여 자동으로 만드는 대신 [`QUEUE_CONFIGURATION`](../environment/variables-deploy.md#queue_configuration) 사이트에 연결하기 위한 환경 변수입니다.

{{service-instruction}}

**RabbitMQ을 활성화하려면**:

1. 필요한 이름, 유형 및 디스크 값(MB)을 `.magento/services.yaml` 설치된 RabbitMQ 버전과 함께 파일입니다.

   ```yaml
   rabbitmq:
       type: rabbitmq:<version>
       disk: 1024
   ```

1. 에서 관계 구성 `.magento.app.yaml` 파일.

   ```yaml
   relationships:
       rabbitmq: "rabbitmq:rabbitmq"
   ```

1. 코드 변경 사항을 추가, 커밋 및 푸시합니다.

   ```bash
   git add .magento/services.yaml .magento.app.yaml
   ```

   ```bash
   git commit -m "Enable RabbitMQ service"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. [서비스 관계 확인](services-yaml.md#service-relationships).

{{service-change-tip}}

## 디버깅을 위해 RabbitMQ에 연결

디버깅을 위해 다음 방법 중 하나로 서비스 인스턴스에 직접 연결하는 것이 유용합니다.

- 로컬 개발 환경에서 연결
- 응용 프로그램에서 연결
- PHP 응용 프로그램에서 연결

### 로컬 개발 환경에서 연결

1. 에 로그인합니다 `magento-cloud` CLI 및 프로젝트:

   ```bash
   magento-cloud login
   ```

1. RabbitMQ이 설치 및 구성된 환경을 확인하십시오.

   ```bash
   magento-cloud environment:checkout <environment-id>
   ```

1. SSH를 사용하여 클라우드 환경에 연결합니다.

   ```bash
   magento-cloud ssh
   ```

1. 에서 RabbitMQ 연결 세부 정보 및 로그인 자격 증명을 검색합니다. [$MAGENTO_클라우드_관계](../application/properties.md#relationships) 변수:

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   또는

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"])));'
   ```

   응답에서 RabbitMQ 정보를 찾습니다. 예를 들면 다음과 같습니다.

   ```json
   {
      "rabbitmq" : [
         {
            "password" : "guest",
            "ip" : "246.0.129.2",
            "scheme" : "amqp",
            "port" : 5672,
            "host" : "rabbitmq.internal",
            "username" : "guest"
         }
      ]
   }
   ```

1. RabbitMQ으로 로컬 포트 전달을 활성화합니다.

   ```bash
   ssh -L <port-number>:rabbitmq.internal:<port-number> <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

   다음 위치에서 RabbitMQ 관리 웹 인터페이스에 액세스하는 예제 `http://localhost:15672` 은(는)

   ```bash
   ssh -L 15672:rabbitmq.internal:15672 <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

1. 세션이 열려 있는 동안 로컬 워크스테이션에서 원하는 RabbitMQ 클라이언트를 시작할 수 있습니다. 이 클라이언트은에 연결하도록 구성되어 있습니다. `localhost:<portnumber>` Magento_CLOUD_RELATIONSHIPS 변수의 포트 번호, 사용자 이름 및 암호 정보 사용.

### 응용 프로그램에서 연결

애플리케이션에서 실행 중인 RabbitMQ에 연결하려면 다음과 같은 클라이언트를 설치합니다. [amqp-utils](https://github.com/dougbarth/amqp-utils)를 프로젝트 종속성으로 사용 `.magento.app.yaml` 파일.

For example,

```yaml
dependencies:
    ruby:
        amqp-utils: "0.5.1"
```

PHP 컨테이너에 로그인하면 `amqp-` 대기열을 관리하는 데 사용할 수 있는 명령입니다.

### PHP 응용 프로그램에서 연결

PHP 응용 프로그램을 사용하여 RabbitMQ에 연결하려면 PHP를 추가합니다 [라이브러리](https://glossary.magento.com/library) 소스 트리에 연결합니다.
