---
title: 로그 핸들러
description: 클라우드 인프라에서 Adobe Commerce에 대한 로그 처리기를 구성하는 방법에 대해 알아봅니다.
feature: Cloud, Logs, Configuration
role: Developer
exl-id: d3be7b6d-5778-4c32-865b-31bdb2852a23
source-git-commit: f8e35ecff4bcafda874a87642348e2d2bff5247b
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 0%

---

# 로그 핸들러

원격 로깅 서버로 메시지를 보내도록 로그 처리기를 구성할 수 있습니다. 로그 처리기는 Slack 및 전자 메일에 로그를 푸시하는 방식과 유사하게 빌드 및 배포 로그를 다른 시스템에 푸시합니다. 하드웨어와 관련된 메시지를 로깅하는 데 적합한 _syslog_ 핸들러나 소프트웨어 응용 프로그램에서 메시지를 로깅하는 데 적합한 GELF(Graylog Extended Log Format) 핸들러를 사용하도록 설정할 수 있습니다.

다음 예제에서는 `.magento.env.yaml` 파일에 구성을 추가하여 이 두 처리기를 모두 구성합니다. 최소 로깅 수준(`min_level`) 값은 [로그 수준](#log-levels)을 참조하십시오.

```yaml
log:
  syslog:
    ident: "<syslog-ident>"
    facility: 8 # https://php.net/manual/en/network.constants.php
    min_level: "info"
    logopts: <syslog-logopts>

  syslog_udp:
    host: "<syslog-host>"
    port: <syslog-port>
    facility: 8  # https://php.net/manual/en/network.constants.php
    ident: "<syslog-ident>"
    min_level: "info"

  gelf:
    min_level: "info"
    use_default_formatter: true
    additional: # Some additional information for each log message
      project: "<some-project-id>"
      app_id: "<some-app-id>"
    transport:
      http:
        host: "<http-host>"
        port: <http-port>
        path: "<http-path>"
        connection_timeout: 60
      tcp:
        host: "<tcp-host>"
        port: <tcp-port>
        connection_timeout: 60
      udp:
        host: "<udp-host>"
        port: <udp-port>
        chunk_size: 1024
```

## 로그 수준

로그 수준은 알림 메시지의 세부 정보 수준을 결정합니다. 다음 로그 수준 카테고리에는 그 아래의 모든 로그 수준이 포함됩니다. 예를 들어 `debug` 수준에는 모든 수준의 로깅이 포함되지만 `alert` 수준에는 경고 및 응급 상황만 표시됩니다.

- **debug**—자세한 디버그 정보
- **info**—사용자 로그인 또는 SQL 로그와 같은 흥미로운 이벤트
- **알림**—일반적이지만 중요한 이벤트
- **경고**—더 이상 사용되지 않는 API의 사용 또는 API의 잘못된 사용과 같이 오류가 아닌 예외적인 발생 횟수
- **오류**—즉각적인 작업이 필요하지 않은 런타임 오류
- **중요** - 사용할 수 없는 응용 프로그램 구성 요소나 예기치 않은 예외와 같은 중요한 조건입니다.
- **경고** - SMS 경고를 트리거하는 즉각적인 조치(예: 웹 사이트가 다운되었거나 데이터베이스를 사용할 수 없음)가 필요합니다.
- **긴급**—시스템을 사용할 수 없습니다.
