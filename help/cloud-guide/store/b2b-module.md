---
title: B2B 모듈 활성화
description: 클라우드 인프라에서 Adobe Commerce에 B2B 모듈을 활성화하는 방법을 알아봅니다.
feature: Cloud, B2B
exl-id: 01d02ea0-1e7d-4608-adbf-1dad7f5e2182
source-git-commit: bb7a866b1896a8a43d01ad3f83dc655bcf383374
workflow-type: tm+mt
source-wordcount: '336'
ht-degree: 0%

---

# B2B 모듈 활성화

고객이 회사인 경우 Adobe Commerce 용 B2B 모듈을 설치하여 Adobe Commerce on cloud infrastructure Pro 프로젝트를 확장하여 B2B 모델을 비즈니스간 수용할 수 있습니다. 이 항목에서는 클라우드 인프라의 Adobe Commerce에 대한 B2B 모듈 설치 및 구성과 관련된 정보를 제공하지만, 다음 안내서에서 추가 B2B 정보를 찾을 수 있습니다.

- [Adobe Commerce B2B 개발자 안내서](https://developer.adobe.com/commerce/webapi/rest/b2b/)
- [Adobe Commerce B2B 사용 안내서](https://experienceleague.adobe.com/docs/commerce-admin/b2b/guide-overview.html)

>[!TIP]
>
>B2B는 클라우드 인프라의 Adobe CommerceAdobe 용 모듈이므로 시작하기 전에 Adobe Commerce 애플리케이션을 통합 또는 스테이징 환경에 배포하는 것이 좋습니다.

## B2B 모듈 설치

Adobe은 프로젝트에 B2B 모듈을 추가할 때 개발 분기에서 작업하는 것을 권장합니다. 분기가 없는 경우 [개발을 위한 분기 만들기](../development/cli-branches.md#create-a-branch-for-development). B2B 모듈을 설치할 때 `Magento_B2b` 모듈 이름은 `app/etc/config.php` 파일. 파일을 직접 편집할 필요는 없습니다.

**B2B 모듈을 설치하려면**:

1. 로컬 워크스테이션에서 프로젝트 디렉터리로 변경합니다.

1. 개발 분기를 만들거나 체크 아웃합니다.

1. B2B 모듈을 `require` 의 섹션 `composer.json` 파일.

   ```bash
   composer require magento/extension-b2b --no-update
   ```

1. 프로젝트 종속성을 업데이트합니다.

   ```bash
   composer update
   ```

1. 코드 변경 사항을 추가, 커밋 및 푸시합니다.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Install the B2B module."
   ```

   ```bash
   git push origin <branch-name>
   ```

1. 빌드 및 배포가 완료되면 SSH를 사용하여 원격 환경에 로그인하고 B2B 모듈이 설치되었는지 확인합니다.

   ```bash
   bin/magento module:status Magento_B2b
   ```

   확장 이름은 다음 형식을 사용합니다. `<VendorName>_<ComponentName>`.

   샘플 응답:

   ```terminal
   Magento_B2b : Module is enabled
   ```

   배포 오류가 발생하는 경우 다음을 참조하십시오 [구성 요소 장애 복구](../deploy/recover-failed-deployment.md).

## B2B 모듈 활성화

Composer를 사용하여 B2B 모듈을 설치하면 배포 프로세스에서 모듈을 자동으로 활성화합니다. B2B 모듈이 이미 설치되어 있는 경우 CLI를 사용하여 모듈을 활성화하거나 비활성화할 수 있습니다

B2B 모듈을 활성화합니다.

```bash
bin/magento module:enable Magento_B2b
```

샘플 응답:

```terminal
The following modules have been enabled:
- Magento_B2b

Cache cleared successfully.
Generated classes cleared successfully. Please run the 'setup:di:compile' command to generate classes.
Info: Some modules might require static view files to be cleared. To do this, run 'module:enable' with the --clear-static-content option to clear them.
```

다음을 참조하십시오 [확장 관리](extensions.md).

## B2B 모듈 구성

Adobe Commerce용 B2B 모듈을 설치한 후 다음을 수행해야 합니다 [메시지 소비자 시작](https://experienceleague.adobe.com/docs/commerce-admin/b2b/install.html#start-message-consumers) 을(를) 활성화하려면 _공유된 카탈로그_ 모듈. [B2B 기능 활성화](https://experienceleague.adobe.com/docs/commerce-admin/b2b/enable-basic-features.html).
