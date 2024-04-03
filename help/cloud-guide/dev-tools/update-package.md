---
title: ECE-Tools 패키지 업데이트
description: ECE-Tools 패키지를 업그레이드하여 Adobe Commerce on cloud infrastructure에 적용된 최신 수정 사항 및 기능을 활용하는 방법에 대해 알아봅니다.
feature: Cloud, Upgrade
exl-id: 7cce45eb-ae53-4468-b16d-4f4d3422ac52
source-git-commit: 513bc5b52f046ffd98005d80f34725b7f60b38bd
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 0%

---

# ECE-Tools 패키지 업데이트

에 대한 업데이트 `ece-tools` 패키지가 다른 항목도 업데이트합니다. [Commerce 패키지용 Cloud Tools 제품군](../release-notes/cloud-tools-suite.md), 다음에 대한 종속성: `ece-tools`. 따라서 다음을 지원하는 클라우드 인프라에서 Adobe Commerce 버전을 사용해야 합니다. `ece-tools` 패키지.

{{ece-tools-package}}

**전제 조건**:

- 업데이트하기 전에 `ece-tools`, 검토 [Cloud Tools Suite for Commerce 릴리스 정보](../release-notes/cloud-tools-suite.md).
- 에서 업데이트하는 경우 `ece-tools` 2002.0.22 또는 이전 버전을 2002.1.0으로, 검토 [이전 버전과 호환 불가능한 변경 사항](../release-notes/backward-incompatible-changes.md) Adobe Commerce on cloud infrastructure 프로젝트를 필요한 대로 변경합니다.
- 리뷰 [업그레이드 및 패치](../development/commerce-version.md#upgrade-from-older-versions) Adobe Commerce on cloud infrastructure 프로젝트와 호환되는 ECE-Tools 버전을 확인하려면 다음을 수행하십시오.

{{upgrade-tip}}

**를 업데이트하려면 `ece-tools` 패키지**:

1. 로컬 워크스테이션에서 Composer를 사용하여 업데이트를 수행합니다.

   ```bash
   composer update magento/ece-tools --with-dependencies
   ```

   >[!NOTE]
   >
   >이후에 업데이트할 수 없는 경우 `ece-tools` 버전 2002.0.8은 다음을 참조하십시오. [ECE-Tools 패키지를 사용하도록 프로젝트 업그레이드](install-package.md).

1. 코드 변경 사항을 추가, 커밋 및 푸시합니다.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Update magento/ece-tools"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. 테스트 유효성 검사 후 이 분기를 통합 분기에 병합합니다.
