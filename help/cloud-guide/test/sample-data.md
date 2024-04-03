---
title: 샘플 데이터
description: 클라우드 인프라에 Adobe Commerce을 사용하여 샘플 데이터를 설치하는 방법을 알아봅니다.
exl-id: 517e549f-15ba-47b3-9236-a1c4d24c917d
source-git-commit: 8a0523f1714b6ea41887e99b5c31294cf5e5255e
workflow-type: tm+mt
source-wordcount: '205'
ht-degree: 0%

---

# 샘플 데이터

스토어를 개발할 때 예제 데이터가 필요한 경우 샘플 데이터를 설치할 수 있습니다. 이 데이터는 고객, 제품 및 기타 데이터와 함께 활성 Adobe Commerce 스토어를 시뮬레이션합니다. 이 샘플 데이터는 클라우드 인프라의 새로운 Adobe Commerce 템플릿 설치에 가장 적합합니다.

가장 좋은 방법은 개발 및 통합 환경에 샘플 데이터를 설치하는 것입니다. 스테이징 또는 프로덕션에서 샘플 데이터를 사용하는 경우 [제거](#reset-or-uninstall-sample-data) 시작하기 전의 정보 및 제품.

## 샘플 데이터 설치

샘플 데이터를 설치하려면:

1. 로컬 워크스테이션에서 프로젝트 디렉터리로 변경합니다.

1. 루트에 다음 명령을 입력하여 샘플 데이터를 추가합니다.

   ```bash
   ./bin/magento sampledata:deploy
   ```

1. 구성 요소가 업데이트될 때까지 기다립니다.

1. 변경 내용을 커밋하고 푸시합니다.

   ```bash
   git add -A && git commit -m "Install sample data"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. 프로젝트가 배포될 때까지 기다립니다.

1. 통합 환경의 상점 첫 번째 페이지로 이동하여 설치가 성공했는지 확인합니다. 다음을 통해 상점 첫 화면의 URL 링크를 찾을 수 있습니다. [!DNL Cloud Console].

1. 환경에 대한 스냅샷 생성:

   ```bash
   magento-cloud snapshot:create -e <environment-ID>
   ```

라이브 데이터로 개발 테스트를 시작할 수 있습니다!

## 샘플 데이터 재설정 또는 제거

샘플 데이터를 설치하는 데 사용되는 것과 동일한 절차에 따라 샘플 데이터를 재설정하거나 제거할 수 있습니다.

- 샘플 데이터 삭제: `./bin/magento sampledata:remove`
- 샘플 데이터 재설정: `./bin/magento sampledata:reset`
