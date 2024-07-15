---
title: 작업자
description: ' [!DNL Commerce] 응용 프로그램 구성 파일에서 작업자 속성을 구성하는 방법을 알아봅니다.'
feature: Cloud, Configuration
exl-id: d6816925-5912-45ca-8255-6c307e58542d
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '339'
ht-degree: 0%

---

# Workers 속성

Nginx 인스턴스를 실행하지 않고 웹 인스턴스와 독립적으로 실행할 작업자를 정의할 수 있지만 작업자는 [!DNL Commerce] 응용 프로그램에서 사용하는 것과 동일한 네트워크 저장소를 사용합니다. 라우터가 공용 요청을 작업자에게 전달할 수 없으므로 Node.js 또는 Go를 사용하여 작업자 인스턴스에 웹 서버를 설정할 필요가 없습니다. 따라서 작업자 인스턴스가 백그라운드 작업 또는 배포 차단의 위험이 있는 작업을 지속적으로 실행하는 데 이상적입니다.

## 작업자 구성

작업자는 Pro Staging 및 프로덕션 환경에서만 사용할 수 있습니다. Pro 통합 및 Starter 환경에서는 [CRON_CONSUMER_RUNNER](../environment/variables-deploy.md#cron_consumers_runner) 변수를 사용하도록 선택할 수 있습니다.

Pro 스테이징 또는 프로덕션에서 작업자를 구성하려면 [Adobe Commerce 지원 티켓을 제출](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)하고 다음 정보를 포함하십시오.

- 프로젝트 ID
- 환경 ID
- 작업자 이름
- 시작 명령

작업자당 하나의 프로세스를 구성할 수 있습니다. `.magento.app.yaml` 파일의 기본 일반 작업자 구성은 다음과 같습니다.

```yaml
workers:
    queue:
        commands:
            start: |
                php ./bin/magento queue:consumers:start commerce.eventing.event.publish
```

이 예제에서는 작은(크기 S) 수준의 리소스 할당을 사용하는 `queue`(이)라는 단일 작업자를 정의하고 시작 시 `php ./bin/magento` 명령을 실행합니다. 그러면 작업자 `queue`이(가) 각 노드에서 작업자 프로세스로 실행됩니다. 명령이 종료되면 자동으로 다시 시작됩니다.

## 명령 및 무시

작업자 응용 프로그램에서 명령을 실행하려면 `commands.start` 키가 필요합니다. 응용 프로그램의 언어를 사용하는 것이 이상적이지만 모든 유효한 셸 명령을 사용할 수 있습니다. `start` 키로 지정된 명령이 종료되면 자동으로 다시 시작됩니다.

>[!IMPORTANT]
>
>`deploy` 및 `post_deploy` 후크와 `crons` 명령은 작업자 인스턴스가 아닌 웹 컨테이너에서만 실행됩니다.

### 상속

`size`, `relationships`, `access`, `disk` 및 `mount` 및 `variables` 속성에 대한 정의는 명시적으로 재정의되지 않는 한 작업자가 상속합니다.

다음 속성은 [최상위 설정](properties.md)을 재정의하는 데 가장 일반적으로 사용됩니다.

- `size` - 단일 백그라운드 프로세스에 더 적은 수의 리소스를 할당
- `variables`—응용 프로그램을 다르게 실행하도록 지시합니다.

### 타이밍 및 큐

각 작업자는 다른 작업자의 대기열에 있지만, 다음 구성은 PHP 코드 내의 8초 휴지에 관계없이 `var/time.txt` 파일에서 일관성 있는 2초 간의 타임스탬프 분리를 생성합니다.

```yaml
workers:
    time1:
        commands:
            start: 'php -r "sleep(8); echo time() . PHP_EOL;" >> var/time.txt& sleep 2'
```
