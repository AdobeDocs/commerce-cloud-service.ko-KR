---
title: 시스템별 설정 관리의 예
description: 클라우드 인프라 환경의 모든 Adobe Commerce에서 스토어 구성 설정을 관리하고 동기화하는 방법에 대한 예를 참조하십시오.
hidefromtoc: true
source-git-commit: 13e76d3e9829155995acbb72d947be3041579298
workflow-type: tm+mt
source-wordcount: '865'
ht-degree: 0%

---


# 시스템별 설정 관리의 예

이 예에서는 구성 관리를 사용하여 모든 환경에서 저장소 설정을 일관되게 유지하는 방법을 보여 줍니다.

예제는에 정의된 다음 절차를 사용합니다. [스토어 설정](store-settings.md):

1. 통합 환경 저장소 관리자에 구성을 입력합니다.
1. 만들기 `config.php` 파일을 작성하여 로컬 워크스테이션에 전송합니다.
1. 푸시 `config.php` 을 원격 통합 환경에 추가합니다.
1. 관리자에서 설정을 편집할 수 없는지 확인합니다.
1. 필요한 사항을 수정합니다.

   * 통합 환경에서 구성 설정을 변경합니다.
   * 구성을 추가하려면 명령을 실행하여 만듭니다 `config.php` 다시. 새 구성이 파일에 추가됩니다.
   * 기존 구성을 제거하거나 편집하려면 파일을 수동으로 편집하십시오.
   * 커밋하고 푸시합니다.

예를 들어 다음 설정을 지정할 수 있습니다.

* 사용 안 함 [로케일](https://glossary.magento.com/locale) 통합 환경의 정적 파일 최적화 설정
* 스테이징 및 프로덕션 환경에서 정적 파일 최적화 활성화
* 각각에 대한 특정 자격 증명을 사용하여 스테이징 및 프로덕션에서 Fastly 구성

_정적 파일 최적화_ 는 JavaScript 및 CSS(Cascading Style Sheet)를 병합 및 축소하고 HTML 템플릿을 축소합니다. 다음을 참조하십시오 [정적 콘텐츠 배포 전략](../deploy/static-content.md).

## 전제 조건

이러한 구성 관리 작업을 완료하려면 다음이 필요합니다.

* 이 있는 프로젝트 리더 역할 [환경 &quot;admin&quot;](../project/user-access.md) 권한
* 통합, 스테이징 및 프로덕션 환경을 위한 관리자 URL 및 자격 증명

## Commerce 관리자 구성

통합 환경에서 관리자에 로그인하여 저장소, 웹 사이트, 모듈 또는 확장, 정적 파일 최적화 및 정적 컨텐츠 배포와 관련된 시스템 값에 대한 시스템 구성 설정을 수정할 수 있습니다. 다음을 참조하십시오 [구성 데이터](store-settings.md#scd-performance).

**로케일 및 정적 파일 최적화 설정을 변경하려면**:

1. 통합 환경 관리자에 로그인합니다. 다음을 통해 이 URL에 액세스할 수 있습니다. [[!DNL Cloud Console]](../project/overview.md).
1. 다음으로 이동 **스토어** > 설정 > **구성** > 일반 > **일반**.
1. 페이지 탐색에서 를 확장합니다. **로케일 옵션**.
1. 다음에서 **로케일** 로케일을 변경합니다. 나중에 다시 변경할 수 있습니다.

   ![로케일 변경](../../assets/locale-options.png)

1. 클릭 **구성 저장**.
1. 메시지가 표시되면 [캐시 초기화](https://docs.magento.com/user-guide/system/cache-management.html).
1. 관리자에서 로그아웃합니다.

## 값을 내보내고 config.php를 로컬 시스템으로 전송합니다.

이 단계에서는 를 만들고 전송합니다. `config.php` 로컬 컴퓨터에서 실행하는 명령을 사용하여 통합 환경의 구성 파일입니다.

이 절차는 의 2단계에 해당합니다. [권장 절차](store-settings.md). 만든 후 `config.php`, 로컬 시스템으로 전송하여 Git에 추가할 수 있습니다.

**생성 및 전송하려면`config.php`**:

1. 로컬 워크스테이션에서 프로젝트 디렉터리로 변경합니다.

1. 통합 환경으로 변경합니다.

   ```bash
   magento-cloud environment:checkout integration
   ```

1. 원격 데이터베이스의 로컬 덤프를 만듭니다.

   ```bash
   magento-cloud db:dump
   ```

의 다음 코드 조각 `config.php` 은 기본 로케일을 로 변경하는 예를 보여 줍니다. `en_GB` 및 정적 파일 최적화 설정 변경:

```php?start_inline=1
'general' => [
     'locale' => [
         'code' => 'en_GB',
         'timezone' => 'UTC',
     ],

     ... more ...

 'dev' => [
     'template' => [
         'allow_symlink' => '0',
         'minify_html' => '0',
     ],
     'js' => [
         'merge_files' => '0',
         'enable_js_bundling' => '0',
         'minify_files' => '0',
     ],
     'css' => [
         'merge_css_files' => '0',
         'minify_files' => '0',
     ],

     ... more ...
```

## 환경에 config.php 푸시 및 배포

이제 을(를) 만들었습니다. `config.php` 로컬 시스템으로 전송하고 Git에 커밋하여 환경에 푸시합니다. 이 절차는 의 3단계와 4단계에 해당합니다. [권장 절차](store-settings.md).

다음 명령은 를 추가하고, 커밋하고, `master` 분기:

```bash
git add app/etc/config.php && git commit -m "Add system-specific configuration" && git push origin master
```

스테이징 및 프로덕션에 전체 코드 배포. Starter의 경우 다음을 누릅니다. `staging` 및 `master` 분기. 배포 명령에 대한 자세한 내용은 [스토어 배포](../deploy/staging-production.md).

모든 환경에서 배포가 완료될 때까지 기다립니다.

## 구성 변경 사항 확인

푸시한 후 `config.php` 환경에서 변경한 모든 값은 관리에서 읽기 전용이어야 합니다. 이 예제에서 수정된 기본 로케일 및 정적 파일 최적화 설정은 관리자에서 편집할 수 없습니다. 이러한 구성 설정은에서 설정됩니다 `config.php`.

구성 변경 사항을 확인하려면:

1. 환경 중 하나에서 관리자에서 로그아웃합니다.
1. 관리자에 다시 로그인합니다.
1. 클릭 **스토어** > 설정 > **구성** > 일반 > **일반**.
1. 오른쪽 창에서 를 확장합니다. **로케일 옵션**.

   다음 샘플과 같이 몇 개의 필드를 편집할 수 없습니다. 이러한 구성 설정은 다음에서 유지 관리됩니다. `config.php`.

   ![특정 값은 관리자에서 더 이상 편집할 수 없습니다](../../assets/locale-options-disabled.png)

1. 관리자에서 로그아웃합니다.

## 시스템별 구성 설정 변경 및 업데이트

이러한 설정을 수정해야 하는 경우 `config.php` 텍스트 편집기를 사용하여 수동으로 파일을 만듭니다. 편집 또는 제거를 완료한 후 이전 단계에 따라 커밋하고 원격 환경에 푸시할 수 있습니다.

구성을 추가하려면 통합 환경을 수정하고 명령을 다시 실행하여 파일을 생성합니다. 모든 새 구성은 파일의 코드에 추가됩니다. 이전 단계에 따라 Git에 푸시합니다.

이 예제에서는 정적 파일 최적화 설정을 수정하고 JavaScript에 대한 새 설정을 추가합니다.

### 통합에서 구성 추가

통합 환경 관리자에서 구성 값을 추가하려면 다음을 수행합니다. 이 예제는 JavaScript 파일을 병합합니다.

1. 통합 관리자에서 로그아웃합니다.
1. 통합 책임자에 다시 로그인합니다.
1. 클릭 **스토어** > 설정 > **구성** > **고급** > **개발자**.
1. 오른쪽 창에서 를 확장합니다. **JavaScript 설정**.
1. 다음에서 **JavaScript 파일 병합** 목록, 클릭 **예**.
1. 클릭 **구성 저장**.
1. 메시지가 표시되면 [캐시 초기화](https://docs.magento.com/user-guide/system/cache-management.html).
1. 관리자에서 로그아웃합니다.

dump 명령을 다시 실행하면 새 구성이 파일에 추가됩니다.

```bash
magento-cloud db:dump
```

### 새 설정으로 config.php 편집

로컬에서 텍스트 편집기를 사용하여 업데이트된 내용을 편집합니다 `app/etc/config.php` 파일. JavaScript, HTML 및 CSS 파일에 대한 축소를 활성화하려면 이 설정을 편집하십시오.

```php?start_inline=1
 'dev' => [
     'template' => [
         'allow_symlink' => '0',
         'minify_html' => '0',
     ],

     ... more ...

     'js' => [
         'merge_files' => '0',
         'enable_js_bundling' => '0',
         'minify_files' => '0',
     ],
     'css' => [
         'merge_css_files' => '0',
         'minify_files' => '0',
     ],
```

축소를 허용하도록 설정을 수정하려면 다음을 편집합니다. `'0'` 끝 `'1'` 대상 `'minify_html'` 및 각 `'minify_files'` 옵션:

```php?start_inline=1
 'dev' => [
     'template' => [
         'allow_symlink' => '0',
         'minify_html' => '1',
     ],

     ... more ...

     'js' => [
         'merge_files' => '0',
         'enable_js_bundling' => '0',
         'minify_files' => '1',
     ],
     'css' => [
         'merge_css_files' => '0',
         'minify_files' => '1',
     ],
```

변경 사항을 파일에 저장합니다.

### 변경 사항을 Git에 푸시

변경 사항을 푸시하려면 다음을 입력합니다.

```bash
git add app/etc/config.php
```

```bash
git commit -m "Add system-specific configuration and edit settings"
```

```bash
git push origin master
```

배포가 완료될 때까지 기다립니다.

배포 프로세스를 반복하여 코드를 모든 환경에 푸시합니다.
