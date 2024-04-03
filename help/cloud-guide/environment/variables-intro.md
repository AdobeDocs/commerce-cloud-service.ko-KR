---
title: 환경 변수
description: 클라우드 인프라의 Adobe Commerce과 관련된 환경 변수 목록을 참조하십시오.
feature: Cloud, Build, Configuration, Deploy
exl-id: bfee2f69-93a6-4d26-bb9e-be8acc5673c3
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 0%

---

# 환경 변수

Adobe Commerce on cloud infrastructure를 사용하면 환경 변수를 할당하여 구성 옵션을 재정의할 수 있습니다. 다음 `ece-tools` 패키지는 `env.php` 의 값을 기반으로 한 파일 [클라우드 변수](variables-cloud.md), 변수에 설정된 값 [!DNL Cloud Console]및 `.magento.env.yaml` 구성 파일입니다.

의 환경 변수 `.magento.env.yaml` 파일 기존 Commerce 구성을 재정의하여 클라우드 환경을 사용자 정의합니다. 기본값이 `Not Set`, 그런 다음 `ece-tools` 패키지 테이크 **아니요** 작업 및 사용 [!DNL Commerce] 기본값 또는 `MAGENTO_CLOUD_RELATIONSHIPS` 구성. 기본값이 설정되면 `ece-tools` 패키지는 해당 기본값을 설정하는 역할을 합니다.

환경 변수의 유형은 다음과 같습니다.

- [관리자](variables-admin.md)—변수가 프로젝트 관리 변수를 재정의합니다.
- [MAGENTO 클라우드](variables-cloud.md)—클라우드 인프라에 관련된 변수
- 에 사용된 변수 `.magento.env.yaml` 파일:
   - [글로벌](variables-global.md)—변수가 빌드, 배포 및 배포 후 단계에 영향을 미칩니다.
   - [빌드](variables-build.md)—변수가 빌드 작업을 제어합니다.
   - [배포](variables-deploy.md)—변수 제어 배포 작업
   - [Post-deploy](variables-post-deploy.md)- 변수는 배포 후 작업을 제어합니다.

변수는 _계층구조_: 변수가 재정의되지 않으면 상위 환경에서 상속됩니다.

다음을 설정할 수 있습니다. [관리자 변수](variables-admin.md) 다음에서 [!DNL Cloud Console] 또는 Adobe Commerce CLI를 사용합니다. 에서 다른 환경 변수를 관리할 수 있습니다. [`.magento.env.yaml`](configure-env-yaml.md) 지원 티켓 없이도 Pro Staging 및 프로덕션을 포함하여 모든 환경에서 빌드 및 배포 작업을 관리할 수 있는 파일입니다.

>[!TIP]
>
>YAML 파일은 대/소문자를 구분하므로 탭을 허용하지 않습니다. 전체에서 일관된 들여쓰기를 사용하도록 주의하십시오. `.magento.env.yaml` 파일 또는 구성이 예상대로 작동하지 않을 수 있습니다. 이 설명서 및 샘플 파일의 예제는 _이중 공간_ 들여쓰기. 사용 [ece-tools validate 명령](configure-env-yaml.md#validate-configuration-file) 구성을 확인합니다.
