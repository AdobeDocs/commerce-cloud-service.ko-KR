---
title: Cloud Docker 패키지
description: Cloud Docker 패키지에 대한 최신 개선 사항 목록을 참조하십시오.
feature: Cloud, Docker, Release Notes
recommendations: noDisplay, catalog
last-substantial-update: 2024-04-08T00:00:00Z
exl-id: 907d977f-2e9c-4553-a46b-000bc6a57b28
source-git-commit: bc76cba0219f16fd055c20289811b51c35c9b026
workflow-type: tm+mt
source-wordcount: '3662'
ht-degree: 0%

---

# Cloud Docker 패키지

[`magento/magento-cloud-docker`](https://github.com/magento/magento-cloud-docker) 패키지는 Adobe Commerce을 로컬 클라우드 환경에 배포하는 기능 및 도커 이미지를 제공합니다. 이 릴리스 노트는 [Commerce용 Cloud Tools 제품군](cloud-tools-suite.md)의 구성 요소인 이 패키지에 대한 최신 개선 사항을 설명합니다.

`magento/magento-cloud-docker` 패키지에서 다음 버전 시퀀스를 사용합니다. `<major>.<minor>.<patch>`

릴리스 노트는 다음과 같습니다.

- ![새 아이콘](../../assets/new.svg) 새 기능
- ![수정 아이콘](../../assets/fix.svg) 수정 사항 및 개선 사항

<!--Add release notes below-->

## v1.3.7 {#latest}

릴리스 날짜: 2024년 4월 8일

- ![새 아이콘](../../assets/new.svg) **PHP** — PHP 8.3 및 PHP 8.3 이미지에 대한 지원을 추가했습니다.
- ![새 아이콘](../../assets/new.svg) **Nginx** — 이미지 nginx v. 1.24가 추가되었습니다.
- ![새 아이콘](../../assets/new.svg) **Opensearch** - 추가된 이미지 OpenSearch v. 2.12, 1.3.
- ![새 아이콘](../../assets/new.svg) **작성기** - 작성기 버전이 2.2.23으로 업데이트되었습니다.

## v1.3.6

릴리스 날짜: 2023년 7월 31일

- ![새 아이콘](../../assets/new.svg) **새 서비스 버전을 추가했습니다**—OpenSearch 2.5.
- ![새 아이콘](../../assets/new.svg) **작성기 캐시 사용**—이제 Docker 컨테이너를 시작할 때 작성기 지우기 캐시를 사용하도록 Docker 구성을 확장할 수 있습니다. _Commerce용 클라우드 도커_ 안내서에서 [도커 구성 확장](https://developer.adobe.com/commerce/cloud-tools/docker/configure/extend-docker-configuration/)을 참조하십시오.

## v1.3.5

릴리스 날짜: 2023년 3월 10일

- ![새 아이콘](../../assets/new.svg) **ionCube**—PHP 8.1 이미지에 대한 ionCube 확장을 추가했습니다.
- ![새 아이콘](../../assets/new.svg) **새 서비스 버전 추가**—OpenSearch 2.3 및 2.4, PHP 8.2, Varnish 7.1.1.
- ![새 아이콘](../../assets/new.svg) **PHP 8.2.3에 대한 향상된 지원**—Commerce 2.4.6을 지원하도록 특정 PHP 8.2.x 버전의 호환성 문제를 해결했습니다.
- ![수정 아이콘](../../assets/fix.svg) **작성기 문제**—도커 컨테이너 내에서 작성기 버전을 업데이트한 후 발생한 문제를 해결했습니다.

## v1.3.4

릴리스 날짜: 2022년 10월 27일

- ![새 아이콘](../../assets/new.svg) **새 바니시 이미지 추가**—바니시 6.5, 7.0, 7.1에 대한 이미지를 추가했습니다.<!-- MCLOUD-7879 -->

## v1.3.3

릴리스 날짜: 2022년 9월 13일

- ![새 아이콘](../../assets/new.svg) **Apple M1(ARM64) 지원**—Apple M1(ARM64) 아키텍처에 대한 지원을 사용하도록 도커 이미지에 변경 내용을 추가했습니다.<!-- MCLOUD-7989-2 MCLOUD-7989 -->
- ![수정 아이콘](../../assets/fix.svg) **Mailhog**—개발자 모드에 있는 동안 Mailhog 서비스가 이메일을 포착하지 못하는 문제를 해결했습니다.<!-- MCLOUD-8643 -->
- ![수정 아이콘](../../assets/fix.svg) **init-docker.sh** - `init-docker.sh` 스크립트에서 서비스 버전 검사기를 수정했습니다.<!-- MCLOUD-8765 -->

## v1.3.2

릴리스 날짜: 2022년 3월 31일

- ![새 아이콘](../../assets/new.svg) **Elasticsearch 7.10 이미지 추가됨**<!-- MCLOUD-8548 -->

## v1.3.1

릴리스 날짜: 2022년 3월 10일

- ![새 아이콘](../../assets/new.svg) **PHP 8.1 지원**—PHP 8.1에 대한 지원이 추가되었습니다.
- ![새 아이콘](../../assets/new.svg) **OpenSearch** - OpenSearch 버전 1.1 및 1.2의 이미지를 추가했습니다.
- ![새 아이콘](../../assets/new.svg) **작성기 2.1**—PHP 8.x 이미지에서 기본적으로 작성기 2.1.x를 설정합니다.
- ![새 아이콘](../../assets/new.svg) **PHP 이미지 개선**—

   - PHP 8.1 이미지가 추가되었습니다.
   - 업그레이드된 xDebug 버전 3.1.2
   - 업그레이드된 xmlrpc 1.0.0RC3

- ![수정 아이콘](../../assets/fix.svg) **Elasticsearch 및 OpenSearch 개선 사항**—Elasticsearch 및 OpenSearch Dockerfiles 개선 사항; Elasticsearch 5.2 이미지를 제거했습니다.
- ![수정 아이콘](../../assets/fix.svg) **나트륨 확장**—모든 PHP 이미지에서 기본적으로 `sodium` 확장을 사용하도록 설정했습니다.
- ![수정 아이콘](../../assets/fix.svg) **작성기 캐시 볼륨** - 작성기 캐시 볼륨에 캐시된 작성기 패키지가 있는 경로가 수정되었습니다.
- ![수정 아이콘](../../assets/fix.svg) **nginx의 메모리 제한**—NGINX 이미지의 메모리 제한이 수정되었습니다.

## v1.3.0

릴리스 날짜: 2021년 10월 25일

- ![수정 아이콘](../../assets/fix.svg) **개발자 모드 개선 워크플로**—이전에는 빌드 및 배포 단계에서 모드를 지정해야 했습니다. 이제 `build` 단계의 `--mode` 옵션에 따라 이후 `deploy` 단계의 모드가 결정됩니다. 배포 후 모드를 설정할 필요가 없습니다. [개발자 모드](https://devdocs.magento.com/cloud/docker/docker-mode-developer.html).<!-- ACMP-1086 --> 보기
- ![수정 아이콘](../../assets/fix.svg) **읽기 전용 파일 시스템 개선 사항**—<!-- ACMP-1106 -->
   - 메일 구성에 대한 PHP 컨테이너를 시작하는 문제를 수정했습니다.
   - INI 파일에서 환경 변수를 사용할 수 있습니다.
   - PHP 진입점에 쓰기 권한이 필요하지 않은지 확인하십시오.
- ![수정 아이콘](../../assets/fix.svg) **노드 업데이트**—번들 노드 버전을 업데이트합니다. PHP-CLI 이미지에 노드를 설치할 때 현재 LTS 버전을 사용합니다.<!-- ACMP-1539 -->
- ![수정 아이콘](../../assets/fix.svg) **Symfony 업데이트**—Adobe Commerce 2.4.4와 호환되도록 Symfony 구성 종속성을 업데이트했습니다.<!-- ACMP-1533 -->

## v1.2.4

릴리스 날짜: 2021년 7월 29일

- ![새 아이콘](../../assets/new.svg) **새 `Zookeeper` 컨테이너**—Cloud Infrastructure의 Adobe Commerce에 배포되지 않은 프로젝트에 대한 잠금 공급자 구성을 관리하기 위해 [Zookeeper 컨테이너](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#zookeeper-container)를 추가했습니다.<!--MCLOUD-8000-->

- ![새 아이콘](../../assets/new.svg) **Composer 2.0에 대한 지원이 추가되었습니다.**—수명이 다되어 가는 Composer 1.0에서 업그레이드를 지원하도록 Composer 버전 2.0을 Composer 구성 파일에 추가했습니다.<!--MCLOUD-8003-->

## v1.2.3

릴리스 날짜: 2021년 6월 14일

- ![새 아이콘](../../assets/new.svg) **PHP 8.0 추가**—PHP를 버전 8.0으로 업데이트하여 PHP 8.0에 포함된 모든 새로운 기능과 최적화를 활용할 수 있습니다.<!--MCLOUD-7941-->
- ![새 아이콘](../../assets/new.svg) **Varnish 6.6 및 Elasticsearch 7.11.2**(으)로 업데이트됨—다음 링크는 [Varnish Cache 6.6](https://varnish-cache.org/releases/rel6.6.0.html#rel6-6-0) 및 Elasticsearch 7.11.2.<!--MCLOUD-7921-->에 대한 릴리스 정보를 제공합니다.
- ![새 아이콘](../../assets/new.svg) **PHP 7.4 이미지용 `ioncube` 확장 추가**—PHP 7.3에서 PHP 7.4로 업그레이드하는 동안 처음 제외되었던 `ioncube` 확장이 PHP 7.4 이미지에 다시 추가되었습니다. *[mattskr에서 제출함](https://github.com/magento/magento-cloud-docker/pull/314).*<!--PR #314-->
- ![새 아이콘](../../assets/new.svg) **파일 동기화 옵션 추가:`manual-native`** - `manual-native` 파일 동기화 옵션은 수동으로 동기화를 제어하므로 macOS 및 Windows 환경에 최상의 성능을 제공합니다. [개발자 모드](https://devdocs.magento.com/cloud/docker/docker-mode-developer.html)에서 `manual-native` 옵션을 사용하는 방법과 [Docker 개발자 환경에서 데이터 동기화](https://devdocs.magento.com/cloud/docker/docker-syncing-data.html#file-synchronization-options)에 대해 읽어 보십시오.<!--MCLOUD-7977-->
- ![새 아이콘](../../assets/new.svg) **`up` 및 `down` 명령에서 볼륨 삭제 제거**—`--volume` 옵션이 `bin/magento-docker up` 및 `bin/magento-docker down` 명령에서 제거되었으며, 데이터 손실 경고가 있는 새 `bin/magento-docker init` 명령으로 대체되었습니다. 이 변경 사항은 우발적인 데이터 손실을 방지하는 데 도움이 됩니다. *[Joeshelton-wagento에서 제출함](https://github.com/magento/magento-cloud-docker/pull/319).*<!--PR #319-->
- ![고정 아이콘](../../assets/fix.svg) **생성된 인증서에 대한 `CN` 값을 업데이트했습니다**—Dockerfile에서 하드코딩된 `CN` 값을 제거했습니다. 이 값으로 인해 인증서 오류(`NET::ERR_CERT_INVALID`)가 발생하여 `ece-docker build:compose` 명령에 대한 `--host` 옵션이 무시됩니다.<!--MCLOUD-7934-->

## v1.2.2

릴리스 날짜: 2021년 4월 20일

- ![새 아이콘](../../assets/new.svg) **플랫폼 독립적이 되도록 `host.docker.internal`을 업데이트함**—이제 Ubuntu, Windows 및 macOS에 대해 동일한 도커 작성 스크립트를 만들 수 있습니다. Ubuntu에서 Xdebug를 사용하는 경우 더 이상 별도의 환경 변수가 필요하지 않습니다. [Igor Vitol이 제출한 수정 내용](https://github.com/magento/magento-cloud-docker/pull/299).<!--Issue #298-->
- ![새 아이콘](../../assets/new.svg) **init-docker.sh를 업데이트했습니다**—`MAGENTO_CLOUD_APPLICATION` 환경 변수에 `mounts` 개체를 추가했습니다. [Chiranjeevi에서 수정 제출](https://github.com/magento/magento-cloud-docker/pull/299).<!--Issue #299-->
- ![새 아이콘](../../assets/new.svg) **init-docker.sh** 업데이트—PHP 7.4 및 Cloud Docker 1.2.1 버전으로 `init-docker.sh` 스크립트를 업데이트했습니다. [Adarsh Manickam에서 수정 제출](https://github.com/magento/magento-cloud-docker/pull/300).<!--Issue #300-->
- ![새 아이콘](../../assets/new.svg) **기본적으로 활성화된 나트륨**—PHP 도커 이미지 내에서 기본적으로 `sodium` PHP 확장을 활성화했습니다.<!--MCLOUD-7548-->
- ![새 아이콘](../../assets/new.svg) **`custom-registry`옵션**—고유한 이미지 레지스트리를 사용하기 위해 `php ./vendor/bin/ece-docker build:compose` 명령에 `--custom-registry` 옵션을 추가했습니다.<!--MCLOUD-7476-->

  ```bash
  ./vendor/bin/ece-docker build:compose --custom-registry=my-registry.example.com
  ```

- ![새 아이콘](../../assets/new.svg) **이전 Elasticsearch 버전을 제거**—Elasticsearch 이미지에서 Elasticsearch 버전 1.7 및 2.4를 제거했습니다.<!--MCLOUD-7504-->
- ![새 아이콘](../../assets/new.svg) **NGINX 인증서 자동 생성**—NGINX 이미지에서 기존 인증서를 제거했습니다. 이제 NGINX 인증서가 보안 향상을 위해 새 배포마다 자동으로 생성됩니다.<!--MCLOUD-7396-->
- ![수정 아이콘](../../assets/fix.svg) **사용`opcache.validate_timestamps`**—개발자 모드에서 기본적으로 `opcache.validate_timestamps` PHP 설정을 사용하도록 설정했습니다. 이 설정을 사용하면 Docker에서 파일 시스템의 변경 사항이 인식되지 않는 문제가 해결되었습니다.<!--MCLOUD-7466-->
- ![수정 아이콘](../../assets/fix.svg) **수정`build:custom:compose`**—빌드 프로세스 중에 파일을 덮어쓸 수 없을 때 오류가 발생하도록 `build:custom:compose` 명령을 수정했습니다. 오류가 발생하면 `docker-compose up`에서 잘못된 파일을 사용하는 상황이 발생하지 않습니다.<!--MCLOUD-7457-->
- ![수정 아이콘](../../assets/fix.svg) **고정 `--sync_engine="native"` 옵션**—프로덕션 모드(`--mode="production"`)에서 `--sync_engine="native"` 옵션이 `docker.composer.yml` 파일에 로컬 폴더에 대한 항목을 만들지 않는 문제가 해결되었습니다.<!--MCLOUD-7254-->
- ![수정 아이콘](../../assets/fix.svg) **서비스 버전 유효성 검사 오류를 수정했습니다**—[!DNL RabbitMQ], Elasticsearch 및 기타 서비스에 대한 서비스 버전을 `MAGENTO_CLOUD_RELATIONSHIP` 변수의 `type` 속성에 추가했습니다. 이러한 버전을 `relationships` 변수에 추가하면 배포 단계에서 발생한 유효성 검사 오류가 해결되었습니다.<!--MCLOUD-7572-->

## v1.2.1

릴리스 날짜: 2020년 12월 21일

- ![새 아이콘](../../assets/new.svg) **NGINX 명령 옵션**—TLS 및 웹 서비스에 대한 NGINX `worker_processes` 및 NGINX `worker_connections`의 수를 변경하는 빌드 명령 옵션을 추가했습니다. `worker_process` 매개 변수는 값을 `auto`(으)로 설정하는 기능을 유지합니다. 예: <!--MCLOUD-7259-->

  ```terminal
  ./vendor/bin/ece-docker build:compose --nginx-worker-processes=2
  ./vendor/bin/ece-docker build:compose --nginx-worker-connections=2048
  ```

- ![새 아이콘](../../assets/new.svg) **TLS 명령 옵션**—TLS 서비스 없이 구성을 만드는 빌드 명령 옵션을 추가했습니다. 예: <!--MCLOUD-7259-->

  ```terminal
  ./vendor/bin/ece-docker build:compose --no-tls
  ```

- ![새 아이콘](../../assets/new.svg) **NGINX 메모리 사용량**—TLS 및 웹 서비스에 대한 NGINX 프로세스에서 사용하는 메모리가 감소했습니다.<!--MCLOUD-7259-->

- ![새 아이콘](../../assets/new.svg) **Blackfire**—Cloud Docker 이미지에서 기본적으로 Blackfire PHP 확장을 사용하지 않도록 설정했습니다.

- ![수정 아이콘](../../assets/fix.svg) **PHP-FPM 컨테이너**—`WEB_PORT`을(를) `80`에서 `8080`(으)로 변경하여 PHP-FPM 컨테이너 상태 검사를 수정했습니다.<!--MCLOUD-7232-->

- ![수정 아이콘](../../assets/fix.svg) **잘못된 볼륨 이름 지정**—개발자 모드에서 잘못된 볼륨 이름 지정으로 오류를 해결했습니다.<!--MCLOUD-7442-->

- ![수정 아이콘](../../assets/fix.svg) **NGINX 업스트림 포트**—무한 루프를 방지하기 위해 포트 8080을 사용하도록 도커 NGINX 1.19 이미지를 업데이트했습니다. [Adarsh Manickam에서 수정 제출](https://github.com/magento/magento-cloud-docker/pull/296).<!--Issue 295-->

## v1.2.0

릴리스 날짜: 2020년 11월 9일

- ![새 아이콘](../../assets/new.svg) **컨테이너 업데이트—**

   - ![새 아이콘](../../assets/new.svg) **PHP-FPM 컨테이너**—gnupg PHP 확장에 대한 지원을 추가했습니다. [Zilker Technology에서 G Arvind가 제출한 수정 사항](https://github.com/magento/magento-cloud-docker/pull/210).<!--MCLOUD-5981-->

   - ![수정 아이콘](../../assets/fix.svg) **데이터베이스 컨테이너**—상태 검사 명령에 필요한 데이터베이스 암호를 추가하여 데이터베이스 컨테이너 상태 검사를 해결했습니다.<!--MCLOUD-7122-->

   - ![새 아이콘](../../assets/new.svg) **Elasticsearch 컨테이너**

      - 예정된 Adobe Commerce 릴리스와의 호환성을 위해 Elasticsearch 7.9에 대한 지원이 추가되었습니다.<!--MCLOUD-7190-->

      - **Elasticsearch 플러그 인 구성** - `services.yaml` 파일의 Elasticsearch 플러그 인 구성 정보를 사용하여 Commerce 환경용 Cloud Docker의 `docker-compose.yaml` 파일을 생성하는 지원이 추가되었습니다. [Elasticsearch 플러그인](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#elasticsearch-plugins).<!--MCLOUD-2789-->을 참조하세요.

      - **Elasticsearch 플러그 인 지원**—다음 Elasticsearch 플러그 인에 대한 지원이 추가되었습니다. `analysis-icu`, `analysis-phonetic`, `analysis-stempel` 및 `analysis-nori`. 기본적으로 `analysis-icu` 및 `analysis-phonetic` 플러그인이 설치됩니다. 필요에 따라 `analysis-stempel` 및 `analysis-nori` 플러그인을 추가하거나 제거할 수 있습니다.<!--MCLOUD-2789-->

   - ![새 아이콘](../../assets/new.svg) **CLI 컨테이너**

      - **Docker PHP 컨테이너 내에서 명령 실행**—이제 Cloud Docker CLI를 사용하여 호스트에 PHP를 설치하지 않고도 Docker 환경의 PHP 컨테이너 내에서 명령을 실행할 수 있습니다. 예를 들어 `./bin/magento-docker php 7.3 vendor/bin/ece-docker build:compose` 명령은 구성을 빌드합니다. [Cloud Docker CLI](https://devdocs.magento.com/cloud/docker/docker-quick-reference.html#magento-cloud-docker-cli)를 참조하십시오. [Zilker Technology에서 G Arvind가 제출한 수정 사항](https://github.com/magento/magento-cloud-docker/pull/209).<!--MCLOUD-5982-->

      - PHP CLI 컨테이너에 OpenSSH-client를 추가했습니다. 이제 `composer.json` 파일에 Composer 명령을 사용하기 위해 ssh 클라이언트가 필요한 개인 git 저장소가 포함된 경우 Composer에 대해 ssh 에이전트 전달을 사용할 수 있습니다.<!--MCLOUD-6008-->

   - ![수정 아이콘](../../assets/fix.svg) **TLS 컨테이너**—이제 [TLS 컨테이너](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#tls-container)은(는) CentOS 이미지 대신 `https://hub.docker.com/r/magento/magento-cloud-docker-nginx` 도커 이미지를 기반으로 합니다. 이 변경 사항은 Cloud Docker 환경의 컨테이너 간에 HTTPS 요청을 보낼 때 오류가 발생하는 문제를 해결합니다.<!--MCLOUD-6469-->

   - ![새 아이콘](../../assets/new.svg) **테스트 컨테이너**—응용 프로그램 테스트를 위한 테스트 컨테이너를 추가하고 Docker `build:compose` 명령에 `--with-test` 옵션을 추가하여 Docker 환경에서 테스트할 때만 컨테이너를 만들었습니다. [응용 프로그램 테스트](https://devdocs.magento.com/cloud/docker/docker-test-app-mftf.html).<!--MCLOUD-6394-->를 참조하세요.

   - ![새 아이콘](../../assets/new.svg) **FPM-XDEBUG 컨테이너**

      - ![새 아이콘](../../assets/new.svg) **Linux에서 Xdebug 구성**—Xdebug 컨테이너에서 `host.docker.internal` 값을 구성하기 위해 `ece-docker build:compose` 명령에 `--set-docker-host` 옵션을 추가했습니다. 이 옵션은 Linux 시스템에서 Xdebug를 사용하는 데 필요합니다. [Docker용 Xdebug 구성](https://devdocs.magento.com/cloud/docker/docker-development-debug.html)을 참조하십시오.<!--MCLOUD-6430-->

      - ![수정 아이콘](../../assets/fix.svg) 로그에서 `uninitialized "with_xdebug" variable` 오류를 해결하기 위해 도커 진입점에 대한 Xdebug 변수 구성을 수정했습니다. [Florent Olivaud에서 제출한 수정 사항](https://github.com/magento/magento-cloud-docker/pull/218)<!--MCLOUD-6043-->

- ![새 아이콘](../../assets/new.svg) **Docker 구성 변경**

   - **MailHog 구성**—이제 `ece-docker build:compose` 명령 옵션을 사용하여 MailHog를 사용하지 않도록 설정하고 포트를 지정할 수 있습니다. `--no-mailhog`, `--mailhog-http-port` 및 `--mailhog-smtp-port`. [전자 메일 설정](https://devdocs.magento.com/cloud/docker/docker-config.html#set-up-email).<!--MCLOUD-6898, MCLOUD-6660-->을 참조하세요.

   - Commerce 1.2.0 이상용 Cloud Docker의 경우, 이제 Adobe에서 각 패치 버전에 대한 Docker 이미지를 제공하며, Docker 구성 생성기가 최신 버전을 사용하는 대신 지정된 패치 버전으로 Docker 구성을 생성합니다. 이전에는 도커 구성 생성기가 최신 패치 버전을 사용하여 구성을 빌드했습니다. 이로 인해 이전 버전을 사용하여 빌드된 Commerce 환경용 Cloud Docker가 중단될 수 있습니다.<!--MCLOUD-7093-->

   - **사용자 지정 클라우드 도커 구성에서 사용자 지정 이미지 및 버전 지정**—사용자 지정 도커 구성 파일(`docker-compose.yaml`)을 생성할 때 사용자 지정 이미지 및 버전을 지정하는 옵션이 있는 `build:custom:compose` 명령을 업데이트했습니다. [사용자 지정 도커 구성 작성](https://devdocs.magento.com/cloud/docker/docker-config-sources.html#build-a-custom-docker-compose-configuration)을 참조하십시오. <!--MCLOUD-7089-->

   - 모든 CLI 컨테이너에서 Adobe Commerce(`https://magento2.docker`)에 액세스할 수 있도록 포트 443을 표시하도록 도커 호스트 구성을 업데이트했습니다. 도커 구성 파일을 생성할 때 `--tls-port` 옵션을 추가하여 기본 포트를 변경할 수 있습니다.<!--MCLOUD-6806-->

- ![수정 아이콘](../../assets/fix.svg) `app/etc/env.php` 파일이 있는 경우 Commerce용 Cloud Docker 빌드가 실패하는 문제를 해결했습니다.<!--MCLOUD-6732-->

- ![수정 아이콘](../../assets/fix.svg) Linux에서 Commerce용 Cloud Docker 또는 Linux용 Windows 하위 시스템(WSL2)을 배포할 때 문제를 방지하기 위해 이름이 지정된 볼륨을 일반 볼륨으로 바꾸도록 빌드 구성을 업데이트했습니다.<!--MCLOUD-6732-->

- ![수정 아이콘](../../assets/fix.svg) 작성기 2.0을 지원하도록 Commerce 기능 테스트용 Cloud Docker를 업데이트했습니다.<!--MCLOUD-7183-->

## v1.1.2

릴리스 날짜: 2020년 9월 9일

- ![새 아이콘](../../assets/new.svg) Elasticsearch 7.7에 대한 지원이 추가됨<!--MCLOUD-6219-->

## v1.1.1

릴리스 날짜: 2020년 8월 5일

- ![수정 아이콘](../../assets/fix.svg) **이메일 구성을 업데이트했습니다**—SendMail을 사용하는 대신 MailHog 서비스를 지원하도록 Commerce용 기본 Cloud Docker 구성을 업데이트했습니다. [전자 메일 설정](https://devdocs.magento.com/cloud/docker/docker-config.html#set-up-email).<!--MCLOUD-5624-->을 참조하세요.

- ![수정 아이콘](../../assets/fix.svg) `ps:  command not found` 오류를 해결하기 위해 PS 라이브러리를 Cloud Docker 환경 구성으로 복원했습니다.<!--MCLOUD-6621-->

- ![수정 아이콘](../../assets/fix.svg) Cloud Docker 환경을 시작할 때 발생할 수 있는 `Cannot create container for service db` 오류를 수정하기 위해 데이터베이스 진입점 및 MariaDB 볼륨의 자동 마운트를 제거하도록 Commerce용 기본 Cloud Docker 구성을 업데이트했습니다.

  이제 `ece-docker build:compose` 명령에 `--with-entry-point` 및 `with-mariadb-conf` 옵션을 추가하여 데이터베이스 디렉터리를 탑재하도록 Cloud Docker 환경을 구성할 수 있습니다. [서비스 구성 옵션](https://devdocs.magento.com/cloud/docker/docker-containers.html#service-configuration-options).<!--MCLOUD-6424-->을 참조하세요.

- ![새 아이콘](../../assets/new.svg) **CLI 명령 업데이트**

| 작업 | 명령 |
| ------------------------------------------------------------------------------- | -------------------------------------------------------------- |
| 데이터베이스 컨테이너에 진입점을 추가하여 백업에서 데이터베이스를 복원합니다. | `./vendor/bin/ece-docker build:compose --db --with-entrypoint` |
| MariaDB 구성 볼륨 추가 | `./vendor/bin/ece-docker build:compose --db --mariadb-conf` |

## v1.1.0

릴리스 날짜: 2020년 6월 25일

- ![새 아이콘](../../assets/new.svg) **분할 데이터베이스 성능 솔루션에 대한 지원이 추가됨**—이제 Cloud Docker 환경에서 분할 데이터베이스 성능 솔루션을 사용하여 저장소를 구성하고 배포할 수 있습니다.<!--MCLOUD-3740-->

- ![새 아이콘](../../assets/new.svg) **Adobe Commerce 및 Magento Open Source 배포 지원**—이제 Commerce용 Cloud Docker를 사용하여 클라우드 인프라의 Adobe Commerce에서 호스팅되지 않은 프로젝트에 대한 로컬 개발 환경을 배포할 수 있습니다.<!--MCLOUD-5667-->

- ![새 아이콘](../../assets/new.svg) **Blackfire.io 지원**—자동화된 성능 테스트를 위해 [Blackfire.io 확장](https://devdocs.magento.com/cloud/docker/docker-config-blackfire-io.html)을 사용할 수 있는 지원이 추가되었습니다. [Zilker Technology에서 Adarsh Manickam이 제출한 수정 내용](https://github.com/magento/magento-cloud-docker/pull/202)<!--MCLOUD-5857-->

- ![새 아이콘](../../assets/new.svg) **컨테이너 업데이트**

   - **Varnish** - 이제 지원되는 버전의 클라우드 애플리케이션 템플릿을 사용하여 클라우드 도커 환경에서 Adobe Commerce을 배포할 때 Varnish가 기본 캐시가 됩니다. [바니시 컨테이너](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#varnish-container).<!--MCLOUD-2634-->를 참조하세요.

   - Cloud Docker 구성 파일을 생성할 때 Varnish 서비스 설치를 건너뛰는 `--no-varnish` 옵션을 추가했습니다.<!--MCLOUD-2634-->

   - ![새 아이콘](../../assets/new.svg) **데이터베이스**

      - MySQL 데이터베이스에 대한 지원이 추가되었습니다. 이제 MariaDB 또는 MySQL을 사용하여 Cloud Docker 환경을 구성할 수 있습니다. [서비스 구성 옵션](https://devdocs.magento.com/cloud/docker/docker-containers.html#service-configuration-options).<!--MCLOUD-5691-->을 참조하세요.

      - Docker 구성 파일을 생성할 때 데이터베이스 복제에 대한 증가 및 오프셋 설정을 설정하는 기능이 추가되었습니다. [서비스 컨테이너](https://devdocs.magento.com/cloud/docker/docker-containers.html#service-containers).<!--MCLOUD-5735-->를 참조하세요.

   - ![새 아이콘](../../assets/new.svg) **PHP-FPM**

      - PHP 7.4에 대한 지원이 추가되었습니다. [Zilker Technology에서 Mohanela Murugan이 제출한 수정 내용](https://github.com/magento/magento-cloud-docker/pull/198)<!--MCLOUD-198-->

      - 루트 프로젝트 디렉터리의 `php.ini` 파일을 Cloud Docker 환경에 복사하고 사용자 지정 PHP 설정을 PHP-FPM 및 CLI 컨테이너에 적용하는 기능이 추가되었습니다. [PHP 설정 사용자 지정](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#customize-php-settings)을 참조하십시오. [Zilker Technology에서 Mathew Beane이 제출한 수정 내용](https://github.com/magento/magento-cloud-docker/pull/130).<!--MCLOUD-6012-->

      - 컨테이너 상태 검사를 추가했습니다. [Zilker Technology에서 Visanth Sampath가 제출한 수정 내용](https://github.com/magento/magento-cloud-docker/pull/188).<!--MCLOUD-5752-->

   - ![수정 아이콘](../../assets/fix.svg) **Node.js** - 보안을 개선하기 위해 기본 Node.js 버전을 버전 8에서 버전 10으로 업데이트했습니다. Node.js 버전 8은 더 이상 사용되지 않으며 버그 수정 또는 보안 패치로 더 이상 업데이트되지 않습니다. [Zilker Technology에서 Mohan Elamurugan이 제출한 수정 내용](https://github.com/magento/magento-cloud-docker/pull/183).<!--MCLOUD-5586-->

   - ![새 아이콘](../../assets/new.svg) **Elasticsearch**

      - Elasticsearch 6.8, 7.2, 7.5 및 7.6.<!--MCLOUD-4050, MCLOUD-5855,MCLOUD-5860-->에 대한 지원을 추가했습니다.

      - 도커 작성 구성 파일을 생성할 때 [Elasticsearch 컨테이너 구성](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#elasticsearch-container)을(를) 사용자 지정하는 기능이 추가되었습니다.<!--MCLOUD-3059-->

      - 도커 작성 구성 파일을 생성하기 위한 서비스 구성 옵션에 `--no-es` 옵션을 추가했습니다. 이 옵션을 사용하여 Elasticsearch 컨테이너 설치를 건너뛰고 대신 MySQL 검색을 사용하십시오. 이 옵션은 Adobe Commerce 버전 2.3.5 이하에서만 지원됩니다.<!--MCLOUD-3766-->

   - ![새 아이콘](../../assets/new.svg) **FPM-XDEBUG 컨테이너**—Cloud Docker 환경에서 PHP를 디버깅하기 위해 Xdebug를 설치하고 구성하는 서비스 구성 옵션이 추가되었습니다. [Xdebug 구성](https://devdocs.magento.com/cloud/docker/docker-development-debug.html).<!--MCLOUD-4098-->을 참조하십시오.

- ![새 아이콘](../../assets/new.svg) **Docker 구성 변경**

   - PHP-FPM, Redis, Elasticsearch 및 MySQL Docker 서비스 컨테이너에 대한 상태 검사를 추가했습니다.<!--MCLOUD-3335 and MCLOUD-5856-->

   - 개발자 모드에서 기본 파일 동기화 모드를 `native`(으)로 변경했습니다.<!--MCLOUD-3890 -->

   - `docker-compose.yml` 파일을 생성할 때 일반 도커 서비스 컨테이너 이미지에 버전 정보를 추가했습니다.<!--MCLOUD-3878-->

   - Nginx 서버의 `fastcgi_buffers` 값을 늘려 업스트림 PHP-FPM 컨테이너에서 큰 응답을 처리하는 기능이 향상되었습니다.<!--MCLOUD-5980-->

   - `vendor` 디렉터리의 파일을 동기화하는 두 번째 동기화 세션을 추가하여 가변 파일 동기화 성능을 개선했습니다. 이 변경 사항으로 인해 파일 동기화 프로세스 중에 돌연변이가 중단되는 것을 방지할 수 있습니다. [Zilker Technology에서 Mathew Beane이 제출한 수정 내용](https://github.com/magento/magento-cloud-docker/pull/127).<!--MCLOUD-6010-->

   - ![새 아이콘](../../assets/new.svg) **CLI 명령 업데이트**

| 작업 | 명령 |
| -------- | --------------- |
| Redis 캐시 지우기 | `bin/magento-docker flush-redis` |
| 니스 캐시 지우기 | `bin/magento-docker flush-varnish` |
| 기본 Vanish 설치 건너뛰기 | `.vendor/bin/ece-docker build:compose --no-varnish`<!--MCLOUD-2634--> |
| [Elasticsearch 옵션 사용자 지정](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#elasticsearch-container) | `.vendor/bin/ece-docker build:compose --es-env-var`<!--MCLOUD-3059--> |
| [Elasticsearch 구성 제거](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#elasticsearch-container) | `.vendor/bin/ece-docker build:compose --no-es`<!--MCLOUD-3766--> |
| MySQL 버전 5.6 또는 5.7로 DB 컨테이너 구성 | `./vendor/bin/ece-docker build:compose --db <mysql-version-number> --db-image mysql`<!--MCLOUD-5691--> |
| 사용자 지정 기본 URL 지정 | `./vendor/bin/ece-docker build:compose --host=<hostname> --port=<port-number>`<!--MCLOUD-3063--> |
| [Xdebug 구성에 대한 컨테이너 추가](https://devdocs.magento.com/cloud/docker/docker-development-debug.html) | `.vendor/bin/ece-docker build:compose --mode developer --sync-engine native --with-xdebug`<!--MCLOUD-4098--> |

- ![수정 아이콘](../../assets/fix.svg) 변경 내용이 오래된 세션을 만들지 않도록 변경 내용 파일 동기화 구성을 수정했습니다. [Zilker Technology에서 Mathew Beane이 제출한 수정 내용](https://github.com/magento/magento-cloud-docker/pull/127).<!--MCLOUD-6010-->

- ![수정 아이콘](../../assets/fix.svg) PHP-FPM 컨테이너를 시작할 때 도커 작성 로그에 구문 오류가 발생하는 구성 문제를 해결했습니다. [Zilker Technology에서 Mathew Beane이 제출한 수정 내용](https://github.com/magento/magento-cloud-docker/pull/129)<!--MCLOUD-3958-->

- ![수정 아이콘](../../assets/fix.svg) 여러 Docker 환경을 사용할 때 발생하는 볼륨 충돌 오류를 수정했습니다. [Zilker Technology에서 G Arvind가 제출한 수정 내용](https://github.com/magento/magento-cloud-docker/pull/168).

- ![수정 아이콘](../../assets/fix.svg) 구성에 Blackfire.io가 포함된 경우 `ece-docker build:compose` 명령이 실패하는 문제를 해결했습니다. [Zilker Technology에서 G Arvind가 제출한 수정 내용](https://github.com/magento/magento-cloud-docker/pull/199). <!--MCLOUD-5797-->

- ![수정 아이콘](../../assets/fix.svg) Commerce용 Cloud Docker를 사용하여 여러 패키지를 설치할 때 발생하는 메모리 부족 오류를 방지하기 위해 PHP CLI 이미지 구성을 업데이트했습니다. [Zilker Technology에서 Mohan Elamurugan이 제출한 수정 내용](https://github.com/magento/magento-cloud-docker/pull/197).*<!--MCLOUD-5818-->

- ![수정 아이콘](../../assets/fix.svg) Cloud Docker 환경에서 여러 MySQL 사용자에 대한 지원을 추가했습니다. 이전 릴리스에서는 `magento.app.yaml` 파일에 여러 데이터베이스 사용자가 지정된 경우 `build:compose` 작업이 실패했습니다. [Zilker Technology에서 G Arvind가 제출한 수정 사항](https://github.com/magento/magento-cloud-docker/pull/181).<!--MCLOUD-5670-->

- ![수정 아이콘](../../assets/fix.svg) 배포 중에 경고 알림을 발생시킨 호환성 문제를 해결하기 위해 Commerce PHP 컨테이너용 Cloud Docker에서 `rsyslog`을(를) 제거했습니다. Cloud Docker는 rsyslog 유틸리티를 사용하지 않습니다.<!--MCLOUD-6173-->

## v1.0.0

릴리스 날짜: 2020년 2월 5일

- ![새 아이콘](../../assets/new.svg) **전달할 별도의 패키지를 만들었습니다.`Cloud Docker for Commerce`**—코드 품질을 유지하고 독립적인 릴리스를 제공하기 위해 Commerce용 Cloud Docker를 전달할 소스 코드를 `ece-tools` 저장소에서 [새 `magento-cloud-docker` 저장소](https://github.com/magento/magento-cloud-docker)(으)로 이동했습니다. 새 패키지는 ECE-Tools v2002.1.0 이상에 종속됩니다.

  ece-tools를 업데이트할 때 `magento/magento-cloud-docker` 패키지도 버전 1.0.0으로 업데이트합니다. 이전 `ece-tools` 릴리스(2002.0.x)와 함께 Commerce용 Cloud Docker를 사용한 경우 [이전 버전과의 비호환성](backward-incompatible-changes.md)을 검토하고 필요에 따라 프로젝트를 스크립트, 명령 및 프로세스로 업데이트합니다.

- ![새 아이콘](../../assets/new.svg) **Docker 이미지에 버전 지정을 추가했습니다**—업데이트된 이미지를 가져오려면 이제 `magento/magento-cloud-docker` 패키지를 업데이트해야 합니다.<!--MAGECLOUD-4737-->

- ![새 아이콘](../../assets/new.svg) **컨테이너 업데이트**—

   - ![새 아이콘](../../assets/new.svg) **PHP-FPM 컨테이너**—

      - ![새 아이콘](../../assets/new.svg) **Node.js 지원이 추가됨**—PHP 컨테이너 내의 node, npm 및 grunt-cli 기능을 지원하도록 PHP-FPM 이미지를 업데이트했습니다.<!--MAGECLOUD-3953-->

      - ![새 아이콘](../../assets/new.svg) **[ionCube](https://www.ioncube.com/)**&#x200B;에 대한 지원이 추가됨—로컬 Docker 개발 환경에서 ionCube를 지원하도록 기본 Docker 구성이 업데이트되었습니다.<!--MAGECLOUD-4354-->

   - ![새 아이콘](../../assets/new.svg) **웹 컨테이너**—

      - ![새 아이콘](../../assets/new.svg) **NGINX 구성 사용자 지정**—사용자 지정 `nginx.conf` 파일을 Commerce 환경용 Cloud Docker에 탑재하는 기능이 추가되었습니다. [웹 컨테이너](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#web-container).<!--MAGECLOUD-4204-->를 참조하세요.

      - ![새 아이콘](../../assets/new.svg) **자동으로 생성된 NGINX 인증서**—이제 Docker 구성 파일에 웹 컨테이너에 대한 NGINX 인증서를 자동으로 생성하는 구성이 포함됩니다.<!--MAGECLOUD-4258-->

   - ![새 아이콘](../../assets/new.svg) **새 Selenium 컨테이너**—MTF(Magento 기능 테스트 프레임워크)를 사용하여 Adobe Commerce 응용 프로그램 테스트를 지원하도록 [Selenium 컨테이너](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#selenium-container)를 추가했습니다.<!--MAGECLOUD-4040-->

   - ![새 아이콘](../../assets/new.svg) **[!DNL RabbitMQ]버전 지원**—[!DNL RabbitMQ] 버전 3.8을 지원하도록 [!DNL RabbitMQ] 컨테이너 구성을 업데이트했습니다.<!--MAGECLOUD-4674-->

   - ![수정 아이콘](../../assets/fix.svg) **영구 데이터베이스 컨테이너**—Docker 구성을 중지했다가 제거하고 Docker 구성을 다시 시작하면 `magento-db: /var/lib/mysql` 데이터베이스 볼륨이 이제 유지됩니다. 이제 데이터베이스 볼륨을 수동으로 삭제해야 합니다. [데이터베이스 컨테이너].<!--MAGECLOUD-3978-->를 참조하세요.

   - ![새 아이콘](../../assets/new.svg) **TLS 컨테이너**—

      - ![새 아이콘](../../assets/new.svg) **공식 이미지를 사용하도록 컨테이너 기본 이미지를 업데이트했습니다**—이제 [Cloud TLS 컨테이너](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#tls-container) 이미지가 공식 `debian:jessie` 도커 이미지를 기반으로 합니다.—<!--MAGECLOUD-4163-->

      - ![새 아이콘](../../assets/new.svg) **파운드 TLS 종료 프록시]**&#x200B;에 대한 지원이 추가됨-[파운드 구성 파일](https://github.com/magento/magento-cloud-docker/blob/1.0/images/tls/)은(는) 다음 ENV 변수를 추가하여 TLS 컨테이너의 도커 구성을 사용자 지정합니다.[

         - **`TimeOut`** - 시간을 첫 번째 바이트(TTFB) 시간 초과 값으로 설정합니다. 기본값은 300초입니다.

         - **`RewriteLocation`** - 기본적으로 파운드 프록시가 위치를 요청 URL에 다시 쓸지 여부를 결정합니다. 다시 작성으로 인해 외부 SSO 사이트와 같은 외부 웹 사이트로의 리디렉션이 중단되지 않도록 하려면 기본값이 `0`입니다. [Sorin Sugar에서 수정 제출됨](https://github.com/magento/magento-cloud-docker/pull/37)<!--MAGECLOUD-4061-->

      - ![새 아이콘](../../assets/new.svg) TLS 컨테이너 구성의 시간 제한 값을 15초에서 300초로 늘렸습니다. [Zilker Technology에서 Mathew Beane이 제출한 수정 내용](https://github.com/magento/magento-cloud-docker/pull/78)<!--MAGECLOUD-4460-->

   - ![새 아이콘](../../assets/new.svg) **바니시 컨테이너**—

      - ![새 아이콘](../../assets/new.svg) **공식 이미지를 사용하도록 컨테이너 기본 이미지를 업데이트했습니다**—이제 [Cloud 바니시 컨테이너](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#varnish-container)가 공식 `centos` 도커 이미지를 기반으로 합니다.<!--MAGECLOUD-4163-->

      - ![새 아이콘](../../assets/new.svg) **기본 시간 초과 구성 개선**-바니시 컨테이너에 `.first_byte_timeout` 및 `.between_bytes_timeout` 구성 추가 두 시간 제한 값은 모두 기본적으로 `300s`(5분)입니다. [Zilker Technology에서 Mathew Beane이 제출한 수정 내용](https://github.com/magento/magento-cloud-docker/pull/78)<!--MAGECLOUD-4460-->

      - ![수정 아이콘](../../assets/fix.svg) **Xdebug 세션 중 바니시 건너뛰기**—Xdebug가 활성화된 경우 받은 요청에 대해 `pass`을(를) 반환하도록 바니시 컨테이너 구성을 업데이트했습니다. 이전 릴리스에서는 도커 환경에 Varnish가 포함된 경우 Xdebug를 사용할 수 없었습니다. [Zilker Technology에서 Mathew Beane이 제출한 수정 내용](https://github.com/magento/magento-cloud-docker/pull/111).<!--MAGECLOUD-4873-->

- ![새 아이콘](../../assets/new.svg) **Docker 구성 변경**—

   - ![새 아이콘](../../assets/new.svg) **프로젝트의 마운트 및 볼륨 관리**—로컬 개발을 위해 Docker 환경을 시작할 때 마운트 및 볼륨을 관리하는 기능을 추가했습니다. [프로젝트 데이터 공유]를 참조하십시오.<!--MAGECLOUD-3248-->

   - ![새 아이콘](../../assets/new.svg) **네트워크 브리지 모드 지원**—로컬 네트워크를 통해 Docker 컨테이너 간 연결을 사용하도록 네트워크 브리지 모드에 대한 지원을 추가했습니다.<!--MAGECLOUD-4165-->

   - ![새 아이콘](../../assets/new.svg) **Cron 컨테이너가 기본적으로 비활성화되어 있음**—성능을 개선하기 위해 Docker 환경을 빌드할 때 Cron 컨테이너가 더 이상 기본적으로 구성되지 않습니다. Docker 빌드 명령의 `--with-cron` 옵션을 사용하여 Cron 컨테이너를 환경에 추가할 수 있습니다. [cron 작업 관리](https://devdocs.magento.com/cloud/docker/docker-manage-cron-jobs.html).<!--MAGECLOUD-5181-->를 참조하십시오.

   - ![새 아이콘](../../assets/new.svg) **대용량 백업 파일 동기화 중지**—DB 덤프 및 보관 파일(ZIP, SQL, GZ 및 BZ2)을 `dist/docker-sync.yml` 및 `dist/mutagen.sh` 파일의 제외 목록에 추가했습니다. 대용량 파일(>1GB)을 동기화하면 사용하지 않는 기간이 발생할 수 있으며 백업 파일을 다시 생성할 수 있으므로 동기화가 일반적으로 필요하지 않습니다.<!--MAGECLOUD-3979-->

- ![새 아이콘](../../assets/new.svg) **명령 변경**—

   - ![수정 아이콘](../../assets/fix.svg) `./bin/docker` 파일이 기존 Docker 이진 파일을 덮어쓰므로 일부 Docker 환경이 중단되는 문제를 해결하기 위해 `./bin/docker` 파일의 이름을 `./bin/magento-docker`(으)로 변경했습니다. 스크립트와 명령을 업데이트해야 하는 [이전 버전과 호환되지 않는 변경 내용](backward-incompatible-changes.md)입니다.<!-- MAGECLOUD-4038 -->

   - ![새 아이콘](../../assets/new.svg) **데이터베이스 포트를 호스트에 표시하는 서비스 구성 옵션을 추가했습니다.**—`docker-compose.yml` 파일을 작성할 때 데이터베이스 포트를 호스트에 표시하려면 `--expose-db-port= [Fix submitted by Adarsh Manickam from Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/101).<PORT>` 옵션을 사용하십시오. `bin/ece-docker build:compose --expose-db-port=<PORT>`<!--MAGECLOUD-4454-->

   - ![새 아이콘](../../assets/new.svg) **새 배포 후 명령** - 이전에는 `cloud-deploy` 명령을 사용하여 Adobe Commerce을 Cloud Docker 컨테이너에 배포한 후 `.magento.app.yaml` 파일에 정의된 배포 후 후크가 자동으로 실행되었습니다. 이제 배포한 후 배포 후 후크를 실행하려면 별도의 `cloud-post-deploy` 명령을 실행해야 합니다. [개발자](https://devdocs.magento.com/cloud/docker/docker-mode-developer.html) 및 [프로덕션](https://devdocs.magento.com/cloud/docker/docker-mode-production.html) 모드에 대해 업데이트된 실행 지침을 참조하십시오.<!--MAGECLOUD-3996-->

   - ![새 아이콘](../../assets/new.svg) 빌드 및 배포 컨테이너에 대한 `./bin/magento-docker` 명령에 `--rm` 옵션을 추가했습니다. 작업이 완료되면 컨테이너가 제거됩니다.<!--MAGECLOUD-4205-->

   - ![새 아이콘](../../assets/new.svg) **명령 `build:compose`에 대한 업데이트**—

      - ![새 아이콘](../../assets/new.svg) 개발자 모드에서 Docker 구성 파일을 생성할 때 파일 동기화를 사용하지 않도록 하기 위해 `docker-build` 명령에 `--sync-engine="native"` 옵션을 추가했습니다. 로컬 도커 개발을 위해 파일 동기화가 필요하지 않은 Linux 시스템에서 개발할 때 이 옵션을 사용합니다. [도커 환경에서 데이터 동기화](https://devdocs.magento.com/cloud/docker/docker-syncing-data.html)를 참조하십시오.<!--MCLOUD-3231, MCLOUD-3890-->

   - ![새 아이콘](../../assets/new.svg) 기본 파일 동기화 설정을 `docker-sync`에서 `native`(으)로 변경했습니다. [Zilker Technology에서 Mathew Beane이 제출한 수정 내용](https://github.com/magento/magento-cloud-docker/pull/124).<!--MAGECLOUD-5066-->

- ![새 아이콘](../../assets/new.svg) **유효성 검사 개선 사항**—

   - ![새 아이콘](../../assets/new.svg) 로컬 Docker 개발 환경에 대한 배포 프로세스에 유효성 검사를 추가하여 클라우드 환경 구성에 데이터베이스를 해독하는 데 필요한 암호화 키가 포함되어 있는지 확인했습니다. 이제 환경 구성이 암호화 키의 값을 지정하지 않으면 로그에 오류 메시지가 표시됩니다.<!--MAGECLOUD-4423-->

   - ![새 아이콘](../../assets/new.svg) 빌드 및 배포 처리를 계속하기 전에 서비스가 준비되었는지 확인하기 위해 Elasticsearch 서비스에 컨테이너 상태 검사를 추가했습니다. 상태 검사에서 오류가 반환되면 컨테이너가 자동으로 다시 시작됩니다.<!--MAGECLOUD-4456-->
