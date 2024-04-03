---
title: 사용자 정의 테마
description: 클라우드 인프라에 Adobe Commerce을 사용하여 사용자 지정 테마를 설치하는 방법을 알아봅니다.
feature: Cloud, Themes
exl-id: f08134ab-daea-471d-a927-02531d36a809
source-git-commit: bb7a866b1896a8a43d01ad3f83dc655bcf383374
workflow-type: tm+mt
source-wordcount: '295'
ht-degree: 0%

---

# 사용자 정의 테마

프로젝트에 있는 하나 이상의 상점 및 사이트에 사용할 하나 이상의 테마를 설치할 수 있습니다. 테마는 이미지, 글꼴, CSS, JavaScript, PHP 등을 포함하여 스토어를 완전히 디자인하는 여러 정적 파일을 포함합니다. 테마의 코드를 파일 시스템에 추출하거나 작성기를 사용하여 테마를 추가할 수 있습니다.

## 수동으로 테마 설치

테마를 수동으로 설치하려면 압축된 아카이브 또는 다음과 유사한 디렉토리 구조에 테마 코드가 있어야 합니다.

```text
<VendorName>
  ├── composer.json
      ├── etc
      │   └── view.xml
      ├── media
      ├── registration.php
      ├── theme.xml
      └── web
          ├── css
          │   └── source
          ├── fonts
          ├── images
          └── js
```

**테마를 수동으로 설치하려면**:

1. 아래의 테마 코드 복사 `<Project root dir>/app/design/frontend` 상점 테마 또는 `<Project root dir>/app/design/adminhtml` 관리 테마의 경우. 최상위 디렉토리가 `<VendorName>`; 그렇지 않으면 테마가 제대로 설치되지 않습니다.

   ```bash
   cp -r ExampleTheme <project-root>/app/design/frontend
   ```

1. 올바른 위치에 복사된 테마를 확인합니다.

   * 상점 테마: `ls <project-root>/app/design/frontend`
   * 관리자 테마: `ls <project-root>/app/design/adminhtml`

   샘플은 다음과 같습니다.

   예시 테마 Adobe Commerce

1. 파일을 추가하고 커밋합니다.

   ```bash
   git add -A && git commit -m "Add theme"
   ```

1. 파일을 분기로 푸시합니다.

   ```bash
   git push origin <branch name>
   ```

1. 배포가 완료될 때까지 기다립니다.
1. 관리자에 로그인합니다.
1. 클릭 **콘텐츠** > 디자인 > **테마**.

   테마가 오른쪽 창에 표시됩니다.

## 작성기를 사용하여 테마 설치

Composer를 사용하여 테마를 설치하는 것은 Composer를 사용하여 다른 확장을 설치하는 것과 같습니다. 다음을 참조하십시오 [모듈 설치, 관리 및 업그레이드](extensions.md) 을 참조하십시오.

작성기를 사용하여 테마를 설치하려면 다음을 수행하십시오.

1. Commerce Marketplace에서 테마를 구매합니다.
1. 테마의 작성기 이름을 가져옵니다.
1. Adobe Commerce 루트 디렉토리로 변경하고 다음 명령을 입력합니다.

   ```bash
   composer require <vendor>/<name>:<version>
   ```

   For example,

   ```bash
   composer require zero1/theme-fashionista-theme:1.0.0
   ```

1. 종속성이 업데이트될 때까지 기다립니다.
1. 다음 명령을 입력합니다.

   ```bash
   git add -A && git commit -m "Add theme"
   ```

   ```bash
   git push origin <branch name>
   ```

1. 관리자에 로그인합니다.
1. 클릭 **콘텐츠** > 디자인 > **테마**.

   테마가 오른쪽 창에 표시됩니다.

## 여러 테마

로케일별로 다른 테마와 같이 여러 테마를 사용할 때는 `SCD_MATRIX` 테마 배포 사용자 지정을 위한 환경 변수입니다. 다음을 참조하십시오. [빌드](../environment/variables-build.md#scd_matrix) 또는 [배포](../environment/variables-deploy.md#scd_matrix) 의 단계 [환경 구성](../environment/configure-env-yaml.md).
