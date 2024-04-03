---
title: 프로젝트 구조
description: 클라우드 인프라의 Adobe Commerce에 대한 파일 구조 및 프로젝트 템플릿에 대해 알아봅니다.
exl-id: 6dc559bd-116b-4745-a85b-731508e113ff
source-git-commit: 47ef728ea46b137eeaabbdbada940143d8773ef0
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 0%

---

# 프로젝트 구조

클라우드 인프라 프로젝트의 Adobe Commerce에는 자격 증명 및 애플리케이션 구성에 필요한 파일이 포함되어 있습니다. 이러한 파일은 Adobe Commerce 버전에 따라에서 템플릿으로 사용할 수 있습니다. 에서 Adobe Commerce 버전을 기반으로 하는 클라우드 템플릿 을 참조하십시오. [`magento/magento-cloud` GitHub 저장소](https://github.com/magento/magento-cloud).

다음 표에서는 클라우드 프로젝트에 포함된 파일에 대해 설명합니다.

| 파일 | 설명 |
| ------------------------- | ------------ |
| `/.magento/routes.yaml` | 리디렉션하는 구성 파일 `www` apex 도메인으로 `php` HTTP를 제공하는 애플리케이션입니다. 다음을 참조하십시오 [경로 구성](../routes/routes-yaml.md). |
| `/.magento/services.yaml` | MySQL 인스턴스(MariaDB), Redis 및 OpenSearch 또는 Elasticsearch을 정의하는 구성 파일입니다. 다음을 참조하십시오 [서비스 구성](../services/services-yaml.md). |
| `/app` | 다음 `code` 폴더는 사용자 지정 모듈에 사용됩니다. 다음 `design` 폴더는 다음에 사용됩니다. [사용자 지정 테마](../store/custom-theme.md). 다음 `etc` 폴더에는 응용 프로그램에 대한 구성 파일이 포함되어 있습니다. |
| `/m2-hotfixes` | 사용자 지정 패치에 사용됩니다. |
| `/update` | 지원 모듈에서 사용하는 서비스 폴더입니다. |
| `.gitignore` | 무시할 파일 및 디렉터리를 지정합니다. 다음을 참조하십시오 [`.gitignore` 참조](#ignoring-files). |
| `.magento.app.yaml` | 응용 프로그램을 빌드할 속성을 정의하는 구성 파일입니다. 다음을 참조하십시오 [애플리케이션 구성](../application/configure-app-yaml.md). |
| `.magento.env.yaml` | 빌드, 배포 및 배포 후 단계에 대한 구성 파일입니다. 다음 `ece-tools` 패키지에는 이 파일의 샘플이 포함되어 있습니다. 다음을 참조하십시오 [환경 구성](../environment/configure-env-yaml.md). |
| `composer.json` | Adobe Commerce 및 구성 스크립트를 가져와 애플리케이션을 준비합니다. 다음을 참조하십시오 [필수 패키지](../development/overview.md#required-packages). |
| `composer.lock` | 모든 패키지에 대한 버전 종속성을 저장합니다. 다음을 참조하십시오 [필수 패키지](../development/overview.md#required-packages). |
| `magento-vars.php` | 을 정의하는 데 사용됨 [여러 저장소](../store/multiple-sites.md) 및 사이트는 변수를 사용합니다. |

{style="table-layout:auto"}

>[!NOTE]
>
>로컬 변경 사항을 원격 서버에 푸시할 때 배포 스크립트는에서 구성 파일에 정의된 값을 사용합니다. `.magento` 디렉토리가 표시되면 스크립트는 디렉토리와 해당 컨텐트를 삭제합니다. 로컬 개발 환경은 영향을 받지 않습니다.

## 응용 프로그램 루트 디렉터리

애플리케이션 루트 디렉토리의 위치는 환경에 따라 다릅니다.

- **Starter 및 Pro 통합**: `/app`
- **스타터 프로덕션**: `/<project-ID>`
- **Pro 스테이징**: `/<project-ID>_stg`
- **프로프로덕션**: `/<project-ID>`

### 쓰기 가능한 디렉터리

원격 통합, 스테이징 및 프로덕션 환경은 읽기 전용입니다. 다음 디렉터리는 *전용* 보안상의 이유로 쓰기 가능한 디렉터리:

- `var`
- `pub/static`
- `pub/media`
- `app/etc`
- `/tmp`

>[!NOTE]
>
>프로덕션 및 스테이징 환경에서 3노드 클러스터의 각 노드에는 `/tmp` 다른 노드와 공유되지 않은 디렉토리.

## 파일 무시

기지가 있어요 `.gitignore` Adobe Commerce on cloud infrastructure 프로젝트 저장소를 사용하여 파일을 생성합니다. 최신 항목 보기 [magento-cloud 저장소의 .giginore 파일](https://github.com/magento/magento-cloud/blob/master/.gitignore). 에 있는 파일을 추가하려면 `.gitignore` list, `-f` 커밋 준비 시 (강제) 옵션:

```bash
git add <path/filename> -f
```

## 기본 템플릿 변경

다음 단계를 사용하여 클라우드 인프라에서 Adobe Commerce에 대한 최신 기본 템플릿을 반영하도록 기존 프로젝트의 구조를 변경할 수 있습니다.

1. 프로젝트를 로컬 워크스테이션에 복제합니다.

1. 업데이트 `composer.json` 다음 값을 가진 파일 `extra` 섹션.

   ```json
   "extra": {
       "magento-force": true
       "magento-deploystrategy": "copy"
   }
   ```

1. 추가 `.gitignore` 기본 템플릿용으로 설계된 파일입니다. 예를 들어, `.gitignore` 버전 2.2.6 템플릿 파일에서는 [.gitigore for 2.2.6](https://github.com/magento/magento-cloud/blob/2.2.6/.gitignore) 파일을 참조로 사용하십시오.

1. git 캐시를 지웁니다.

   ```bash
   git rm -r --cached .
   ```

1. 변경 사항을 추가하고 커밋합니다.

   ```bash
   git add -A && git commit -m "Update base template"
   ```

{{redeploy-warning}}
