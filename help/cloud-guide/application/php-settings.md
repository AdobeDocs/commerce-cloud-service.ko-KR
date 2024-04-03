---
title: PHP 설정
description: 클라우드 인프라에서 Commerce 애플리케이션 구성에 대한 최적의 PHP 설정에 대해 알아봅니다.
feature: Cloud, Configuration, Extensions
exl-id: b4180265-f7a1-48e4-8c23-27835253e171
source-git-commit: 9b3772cf640ebc56063434e1aa8acb1ec51dc63c
workflow-type: tm+mt
source-wordcount: '553'
ht-degree: 0%

---

# PHP 설정

선택할 수 있습니다. [PHP 버전](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html) 을(를) 실행하여 `.magento.app.yaml` 파일:

```yaml
name: mymagento
type: php:<version>
```

>[!TIP]
>
>PHP 8.1 이상으로 업그레이드하는 경우 [`runtime: extensions:` 속성](properties.md#runtime) 다음에서 `.magento.app.yaml` 파일 및 재배포. JSON 확장은 PHP 8.0 이후 Cloud 환경에 설치됩니다.

## PHP 구성

를 사용하여 환경에 맞게 PHP 설정을 사용자 지정할 수 있습니다. `php.ini` Adobe Commerce에서 유지 관리하는 구성에 추가되는 파일입니다.

저장소에서 를 추가합니다. `php.ini` 파일을 애플리케이션의 루트(저장소 루트)에 저장합니다.

>[!TIP]
>
>PHP 설정을 잘못 구성하면 문제가 발생할 수 있으므로 고급 관리자만 이러한 옵션을 설정해야 합니다.

### PHP 메모리 제한 늘이기

PHP 메모리 제한을 늘리려면 다음 설정을 `php.ini` 파일:

```ini
memory_limit = 1G
```

디버깅을 위해 값을 2G로 늘립니다.

### realpath_cache 구성 최적화

다음 설정 `realpath_cache` 응용 프로그램 성능을 개선하기 위한 설정입니다.

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

이러한 설정을 사용하면 PHP 프로세스가 각 페이지 로드 시 경로를 조회하는 대신 파일에 경로를 캐시할 수 있습니다. 다음을 참조하십시오 [성능 조정](https://www.php.net/manual/en/ini.core.php) PHP 설명서에서 참조하십시오.

>[!NOTE]
>
>권장 PHP 구성 설정 목록은 다음을 참조하십시오. [필수 PHP 설정](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/php-settings.html) 다음에서 _설치 안내서_.

### 사용자 정의 PHP 설정 확인

을(를) 푸시한 후 `php.ini` 클라우드 환경을 변경하면 사용자 정의 PHP 구성이 환경에 추가되었는지 확인할 수 있습니다. 예를 들어, SSH를 사용하여 원격 환경에 로그인하고 다음과 유사한 방법을 사용하여 파일을 볼 수 있습니다.

```bash
cat /etc/php/<php-version>/fpm/php.ini
```

>[!WARNING]
>
>로컬 개발에 Cloud Docker for Commerce를 사용하는 경우 다음을 참조하십시오. [Docker 서비스 컨테이너](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#fpm-container) 사용자 지정 사용에 대한 자세한 내용 `php.ini` Docker 환경의 파일입니다.

## 확장 활성화

에서 PHP 확장을 활성화하거나 비활성화할 수 있습니다. `runtime:extension` 섹션. 또한 지정된 확장 프로그램은 Docker PHP 컨테이너에서 사용할 수 있습니다.

>[!IMPORTANT]
>
>확장을 활성화하기 전에 PHP 버전이 프로젝트를 호스팅하는 운영 체제와 호환되어야 한다는 것을 이해하는 것이 중요합니다. 계속하려면 인프라 팀에서 프로젝트 환경을 업그레이드해야 할 수 있습니다.

의 예 `.magento.app.yaml` 파일:

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

특정 PHP 확장에 대한 자세한 내용은 [PHP 확장 목록](https://www.php.net/manual/en/extensions.alphabetical.php).

다음 표에서는 Adobe Commerce을 클라우드 플랫폼에 배포할 때 지원되는 PHP 확장을 보여 줍니다.

| 기본 확장 | 설치된 확장<br>제거할 수 없습니다. | 설치할 수 있는 확장<br>필요에 따라 제거됨 |
| ------------------ | --------------------- | --------------------- |
| `bcmath`<br>`bz2`<br>`calendar`<br>`exif`<br>`gd`<br>`gettext`<br> `intl`<br> `mysqli`<br> `openswoole`<br> `pcntl`<br> `pdo_mysql`<br> `soap`<br> `sockets`<br>  `sysvmsg`<br> `sysvsem`<br> `sysvshm`<br> `opcache`<br> `zip` | `ctype`<br> `curl`<br>`date`<br> `dom`<br> `fileinfo`<br> `filter`<br> `ftp`<br> `hash`<br> `iconv`<br> `json`<br> `mbstring`<br> `mysqlnd`<br> `openssl`<br> `pcre`<br> `pdo`<br> `pdo_sqlite`<br> `phar`<br>`posix`<br> `readline`<br> `session`<br> `sqlite3`<br> `tokenizer`<br> `xml`<br> `xmlreader`<br> `xmlwriter`<br> | `geoip`<br>`gmp`<br> `igbinary`<br> `imagick`<br> `imap`<br> `ioncube` <br>`ldap`<br> `mailparse`<br> `mcrypt`<br> `msgpack`<br> `mysqli`<br> `oauth`<br> `pdo_mysql`<br> `propro`<br> `pspell`<br> `raphf`<br> `recode`<br> `redis`<br> `shmop` `sockets`<br> `sodium`<br> `ssh2`<br>`tidy`<br> `xdebug`<br> `xmlrpc`<br> `xsl`<br> `yaml` |

PHP 모듈 요구 사항은 Adobe Commerce 버전에 연결되어 있습니다. 다음을 참조하십시오 [PHP 요구 사항](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/php-settings.html).

### 확장 지원

Pro 프로젝트의 경우 다음 확장을 설치하려면 추가 지원이 필요합니다.

- `sourceguardian`

예를 들어, 모든 환경에서 SourceGuardian으로 보호된 스크립트만 실행하도록 PHP를 설정하려면 `php.ini` 파일:

```ini
[SourceGuardian]
sourceguardian.restrict_unencoded = "1"
```

다음을 참조하십시오 [sourceGuardian 설명서의 섹션 3.5](https://sourceguardian.com/demofiles/files/SourceGuardian%20for%20Linux%20User%20Manual.pdf). _PDF 링크입니다._.

[Adobe Commerce 지원 티켓 제출](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) 모든 프로덕션 환경 및 Pro 스테이징 환경에서 이러한 PHP 확장을 설치하는 데 도움이 되도록 합니다. 업데이트된 항목 포함 `.magento/services.yaml` 파일, `.magento.app.yaml` 업데이트된 PHP 버전과 추가 PHP 확장명이 포함된 파일입니다. 라이브 프로덕션 환경을 변경하는 경우 최소 48시간 이상 알림을 제공해야 합니다. 클라우드 인프라 팀이 프로젝트를 업데이트하는 데 최대 48시간이 걸릴 수 있습니다.

>[!WARNING]
>
>디버그로 컴파일된 PHP는 지원되지 않으며 Probe가 [!DNL XDebug] 또는 [!DNL XHProf]. Probe를 활성화할 때 이러한 확장을 비활성화합니다. 프로브는 다음과 같은 일부 PHP 확장과 충돌합니다. [!DNL Pinba] 또는 IonCube입니다.
