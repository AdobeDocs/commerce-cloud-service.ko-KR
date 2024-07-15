---
title: Redis 서비스 설정
description: 클라우드 인프라에서 Adobe Commerce을 위한 백엔드 캐시 솔루션으로서 Redis를 설정하고 최적화하는 방법에 대해 알아봅니다.
feature: Cloud, Cache, Services
exl-id: d6971875-d302-495a-ad10-a81c507c2bc9
source-git-commit: 1253d8357fd2554050d1775fefbc420a2097db5f
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 0%

---

# Redis 서비스 설정

[Redis](https://redis.io)은(는) Adobe Commerce에서 기본적으로 사용하는 Zend 프레임워크 Zend_Cache_Backend_File을 대체하는 선택적 백엔드 캐시 솔루션입니다.

_구성 가이드_&#x200B;에서 [Redis 구성](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/redis/config-redis.html)을 참조하십시오.

{{service-instruction}}

**Redis를 사용하려면**:

1. `.magento/services.yaml` 파일에 필요한 이름과 형식을 추가합니다.

   ```yaml
   myredis:
       type: redis:<version>
   ```

   고유한 Redis 구성을 제공하려면 `.magento/services.yaml` 파일에 `core_config` 키를 추가하십시오.

   ```yaml
   cache:
       type: redis:<version>
   ```

1. `.magento.app.yaml` 파일에서 관계를 구성합니다.

   ```yaml
   runtime:
       extensions:
           - redis
   
   relationships:
       redis: "redis:redis"
   ```

1. 코드 변경 사항을 추가, 커밋 및 푸시합니다.

   ```bash
   git add .magento/services.yaml .magento.app.yaml && git commit -m "Enable redis service" && git push origin <branch-name>
   ```

1. [서비스 관계를 확인](services-yaml.md#service-relationships)합니다.

{{service-change-tip}}

## Redis CLI 사용

Redis 관계의 이름이 `redis`이라고 가정하면 `redis-cli` 도구를 사용하여 액세스할 수 있습니다.

1. SSH를 사용하여 Redis가 설치 및 구성된 통합 환경에 연결합니다.

1. 호스트에 대한 SSH 터널을 엽니다.

   ```bash
   redis-cli -h redis.internal
   ```

## 설치된 Redis 버전 가져오기

다음 명령을 사용하여 통합 환경에 설치된 Redis 버전을 가져옵니다.

```bash
redis-cli -h redis.internal info | grep version
```

샘플 응답:

```terminal
redis_version:7.0.5
gcc_version:8.3.0
```

### Pro 스테이징 및 프로덕션에 있는 Redis

스테이징 또는 프로덕션 환경에 Redis 버전을 설치하려면 `redis-server` 명령을 사용합니다.

```bash
redis-server -v
```

```terminal
Redis server v=7.0.5 ...
```

다음 명령을 사용하여 Pro 스테이징 또는 프로덕션 환경에 Redis 구성을 설치합니다.

```bash
echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
```

샘플 응답:

```terminal
"redis" : [
    {
        "cluster" : "project-master-123abc4",
        "fragment" : null,
        "host" : "redis.internal",
        "host_mapped" : false,
        "hostname" : "oblahblahblahblahe.redis.service._.magentosite.cloud",
        "ip" : "169.254.10.10",
        "password" : null,
        "path" : null,
        "port" : 6379,
        "public" : false,
        "query" : {},
        "rel" : "redis",
        "scheme" : "redis",
        "service" : "redis",
        "type" : "redis:7.0.5",
        "username" : null
    }
]
```

## Redis 문제 해결

Redis 문제 해결에 대한 도움말은 다음 Adobe Commerce 지원 문서를 참조하십시오.

- [문제 지연 관리자 로그인 또는 체크 아웃](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/redis-issue-delay-magento-admin-login-or-checkout.html)
- [확장된 Redis 캐시 구현 Adobe Commerce 2.3.5+](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-service-configuration.html)
- [MDVA-30102: Redis 캐시가 가득 참](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/support-tools/patches/v1-0-6/mdva-30102-magento-patch-redis-cache-getting-full.html)
- [Adobe Commerce에 대한 관리 경고: 메모리 경고 Redis](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-on-magento-commerce-redis-memory-warning-alert.html)
- [Adobe Commerce에 대한 관리 경고: 메모리 위험 경고 준비](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-on-magento-commerce-redis-memory-critical-alert.html)
- [Redis 문제 해결사](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/redis-troubleshooter.html)
