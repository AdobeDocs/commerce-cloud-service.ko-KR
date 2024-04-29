---
source-git-commit: 74ce2b69c1d4f9412f9eac98dfea93cc08f737b6
workflow-type: tm+mt
source-wordcount: '2180'
ht-degree: 0%

---
# Adobe Commerce용 클라우드 패키지

<!-- The 'packages' variable contains the 'packages' node of the '_data/codebase/cloud/composer_lock.json' file
 -->

<!-- The 'packages-dev' variable contains the 'packages-dev' node of the '_data/codebase/cloud/composer_lock.json' file
 -->

<!-- The 'product' variable contains data of the 'magento/magento-cloud-metapackage' package
 -->

<!-- The edition variable contains `cloud` value from the _data/names.yml file
 -->

클라우드 인프라의 Adobe Commerce은 Composer를 사용하여 PHP 패키지를 관리합니다.

다음 `composer.json` 파일은 패키지 목록을 선언하지만 `composer.lock` 파일에는 Adobe Commerce 설치를 빌드하는 데 사용되는 패키지의 전체 목록(각 패키지 및 해당 종속 항목의 전체 버전)이 저장됩니다.

다음 참조 설명서는 `composer.lock` 파일 및 이 패키지는 Adobe Commerce on cloud infrastructure 2.4.7에 포함된 필수 패키지를 다룹니다.

## 종속성

`magento/magento-cloud-metapackage 2.4.7` 에는 다음과 같은 종속성이 있습니다.

```config
fastly/magento2: ^1.2.34
magento/ece-tools: ^2002.1.0
magento/module-paypal-on-boarding: ~100.5.0
magento/product-enterprise-edition: >=2.4.7 <2.4.8
```

## 타사 라이선스

### Apache-2.0, LGPL-2.1 전용

<table>
  <thead>
    <tr>
      <th>이름</th>
      <th>유형</th>
      <th>설명</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td>
      <a href="https://github.com/elastic/elasticsearch-php.git">elasticsearch/elasticsearch</a>
    </td>
    <td>라이브러리</td>
    <td>Elasticsearch용 PHP 클라이언트</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/opensearch-project/opensearch-php.git">opensearch-project/opensearch-php</a>
    </td>
    <td>라이브러리</td>
    <td>OpenSearch용 PHP 클라이언트</td>
  </tr>
  </tbody>
</table>

### Apache-2.0

<table>
  <thead>
    <tr>
      <th>이름</th>
      <th>유형</th>
      <th>설명</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td>
      <a href="https://github.com/adobe/stock-api-libphp.git">astock/stock-api-libphp</a>
    </td>
    <td>라이브러리</td>
    <td>Adobe Stock API 라이브러리</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/awslabs/aws-crt-php.git">aws/aws-crt-php</a>
    </td>
    <td>라이브러리</td>
    <td>PHP용 AWS 공용 런타임</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/aws/aws-sdk-php.git">aws/aws-sdk-php</a>
    </td>
    <td>라이브러리</td>
    <td>PHP용 AWS SDK - PHP 프로젝트에서 Amazon Web Services 사용</td>
  </tr>
  <tr>
    <td>
      paypal/module-braintree
    </td>
    <td>메타패키지</td>
    <td>Braintree Magento</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/wikimedia/less.php.git">wikimedia/less.php</a>
    </td>
    <td>라이브러리</td>
    <td>LESS 프로세서의 PHP 포트</td>
  </tr>
  </tbody>
</table>

### - 조항

<table>
  <thead>
    <tr>
      <th>이름</th>
      <th>유형</th>
      <th>설명</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td>
      <a href="https://github.com/Bacon/BaconQrCode.git">베이컨/베이컨-qr-코드</a>
    </td>
    <td>라이브러리</td>
    <td>BaconQrCode는 PHP용 QR 코드 생성기입니다.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/DASPRiD/Enum.git">dasprid/enum</a>
    </td>
    <td>라이브러리</td>
    <td>PHP 7.1 enum 구현</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/webimpress/safe-writer.git">webimpress/safe-writer</a>
    </td>
    <td>라이브러리</td>
    <td>경합 조건을 피하기 위해 파일을 안전하게 작성하는 도구</td>
  </tr>
  </tbody>
</table>

### - 조항

<table>
  <thead>
    <tr>
      <th>이름</th>
      <th>유형</th>
      <th>설명</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td>
      <a href="https://github.com/colinmollenhour/Cm_Cache_Backend_File.git">colinmollenhour/cache-backend-file</a>
    </td>
    <td>magento-module</td>
    <td>Stock Zend_Cache_Backend_File 백엔드는 캐시된 항목의 수가 증가하면 사용할 수 없게 되기 때문에 태그로 정리하는 성능이 매우 낮습니다. 이 백엔드는 특히 태그 정리를 위해 많은 변경 사항을 적용하여 큰 성능 향상을 가져옵니다.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/colinmollenhour/php-redis-session-abstract.git">colinmollenhour/php-redis-session-abstract</a>
    </td>
    <td>라이브러리</td>
    <td>낙관적 잠금을 사용하는 Redis 기반 세션 처리기</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/fastly/fastly-magento2.git">fastly/magento2</a>
    </td>
    <td>magento2-module</td>
    <td>Magento 2.4.x용 Fastly CDN 모듈</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/firebase/php-jwt.git">firebase/php-jwt</a>
    </td>
    <td>라이브러리</td>
    <td>PHP에서 JSON 웹 토큰(JWT)을 인코딩하고 디코딩하는 간단한 라이브러리입니다. 현재 사양을 준수해야 합니다.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/google/recaptcha.git">google/recaptcha</a>
    </td>
    <td>라이브러리</td>
    <td>스팸 및 악용으로부터 웹 사이트를 보호하는 무료 서비스인 reCAPTCHA용 클라이언트 라이브러리.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-captcha.git">라미나스/라미나스-캡차</a>
    </td>
    <td>라이브러리</td>
    <td>Figlet, 이미지, ReCaptcha 등을 사용하여 CAPTCHA를 생성하고 확인합니다.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-code.git">라미나스/라미나스 코드</a>
    </td>
    <td>라이브러리</td>
    <td>PHP Reflection API, 정적 코드 스캔 및 코드 생성에 대한 확장</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-config.git">라미나스/라미나스-config</a>
    </td>
    <td>라이브러리</td>
    <td>는 응용 프로그램 코드 내에서 이 구성 데이터에 액세스하기 위한 중첩된 개체 속성 기반 사용자 인터페이스를 제공합니다</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-crypt.git">라미나스/라미나스 크립트</a>
    </td>
    <td>라이브러리</td>
    <td>강력한 암호화 도구 및 암호 해싱</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-db.git">laminas/laminas-db</a>
    </td>
    <td>라이브러리</td>
    <td>데이터베이스 추상화 계층, SQL 추상화, 결과 집합 추상화, RowDataGateway 및 TableDataGateway 구현</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-di.git">라미나스/라미나스</a>
    </td>
    <td>라이브러리</td>
    <td>PSR-11 컨테이너에 대한 자동 종속성 삽입</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-escaper.git">라미나스/라미나스 탈진기</a>
    </td>
    <td>라이브러리</td>
    <td>HTML, HTML 속성, JavaScript, CSS 및 URL을 안전하고 안전하게 이스케이프 처리합니다.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-eventmanager.git">laminas/laminas-eventmanager</a>
    </td>
    <td>라이브러리</td>
    <td>PHP 응용 프로그램 내에서 이벤트를 트리거하고 수신</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-feed.git">라미나스/라미나스 공급</a>
    </td>
    <td>라이브러리</td>
    <td>에서는 RSS 및 Atom 피드를 만들고 소비하는 기능을 제공합니다.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-file.git">라미나스/라미나스 파일</a>
    </td>
    <td>라이브러리</td>
    <td>PHP 클래스 파일 찾기</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-filter.git">라미나스/라미나스 필터</a>
    </td>
    <td>라이브러리</td>
    <td>프로그래밍 방식으로 데이터 및 파일 필터링 및 정규화</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-http.git">laminas/laminas-http</a>
    </td>
    <td>라이브러리</td>
    <td>HTTP(Hyper-Text Transfer Protocol) 요청을 수행할 수 있는 간편한 인터페이스 제공</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-i18n.git">라미나스/라미나스-i18n</a>
    </td>
    <td>라이브러리</td>
    <td>애플리케이션에 대한 번역을 제공하고 국제화된 값을 필터링하고 검증합니다.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-json.git">laminas/laminas-json</a>
    </td>
    <td>라이브러리</td>
    <td>는 기본 PHP를 JSON으로 serialize하고 JSON을 기본 PHP로 decoding하는 편리한 방법을 제공합니다.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-loader.git">라미나스/라미나스 로더</a>
    </td>
    <td>라이브러리</td>
    <td>자동 로드 및 플러그인 로드 전략</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-mail.git">라미나스/라미나스 메일</a>
    </td>
    <td>라이브러리</td>
    <td>텍스트 및 MIME 호환 다중 파트 전자 메일 메시지를 모두 작성하고 전송하는 일반 기능을 제공합니다.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-math.git">라미나스/라미나스-수학</a>
    </td>
    <td>라이브러리</td>
    <td>암호학적으로 안전한 의사 난수를 만들고 큰 정수를 관리합니다.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-mime.git">laminas/laminas-mime</a>
    </td>
    <td>라이브러리</td>
    <td>MIME 메시지 및 부분 만들기 및 구문 분석</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-modulemanager.git">laminas/laminas-modulemanager</a>
    </td>
    <td>라이브러리</td>
    <td>Laminas-mvc 응용 프로그램용 모듈식 응용 시스템</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-mvc.git">라미나스/라미나스-mvc</a>
    </td>
    <td>라이브러리</td>
    <td>MVC 응용 프로그램, 컨트롤러 및 플러그인을 포함한 Laminas의 이벤트 기반 MVC 계층</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-oauth.git">laminas/laminas-oauth</a>
    </td>
    <td>라이브러리</td>
    <td></td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-permissions-acl.git">laminas/laminas-permissions-acl</a>
    </td>
    <td>라이브러리</td>
    <td>권한 관리를 위한 가볍고 유연한 ACL(액세스 제어 목록) 구현 제공</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-recaptcha.git">laminas/laminas-recaptcha</a>
    </td>
    <td>라이브러리</td>
    <td>ReCaptcha 웹 서비스에 대한 OOP 래퍼</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-router.git">라미나스/라미나스 라우터</a>
    </td>
    <td>라이브러리</td>
    <td>HTTP 및 콘솔 애플리케이션을 위한 유연한 라우팅 시스템</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-server.git">라미나스/라미나스 서버</a>
    </td>
    <td>라이브러리</td>
    <td>리플렉션 기반 RPC 서버 만들기</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-servicemanager.git">laminas/laminas-servicemanager</a>
    </td>
    <td>라이브러리</td>
    <td>공장 구동 의존 주입 용기</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-session.git">라미나스/라미나스-세션</a>
    </td>
    <td>라이브러리</td>
    <td>PHP 세션 및 스토리지에 대한 객체 지향 인터페이스</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-soap.git">라미나스/라미나스 비누</a>
    </td>
    <td>라이브러리</td>
    <td></td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-stdlib.git">laminas/laminas-stdlib</a>
    </td>
    <td>라이브러리</td>
    <td>SPL 확장, 배열 유틸리티, 오류 처리기 등</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-text.git">라미나스/라미나스 텍스트</a>
    </td>
    <td>라이브러리</td>
    <td>FIGlets 및 텍스트 기반 표 만들기</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-uri.git">laminas/laminas-uri</a>
    </td>
    <td>라이브러리</td>
    <td>"URI(Uniform Resource Identifier)"를 조작하고 확인하는 데 도움이 되는 구성 요소</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-validator.git">laminas/laminas-validator</a>
    </td>
    <td>라이브러리</td>
    <td>다양한 도메인에 대한 유효성 검사 클래스 및 복잡한 유효성 검사 기준을 만들기 위한 유효성 검사기 체인 기능</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-view.git">라미나스/라미나스 뷰</a>
    </td>
    <td>라이브러리</td>
    <td>여러 보기 레이어, 도우미 등을 지원하고 제공하는 유연한 보기 레이어</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/nikic/PHP-Parser.git">nikic/php-parser</a>
    </td>
    <td>라이브러리</td>
    <td>PHP로 작성된 PHP 파서</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/tedious/JShrink.git">tedivm/jshrink</a>
    </td>
    <td>라이브러리</td>
    <td>PHP에 내장된 Javascript 축소기</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/tubalmartin/YUI-CSS-compressor-PHP-port.git">tubalmartin/cssmin</a>
    </td>
    <td>라이브러리</td>
    <td>YUI CSS 압축기의 PHP 포트</td>
  </tr>
  </tbody>
</table>

### BSD-3-조항-수정

<table>
  <thead>
    <tr>
      <th>이름</th>
      <th>유형</th>
      <th>설명</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td>
      <a href="https://github.com/colinmollenhour/Cm_Cache_Backend_Redis.git">colinmollenhour/cache-backend-redis</a>
    </td>
    <td>magento-module</td>
    <td>태그에 대한 모든 지원을 제공하는 Redis를 사용하는 Zend_Cache 백엔드.</td>
  </tr>
  </tbody>
</table>

### ISC

<table>
  <thead>
    <tr>
      <th>이름</th>
      <th>유형</th>
      <th>설명</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td>
      <a href="https://github.com/paragonie/sodium_compat.git">파라고니/나트륨 콤파트</a>
    </td>
    <td>라이브러리</td>
    <td>libsodium의 순수 PHP 구현; 존재하는 경우 PHP 확장을 사용합니다.</td>
  </tr>
  </tbody>
</table>

### LGPL-2.1 이상

<table>
  <thead>
    <tr>
      <th>이름</th>
      <th>유형</th>
      <th>설명</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td>
      <a href="https://github.com/ezyang/htmlpurifier.git">에즈양/htmlclearer</a>
    </td>
    <td>라이브러리</td>
    <td>PHP로 작성된 표준 준수 HTML 필터</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-amqplib/php-amqplib.git">php-amqplib/php-amqplib</a>
    </td>
    <td>라이브러리</td>
    <td>이전 videalvaro/php-amqplib.  이 라이브러리는 AMQP 프로토콜의 순수한 PHP 구현입니다. RabbitMQ에 대해 테스트되었습니다.</td>
  </tr>
  </tbody>
</table>

### MIT

<table>
  <thead>
    <tr>
      <th>이름</th>
      <th>유형</th>
      <th>설명</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td>
      <a href="https://github.com/braintree/braintree_php.git">braintree/braintree_php</a>
    </td>
    <td>라이브러리</td>
    <td>Braintree PHP 클라이언트 라이브러리</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/brick/math.git">brick/math</a>
    </td>
    <td>라이브러리</td>
    <td>임의 정밀도 산술 라이브러리</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/brick/varexporter.git">brick/varexporter</a>
    </td>
    <td>라이브러리</td>
    <td>var_export()에 대한 강력한 대체 요소로, __set_state() 없이 닫기와 객체를 내보낼 수 있습니다.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/CarbonPHP/carbon-doctrine-types.git">카보닐/탄소-도트린-타입</a>
    </td>
    <td>라이브러리</td>
    <td>탄소의 원리 사용 유형</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/ChristianRiesen/base32.git">크리스티안 리센/base32</a>
    </td>
    <td>라이브러리</td>
    <td>RFC 4648에 따른 Base32 인코더/디코더</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/colinmollenhour/credis.git">colinmollenhour/credis</a>
    </td>
    <td>라이브러리</td>
    <td>Credis는 성능 향상을 위해 사용 가능한 경우 Phpredis 라이브러리를 래핑하는 Redis 키-값 저장소에 대한 간단한 인터페이스입니다.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/composer/ca-bundle.git">composer/ca-bundle</a>
    </td>
    <td>라이브러리</td>
    <td>시스템 CA 번들에 대한 경로를 찾을 수 있도록 하고, Mozilla CA 번들에 대한 대체를 포함합니다.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/composer/class-map-generator.git">composer/class-map-generator</a>
    </td>
    <td>라이브러리</td>
    <td>PHP 코드를 스캔하고 클래스 맵을 생성하는 유틸리티입니다.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/composer/composer.git">작곡가/작곡가</a>
    </td>
    <td>라이브러리</td>
    <td>Composer를 사용하면 PHP 프로젝트의 종속성을 선언하고, 관리하고, 설치할 수 있습니다. 어디에나 올바른 스택을 유지할 수 있습니다.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/composer/metadata-minifier.git">작성기/메타데이터 축소기</a>
    </td>
    <td>라이브러리</td>
    <td>메타데이터 축소 및 확장을 처리하는 작은 유틸리티 라이브러리입니다.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/composer/pcre.git">작성기/pcre</a>
    </td>
    <td>라이브러리</td>
    <td>PCRE 래핑 라이브러리로, 유형에 맞는 프레그_* 대체 기능을 제공합니다.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/composer/semver.git">작곡가/semver</a>
    </td>
    <td>라이브러리</td>
    <td>유틸리티, 버전 제약 조건 구문 분석 및 유효성 검사를 제공하는 Semver 라이브러리입니다.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/composer/spdx-licenses.git">composer/spdx-licenses</a>
    </td>
    <td>라이브러리</td>
    <td>SPDX 라이선스 목록 및 유효성 검사 라이브러리.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/composer/xdebug-handler.git">composer/xdebug-handler</a>
    </td>
    <td>라이브러리</td>
    <td>Xdebug 없이 프로세스를 다시 시작합니다.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/endroid/qr-code.git">엔드로이드/qr-코드</a>
    </td>
    <td>라이브러리</td>
    <td>엔드로이드 QR 코드</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/ezimuel/guzzlestreams.git">이지무엘/거즐스트림</a>
    </td>
    <td>라이브러리</td>
    <td>elasticsearch-php와 함께 사용할 구글/스트림 (중단됨)의 포크</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/ezimuel/ringphp.git">이지무엘/링php</a>
    </td>
    <td>라이브러리</td>
    <td>Elasticsearch-php와 함께 사용할 Guzzle/RingPHP (포크)</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/FriendsOfPHP/proxy-manager-lts.git">friendsofphp/proxy-manager-lts</a>
    </td>
    <td>라이브러리</td>
    <td>Ochromius/proxy-manager에 광범위한 PHP 버전에 대한 지원 추가</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/bzikarsky/gelf-php.git">graylog2/gelf-php</a>
    </td>
    <td>라이브러리</td>
    <td>Graylog2와 같은 GELF 호환 백엔드에 로그 메시지를 전송하는 php 구현입니다.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/guzzle/guzzle.git">guzzlehttp/guzzle</a>
    </td>
    <td>라이브러리</td>
    <td>Guzzle은 PHP HTTP 클라이언트 라이브러리입니다</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/guzzle/promises.git">guzzlehttp/promises</a>
    </td>
    <td>라이브러리</td>
    <td>Guzzle 약속 라이브러리</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/guzzle/psr7.git">guzzlehttp/psr7</a>
    </td>
    <td>라이브러리</td>
    <td>일반적인 유틸리티 메서드를 제공하는 PSR-7 메시지 구현</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/illuminate/collections.git">조명/컬렉션</a>
    </td>
    <td>라이브러리</td>
    <td>Lighting Collections 패키지</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/illuminate/config.git">설정/구성</a>
    </td>
    <td>라이브러리</td>
    <td>조명 구성 패키지.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/illuminate/contracts.git">정보 제공/계약</a>
    </td>
    <td>라이브러리</td>
    <td>조명 컨트랙트 패키지.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/illuminate/macroable.git">조명/거시 가능</a>
    </td>
    <td>라이브러리</td>
    <td>조명 마크로블 패키지.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/justinrainbow/json-schema.git">justinrainbow/json-schema</a>
    </td>
    <td>라이브러리</td>
    <td>JSON 스키마의 유효성을 검사하는 라이브러리입니다.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/thephpleague/flysystem.git">league/flysystem</a>
    </td>
    <td>라이브러리</td>
    <td>PHP용 파일 스토리지 추상화</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/thephpleague/flysystem-aws-s3-v3.git">league/flysystem-aws-s3-v3</a>
    </td>
    <td>라이브러리</td>
    <td>Flysystem용 AWS S3 파일 시스템 어댑터</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/thephpleague/mime-type-detection.git">league/mime-type-탐지</a>
    </td>
    <td>라이브러리</td>
    <td>Flysystem에 대한 MIME 유형 감지</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/Seldaek/monolog.git">독백/독백</a>
    </td>
    <td>라이브러리</td>
    <td>파일, 소켓, 받은 편지함, 데이터베이스 및 다양한 웹 서비스에 로그를 보냅니다.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/jmespath/jmespath.php.git">mtdowling/jmespath.php</a>
    </td>
    <td>라이브러리</td>
    <td>JSON 문서에서 요소를 추출하는 방법을 선언적으로 지정</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/briannesbitt/Carbon.git">nesbot/carbon</a>
    </td>
    <td>라이브러리</td>
    <td>281개의 다른 언어를 지원하는 DateTime용 API 확장입니다.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/paragonie/constant_time_encoding.git">단락/constant_time_encoding</a>
    </td>
    <td>라이브러리</td>
    <td>RFC 4648 인코딩의 일정 시간 구현(Base-64, Base-32, Base-16)</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/paragonie/random_compat.git">단락/random_compat</a>
    </td>
    <td>라이브러리</td>
    <td>PHP 7의 random_bytes() 및 random_int()에 대한 PHP 5.x polyfill</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/MyIntervals/emogrifier.git">펠라고/이모그리퍼</a>
    </td>
    <td>라이브러리</td>
    <td>CSS 스타일을 HTML 코드의 인라인 스타일 속성으로 변환합니다.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/PhpGt/CssXPath.git">phpgt/cssxpath</a>
    </td>
    <td>라이브러리</td>
    <td>CSS 선택기를 XPath 쿼리로 변환합니다.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/PhpGt/Dom.git">phpgt/dom</a>
    </td>
    <td>라이브러리</td>
    <td>최신 DOM API.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/PhpGt/PropFunc.git">phpgt/propfunc</a>
    </td>
    <td>라이브러리</td>
    <td>속성 접근자 및 변경자 함수.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/phpseclib/mcrypt_compat.git">phpseclib/mcrypt_compat</a>
    </td>
    <td>라이브러리</td>
    <td>PHP 5.x-8.x polyfill for mcrypt extension</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/phpseclib/phpseclib.git">phpseclib/phpseclib</a>
    </td>
    <td>라이브러리</td>
    <td>PHP 보안 통신 라이브러리 - RSA, AES, SSH2, SFTP, X.509 등의 순수 PHP 구현.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-fig/clock.git">psr/clock</a>
    </td>
    <td>라이브러리</td>
    <td>시계를 읽기 위한 공통 인터페이스.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-fig/container.git">psr/container</a>
    </td>
    <td>라이브러리</td>
    <td>공통 컨테이너 인터페이스 (PHP 그림 PSR-11)</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-fig/event-dispatcher.git">psr/event-dispatcher</a>
    </td>
    <td>라이브러리</td>
    <td>이벤트 처리를 위한 표준 인터페이스입니다.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-fig/http-client.git">psr/http-client</a>
    </td>
    <td>라이브러리</td>
    <td>HTTP 클라이언트용 공통 인터페이스</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-fig/http-factory.git">psr/http-factory</a>
    </td>
    <td>라이브러리</td>
    <td>PSR-7 HTTP 메시지 팩토리에 대한 일반 인터페이스</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-fig/http-message.git">psr/http-message</a>
    </td>
    <td>라이브러리</td>
    <td>HTTP 메시지에 대한 일반 인터페이스</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-fig/log.git">psr/log</a>
    </td>
    <td>라이브러리</td>
    <td>라이브러리 로깅을 위한 공통 인터페이스</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-fig/simple-cache.git">psr/simple-cache</a>
    </td>
    <td>라이브러리</td>
    <td>단순 캐싱을 위한 공통 인터페이스</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/ralouphie/getallheaders.git">레이아웃/getallheaders</a>
    </td>
    <td>라이브러리</td>
    <td>getallheaders에 대한 폴리필.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/ramsey/collection.git">램지/컬렉션</a>
    </td>
    <td>라이브러리</td>
    <td>컬렉션을 나타내고 조작하기 위한 PHP 라이브러리입니다.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/ramsey/uuid.git">램지/uuid</a>
    </td>
    <td>라이브러리</td>
    <td>UUID(범용 고유 식별자)를 생성하고 사용하기 위한 PHP 라이브러리입니다.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/reactphp/promise.git">반응/약속</a>
    </td>
    <td>라이브러리</td>
    <td>PHP용 CommonJS Promises/A의 간단한 구현</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/MyIntervals/PHP-CSS-Parser.git">sabberworm/php-css-parser</a>
    </td>
    <td>라이브러리</td>
    <td>PHP로 작성된 CSS 파일용 파서</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/Seldaek/jsonlint.git">seld/jsonlint</a>
    </td>
    <td>라이브러리</td>
    <td>JSON 린터</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/Seldaek/phar-utils.git">seld/phar-utils</a>
    </td>
    <td>라이브러리</td>
    <td>PHAR 파일 형식 유틸리티, PHP가 당신을 호출 할 때</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/Seldaek/signal-handler.git">seld/signal-handler</a>
    </td>
    <td>라이브러리</td>
    <td>쉬운 교차 플랫폼 개발을 위해 신호가 지원되지 않는 곳에서 자동으로 실패하는 간단한 unix 신호 처리기</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/Spomky-Labs/aes-key-wrap.git">spomky-labs/aes-key-wrap</a>
    </td>
    <td>라이브러리</td>
    <td>PHP용 AES Key Wrap.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/Spomky-Labs/otphp.git">spomky-labs/otphp</a>
    </td>
    <td>라이브러리</td>
    <td>RFC 4226(HOTP 알고리즘) 및 RFC 6238(TOTP 알고리즘)에 따라 1회 암호를 생성하고 Google Authenticator와 호환되는 PHP 라이브러리</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/Spomky-Labs/pki-framework.git">spomky-labs/pki-framework</a>
    </td>
    <td>라이브러리</td>
    <td>공개 키 인프라 관리를 위한 PHP 프레임워크입니다. X.509 공개 키 인증서, 속성 인증서, 인증 요청 및 인증 경로 유효성 검사로 구성됩니다.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/config.git">symfony/config</a>
    </td>
    <td>라이브러리</td>
    <td>모든 종류의 구성 값을 찾고, 로드하고, 결합하고, 자동으로 채우고, 유효성을 검사하는 데 도움이 됩니다.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/console.git">교감/콘솔</a>
    </td>
    <td>라이브러리</td>
    <td>아름답고 테스트 가능한 명령줄 인터페이스 생성 용이</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/css-selector.git">symfony/css-selector</a>
    </td>
    <td>라이브러리</td>
    <td>CSS 선택기를 XPath 표현식으로 변환</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/dependency-injection.git">교감/의존-주입</a>
    </td>
    <td>라이브러리</td>
    <td>애플리케이션에서 객체가 구성되는 방식을 표준화하고 중앙 집중화할 수 있습니다</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/deprecation-contracts.git">동조화/사용 중지 계약</a>
    </td>
    <td>라이브러리</td>
    <td>사용 중단 알림을 트리거하는 일반 함수 및 규칙</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/error-handler.git">symfony/error-handler</a>
    </td>
    <td>라이브러리</td>
    <td>오류를 관리하고 PHP 코드를 쉽게 디버깅할 수 있는 도구를 제공합니다.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/event-dispatcher.git">symfony/event-dispatcher</a>
    </td>
    <td>라이브러리</td>
    <td>이벤트를 발송하고 이벤트를 수신하여 애플리케이션 구성 요소 간에 통신할 수 있는 도구를 제공합니다.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/event-dispatcher-contracts.git">symfony/event-dispatcher-contracts</a>
    </td>
    <td>라이브러리</td>
    <td>이벤트 발송과 관련된 일반 요약</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/filesystem.git">symfony/filesystem</a>
    </td>
    <td>라이브러리</td>
    <td>파일 시스템의 기본 유틸리티 제공</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/finder.git">symfony/finder</a>
    </td>
    <td>라이브러리</td>
    <td>직관적인 유창한 인터페이스를 통해 파일 및 디렉터리 찾기</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/http-client.git">symfony/http 클라이언트</a>
    </td>
    <td>라이브러리</td>
    <td>HTTP 리소스를 동기식으로 또는 비동기식으로 가져오는 강력한 방법 제공</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/http-client-contracts.git">symfony/http-client-contract</a>
    </td>
    <td>라이브러리</td>
    <td>HTTP 클라이언트와 관련된 일반 추상화</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/http-foundation.git">symfony/http-foundation</a>
    </td>
    <td>라이브러리</td>
    <td>HTTP 사양에 대한 개체 지향 레이어를 정의합니다.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/http-kernel.git">symfony/http 커널</a>
    </td>
    <td>라이브러리</td>
    <td>요청을 응답으로 변환하는 구조화된 프로세스를 제공합니다.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/intl.git">교감/국제</a>
    </td>
    <td>라이브러리</td>
    <td>ICU 라이브러리의 로컬라이제이션 데이터에 대한 액세스를 제공합니다.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/polyfill-ctype.git">symfony/polyfill-type</a>
    </td>
    <td>라이브러리</td>
    <td>ctype 함수에 대한 교감 폴리필</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/polyfill-intl-grapheme.git">교감/폴리필-국제-자소</a>
    </td>
    <td>라이브러리</td>
    <td>국제 자소_* 함수에 대한 교감 폴리필</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/polyfill-intl-idn.git">symfony/polyfill-intl-idn</a>
    </td>
    <td>라이브러리</td>
    <td>국제 전화 번호 idn_to_ascii 및 idn_to_utf8 함수에 대한 동종 폴리필</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/polyfill-intl-normalizer.git">교감/폴리필-국제-정규화기</a>
    </td>
    <td>라이브러리</td>
    <td>국제 표준화 클래스 및 관련 함수에 대한 Symfony polyfill</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/polyfill-mbstring.git">symfony/polyfill-mbstring</a>
    </td>
    <td>라이브러리</td>
    <td>Mbstring 확장에 대한 Symfony polyfill</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/polyfill-php72.git">symfony/polyfill-php72</a>
    </td>
    <td>라이브러리</td>
    <td>일부 PHP 7.2+ 기능을 더 낮은 PHP 버전으로 지원하는 Symfony polyfill</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/polyfill-php73.git">symfony/polyfill-php73</a>
    </td>
    <td>라이브러리</td>
    <td>일부 PHP 7.3+ 기능을 더 낮은 PHP 버전으로 지원하는 Symfony polyfill</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/polyfill-php80.git">symfony/polyfill-php80</a>
    </td>
    <td>라이브러리</td>
    <td>일부 PHP 8.0+ 기능을 더 낮은 PHP 버전으로 지원하는 Symfony polyfill</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/polyfill-php81.git">symfony/polyfill-php81</a>
    </td>
    <td>라이브러리</td>
    <td>일부 PHP 8.1+ 기능을 더 낮은 PHP 버전으로 지원하는 Symfony polyfill</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/polyfill-php83.git">symfony/polyfill-php83</a>
    </td>
    <td>라이브러리</td>
    <td>일부 PHP 8.3+ 기능을 더 낮은 PHP 버전으로 지원하는 Symfony polyfill</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/process.git">교감/과정</a>
    </td>
    <td>라이브러리</td>
    <td>하위 프로세스에서 명령 실행</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/proxy-manager-bridge.git">symfony/proxy-manager-bridge</a>
    </td>
    <td>교향교</td>
    <td>ProxyManager와 다양한 Symfony 구성 요소 통합 제공</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/serializer.git">친숙한/직렬 변환기</a>
    </td>
    <td>라이브러리</td>
    <td>개체 그래프를 포함한 데이터 구조를 배열 구조나 XML 및 JSON과 같은 다른 형식으로 serialize 및 deserialize하는 작업을 처리합니다.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/service-contracts.git">교감/서비스 계약</a>
    </td>
    <td>라이브러리</td>
    <td>쓰기 서비스와 관련된 일반적인 추상화</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/string.git">symfony/string</a>
    </td>
    <td>라이브러리</td>
    <td>문자열에 개체 지향 API를 제공하고 바이트, UTF-8 코드 포인트 및 자소 클러스터를 통합 방식으로 처리합니다.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/translation.git">교감/번역</a>
    </td>
    <td>라이브러리</td>
    <td>애플리케이션을 국제화하기 위한 툴 제공</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/translation-contracts.git">동종/번역 계약</a>
    </td>
    <td>라이브러리</td>
    <td>번역과 관련된 일반 요약</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/var-dumper.git">symfony/var-dumper</a>
    </td>
    <td>라이브러리</td>
    <td>임의의 PHP 변수를 통과하기 위한 메커니즘을 제공합니다.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/var-exporter.git">symfony/var-exporter</a>
    </td>
    <td>라이브러리</td>
    <td>직렬화할 수 있는 PHP 데이터 구조를 일반 PHP 코드로 내보낼 수 있습니다.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/yaml.git">교감/yaml</a>
    </td>
    <td>라이브러리</td>
    <td>YAML 파일을 로드하고 덤프합니다</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/web-token/jwt-framework.git">web-token/jwt-framework</a>
    </td>
    <td>교향다발</td>
    <td>PHP 및 Symfony 번들을 위한 JSON 개체 서명 및 암호화 라이브러리입니다.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/webmozarts/assert.git">webmozart/assert</a>
    </td>
    <td>라이브러리</td>
    <td>좋은 오류 메시지와 함께 메서드 입/출력의 유효성을 검사하는 어설션입니다.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/webonyx/graphql-php.git">webonyx/graphql-php</a>
    </td>
    <td>라이브러리</td>
    <td>GraphQL 참조 구현의 PHP 포트</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/zordius/lightncandy.git">조르디우스/라이트캔디</a>
    </td>
    <td>라이브러리</td>
    <td>Handlebars ( http://handlebarsjs.com/ )와 mustache ( http://mustache.github.io/ )의 매우 빠른 PHP 구현입니다.</td>
  </tr>
  </tbody>
</table>

### OSL-3.0, AFL-3.0

<table>
  <thead>
    <tr>
      <th>이름</th>
      <th>유형</th>
      <th>설명</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td>
      paypal/module-braintree-customer-balance
    </td>
    <td>magento2-module</td>
    <td>해당 사항 없음</td>
  </tr>
  <tr>
    <td>
      paypal/module-braintree-gift-card-account
    </td>
    <td>magento2-module</td>
    <td>해당 사항 없음</td>
  </tr>
  <tr>
    <td>
      paypal/module-braintree-gift-wrapping
    </td>
    <td>magento2-module</td>
    <td>해당 사항 없음</td>
  </tr>
  <tr>
    <td>
      paypal/module-braintree-graph-ql
    </td>
    <td>magento2-module</td>
    <td>해당 사항 없음</td>
  </tr>
  </tbody>
</table>

### OSL-3.0

<table>
  <thead>
    <tr>
      <th>이름</th>
      <th>유형</th>
      <th>설명</th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>

### PHP

<table>
  <thead>
    <tr>
      <th>이름</th>
      <th>유형</th>
      <th>설명</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td>
      <a href="https://github.com/2tvenom/CBOREncode.git">2tvenom/cborencode</a>
    </td>
    <td>라이브러리</td>
    <td>PHP용 CBOR 인코더</td>
  </tr>
  </tbody>
</table>

### 소유

<table>
  <thead>
    <tr>
      <th>이름</th>
      <th>유형</th>
      <th>설명</th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>

### 소유

<table>
  <thead>
    <tr>
      <th>이름</th>
      <th>유형</th>
      <th>설명</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td>
      paypal/module-braintree-core
    </td>
    <td>magento2-module</td>
    <td>Gene Commerce for PayPal의 Magento Braintree 2.2.0 모듈에서 포크합니다.</td>
  </tr>
  </tbody>
</table>
