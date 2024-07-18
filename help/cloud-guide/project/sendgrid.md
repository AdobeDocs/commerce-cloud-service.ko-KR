---
title: SendGrid 이메일 서비스
description: 클라우드 인프라의 Adobe Commerce용 SendGrid 이메일 서비스와 DNS 구성을 테스트하는 방법에 대해 알아봅니다.
exl-id: 30d3c780-603d-4cde-ab65-44f73c04f34d
source-git-commit: b49a51aba56f79b5253eeacb1adf473f42bb8959
workflow-type: tm+mt
source-wordcount: '1090'
ht-degree: 0%

---

# SendGrid 이메일 서비스

SendGrid SMTP(Simple Mail Transfer Protocol) 프록시 서비스는 다음을 지원하는 등 아웃바운드 이메일 인증 및 신뢰도 모니터링 서비스를 제공합니다.

* 모든 아웃바운드 트랜잭션 이메일
* 전용 IP 주소
* 이메일 도메인 유효성 검사를 위한 도메인 등록, DKIM(DomainKeys Identified Mail) 서명(Pro 스테이징 및 프로덕션 환경에만 해당)
* 사용자 정의 도메인 등록(Pro 전용)
* Starter 및 Pro 통합 환경을 위한 자동 통합 Pro Production and Staging 환경에서는 IaaS(Infrastructure as a Service) 하드웨어 프로비저닝 프로세스 중에 수동 프로비저닝 및 구성이 필요합니다

SendGrid SMTP 프록시는 수신 이메일을 수신하기 위한 범용 이메일 서버로 사용하거나 이메일 마케팅 캠페인과 함께 사용하기 위한 것이 아닙니다.

>[!TIP]
>
>[온보딩 UI](https://cloud.magento.com)에서 계정에 대한 SendGrid 세부 정보를 찾아 **프로젝트 세부 정보** > **호스팅 정보** 탭을 선택할 수 있습니다.

## 이메일 활성화 또는 비활성화

Cloud Console 또는 명령줄에서 각 환경에 대해 발신 이메일을 활성화하거나 비활성화할 수 있습니다.

기본적으로 발신 이메일은 Pro 프로덕션 및 스테이징 환경에서 활성화됩니다. 그러나 [명령줄](outgoing-emails.md#enable-emails-in-the-cli) 또는 [클라우드 콘솔](outgoing-emails.md#enable-emails-in-the-cloud-console)을 통해 `enable_smtp` 속성을 설정할 때까지 [!UICONTROL Outgoing emails]이(가) 환경 설정에서 비활성화되어 표시될 수 있습니다. 통합 및 스테이징 환경에 대해 발신 이메일을 활성화하여 Cloud 프로젝트 사용자에 대해 이중 인증 또는 암호 재설정 이메일을 보낼 수 있습니다. [테스트를 위한 전자 메일 구성](outgoing-emails.md)을 참조하세요.

Pro 프로덕션 또는 스테이징 환경에서 발신 이메일을 사용하지 않도록 설정하거나 다시 사용하도록 설정해야 하는 경우 [Adobe Commerce 지원 티켓](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide)을 제출할 수 있습니다.

>[!TIP]
>
>[명령줄](outgoing-emails.md#enable-emails-in-the-cli)에 의해 [!UICONTROL enable_smtp] 속성 값을 업데이트하면 [클라우드 콘솔](outgoing-emails.md#enable-emails-in-the-cloud-console)에서 이 환경에 대한 [!UICONTROL Enable outgoing emails] 설정 값도 변경됩니다.

## SendGrid 대시보드

모든 클라우드 프로젝트는 중앙 계정에서 관리되므로 지원만 SendGrid 대시보드에 액세스할 수 있습니다. SendGrid는 하위 계정 제한 기능을 제공하지 않습니다.

활동 로그에서 게재 상태 또는 반송되거나 거부되거나 차단된 전자 메일 주소 목록을 검토하려면 [Adobe Commerce 지원 티켓을 제출](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#submit-ticket)하십시오. 지원 팀 **은(는) 30일 넘는 활동 로그를 검색할 수 없습니다**.

가능한 경우 요청에 다음 정보를 포함하십시오.

* 영향을 받는 이메일 주소
* 해당 일정(지난 30일 이내만)
* 이메일 제목

## 도메인 키 식별된 메일(DKIM)

DKIM은 인터넷 서비스 제공자(ISP)가 합법적인 발신자 주소와 가짜 발신자 주소를 모두 식별할 수 있도록 하는 이메일 인증 기술로, 피싱과 이메일 사기에 일반적으로 사용되는 기술입니다. DKIM은 DNS 레코드를 관리하는 도메인 소유자에 의존합니다. DKIM을 사용할 때 보낸 사람 서버는 개인 키를 사용하여 메시지에 서명합니다. 또한 도메인 소유자는 수정된 `TXT` 레코드인 DKIM 레코드를 보낸 사람 도메인의 DNS 레코드에 추가합니다. 이 `TXT` 레코드에는 받는 사람 메일 서버에서 메시지 서명을 확인하는 데 사용하는 공개 키가 포함되어 있습니다. DKIM 공개 키 암호화 절차를 통해 수신자는 발신자의 신원을 확인할 수 있습니다. [DKIM 레코드 설명](https://docs.sendgrid.com/ui/account-and-settings/dkim-records)을 참조하세요.

>[!WARNING]
>
>SendGrid DKIM 서명 및 도메인 인증 지원은 Starter 프로젝트가 아닌 Pro 프로젝트에만 사용할 수 있습니다. 그 결과, 아웃바운드 트랜잭션 이메일은 스팸 필터에 의해 플래그가 지정될 수 있습니다. DKIM을 사용하면 인증된 이메일 발신자로서의 게재 속도가 향상됩니다. 메시지 게재 속도를 향상시키기 위해 Starter에서 Pro로 업그레이드하거나 자체 SMTP 서버 또는 이메일 게재 서비스 공급자를 사용할 수 있습니다. _관리 시스템 안내서_&#x200B;에서 [전자 메일 연결 구성](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/communications/email-communications)을 참조하십시오.

### 보낸 사람 및 도메인 인증

SendGrid가 Pro 프로덕션 또는 스테이징 환경에서 사용자를 대신하여 트랜잭션 이메일을 보내려면 세 개의 SendGrid 하위 도메인 DNS 항목을 포함하도록 DNS 설정을 구성해야 합니다. 각 SendGrid 계정에는 아웃바운드 전자 메일을 인증하는 데 사용되는 고유한 `TXT` 레코드가 할당됩니다.

**도메인 인증을 사용하려면**:

1. 특정 도메인(**Pro 스테이징 및 프로덕션 환경만**)에 대해 DKIM 활성화를 요청하려면 [지원 티켓](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#submit-ticket)을 제출하십시오.
1. 지원 티켓에서 제공한 `TXT` 및 `CNAME` 레코드로 DNS 구성을 업데이트합니다.

**계정 ID가**&#x200B;인 `TXT` 레코드 예:

```text
v=spf1 include:u17504801.wl.sendgrid.net -all
```

**레코드 `CNAME`개 예**:

| 도메인 | 포인트 대상 | 레코드 유형 |
| ---------- | ---------- | ------------- |
| em.emaildomain.com | uxxxxxx.wl.sendgrid.net | CNAME |
| s1._domainkey.emaildomain.com | s1.domainkey.uxxxxxx.wl.sendgrid.net | CNAME |
| s2._domainkey.emaildomain.com | s2.domainkey.uxxxxxx.wl.sendgrid.net | CNAME |

### DKIM 서명 및 자동화된 보안

인증된 도메인을 설정할 때 자동화된 보안과 수동 보안 중에서 선택할 수 있습니다. 자동화된 보안을 선택하면 SendGrid가 DKIM 및 SPF 레코드를 자동으로 관리합니다. 계정에 새로운 전용 전송 IP 주소를 추가하면 SendGrid가 DNS 설정 및 DKIM 서명을 즉시 업데이트합니다. 자동화된 보안을 해제하면 전송 도메인을 변경할 때마다 DKIM 서명을 업데이트할 책임이 있습니다.

**자동화된 보안 사용 예**:

```text
subdomain.mydomain.com. | CNAME | uxxxxxx.wl.sendgrid.net
s1._domainkey.mydomain.com. | CNAME | s1.domainkey.uxxxxxx.wl.sendgrid.net
s2._domainkey.mydomain.com. | CNAME | s2.domainkey.uxxxxxx.wl.sendgrid.net
```

**자동화된 보안 사용 안 함**:

```text
me12345.mydomain.com | MX | mx.sendgrid.net
me12345.mydomain.com | TXT | v=spf1 include:sendgrid.net ~all
m1._mydomain.com | TXT | k=rsa; t=s; p=<public-key>
```

도메인 인증이 설정되면 SendGrid가 SPF(보안 정책 프레임워크) 및 DKIM 레코드를 자동으로 처리합니다. SendGrid가 DNS 레코드에 추가할 `CNAME` 레코드를 제공하면 SPF 레코드를 수동으로 관리하지 않고도 전용 IP 주소를 추가하고 다른 계정을 업데이트할 수 있습니다. [자동화된 보안 및 DKIM 서명](https://docs.sendgrid.com/ui/account-and-settings/dkim-records#automated-security-and-your-dkim-signature)을 참조하세요.

DNS 구성을 테스트하려면:

```
dig CNAME em.domain_name
dig CNAME s1._domainkey.domain_name
dig CNAME s2._domainkey.domain_name
```

## 트랜잭션 이메일 임계값

트랜잭션 이메일 임계값은 비프로덕션 환경에서 매월 12,000개의 이메일을 보내는 것과 같이 특정 기간 내에 Pro 환경에서 보낼 수 있는 트랜잭션 이메일 메시지 수를 나타냅니다. 임계값은 스팸 전송을 방지하고 이메일 평판을 손상시킬 수 있는 가능성을 방지하기 위해 설계되었습니다.

보낸 사람의 신뢰도 점수가 95% 이상인 한 프로덕션 환경에서 보낼 수 있는 이메일 수에는 엄격한 제한이 없습니다. 이러한 신뢰도는 반송되거나 거부된 이메일 수와 DNS 기반 스팸 레지스트리가 도메인을 잠재적인 스팸 소스로 플래그를 지정했는지 여부에 따라 영향을 받습니다. _Commerce 지원 기술 자료_&#x200B;에서 [Adobe Commerce에서 SendGrid 크레딧이 초과되면 전송되지 않은 전자 메일](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/emails-not-being-sent-sendgrid-credits-exceeded)을(를) 참조하십시오.

**최대 크레딧이 초과되었는지 확인**:

1. 로컬 워크스테이션에서 프로젝트 디렉터리로 변경합니다.

1. SSH를 사용하여 원격 환경에 로그인합니다.

   ```bash
   magento-cloud ssh
   ```

1. `authentication failed : Maxium credits exceeded`개 항목에 대한 `/var/log/mail.log`을(를) 확인하십시오.

   `authentication failed`개의 로그 항목이 표시되고 **이메일 전송 신뢰도**&#x200B;가 최소 95개라면 [Adobe Commerce 지원 티켓을 제출](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#submit-ticket)하여 크레딧 배분을 늘릴 수 있습니다.

### 이메일 전송 신뢰도

이메일 전송 신뢰도는 인터넷 서비스 공급자(ISP)가 이메일 메시지를 보내는 회사에 할당하는 점수입니다. 점수가 높을수록 ISP가 수신자의 받은 편지함에 메시지를 전달할 가능성이 높습니다. 점수가 특정 수준 아래로 떨어지면 ISP는 메시지를 수신자의 스팸 폴더로 라우팅하거나 메시지를 완전히 거부할 수도 있습니다. 신뢰도 점수는 다른 IP 주소와 비교하여 IP 주소 등급의 30일 평균 및 스팸 고객 불만 비율과 같은 몇 가지 요인에 의해 결정됩니다. 전자 메일 전송 평판을 확인하는 [8가지 방법](https://sendgrid.com/en-us/blog/5-ways-check-sending-reputation)을 참조하세요.
