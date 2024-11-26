---
title: RabbitMQ 서비스 설정
description: RabbitMQ 서비스를 사용하여 클라우드 인프라에서 Adobe Commerce에 대한 메시지 대기열을 관리하는 방법을 알아봅니다.
feature: Cloud, Services
exl-id: 85794b8f-2260-4a6e-b5a6-a1b4c356594e
source-git-commit: 269681efb9925d78ffb608ecbef657be740b5531
workflow-type: tm+mt
source-wordcount: '398'
ht-degree: 0%

---

# [!DNL RabbitMQ] 서비스 설정

[MQF(메시지 큐 프레임워크)](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/message-queues/message-queue-framework.html)은(는) [모듈](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/glossary#module)이(가) 메시지를 큐에 게시할 수 있도록 하는 Adobe Commerce 내의 시스템입니다. 또한 비동기적으로 메시지를 수신하는 소비자도 정의합니다.

MQF는 [RabbitMQ](https://www.rabbitmq.com/)을(를) 메시징 브로커로 사용하여 메시지를 보내고 받는 확장 가능한 플랫폼을 제공합니다. 게재되지 않은 메시지를 저장하는 메커니즘도 포함됩니다. [!DNL RabbitMQ]은(는) AMQP(고급 메시지 대기열 프로토콜) 0.9.1 사양을 기반으로 합니다.

>[!WARNING]
>
>[!DNL RabbitMQ]과(와) 같은 기존 AMQP 기반 서비스를 사용하는 경우 클라우드 인프라의 Adobe Commerce을 사용하여 서비스를 만드는 대신 [`QUEUE_CONFIGURATION`](../environment/variables-deploy.md#queue_configuration) 환경 변수를 사용하여 사이트에 연결합니다.

{{service-instruction}}

**RabbitMQ을 사용하려면**:

1. 설치된 RabbitMQ 버전과 함께 필요한 이름, 유형 및 디스크 값(MB)을 `.magento/services.yaml` 파일에 추가합니다.

   ```yaml
   rabbitmq:
       type: rabbitmq:<version>
       disk: 1024
   ```

1. `.magento.app.yaml` 파일에서 관계를 구성합니다.

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

1. [서비스 관계를 확인](services-yaml.md#service-relationships)합니다.

{{service-change-tip}}

## 디버깅을 위해 RabbitMQ에 연결

디버깅을 위해 다음 방법 중 하나로 서비스 인스턴스에 직접 연결하는 것이 유용합니다.

- 로컬 개발 환경에서 연결
- 응용 프로그램에서 연결
- PHP 응용 프로그램에서 연결

### 로컬 개발 환경에서 연결

1. `magento-cloud` CLI 및 프로젝트에 로그인합니다.

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

1. [$CLOUD_RELATIONSHIPS](../application/properties.md#relationships) 변수에서 RabbitMQ MAGENTO 연결 세부 정보 및 로그인 자격 증명을 검색합니다.

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

1. RabbitMQ으로 로컬 포트 전달을 활성화합니다(프로젝트가 US-3, EU-5 또는 AP-3 지역과 같은 다른 지역에 있는 경우 ``us``을(를) ``us-3``/``eu-5``/``ap-3``(으)로 대체)

   ```bash
   ssh -L <port-number>:rabbitmq.internal:<port-number> <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

   `http://localhost:15672`에서 RabbitMQ 관리 웹 인터페이스에 액세스하는 예는 다음과 같습니다.

   ```bash
   ssh -L 15672:rabbitmq.internal:15672 <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

1. 세션이 열려 있는 동안 로컬 워크스테이션에서 원하는 RabbitMQ 클라이언트를 시작할 수 있습니다. MAGENTO_CLOUD_RELATIONSHIPS 변수의 포트 번호, 사용자 이름 및 암호 정보를 사용하여 `localhost:<portnumber>`에 연결하도록 구성되었습니다.

### 응용 프로그램에서 연결

응용 프로그램에서 실행 중인 RabbitMQ에 연결하려면 `.magento.app.yaml` 파일에 프로젝트 종속성으로 [amqp-utils](https://github.com/dougbarth/amqp-utils)과(와) 같은 클라이언트를 설치합니다.

For example,

```yaml
dependencies:
    ruby:
        amqp-utils: "0.5.1"
```

PHP 컨테이너에 로그인하면 대기열을 관리하는 데 사용할 수 있는 `amqp-` 명령을 입력합니다.

### PHP 응용 프로그램에서 연결

PHP 애플리케이션을 사용하여 RabbitMQ에 연결하려면 소스 트리에 PHP 라이브러리를 추가합니다.
