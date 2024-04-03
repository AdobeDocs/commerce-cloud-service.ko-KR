---
title: 후크 속성
description: 에서 후크 속성을 구성하는 방법에 대한 예를 참조하십시오. [!DNL Commerce] 응용 프로그램 구성 파일입니다.
feature: Cloud, Configuration, Build, Deploy
exl-id: d9561f09-5129-4b72-978e-2e3873e8efae
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '316'
ht-degree: 0%

---

# 후크 속성

사용 `hooks` 빌드, 배포 및 배포 후 단계 동안 셸 명령을 실행하는 섹션:

- **`build`**- 명령 실행 _다음 이전_ 애플리케이션 패키징. 데이터베이스 또는 Redis와 같은 서비스는 응용 프로그램이 아직 배포되지 않았기 때문에 사용할 수 없습니다. 사용자 지정 명령 추가 _다음 이전_ 기본값 `php ./vendor/bin/ece-tools` 사용자 지정 생성 컨텐츠가 배포 단계로 이어지도록 명령합니다.

- **`deploy`**- 명령 실행 _이후_ 애플리케이션 패키징 및 배포 이 시점에서 다른 서비스에 액세스할 수 있습니다. 기본 이후 `php ./vendor/bin/ece-tools` 명령이 `app/etc` 디렉토리가 올바른 위치에 있으면 사용자 정의 명령을 추가해야 합니다. _이후_ 사용자 지정 명령의 실패를 방지하기 위한 deploy 명령입니다.

- **`post_deploy`**- 명령 실행 _이후_ 애플리케이션 배포 및 _이후_ 컨테이너가 연결을 수락하기 시작합니다. 다음 `post_deploy` 후크는 캐시를 지우고 캐시를 미리 로드합니다(warms). 다음을 사용하여 페이지 목록을 사용자 지정할 수 있습니다. `WARM_UP_PAGES` 의 변수 [배포 후 단계](../environment/variables-post-deploy.md). 필수는 아니지만 과 함께 작동합니다. `SCD_ON_DEMAND` 환경 변수입니다.

다음 예제는 의 기본 구성을 보여 줍니다. `.magento.app.yaml` 파일. 아래에 CLI 명령 추가 `build`, `deploy`, 또는 `post_deploy` 섹션 _다음 이전_ 다음 `ece-tools` 명령:

```yaml
hooks:
    # We run build hooks before your application has been packaged.
    build: |
        set -e
        composer install
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    # We run deploy hook after your application has been deployed and started.
    deploy: |
        php ./vendor/bin/ece-tools run scenario/deploy.xml
    # We run post deploy hook to clean and warm the cache. Available with ECE-Tools 2002.0.10.
    post_deploy: |
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
```

또한 를 사용하여 빌드 단계를 추가로 사용자 지정할 수 있습니다. `generate` 및 `transfer` 코드를 빌드하거나 파일을 이동할 때 추가 작업을 수행하는 명령입니다.

```yaml
hooks:
    # We run build hooks before your application has been packaged.
    build: |
        set -e
        php ./vendor/bin/ece-tools build:generate
        # php /path/to/your/script
        php ./vendor/bin/ece-tools build:transfer
```

- `set -e`- 최종 실패한 명령 대신 첫 번째 실패한 명령에서 후크가 실패합니다.
- `build:generate`- 빌드 단계에 대해 SCD가 활성화된 경우 패치를 적용하고 구성을 검증하며 ID를 생성하고 정적 콘텐츠를 생성합니다.
- `build:transfer`- 생성된 코드 및 정적 콘텐츠를 최종 대상으로 전송합니다.

응용 프로그램에서 실행되는 명령(`/app`) 디렉토리. 다음을 사용할 수 있습니다. `cd` 디렉터리를 변경하는 명령입니다. 후크는 마지막 명령이 실패하면 실패합니다. 실패한 첫 번째 명령에서 오류가 발생하도록 하려면 다음을 추가합니다. `set -e` 후크 시작 부분까지.

**grunt를 사용하여 Sass 파일 컴파일하기**:

```yaml
dependencies:
    ruby:
        sass: "3.4.7"
    nodejs:
        grunt-cli: "~0.1.13"

hooks:
    build: |
        cd public/profiles/project_name/themes/custom/theme_name
        npm install
        grunt
        cd
        php ./vendor/bin/ece-tools build
```

다음을 사용하여 Sass 파일 컴파일 `grunt` 빌드 중에 발생하는 정적 콘텐츠 배포 전. 배치 `grunt` 다음 앞에 명령: `build` 명령입니다.

{{scenarios}}
