---
title: PHP 설정
description: 클라우드 인프라에서 Commerce 애플리케이션 구성에 대한 최적의 PHP 설정에 대해 알아봅니다.
feature: Cloud, Configuration, Extensions
exl-id: b4180265-f7a1-48e4-8c23-27835253e171
source-git-commit: 94c1e16a07567471d446478e3bd2a33977247ef3
workflow-type: tm+mt
source-wordcount: '536'
ht-degree: 0%

---

# PHP 설정

`.magento.app.yaml` 파일에서 실행할 [PHP 버전](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html)을(를) 선택할 수 있습니다.

```yaml
name: mymagento
type: php:<version>
```

>[!TIP]
>
>PHP 8.1 이상으로 업그레이드하는 경우 `.magento.app.yaml` 파일의 [`runtime: extensions:` 속성](properties.md#runtime)에서 JSON을 제거하고 다시 배포하십시오. JSON 확장은 PHP 8.0 이후 Cloud 환경에 설치됩니다.

## PHP 구성

Adobe Commerce에서 유지 관리하는 구성에 추가된 `php.ini` 파일을 사용하여 환경에 대한 PHP 설정을 사용자 지정할 수 있습니다.

리포지토리에서 응용 프로그램의 루트(리포지토리 루트)에 `php.ini` 파일을 추가합니다.

>[!TIP]
>
>PHP 설정을 잘못 구성하면 문제가 발생할 수 있으므로 고급 관리자만 이러한 옵션을 설정해야 합니다.

### PHP 메모리 제한 늘이기

PHP 메모리 제한을 늘리려면 `php.ini` 파일에 다음 설정을 추가하십시오.

```ini
memory_limit = 1G
```

디버깅을 위해 값을 2G로 늘립니다.

### realpath_cache 구성 최적화

응용 프로그램 성능을 향상시키려면 다음 `realpath_cache` 설정을 설정하십시오.

```conf
;
; Increase realpath cache size
;
realpath_cache_size = 10M

;
; Increase realpath cache ttl
;
realpath_cache_ttl = 7200
```

이러한 설정을 사용하면 PHP 프로세스가 각 페이지 로드 시 경로를 조회하는 대신 파일에 경로를 캐시할 수 있습니다. PHP 설명서에서 [성능 조정](https://www.php.net/manual/en/ini.core.php)을 참조하십시오.

>[!NOTE]
>
>권장 PHP 구성 설정 목록은 _설치 가이드_&#x200B;의 [필수 PHP 설정](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/php-settings.html)을 참조하십시오.

### 사용자 정의 PHP 설정 확인

클라우드 환경에 대한 `php.ini` 변경 사항을 푸시한 후 사용자 지정 PHP 구성이 환경에 추가되었는지 확인할 수 있습니다. 예를 들어, SSH를 사용하여 원격 환경에 로그인하고 다음과 유사한 방법을 사용하여 파일을 볼 수 있습니다.

```bash
cat /etc/php/<php-version>/fpm/php.ini
```

>[!WARNING]
>
>로컬 개발에 Commerce용 Cloud Docker를 사용하는 경우 Docker 환경에서 사용자 지정 `php.ini` 파일을 사용하는 방법에 대한 자세한 내용은 [Docker 서비스 컨테이너](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#fpm-container)를 참조하십시오.

## 확장 활성화

`runtime:extension` 섹션에서 PHP 확장을 활성화하거나 비활성화할 수 있습니다. 또한 지정된 확장 프로그램은 Docker PHP 컨테이너에서 사용할 수 있습니다.

>[!IMPORTANT]
>
>확장을 활성화하기 전에 PHP 버전이 프로젝트를 호스팅하는 운영 체제와 호환되어야 한다는 것을 이해하는 것이 중요합니다. 계속하려면 인프라 팀에서 프로젝트 환경을 업그레이드해야 할 수 있습니다.

`.magento.app.yaml` 파일의 예:

```yaml
runtime:
    extensions:
        - sockets
        - sodium
        - ssh2
    disabled_extensions:
        - bcmath
        - bz2
        - calendar
        - exif
```

SSH를 사용하여 환경에 로그인하고 PHP 확장을 나열합니다.

```bash
php -m
```

특정 PHP 확장에 대한 자세한 내용은 [PHP 확장 목록](https://www.php.net/manual/en/extensions.alphabetical.php)을 참조하세요.

다음 표에서는 Adobe Commerce을 클라우드 플랫폼에 배포할 때 지원되는 PHP 확장을 보여 줍니다.

{{$include /help/_includes/templated/php-extensions-cloud.md}}

PHP 모듈 요구 사항은 Adobe Commerce 버전에 연결되어 있습니다. [PHP 요구 사항](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/php-settings.html)을 참조하세요.

### 확장 지원

Pro 프로젝트의 경우 다음 확장을 설치하려면 추가 지원이 필요합니다.

- `sourceguardian`

예를 들어, 모든 환경에서 SourceGuardian으로 보호된 스크립트만 실행하도록 PHP를 설정하려면 `php.ini` 파일에서 다음 옵션을 설정해야 합니다.

```ini
[SourceGuardian]
sourceguardian.restrict_unencoded = "1"
```

SourceGuardian 설명서의 [섹션 3.5를 참조하십시오](https://sourceguardian.com/demofiles/files/SourceGuardian%20for%20Linux%20User%20Manual.pdf). _PDF 링크_&#x200B;입니다.

[Adobe Commerce 지원 티켓을 제출](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)하여 모든 프로덕션 환경 및 Pro 스테이징 환경에서 이러한 PHP 확장을 설치하는 데 도움을 받으십시오. 업데이트된 `.magento/services.yaml` 파일, 업데이트된 PHP 버전 및 추가 PHP 확장명을 포함하는 `.magento.app.yaml` 파일을 포함하십시오. 라이브 프로덕션 환경을 변경하는 경우 최소 48시간 이상 알림을 제공해야 합니다. 클라우드 인프라 팀이 프로젝트를 업데이트하는 데 최대 48시간이 걸릴 수 있습니다.

>[!WARNING]
>
>디버그로 컴파일된 PHP는 지원되지 않으며 Probe가 [!DNL XDebug] 또는 [!DNL XHProf]과(와) 충돌할 수 있습니다. Probe를 활성화할 때 이러한 확장을 비활성화합니다. Probe가 [!DNL Pinba] 또는 IonCube와 같은 일부 PHP 확장과 충돌합니다.
