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

Adobe Commerce on cloud infrastructure를 사용하면 환경 변수를 할당하여 구성 옵션을 재정의할 수 있습니다. `ece-tools` 패키지는 [클라우드 변수](variables-cloud.md), [!DNL Cloud Console]에 설정된 변수 및 `.magento.env.yaml` 구성 파일의 값을 기반으로 `env.php` 파일에 값을 설정합니다.

`.magento.env.yaml` 파일의 환경 변수는 기존 Commerce 구성을 재정의하여 클라우드 환경을 사용자 지정합니다. 기본값이 `Not Set`인 경우 `ece-tools` 패키지는 **NO** 작업을 수행하고 [!DNL Commerce] 기본값 또는 `MAGENTO_CLOUD_RELATIONSHIPS` 구성의 값을 사용합니다. 기본값이 설정되면 `ece-tools` 패키지가 해당 기본값을 설정하는 역할을 합니다.

환경 변수의 유형은 다음과 같습니다.

- [ADMIN](variables-admin.md)—변수가 프로젝트 관리자 변수를 재정의합니다.
- [MAGENTO_CLOUD](variables-cloud.md) - 클라우드 인프라에 관련된 변수
- `.magento.env.yaml` 파일에 사용된 변수:
   - [전역](variables-global.md)—변수가 빌드, 배포 및 배포 후 단계에 영향을 줍니다.
   - [빌드](variables-build.md)—변수가 빌드 작업을 제어합니다.
   - [배포](variables-deploy.md)—변수가 배포 작업을 제어합니다.
   - [Post-deploy](variables-post-deploy.md)—변수는 배포 후 작업을 제어합니다.

변수는 _hierarchical_&#x200B;입니다. 즉, 변수가 재정의되지 않으면 부모 환경에서 상속됩니다.

[!DNL Cloud Console]에서 또는 Adobe Commerce CLI를 사용하여 [관리자 변수](variables-admin.md)을(를) 설정할 수 있습니다. [`.magento.env.yaml`](configure-env-yaml.md) 파일에서 다른 환경 변수를 관리하여 지원 티켓 없이도 Pro Staging 및 Production을 비롯한 모든 환경에서 빌드 및 배포 작업을 관리할 수 있습니다.

>[!TIP]
>
>YAML 파일은 대/소문자를 구분하므로 탭을 허용하지 않습니다. `.magento.env.yaml` 파일 전체에서 일관된 들여쓰기를 사용하십시오. 그렇지 않으면 구성이 예상대로 작동하지 않을 수 있습니다. 이 설명서와 샘플 파일의 예제에서는 _두 개의 공간_ 들여쓰기를 사용합니다. [ece-tools validate 명령](configure-env-yaml.md#validate-configuration-file)을 사용하여 구성을 확인하십시오.
