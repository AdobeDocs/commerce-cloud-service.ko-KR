---
title: 방화벽 속성
description: Commerce 애플리케이션 구성 파일에서 방화벽 속성을 구성하는 방법에 대한 예를 참조하십시오.
feature: Cloud, Configuration, Security
exl-id: f169c008-c62a-41b7-a98d-cccd81c7291a
source-git-commit: a8ecebc87bfae5deaf0fc7ff3e7dd3b255fe3f24
workflow-type: tm+mt
source-wordcount: '844'
ht-degree: 0%

---

# 방화벽 속성

>[!IMPORTANT]
>
>초보자 프로젝트만

시작 프로젝트의 경우 `firewall` 속성은 응용 프로그램에 _아웃바운드_ 방화벽을 추가합니다. 이 방화벽은 수신 요청에 영향을 주지 않습니다. Adobe Commerce 사이트를 _떠날_&#x200B;수 있는 `tcp`개의 아웃바운드 요청을 정의합니다. 이를 이그레스 필터링이라고 합니다. 아웃바운드 방화벽은 이그레스(사이트를 종료하거나 이탈할 수 있는 항목)를 필터링합니다. 이스케이프 처리를 제한하면 서버에 강력한 보안 도구가 추가됩니다.

## 기본 제한 정책

방화벽은 아웃바운드 트래픽을 제어하는 두 가지 기본 정책을 제공합니다. `allow` 및 `deny`. `allow` 정책은 기본적으로 모든 아웃바운드 트래픽을 _허용_&#x200B;합니다. 그리고 `deny` 정책은 기본적으로 모든 아웃바운드 트래픽을 _거부_&#x200B;합니다. 그러나 규칙을 추가할 때 기본 정책이 재정의되며 방화벽에서 규칙에 허용되지 않는 **모두** 아웃바운드 트래픽을 차단합니다.

시작 계획의 경우 기본 정책이 `allow`(으)로 설정됩니다. 이 설정을 사용하면 송신 필터링 규칙을 추가할 때까지 모든 현재 아웃바운드 트래픽이 차단 해제된 상태로 유지됩니다. 요청 시 기본 정책을 `deny`(으)로 설정할 수 있습니다.

**기본 정책을 확인하려면**:

```bash
magento-cloud p:curl --project PROJECT_ID /settings | grep -i outbound
```

정책에 대해 `deny`을(를) 요청하지 않은 경우 명령은 `allow`(으)로 설정된 정책을 표시해야 합니다.

```json
"outbound_restrictions_default_policy": "allow"
```

>[!NOTE]
>
>**키 가져오기**: 아웃바운드 규칙을 추가할 때 규칙에 추가한 도메인, IP 주소 또는 포트를 제외한 모든 아웃바운드 트래픽을 차단합니다. 따라서 프로덕션 사이트에 추가하기 전에 전체 아웃바운드 목록을 정의하고 테스트하는 것이 중요합니다.

## 방화벽 옵션

`.magento.app.yaml` 파일의 다음 예제 구성은 이그레스 필터링에 대한 규칙을 추가하는 데 사용할 수 있는 모든 `firewall` 옵션을 표시합니다.

```yaml
firewall:
    outbound:
        - # Common accessed domains
            domains:
                - newrelic.com
                - fastly.com
                - magento.com
                - magentocommerce.com
                - google.com
            ports:
                - 80
                - 443
            protocol: tcp # Can be omitted from rules.

        - # Adobe Stock integration
            domains:
                - account.adobe.com
                - stock.adobe.com
                - console.adobe.io
            ports:
                - 80
                - 443

        - # Payment services
            domains:
                - braintreepayments.com
                - paypal.com
            ports:
                - 80
                - 443

        - # Shipping services
            domains:
                - ups.com
                - usps.com
                - fedex.com
                - dhl.com
            ports:
                - 80
                - 443

        - # Vertex Integrated Address Cleansing
            domains:
                - mgcsconnect.vertexsmb.com
            ports:
                - 80
                - 443

        - # New Relic networks
            ips:
                - 162.247.240.0/22 # US region accounts
                - 185.221.84.0/22 # EU region accounts
            ports:
                - 443

        - # New Relic endpoints
            domains:
                - collector.newrelic.com, # US region accounts
                - collector.eu01.nr-data.net # EU region accounts
            ports:
                - 443

        - # Fastly IP ranges
            ips:
                - 23.235.32.0/20
                - 43.249.72.0/22
                - 103.244.50.0/24
                - 103.245.222.0/23
                - 103.245.224.0/24
                - 104.156.80.0/20
                - 146.75.0.0/16
                - 151.101.0.0/16
                - 157.52.64.0/18
                - 167.82.0.0/17
                - 167.82.128.0/20
                - 167.82.160.0/20
                - 167.82.224.0/20
                - 172.111.64.0/18
                - 185.31.16.0/22
                - 199.27.72.0/21
                - 199.232.0.0/16
            ports:
                - 80
                - 443
```

## 이그레스 필터링 규칙

아웃바운드 방화벽 구성은 규칙으로 구성됩니다. 필요한 만큼 규칙을 정의할 수 있습니다. 규칙의 요건은 다음과 같습니다.

**각 규칙:**

- 하이픈(`-`)으로 시작해야 합니다. 동일한 줄에 주석을 추가하면 한 규칙을 문서화하고 다음 규칙과 시각적으로 구분할 수 있습니다.
- `domains`, `ips` 또는 `ports` 옵션 중 하나 이상을 정의해야 합니다.
- `tcp` 프로토콜을 사용해야 합니다. 모든 규칙의 기본 프로토콜이므로 규칙에서 생략할 수 있습니다.
- `domains` 또는 `ips`을(를) 정의할 수 있지만 둘 다 동일한 규칙에 있을 수는 없습니다.
- `yaml`개의 주석(`#`)과 줄 바꿈을 포함하여 허용된 도메인, IP 주소 및 포트를 구성할 수 있습니다.

각 규칙은 다음 속성을 사용합니다.

### `domains`

`domains` 옵션은 FQDN(정규화된 도메인 이름) 목록을 허용합니다.

규칙이 `domains`을(를) 정의하지만 `ports`은(는) 정의하지 않는 경우 방화벽에서 모든 포트에 대한 도메인 요청을 허용합니다.

### `ips`

`ips` 옵션을 사용하면 CIDR 표기법으로 IP 주소 목록을 사용할 수 있습니다. 단일 IP 주소 또는 IP 주소 범위를 지정할 수 있습니다.

단일 IP 주소를 지정하려면 IP 주소 끝에 `/32` CIDR 접두사를 추가하십시오.

```
172.217.11.174/32  # google.com
```

IP 주소 범위를 지정하려면 [CIDR에 대한 IP 범위](https://ipaddressguide.com/cidr) 계산기를 사용하십시오.

규칙이 `ips`을(를) 정의하지만 `ports`은(는) 정의하지 않는 경우 방화벽에서 모든 포트에서 IP 요청을 허용합니다.

### `ports`

`ports` 옵션을 사용하면 포트 목록이 1부터 65535까지 허용됩니다. 이 예제의 대부분의 규칙에서 포트 `80` 및 `443`은(는) HTTP 요청과 HTTPS 요청을 모두 허용합니다. 하지만 New Relic의 경우 [네트워크 트래픽](https://docs.newrelic.com/docs/new-relic-solutions/get-started/networks/#agents)의 New Relic 설명서에 권장된 대로 규칙은 `443` 포트의 도메인 및 IP 주소에만 액세스할 수 있습니다.

규칙이 `ports`만 정의하는 경우 방화벽에서 정의된 포트의 모든 도메인 및 IP 주소에 대한 액세스를 허용합니다.

>[!NOTE]
>
>전자 메일을 보낼 SMTP 포트인 `25` 포트는 예외 없이 항상 차단됩니다.

### `protocol`

언급된 바와 같이, TCP는 기본값이며 규칙에 허용되는 유일한 프로토콜입니다. UDP 및 해당 포트는 허용되지 않습니다. 따라서 모든 규칙에서 `protocol` 옵션을 생략할 수 있습니다. 이 값을 포함하려면 예제의 첫 번째 규칙과 같이 값을 `tcp`(으)로 설정해야 합니다.

## 허용할 도메인 이름 찾기

이그레스 필터링 규칙에 포함할 도메인을 식별하려면 다음 명령을 사용하여 서버의 `dns.log` 파일을 구문 분석하고 사이트에 기록된 모든 DNS 요청 목록을 표시합니다.

```shell
awk '($5 ~/query/)' /var/log/dns.log | awk '{print $6}' | sort | uniq -c | sort -rn
```

또한 이 명령은 이그레스 필터링 규칙에 의해 만들어졌지만 차단된 DNS 요청을 표시합니다. 차단된 도메인은 출력에서 표시되지 않고 해당 요청만 수행됩니다. 출력에는 IP 주소를 사용하여 수행한 요청이 표시되지 않습니다.

```
Example output:

97 magento.com
93 magentocommerce.com
88 google.com
70 metadata.google.internal.0
70 metadata.google.internal
65 newrelic.com
56 fastly.com
17 mcprod-0vunku5xn24ip.ap-4.magentosite.cloud
6 advancedreporting.rjmetrics.com
```

IP 주소와 달리 도메인은 일반적으로 이그레스 필터링에 더 구체적이고 안전합니다. 예를 들어 CDN을 사용하는 서비스에 대한 IP 주소를 추가하는 경우 수백 또는 수천 개의 다른 도메인에서 사용할 수 있는 CDN에 대한 IP 주소를 허용할 수 있습니다. IP 주소 하나로 수천 개의 다른 서버에 대한 아웃바운드 액세스를 허용할 수 있습니다.

## 이그레스 필터링 규칙 테스트

사이트에 필요한 도메인 및 IP 주소에 대한 액세스 규칙을 수집하고 구성한 후 푸시 및 테스트할 차례입니다.

이그레스 필터링 규칙을 테스트하려면 다음을 수행하십시오.

1. `curl` 명령의 셸 스크립트를 만들어 규칙의 도메인 및 IP 주소에 액세스합니다. 차단해야 하는 도메인 및 IP 주소에 대한 액세스를 테스트하는 명령을 포함합니다.

1. `.magento.app.yaml` 파일에서 `post_deploy` 후크를 구성하여 스크립트를 실행합니다.

1. `firewall` 구성과 테스트 스크립트를 `integration` 분기에 푸시합니다.

1. `curl` 명령의 `post_deploy` 출력을 검사합니다.

1. `firewall` 규칙을 세분화하고 `curl` 스크립트를 업데이트하며 커밋하고, 푸시하고, 반복합니다.

### `curl` 스크립트 예

```shell
# curl-tests-for-egress-filtering.sh

# Use the -v option to display connection details

# Check domain access
curl -v newrelic.com
curl -v fastly.com
curl -v magento.com
curl -v magentocommerce.com
curl -v google.com
curl -v account.adobe.com
curl -v stock.adobe.com
curl -v console.adobe.io
curl -v braintreepayments.com
curl -v paypal.com
curl -v ups.com
curl -v usps.com
curl -v fedex.com
curl -v dhl.com
curl -v devdocs.magento.com

# Check domain denials
curl -v amazon.com
curl -v facebook.com
curl -v twitter.com

# IP address access
...

# IP address denials
...
```

### `post_deploy` 예

```yaml
hooks:
    build: |
        set -e
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    deploy: "php ./vendor/bin/ece-tools run scenario/deploy.xml\n"
    post_deploy: |
        set -e
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
        echo "[$(date)] post-deploy hook end"
        ./curl-tests-for-egress-filtering.sh
        echo "[$(date)] curl finished"
```
