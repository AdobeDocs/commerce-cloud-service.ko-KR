---
title: 후크 속성
description: ' [!DNL Commerce] 응용 프로그램 구성 파일에서 hooks 속성을 구성하는 방법에 대한 예를 참조하십시오.'
feature: Cloud, Configuration, Build, Deploy
exl-id: d9561f09-5129-4b72-978e-2e3873e8efae
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '316'
ht-degree: 0%

---

# 후크 속성

빌드, 배포 및 배포 후 단계에서 셸 명령을 실행하려면 `hooks` 섹션을 사용합니다.

- **`build`**—응용 프로그램을 패키지하기 전에 _명령을 실행합니다_. 데이터베이스 또는 Redis와 같은 서비스는 응용 프로그램이 아직 배포되지 않았기 때문에 사용할 수 없습니다. 사용자 지정 생성 콘텐츠가 배포 단계로 계속 진행될 수 있도록 사용자 지정 명령 _이전_&#x200B;에 기본 `php ./vendor/bin/ece-tools` 명령을 추가합니다.

- **`deploy`** - 응용 프로그램 패키징 및 배포 _after_ 명령을 실행합니다. 이 시점에서 다른 서비스에 액세스할 수 있습니다. 기본 `php ./vendor/bin/ece-tools` 명령은 `app/etc` 디렉터리를 올바른 위치에 복사하므로 사용자 지정 명령이 실패하지 않도록 사용자 지정 명령 _after_&#x200B;를 deploy 명령에 추가해야 합니다.

- **`post_deploy`**—응용 프로그램을 배포하는 _후_&#x200B;와 컨테이너가 연결을 수락하는 _후_&#x200B;에 명령을 실행합니다. `post_deploy` 후크를 사용하면 캐시가 지워지고 캐시가 미리 로드됩니다(준비). [Post 배포 단계](../environment/variables-post-deploy.md)에서 `WARM_UP_PAGES` 변수를 사용하여 페이지 목록을 사용자 지정할 수 있습니다. 필수는 아니지만 `SCD_ON_DEMAND` 환경 변수와 함께 작동합니다.

다음 예제에서는 `.magento.app.yaml` 파일의 기본 구성을 보여 줍니다. `ece-tools` 명령의 _이전_ 섹션 `build`, `deploy` 또는 `post_deploy` 아래에 CLI 명령을 추가합니다.

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

또한 특별히 코드를 빌드하거나 파일을 이동할 때 추가 작업을 수행하기 위해 `generate` 및 `transfer` 명령을 사용하여 빌드 단계를 추가로 사용자 지정할 수 있습니다.

```yaml
hooks:
    # We run build hooks before your application has been packaged.
    build: |
        set -e
        php ./vendor/bin/ece-tools build:generate
        # php /path/to/your/script
        php ./vendor/bin/ece-tools build:transfer
```

- `set -e` - 최종 실패한 명령 대신 첫 번째 실패한 명령에서 후크가 실패합니다.
- `build:generate` - 빌드 단계에 대해 SCD가 활성화된 경우 패치를 적용하고 구성을 확인하고 ID를 생성하고 정적 콘텐츠를 생성합니다.
- `build:transfer` - 생성된 코드 및 정적 콘텐츠를 최종 대상으로 전송합니다.

명령은 응용 프로그램(`/app`) 디렉터리에서 실행됩니다. `cd` 명령을 사용하여 디렉터리를 변경할 수 있습니다. 후크는 마지막 명령이 실패하면 실패합니다. 실패한 첫 번째 명령에서 오류가 발생하도록 하려면 후크의 시작 부분에 `set -e`을(를) 추가하십시오.

**grunt를 사용하여 Sass 파일을 컴파일하려면**:

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

빌드 중에 발생하는 정적 콘텐츠 배포 전에 `grunt`을(를) 사용하여 Sass 파일을 컴파일합니다. `grunt` 명령을 `build` 명령 앞에 놓습니다.

{{scenarios}}
