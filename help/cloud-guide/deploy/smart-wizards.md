---
title: 스마트 마법사
description: 스마트 마법사를 사용하여 Adobe Commerce on cloud infrastructure 프로젝트가 배포 모범 사례를 준수하는지 평가하는 방법을 알아봅니다.
feature: Cloud, Build, Deploy, SCD
exl-id: eb79431c-8835-4ae4-b453-9c4932c5d5ac
source-git-commit: 225fba1acfd8b3ce4d7ce989c7851e7b0b218680
workflow-type: tm+mt
source-wordcount: '322'
ht-degree: 0%

---

# 스마트 마법사

스마트 마법사는 클라우드 구성이 모범 사례를 따르는지 여부를 확인하는 데 도움이 됩니다. 사용 가능한 마법사는 다음 구성을 지원합니다.

- 배포 가동 중지 시간을 최소화하는 데 이상적인 상태
- 데이터베이스 및 Redis에 대한 로드 밸런싱 구성
- 온디맨드, 빌드 단계 또는 배포 단계에 대한 정적 콘텐츠 배포(SCD)

각 스마트 마법사 명령은 올바른 구성에 대한 권장 사항과 확인 응답을 제공합니다(해당하는 경우).

| 명령 | 설명 |
| ------- | ------------|
| `wizard:ideal-state` | SCD가 _build_ 단계에 있고 `SKIP_HTML_MINIFICATION` 변수가 `true`이고 post_deploy 후크가 클라우드 환경에 구성되어 있는지 확인하십시오. 로컬 개발 환경에서 사용할 수 없습니다. |
| `wizard:master-slave` | `REDIS_USE_SLAVE_CONNECTION` 변수와 `MYSQL_USE_SLAVE_CONNECTION` 변수가 `true`인지 확인하십시오. |
| `wizard:scd-on-demand` | `SCD_ON_DEMAND` 전역 환경 변수가 `true`인지 확인하십시오. |
| `wizard:scd-on-build` | _build_ 단계에 대해 `SCD_ON_DEMAND` 전역 환경 변수가 `false`이고 `SKIP_SCD` 환경 변수가 `false`인지 확인하십시오. `config.php` 파일에 저장소, 저장소 그룹 및 웹 사이트에 대한 정보가 포함되어 있는지 확인합니다. |
| `wizard:scd-on-deploy` | _배포_ 단계에 대해 `SCD_ON_DEMAND` 전역 환경 변수가 `false`이고 `SKIP_SCD` 환경 변수가 `false`인지 확인하십시오. `config.php` 파일에 관련 정보가 있는 저장소, 저장소 그룹 및 웹 사이트 목록이 포함되어 있는지 _NOT_&#x200B;을(를) 확인합니다. |

예를 들어 구성이 SCD 온디맨드 기능을 제대로 활성화하는지 확인할 수 있습니다.

```bash
./vendor/bin/ece-tools wizard:scd-on-demand
```

성공적인 구성은 다음을 반환합니다.

```terminal
SCD on-demand is enabled
```

실패한 구성은 다음을 반환합니다.

```terminal
SCD on-demand is disabled
```

## 이상적인 구성 확인

클라우드 프로젝트에 대한 _이상적인_ 구성은 사용자가 요청할 때 캐시를 예열하고 정적 콘텐츠를 생성하여 배포 중단 시간을 최소화하는 데 도움이 됩니다. 이 마법사는 배포 프로세스 중에 자동으로 실행됩니다. 클라우드가 이 _이상적인 상태_&#x200B;에 대해 구성되지 않은 경우 다음과 유사한 메시지가 표시됩니다.

```terminal
- SCD on build is not configured
- Post-deploy hook is not configured
- Skip HTML minification is disabled

Ideal state is not configured
```

출력을 기반으로 구성에 대해 다음과 같이 수정해야 합니다.

1. HTML 건너뛰기 축소 변수를 활성화합니다.

   > .magento.env.yaml

   ```yaml
   stage:
     global:
       SKIP_HTML_MINIFICATION: true
   ```

1. 사후 배포 후크를 구성합니다.

   > .magento.app.yaml

   ```yaml
       post_deploy: |
           php ./vendor/bin/ece-tools post-deploy
   ```

1. 코드 변경 사항을 푸시하고 테스트를 다시 실행합니다. 구성이 _이상적_&#x200B;인 경우 다음 메시지가 표시됩니다.

   ```terminal
   Ideal state is configured
   ```
