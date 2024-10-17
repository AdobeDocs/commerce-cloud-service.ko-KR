---
source-git-commit: 63c86bab0f3feb5a3a641a7a785ea625338045f4
workflow-type: tm+mt
source-wordcount: '13341'
ht-degree: 0%

---
# magento-cloud(클라우드 인프라의 Adobe Commerce)

<!-- The template to render with above values -->

**버전**: 1.46.1

이 참조에는 `magento-cloud` 명령줄 도구를 통해 사용할 수 있는 119개의 명령이 포함되어 있습니다.
클라우드 인프라의 Adobe Commerce에서 `magento-cloud list` 명령을 사용하여 초기 목록이 자동으로 생성됩니다.

## 일반

이 참조는 응용 프로그램 코드베이스에서 생성됩니다. 내용을 변경하려면 [codebase](https://github.com/magento/magento-cloud-cli) 리포지토리에서 해당 명령 구현의 소스 코드를 업데이트한 다음 변경 내용을 제출하여 검토할 수 있습니다. 다른 방법은 _피드백을 제공_&#x200B;하는 것입니다(오른쪽 상단에서 링크 찾기). 기여도 지침이 필요하면 [코드 기여도](https://developer.adobe.com/commerce/contributor/guides/code-contributions/)를 참조하십시오.

### 글로벌 옵션

#### `--help`, `-h`

이 도움말 메시지 표시

- 기본값: `false`
- 값을 수락하지 않음

#### `--verbose`, `-v|-vv|-vvv`

메시지의 다양성 증가

- 기본값: `false`
- 값을 수락하지 않음

#### `--version`, `-V`

이 응용 프로그램 버전 표시

- 기본값: `false`
- 값을 수락하지 않음

#### `--yes`, `-y`

확인 질문에 &quot;예&quot;라고 답하고, 다른 질문에 대한 기본값을 적용하고, 상호 작용을 비활성화합니다.

- 기본값: `false`
- 값을 수락하지 않음

#### `--no-interaction`

대화식 질문을 하지 말고 기본값을 그대로 사용하십시오. 환경 변수를 사용하는 것과 같습니다. MAGENTO_CLOUD_CLI_NO_INTERACTION=1

- 기본값: `false`
- 값을 수락하지 않음


## `clear-cache`

```bash
magento-cloud magento-cloud cc
```

CLI 캐시 지우기

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.


## `decode`

```bash
magento-cloud magento-cloud decode [-P|--property PROPERTY] [--] <value>
```

Magento_CLOUD_VARIABLES와 같은 인코딩된 문자열 디코딩

### 인수

#### `value`

디코딩할 변수 값

- 필수

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--property`, `-P`

변수 내에서 볼 속성

- 값 필요


## `docs`

```bash
magento-cloud magento-cloud docs [--browser BROWSER] [--pipe] [--] [<search>]...
```

온라인 설명서 열기

### 인수

#### `search`

검색어

- 기본값: `[]`
- 배열

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--browser`

URL을 여는 데 사용할 브라우저입니다. 없음에 대해 0을 설정합니다.

- 값 필요

#### `--pipe`

URL을 stdout으로 출력합니다.

- 기본값: `false`
- 값을 수락하지 않음


## `help`

```bash
magento-cloud magento-cloud help [--format FORMAT] [--raw] [--] [<command_name>]
```

명령에 대한 도움말을 표시합니다.

```
The help command displays help for a given command:

  magento-cloud help list

You can also output the help in other formats by using the --format option:

  magento-cloud help --format=json list

To display the list of available commands, please use the list command.
```

### 인수

#### `command_name`

명령 이름

- 기본값: `help`

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--format`

출력 형식(txt, json 또는 md)

- 기본값: `txt`
- 값 필요

#### `--raw`

원시 명령 도움말을 출력하려면

- 기본값: `false`
- 값을 수락하지 않음


## `list`

```bash
magento-cloud magento-cloud list [--raw] [--format FORMAT] [--all] [--] [<namespace>]
```

명령 나열

```
The list command lists all commands:

  magento-cloud list

You can also display the commands for a specific namespace:

  magento-cloud list project

You can also output the information in other formats by using the --format option:

  magento-cloud list --format=xml

It's also possible to get raw list of commands (useful for embedding command runner):

  magento-cloud list --raw
```

### 인수

#### `command`

실행할 명령

- 필수


#### `namespace`

네임스페이스 이름

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--raw`

원시 명령 목록을 출력하려면

- 기본값: `false`
- 값을 수락하지 않음

#### `--format`

출력 형식(txt, xml, json 또는 md)

- 기본값: `txt`
- 값 필요

#### `--all`

숨겨진 명령을 포함하여 모든 명령 표시

- 기본값: `false`
- 값을 수락하지 않음


## `multi`

```bash
magento-cloud magento-cloud multi [-p|--projects PROJECTS] [--continue] [--sort SORT] [--reverse] [--] <cmd> (<cmd>)...
```

여러 프로젝트에서 명령 실행

### 인수

#### `cmd`

실행할 명령

- 기본값: `[]`
- 필수

- 배열

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--projects`, `-p`

쉼표 및/또는 공백으로 구분된 프로젝트 ID 목록

- 값 필요

#### `--continue`

예외가 발생한 경우에도 명령 실행을 계속합니다.

- 기본값: `false`
- 값을 수락하지 않음

#### `--sort`

프로젝트 옵션 목록을 정렬할 속성입니다.

- 기본값: `title`
- 값 필요

#### `--reverse`

프로젝트 옵션 순서 바꾸기

- 기본값: `false`
- 값을 수락하지 않음


## `web`

```bash
magento-cloud magento-cloud web [--browser BROWSER] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

웹 UI에서 프로젝트 열기

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--browser`

URL을 여는 데 사용할 브라우저입니다. 없음에 대해 0을 설정합니다.

- 값 필요

#### `--pipe`

URL을 stdout으로 출력합니다.

- 기본값: `false`
- 값을 수락하지 않음

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요


## `activity:cancel`

```bash
magento-cloud magento-cloud activity:cancel [-t|--type TYPE] [-x|--exclude-type EXCLUDE-TYPE] [-a|--all] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<id>]
```

활동 취소

### 인수

#### `id`

활동 ID. 기본값은 가장 최근에 취소할 수 있는 활동으로 설정됩니다.

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--type`, `-t`

유형별로 필터링(기본 활동을 선택할 때). 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다. % 또는 * 문자는 유형에 대한 와일드카드로 사용할 수 있습니다(예: &#39;%var%&#39;). 변수 관련 활동을 선택할 수 있습니다.

- 기본값: `[]`
- 값 필요

#### `--exclude-type`, `-x`

유형별로 제외 (기본 활동을 선택할 때). 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다. % 또는 * 문자를 와일드카드로 사용하여 유형을 제외할 수 있습니다.

- 기본값: `[]`
- 값 필요

#### `--all`, `-a`

모든 환경에서 최근 활동 확인(기본 활동 선택 시)

- 기본값: `false`
- 값을 수락하지 않음

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요


## `activity:get`

```bash
magento-cloud magento-cloud activity:get [-P|--property PROPERTY] [-t|--type TYPE] [-x|--exclude-type EXCLUDE-TYPE] [--state STATE] [--result RESULT] [-i|--incomplete] [-a|--all] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [--] [<id>]
```

단일 활동에 대한 세부 정보 보기

### 인수

#### `id`

활동 ID. 기본값은 가장 최근 활동으로 설정됩니다.

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--property`, `-P`

볼 속성

- 값 필요

#### `--type`, `-t`

유형별로 필터링(기본 활동을 선택할 때). 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다. % 또는 * 문자는 유형에 대한 와일드카드로 사용할 수 있습니다(예: &#39;%var%&#39;). 변수 관련 활동을 선택할 수 있습니다.

- 기본값: `[]`
- 값 필요

#### `--exclude-type`, `-x`

유형별로 제외 (기본 활동을 선택할 때). 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다. % 또는 * 문자를 와일드카드로 사용하여 유형을 제외할 수 있습니다.

- 기본값: `[]`
- 값 필요

#### `--state`

상태 기준 필터링(기본 활동 선택 시): in_progress, pending, complete 또는 cancelled. 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다.

- 기본값: `[]`
- 값 필요

#### `--result`

결과 기준 필터링(기본 활동 선택 시): 성공 또는 실패

- 값 필요

#### `--incomplete`, `-i`

완료되지 않은 활동만 포함합니다(기본 활동을 선택할 때). —state=in_progress,pending의 줄임표입니다.

- 기본값: `false`
- 값을 수락하지 않음

#### `--all`, `-a`

모든 환경에서 최근 활동 확인(기본 활동 선택 시)

- 기본값: `false`
- 값을 수락하지 않음

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--format`

출력 형식: table, csv, tsv 또는 plain

- 기본값: `table`
- 값 필요

#### `--columns`, `-c`

표시할 열입니다. % 또는 * 문자는 와일드카드로 사용될 수 있습니다. 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다.

- 기본값: `[]`
- 값 필요

#### `--no-header`

테이블 머리글 출력 안 함

- 기본값: `false`
- 값을 수락하지 않음

#### `--date-fmt`

날짜 형식(PHP 날짜 형식 문자열로서)

- 기본값: `c`
- 값 필요


## `activity:list`

```bash
magento-cloud magento-cloud activities [-t|--type TYPE] [-x|--exclude-type EXCLUDE-TYPE] [--limit LIMIT] [--start START] [--state STATE] [--result RESULT] [-i|--incomplete] [-a|--all] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

환경 또는 프로젝트에 대한 활동 목록 가져오기

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--type`, `-t`

유형별 활동 필터링 값은 쉼표(예: &quot;a,b,c&quot;) 및/또는 공백으로 나눌 수 있습니다. 활동 이름의 첫 번째 부분을 생략할 수 있습니다. 예를 들어 &#39;cron&#39;은 &#39;environment.cron&#39; 활동을 선택할 수 있습니다. % 또는 * 문자를 와일드카드로 사용할 수 있습니다(예: &#39;%var%&#39;). 변수 관련 활동을 선택할 수 있습니다.

- 기본값: `[]`
- 값 필요

#### `--exclude-type`, `-x`

유형별로 활동을 제외합니다. 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다. 활동 이름의 첫 번째 부분을 생략할 수 있습니다. 예를 들어 &#39;cron&#39;은 &#39;environment.cron&#39; 활동을 제외할 수 있습니다. % 또는 * 문자를 와일드카드로 사용하여 유형을 제외할 수 있습니다.

- 기본값: `[]`
- 값 필요

#### `--limit`

표시되는 결과 수 제한

- 기본값: `10`
- 값 필요

#### `--start`

이 날짜 이전에 만든 활동만 나열됩니다.

- 값 필요

#### `--state`

진행 중, 보류 중, 완료 또는 취소된 상태별로 활동을 필터링합니다. 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다.

- 기본값: `[]`
- 값 필요

#### `--result`

성공 또는 실패의 결과로 활동 필터링

- 값 필요

#### `--incomplete`, `-i`

완료되지 않은 활동만 나열

- 기본값: `false`
- 값을 수락하지 않음

#### `--all`, `-a`

모든 환경의 활동 나열

- 기본값: `false`
- 값을 수락하지 않음

#### `--format`

출력 형식: table, csv, tsv 또는 plain

- 기본값: `table`
- 값 필요

#### `--columns`, `-c`

표시할 열입니다. 사용 가능한 열: id*, created*, description*, progress*, state*, result*, completed, environments, type (* = 기본 열). &quot;+&quot; 문자는 기본 열의 자리 표시자로 사용할 수 있습니다. % 또는 * 문자는 와일드카드로 사용될 수 있습니다. 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다.

- 기본값: `[]`
- 값 필요

#### `--no-header`

테이블 머리글 출력 안 함

- 기본값: `false`
- 값을 수락하지 않음

#### `--date-fmt`

날짜 형식(PHP 날짜 형식 문자열로서)

- 기본값: `c`
- 값 필요

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요


## `activity:log`

```bash
magento-cloud magento-cloud activity:log [--refresh REFRESH] [-t|--timestamps] [--type TYPE] [-x|--exclude-type EXCLUDE-TYPE] [--state STATE] [--result RESULT] [-i|--incomplete] [-a|--all] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<id>]
```

활동에 대한 로그 표시

### 인수

#### `id`

활동 ID. 기본값은 가장 최근 활동으로 설정됩니다.

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--refresh`

활동 새로 고침 간격(초). 새로 고침을 비활성화하려면 0으로 설정합니다.

- 기본값: `3`
- 값 필요

#### `--timestamps`, `-t`

각 메시지 옆에 타임스탬프 표시

- 기본값: `false`
- 값을 수락하지 않음

#### `--type`

유형별로 필터링(기본 활동을 선택할 때). 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다. % 또는 * 문자는 유형에 대한 와일드카드로 사용할 수 있습니다(예: &#39;%var%&#39;). 변수 관련 활동을 선택할 수 있습니다.

- 기본값: `[]`
- 값 필요

#### `--exclude-type`, `-x`

유형별로 제외 (기본 활동을 선택할 때). 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다. % 또는 * 문자를 와일드카드로 사용하여 유형을 제외할 수 있습니다.

- 기본값: `[]`
- 값 필요

#### `--state`

상태 기준 필터링(기본 활동 선택 시): in_progress, pending, complete 또는 cancelled. 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다.

- 기본값: `[]`
- 값 필요

#### `--result`

결과 기준 필터링(기본 활동 선택 시): 성공 또는 실패

- 값 필요

#### `--incomplete`, `-i`

완료되지 않은 활동만 포함합니다(기본 활동을 선택할 때). —state=in_progress,pending의 줄임표입니다.

- 기본값: `false`
- 값을 수락하지 않음

#### `--all`, `-a`

모든 환경에서 최근 활동 확인(기본 활동 선택 시)

- 기본값: `false`
- 값을 수락하지 않음

#### `--date-fmt`

날짜 형식(PHP 날짜 형식 문자열로서)

- 기본값: `c`
- 값 필요

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요


## `app:config-get`

```bash
magento-cloud magento-cloud app:config-get [-P|--property PROPERTY] [--refresh] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-i|--identity-file IDENTITY-FILE]
```

앱 구성 보기

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--property`, `-P`

보려는 구성 속성

- 값 필요

#### `--refresh`

캐시 새로 고침 여부

- 기본값: `false`
- 값을 수락하지 않음

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--app`, `-A`

원격 애플리케이션 이름

- 값 필요

#### `--identity-file`, `-i`

[사용되지 않는 옵션, 더 이상 사용되지 않음]

- 값 필요


## `app:list`

```bash
magento-cloud magento-cloud apps [--refresh] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

프로젝트의 앱 나열

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--refresh`

캐시 새로 고침 여부

- 기본값: `false`
- 값을 수락하지 않음

#### `--pipe`

앱 이름 목록만 출력

- 기본값: `false`
- 값을 수락하지 않음

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--format`

출력 형식: table, csv, tsv 또는 plain

- 기본값: `table`
- 값 필요

#### `--columns`, `-c`

표시할 열입니다. 사용 가능한 열: 이름*, 유형*, 디스크, 경로, 크기(* = 기본 열). &quot;+&quot; 문자는 기본 열의 자리 표시자로 사용할 수 있습니다. % 또는 * 문자는 와일드카드로 사용될 수 있습니다. 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다.

- 기본값: `[]`
- 값 필요

#### `--no-header`

테이블 머리글 출력 안 함

- 기본값: `false`
- 값을 수락하지 않음


## `auth:api-token-login`

```bash
magento-cloud magento-cloud auth:api-token-login
```

API 토큰을 사용하여 Magento 클라우드에 로그인

```
Use this command to log in to your Magento Cloud account using an API token.

You can create an account at:
    https://business.adobe.com/products/magento/magento-commerce.html

If you have an account, but you do not already have an API token, you can create one here:
    https://accounts.magento.cloud/user/api-tokens

Alternatively, to log in to the CLI with a browser, run:
    magento-cloud auth:browser-login
```

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.


## `auth:browser-login`

```bash
magento-cloud magento-cloud login [-f|--force] [--browser BROWSER] [--pipe]
```

브라우저를 통해 Magento 클라우드에 로그인합니다

```
Use this command to log in to the Magento Cloud CLI using a web browser.

It launches a temporary local website which redirects you to log in if
necessary, and then captures the resulting authorization code.

Your system's default browser will be used. You can override this using the
--browser option.

Alternatively, to log in using an API token (without a browser), run:
magento-cloud auth:api-token-login

To authenticate non-interactively, configure an API token using the
MAGENTO_CLOUD_CLI_TOKEN environment variable.
```

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--force`, `-f`

이미 로그인했더라도 다시 로그인

- 기본값: `false`
- 값을 수락하지 않음

#### `--browser`

URL을 여는 데 사용할 브라우저입니다. 없음에 대해 0을 설정합니다.

- 값 필요

#### `--pipe`

URL을 stdout으로 출력합니다.

- 기본값: `false`
- 값을 수락하지 않음


## `auth:info`

```bash
magento-cloud magento-cloud auth:info [--no-auto-login] [-P|--property PROPERTY] [--refresh] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--] [<property>]
```

계정 정보 표시

### 인수

#### `property`

조회할 계정 속성

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--no-auto-login`

자동 로그인을 건너뜁니다. 로그인하지 않으면 아무 것도 출력되지 않으며 다른 오류가 없다고 가정할 때 종료 코드는 0입니다.

- 기본값: `false`
- 값을 수락하지 않음

#### `--property`, `-P`

볼 계정 속성(대체 구문)

- 값 필요

#### `--refresh`

캐시 새로 고침 여부

- 기본값: `false`
- 값을 수락하지 않음

#### `--format`

출력 형식: table, csv, tsv 또는 plain

- 기본값: `table`
- 값 필요

#### `--columns`, `-c`

표시할 열입니다. % 또는 * 문자는 와일드카드로 사용될 수 있습니다. 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다.

- 기본값: `[]`
- 값 필요

#### `--no-header`

테이블 머리글 출력 안 함

- 기본값: `false`
- 값을 수락하지 않음


## `auth:logout`

```bash
magento-cloud magento-cloud logout [-a|--all] [--other]
```

Magento 클라우드에서 로그아웃

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--all`, `-a`

모든 로컬 세션에서 로그아웃

- 기본값: `false`
- 값을 수락하지 않음

#### `--other`

다른 로컬 세션에서 로그아웃

- 기본값: `false`
- 값을 수락하지 않음


## `blackfire:setup`

```bash
magento-cloud magento-cloud blackfire:setup [--server_id SERVER_ID] [--server_token SERVER_TOKEN] [-p|--project PROJECT] [-W|--no-wait] [--wait]
```

프로젝트에 대한 Blackfire.io 통합 설정

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--server_id`

서버 ID

- 값 필요

#### `--server_token`

서버 토큰

- 값 필요

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--no-wait`, `-W`

작업이 완료될 때까지 기다리지 마십시오.

- 기본값: `false`
- 값을 수락하지 않음

#### `--wait`

작업이 완료될 때까지 대기(기본값)

- 기본값: `false`
- 값을 수락하지 않음


## `certificate:add`

```bash
magento-cloud magento-cloud certificate:add [--cert CERT] [--key KEY] [--chain CHAIN] [-p|--project PROJECT] [-W|--no-wait] [--wait]
```

프로젝트에 SSL 인증서 추가

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--cert`

인증서 파일의 경로

- 값 필요

#### `--key`

인증서 개인 키 파일의 경로

- 값 필요

#### `--chain`

인증서 체인 파일의 경로

- 기본값: `[]`
- 값 필요

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--no-wait`, `-W`

작업이 완료될 때까지 기다리지 마십시오.

- 기본값: `false`
- 값을 수락하지 않음

#### `--wait`

작업이 완료될 때까지 대기(기본값)

- 기본값: `false`
- 값을 수락하지 않음


## `certificate:delete`

```bash
magento-cloud magento-cloud certificate:delete [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] <id>
```

프로젝트에서 인증서 삭제

### 인수

#### `id`

인증서 ID(또는 인증서 ID의 시작)

- 필수

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--no-wait`, `-W`

작업이 완료될 때까지 기다리지 마십시오.

- 기본값: `false`
- 값을 수락하지 않음

#### `--wait`

작업이 완료될 때까지 대기(기본값)

- 기본값: `false`
- 값을 수락하지 않음


## `certificate:get`

```bash
magento-cloud magento-cloud certificate:get [-P|--property PROPERTY] [--date-fmt DATE-FMT] [-p|--project PROJECT] [--] <id>
```

인증서 보기

### 인수

#### `id`

인증서 ID(또는 인증서 ID의 시작)

- 필수

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--property`, `-P`

조회할 인증서 속성

- 값 필요

#### `--date-fmt`

날짜 형식(PHP 날짜 형식 문자열로서)

- 기본값: `c`
- 값 필요

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요


## `certificate:list`

```bash
magento-cloud magento-cloud certificates [--domain DOMAIN] [--exclude-domain EXCLUDE-DOMAIN] [--issuer ISSUER] [--only-auto] [--no-auto] [--ignore-expiry] [--only-expired] [--no-expired] [--pipe-domains] [--date-fmt DATE-FMT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT]
```

프로젝트 인증서 나열

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--domain`

도메인 이름으로 필터링(대/소문자 구분 안 함 검색)

- 값 필요

#### `--exclude-domain`

도메인 이름별로 일치하는 인증서 제외(대/소문자 구분 안 함 검색)

- 값 필요

#### `--issuer`

발급자별 필터링

- 값 필요

#### `--only-auto`

자동 프로비저닝된 인증서만 표시

- 기본값: `false`
- 값을 수락하지 않음

#### `--no-auto`

수동으로 추가된 인증서만 표시

- 기본값: `false`
- 값을 수락하지 않음

#### `--ignore-expiry`

만료된 인증서와 만료되지 않은 인증서 모두 표시

- 기본값: `false`
- 값을 수락하지 않음

#### `--only-expired`

만료된 인증서만 표시

- 기본값: `false`
- 값을 수락하지 않음

#### `--no-expired`

만료되지 않은 인증서만 표시(기본값)

- 기본값: `false`
- 값을 수락하지 않음

#### `--pipe-domains`

인증서에 포함된 도메인 이름 목록만 반환

- 기본값: `false`
- 값을 수락하지 않음

#### `--date-fmt`

날짜 형식(PHP 날짜 형식 문자열로서)

- 기본값: `c`
- 값 필요

#### `--format`

출력 형식: table, csv, tsv 또는 plain

- 기본값: `table`
- 값 필요

#### `--columns`, `-c`

표시할 열입니다. 사용 가능한 열: 생성됨, 도메인, 만료, ID, 발급자. % 또는 * 문자는 와일드카드로 사용될 수 있습니다. 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다.

- 기본값: `[]`
- 값 필요

#### `--no-header`

테이블 머리글 출력 안 함

- 기본값: `false`
- 값을 수락하지 않음

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요


## `commit:get`

```bash
magento-cloud magento-cloud commit:get [-P|--property PROPERTY] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--date-fmt DATE-FMT] [--] [<commit>]
```

커밋 세부 정보 표시

### 인수

#### `commit`

커밋 SHA. 상위 커밋에 대해 &quot;HEAD&quot; 및 삽입(^) 또는 물결표(~) 접미사를 사용할 수도 있습니다.

- 기본값: `HEAD`

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--property`, `-P`

표시할 커밋 속성입니다.

- 값 필요

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--date-fmt`

날짜 형식(PHP 날짜 형식 문자열로서)

- 기본값: `c`
- 값 필요


## `commit:list`

```bash
magento-cloud magento-cloud commits [--limit LIMIT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [--] [<commit>]
```

목록 커밋

### 인수

#### `commit`

Git 커밋 SHA를 시작하는 중입니다. 상위 커밋에 대해 &quot;HEAD&quot; 및 삽입(^) 또는 물결표(~) 접미사를 사용할 수도 있습니다.

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--limit`

표시할 커밋 수입니다.

- 기본값: `10`
- 값 필요

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--format`

출력 형식: table, csv, tsv 또는 plain

- 기본값: `table`
- 값 필요

#### `--columns`, `-c`

표시할 열입니다. 사용 가능한 열: 작성자, 날짜, sha, 요약 % 또는 * 문자는 와일드카드로 사용될 수 있습니다. 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다.

- 기본값: `[]`
- 값 필요

#### `--no-header`

테이블 머리글 출력 안 함

- 기본값: `false`
- 값을 수락하지 않음

#### `--date-fmt`

날짜 형식(PHP 날짜 형식 문자열로서)

- 기본값: `c`
- 값 필요


## `db:dump`

```bash
magento-cloud magento-cloud db:dump [--schema SCHEMA] [-f|--file FILE] [-d|--directory DIRECTORY] [-z|--gzip] [-t|--timestamp] [-o|--stdout] [--table TABLE] [--exclude-table EXCLUDE-TABLE] [--schema-only] [--charset CHARSET] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE]
```

원격 데이터베이스의 로컬 덤프 만들기

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--schema`

덤프할 스키마입니다. 기본 스키마(일반적으로 &quot;기본&quot;)를 사용하려면 을 생략합니다.

- 값 필요

#### `--file`, `-f`

덤프에 대한 사용자 정의 파일 이름

- 값 필요

#### `--directory`, `-d`

덤프에 대한 사용자 정의 디렉토리

- 값 필요

#### `--gzip`, `-z`

gzip을 사용하여 덤프 압축

- 기본값: `false`
- 값을 수락하지 않음

#### `--timestamp`, `-t`

덤프 파일 이름에 타임스탬프 추가

- 기본값: `false`
- 값을 수락하지 않음

#### `--stdout`, `-o`

파일 대신 STDOUT으로 출력

- 기본값: `false`
- 값을 수락하지 않음

#### `--table`

포함할 테이블

- 기본값: `[]`
- 값 필요

#### `--exclude-table`

제외할 테이블

- 기본값: `[]`
- 값 필요

#### `--schema-only`

스키마만 덤프, 데이터 없음

- 기본값: `false`
- 값을 수락하지 않음

#### `--charset`

덤프에 대한 문자 세트 인코딩

- 값 필요

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--app`, `-A`

원격 애플리케이션 이름

- 값 필요

#### `--relationship`, `-r`

사용할 서비스 관계

- 값 필요

#### `--identity-file`, `-i`

사용할 SSH ID(개인 키)

- 값 필요


## `db:size`

```bash
magento-cloud magento-cloud db:size [-B|--bytes] [-C|--cleanup] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-r|--relationship RELATIONSHIP] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-i|--identity-file IDENTITY-FILE]
```

데이터베이스의 디스크 사용량 예측

```
This is an estimate of the database disk usage. The real size on disk is usually higher because of overhead.

To see more accurate disk usage, run: magento-cloud disk
```

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--bytes`, `-B`

크기(바이트)를 표시합니다.

- 기본값: `false`
- 값을 수락하지 않음

#### `--cleanup`, `-C`

테이블을 정리할 수 있는지 확인하고 권장 사항을 표시합니다(InnoDb만 해당).

- 기본값: `false`
- 값을 수락하지 않음

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--app`, `-A`

원격 애플리케이션 이름

- 값 필요

#### `--relationship`, `-r`

사용할 서비스 관계

- 값 필요

#### `--format`

출력 형식: table, csv, tsv 또는 plain

- 기본값: `table`
- 값 필요

#### `--columns`, `-c`

표시할 열입니다. 사용 가능한 열: max, percent_used, used % 또는 * 문자는 와일드카드로 사용될 수 있습니다. 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다.

- 기본값: `[]`
- 값 필요

#### `--no-header`

테이블 머리글 출력 안 함

- 기본값: `false`
- 값을 수락하지 않음

#### `--identity-file`, `-i`

사용할 SSH ID(개인 키)

- 값 필요


## `db:sql`

```bash
magento-cloud magento-cloud sql [--raw] [--schema SCHEMA] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE] [--] [<query>]
```

원격 데이터베이스에서 SQL 실행

### 인수

#### `query`

실행할 SQL 문

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--raw`

원시 테이블 형식이 아닌 출력 생성

- 기본값: `false`
- 값을 수락하지 않음

#### `--schema`

사용할 스키마. 기본 스키마(일반적으로 &quot;기본&quot;)를 사용하려면 을 생략합니다. 스키마를 사용하지 않으려면 빈 문자열을 전달하십시오.

- 값 필요

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--app`, `-A`

원격 애플리케이션 이름

- 값 필요

#### `--relationship`, `-r`

사용할 서비스 관계

- 값 필요

#### `--identity-file`, `-i`

사용할 SSH ID(개인 키)

- 값 필요


## `domain:add`

```bash
magento-cloud magento-cloud domain:add [--cert CERT] [--key KEY] [--chain CHAIN] [--attach ATTACH] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <name>
```

프로젝트에 새 도메인 추가

### 인수

#### `name`

도메인 이름

- 필수

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--cert`

이 도메인의 인증서 파일 경로

- 값 필요

#### `--key`

제공된 인증서의 개인 키 파일 경로.

- 값 필요

#### `--chain`

제공된 인증서의 인증서 체인 파일 경로

- 기본값: `[]`
- 값 필요

#### `--attach`

환경 경로에서 이 도메인으로 바꾸는 프로덕션 도메인입니다. 비프로덕션 환경 도메인에 필요합니다.

- 값 필요

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--no-wait`, `-W`

작업이 완료될 때까지 기다리지 마십시오.

- 기본값: `false`
- 값을 수락하지 않음

#### `--wait`

작업이 완료될 때까지 대기(기본값)

- 기본값: `false`
- 값을 수락하지 않음


## `domain:delete`

```bash
magento-cloud magento-cloud domain:delete [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <name>
```

프로젝트에서 도메인 삭제

### 인수

#### `name`

도메인 이름

- 필수

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--no-wait`, `-W`

작업이 완료될 때까지 기다리지 마십시오.

- 기본값: `false`
- 값을 수락하지 않음

#### `--wait`

작업이 완료될 때까지 대기(기본값)

- 기본값: `false`
- 값을 수락하지 않음


## `domain:get`

```bash
magento-cloud magento-cloud domain:get [-P|--property PROPERTY] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<name>]
```

도메인에 대한 세부 정보 표시

### 인수

#### `name`

도메인 이름

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--property`, `-P`

조회할 도메인 속성

- 값 필요

#### `--format`

출력 형식: table, csv, tsv 또는 plain

- 기본값: `table`
- 값 필요

#### `--columns`, `-c`

표시할 열입니다. % 또는 * 문자는 와일드카드로 사용될 수 있습니다. 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다.

- 기본값: `[]`
- 값 필요

#### `--no-header`

테이블 머리글 출력 안 함

- 기본값: `false`
- 값을 수락하지 않음

#### `--date-fmt`

날짜 형식(PHP 날짜 형식 문자열로서)

- 기본값: `c`
- 값 필요

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요


## `domain:list`

```bash
magento-cloud magento-cloud domains [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

모든 도메인 목록 가져오기

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--format`

출력 형식: table, csv, tsv 또는 plain

- 기본값: `table`
- 값 필요

#### `--columns`, `-c`

표시할 열입니다. 사용 가능한 열: name*, ssl*, created_at*, registered_name, replacement_for, type, updated_at (* = 기본 열). &quot;+&quot; 문자는 기본 열의 자리 표시자로 사용할 수 있습니다. % 또는 * 문자는 와일드카드로 사용될 수 있습니다. 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다.

- 기본값: `[]`
- 값 필요

#### `--no-header`

테이블 머리글 출력 안 함

- 기본값: `false`
- 값을 수락하지 않음

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요


## `domain:update`

```bash
magento-cloud magento-cloud domain:update [--cert CERT] [--key KEY] [--chain CHAIN] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <name>
```

도메인 업데이트

### 인수

#### `name`

도메인 이름

- 필수

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--cert`

이 도메인의 인증서 파일 경로

- 값 필요

#### `--key`

제공된 인증서의 개인 키 파일 경로.

- 값 필요

#### `--chain`

제공된 인증서의 인증서 체인 파일 경로

- 기본값: `[]`
- 값 필요

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--no-wait`, `-W`

작업이 완료될 때까지 기다리지 마십시오.

- 기본값: `false`
- 값을 수락하지 않음

#### `--wait`

작업이 완료될 때까지 대기(기본값)

- 기본값: `false`
- 값을 수락하지 않음


## `environment:activate`

```bash
magento-cloud magento-cloud environment:activate [--parent PARENT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<environment>]...
```

환경 활성화

### 인수

#### `environment`

활성화할 환경

- 기본값: `[]`
- 배열

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--parent`

활성화하기 전에 새 환경 상위 설정

- 값 필요

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--no-wait`, `-W`

작업이 완료될 때까지 기다리지 마십시오.

- 기본값: `false`
- 값을 수락하지 않음

#### `--wait`

작업이 완료될 때까지 대기(기본값)

- 기본값: `false`
- 값을 수락하지 않음


## `environment:branch`

```bash
magento-cloud magento-cloud branch [--title TITLE] [--type TYPE] [--no-clone-parent] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<id>] [<parent>]
```

환경 분기

### 인수

#### `id`

새 환경의 ID(분기 이름)


#### `parent`

새 환경의 상위

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--title`

새 환경의 제목

- 값 필요

#### `--type`

새 환경의 유형

- 값 필요

#### `--no-clone-parent`

상위 환경의 데이터를 복제하지 않음

- 기본값: `false`
- 값을 수락하지 않음

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--no-wait`, `-W`

작업이 완료될 때까지 기다리지 마십시오.

- 기본값: `false`
- 값을 수락하지 않음

#### `--wait`

작업이 완료될 때까지 대기(기본값)

- 기본값: `false`
- 값을 수락하지 않음


## `environment:checkout`

```bash
magento-cloud magento-cloud checkout [-i|--identity-file IDENTITY-FILE] [--] [<id>]
```

환경 확인

### 인수

#### `id`

체크 아웃할 환경의 ID입니다. 예: &quot;sprint2&quot;

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--identity-file`, `-i`

사용할 SSH ID(개인 키)

- 값 필요


## `environment:delete`

```bash
magento-cloud magento-cloud environment:delete [--delete-branch] [--no-delete-branch] [--type TYPE] [-t|--only-type ONLY-TYPE] [--exclude EXCLUDE] [--exclude-type EXCLUDE-TYPE] [--inactive] [--merged] [--allow-delete-parent] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<environment>]...
```

하나 이상의 환경 삭제

```
When a Magento Cloud environment is deleted, it will become "inactive": it will
exist only as a Git branch, containing code but no services, databases nor
files.

This command allows you to delete environments as well as their Git branches.
```

### 인수

#### `environment`

삭제할 환경입니다. % 또는 * 문자는 와일드카드로 사용될 수 있습니다. 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다.

- 기본값: `[]`
- 배열

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--delete-branch`

확인하지 않고 비활성 환경에 대한 Git 분기 삭제

- 기본값: `false`
- 값을 수락하지 않음

#### `--no-delete-branch`

Git 분기(비활성 환경)를 삭제하지 마십시오

- 기본값: `false`
- 값을 수락하지 않음

#### `--type`

유형의 모든 환경 삭제(선택한 다른 환경에 추가) 값은 쉼표(예: &quot;a,b,c&quot;) 및/또는 공백으로 분할할 수 있습니다.

- 기본값: `[]`
- 값 필요

#### `--only-type`, `-t`

특정 유형의 삭제 환경만 쉼표(예: &quot;a,b,c&quot;) 및/또는 공백으로 분할할 수 있습니다.

- 기본값: `[]`
- 값 필요

#### `--exclude`

환경을 삭제하지 마십시오. % 또는 * 문자는 와일드카드로 사용될 수 있습니다. 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다.

- 기본값: `[]`
- 값 필요

#### `--exclude-type`

값을 삭제하지 않는 환경 유형은 쉼표(예: &quot;a,b,c&quot;) 및/또는 공백으로 분할할 수 있습니다.

- 기본값: `[]`
- 값 필요

#### `--inactive`

모든 비활성 환경 삭제(선택한 다른 환경에 추가)

- 기본값: `false`
- 값을 수락하지 않음

#### `--merged`

모든 병합된 환경 삭제(선택한 다른 환경에 추가)

- 기본값: `false`
- 값을 수락하지 않음

#### `--allow-delete-parent`

하위 항목이 있는 환경 삭제 허용

- 기본값: `false`
- 값을 수락하지 않음

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--no-wait`, `-W`

작업이 완료될 때까지 기다리지 마십시오.

- 기본값: `false`
- 값을 수락하지 않음

#### `--wait`

작업이 완료될 때까지 대기(기본값)

- 기본값: `false`
- 값을 수락하지 않음


## `environment:http-access`

```bash
magento-cloud magento-cloud httpaccess [--access ACCESS] [--auth AUTH] [--enabled ENABLED] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait]
```

환경에 대한 HTTP 액세스 설정 업데이트

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--access`

&quot;permission:address&quot; 형식의 액세스 제한. 모든 주소를 지우려면 0을 사용합니다.

- 기본값: `[]`
- 값 필요

#### `--auth`

&quot;사용자 이름:암호&quot; 형식의 HTTP 기본 인증 자격 증명입니다. 모든 자격 증명을 지우려면 0을 사용합니다.

- 기본값: `[]`
- 값 필요

#### `--enabled`

액세스 제어를 활성화할지 여부: 활성화하려면 1, 비활성화하려면 0

- 값 필요

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--no-wait`, `-W`

작업이 완료될 때까지 기다리지 마십시오.

- 기본값: `false`
- 값을 수락하지 않음

#### `--wait`

작업이 완료될 때까지 대기(기본값)

- 기본값: `false`
- 값을 수락하지 않음


## `environment:info`

```bash
magento-cloud magento-cloud environment:info [--refresh] [--date-fmt DATE-FMT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<property>] [<value>]
```

환경에 대한 속성 읽기 또는 설정

### 인수

#### `property`

속성 이름


#### `value`

속성에 대한 새 값 설정

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--refresh`

캐시 새로 고침 여부

- 기본값: `false`
- 값을 수락하지 않음

#### `--date-fmt`

날짜 형식(PHP 날짜 형식 문자열로서)

- 기본값: `c`
- 값 필요

#### `--format`

출력 형식: table, csv, tsv 또는 plain

- 기본값: `table`
- 값 필요

#### `--columns`, `-c`

표시할 열입니다. % 또는 * 문자는 와일드카드로 사용될 수 있습니다. 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다.

- 기본값: `[]`
- 값 필요

#### `--no-header`

테이블 머리글 출력 안 함

- 기본값: `false`
- 값을 수락하지 않음

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--no-wait`, `-W`

작업이 완료될 때까지 기다리지 마십시오.

- 기본값: `false`
- 값을 수락하지 않음

#### `--wait`

작업이 완료될 때까지 대기(기본값)

- 기본값: `false`
- 값을 수락하지 않음


## `environment:init`

```bash
magento-cloud magento-cloud environment:init [--profile PROFILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <url>
```

공개 Git 저장소에서 환경 초기화

### 인수

#### `url`

Git 저장소의 URL

- 필수

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--profile`

프로필 이름

- 값 필요

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--no-wait`, `-W`

작업이 완료될 때까지 기다리지 마십시오.

- 기본값: `false`
- 값을 수락하지 않음

#### `--wait`

작업이 완료될 때까지 대기(기본값)

- 기본값: `false`
- 값을 수락하지 않음


## `environment:list`

```bash
magento-cloud magento-cloud environments [-I|--no-inactive] [--pipe] [--refresh REFRESH] [--sort SORT] [--reverse] [--type TYPE] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT]
```

환경 목록 가져오기

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--no-inactive`, `-I`

비활성 환경 표시 안 함

- 기본값: `false`
- 값을 수락하지 않음

#### `--pipe`

간단한 환경 ID 목록을 출력합니다.

- 기본값: `false`
- 값을 수락하지 않음

#### `--refresh`

목록을 새로 고칠지 여부.

- 기본값: `1`
- 값 필요

#### `--sort`

정렬 기준 속성

- 기본값: `title`
- 값 필요

#### `--reverse`

역순(내림차순) 정렬

- 기본값: `false`
- 값을 수락하지 않음

#### `--type`

환경 유형별로 목록을 필터링합니다. 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다.

- 기본값: `[]`
- 값 필요

#### `--format`

출력 형식: table, csv, tsv 또는 plain

- 기본값: `table`
- 값 필요

#### `--columns`, `-c`

표시할 열입니다. 사용 가능한 열: id*, title*, status*, type*, created, machine_name, updated (* = 기본 열). &quot;+&quot; 문자는 기본 열의 자리 표시자로 사용할 수 있습니다. % 또는 * 문자는 와일드카드로 사용될 수 있습니다. 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다.

- 기본값: `[]`
- 값 필요

#### `--no-header`

테이블 머리글 출력 안 함

- 기본값: `false`
- 값을 수락하지 않음

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요


## `environment:logs`

```bash
magento-cloud magento-cloud log [--lines LINES] [--tail] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [--] [<type>]
```

환경 로그 읽기

### 인수

#### `type`

로그 유형(예: &quot;access&quot; 또는 &quot;error&quot;)

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--lines`

표시할 라인 수

- 기본값: `100`
- 값 필요

#### `--tail`

로그를 지속적으로 추적합니다.

- 기본값: `false`
- 값을 수락하지 않음

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--app`, `-A`

원격 애플리케이션 이름

- 값 필요

#### `--worker`

작업자 이름

- 값 필요

#### `--instance`, `-I`

인스턴스 ID

- 값 필요


## `environment:merge`

```bash
magento-cloud magento-cloud merge [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<environment>]
```

환경 병합

```
This command will initiate a Git merge of the specified environment into its parent environment.
```

### 인수

#### `environment`

병합할 환경

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--no-wait`, `-W`

작업이 완료될 때까지 기다리지 마십시오.

- 기본값: `false`
- 값을 수락하지 않음

#### `--wait`

작업이 완료될 때까지 대기(기본값)

- 기본값: `false`
- 값을 수락하지 않음


## `environment:pause`

```bash
magento-cloud magento-cloud environment:pause [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait]
```

환경 일시 중지

```
Pausing an environment helps to reduce resource consumption and carbon emissions.

The environment will be unavailable until it is resumed. No data will be lost.
```

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--no-wait`, `-W`

작업이 완료될 때까지 기다리지 마십시오.

- 기본값: `false`
- 값을 수락하지 않음

#### `--wait`

작업이 완료될 때까지 대기(기본값)

- 기본값: `false`
- 값을 수락하지 않음


## `environment:push`

```bash
magento-cloud magento-cloud push [--target TARGET] [-f|--force] [--force-with-lease] [-u|--set-upstream] [--activate] [--parent PARENT] [--type TYPE] [--no-clone-parent] [-W|--no-wait] [--wait] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-i|--identity-file IDENTITY-FILE] [--] [<source>]
```

환경에 코드 푸시

### 인수

#### `source`

소스 참조: 분기 이름 또는 커밋 해시

- 기본값: `HEAD`

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--target`

대상 분기 이름입니다. 기본값은 현재 분기로 설정됩니다.

- 값 필요

#### `--force`, `-f`

전달 속도가 느린 업데이트 허용

- 기본값: `false`
- 값을 수락하지 않음

#### `--force-with-lease`

원격 추적 분기가 최신 상태인 경우 포워드하지 않는 업데이트 허용

- 기본값: `false`
- 값을 수락하지 않음

#### `--set-upstream`, `-u`

대상 환경을 소스 분기에 대한 업스트림으로 설정합니다. 또한 대상 프로젝트를 로컬 저장소의 원격으로 설정합니다.

- 기본값: `false`
- 값을 수락하지 않음

#### `--activate`

푸시하기 전에 환경 활성화

- 기본값: `false`
- 값을 수락하지 않음

#### `--parent`

새 환경 상위 설정( —activate와만 사용)

- 값 필요

#### `--type`

환경 유형 설정( —activate와만 사용)

- 값 필요

#### `--no-clone-parent`

상위 분기의 데이터를 복제하지 않음( —activate와만 사용)

- 기본값: `false`
- 값을 수락하지 않음

#### `--no-wait`, `-W`

작업이 완료될 때까지 기다리지 마십시오.

- 기본값: `false`
- 값을 수락하지 않음

#### `--wait`

작업이 완료될 때까지 대기(기본값)

- 기본값: `false`
- 값을 수락하지 않음

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--identity-file`, `-i`

사용할 SSH ID(개인 키)

- 값 필요


## `environment:redeploy`

```bash
magento-cloud magento-cloud redeploy [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait]
```

환경 재배포

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--no-wait`, `-W`

작업이 완료될 때까지 기다리지 마십시오.

- 기본값: `false`
- 값을 수락하지 않음

#### `--wait`

작업이 완료될 때까지 대기(기본값)

- 기본값: `false`
- 값을 수락하지 않음


## `environment:relationships`

```bash
magento-cloud magento-cloud relationships [-P|--property PROPERTY] [--refresh] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-i|--identity-file IDENTITY-FILE] [--] [<environment>]
```

환경의 관계 표시

### 인수

#### `environment`

환경

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--property`, `-P`

볼 관계 속성

- 값 필요

#### `--refresh`

관계를 새로 고칠지 여부

- 기본값: `false`
- 값을 수락하지 않음

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--app`, `-A`

원격 애플리케이션 이름

- 값 필요

#### `--identity-file`, `-i`

사용할 SSH ID(개인 키)

- 값 필요


## `environment:resume`

```bash
magento-cloud magento-cloud environment:resume [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait]
```

일시 중지된 환경 다시 시작

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--no-wait`, `-W`

작업이 완료될 때까지 기다리지 마십시오.

- 기본값: `false`
- 값을 수락하지 않음

#### `--wait`

작업이 완료될 때까지 대기(기본값)

- 기본값: `false`
- 값을 수락하지 않음


## `environment:scp`

```bash
magento-cloud magento-cloud scp [-r|--recursive] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [-i|--identity-file IDENTITY-FILE] [--] [<files>]...
```

scp를 사용하여 환경 간에 파일 복사

### 인수

#### `files`

복사할 파일. remote: 접두사를 사용하여 원격 위치를 정의합니다.

- 기본값: `[]`
- 배열

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--recursive`, `-r`

전체 디렉토리를 재귀적으로 복사

- 기본값: `false`
- 값을 수락하지 않음

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--app`, `-A`

원격 애플리케이션 이름

- 값 필요

#### `--worker`

작업자 이름

- 값 필요

#### `--instance`, `-I`

인스턴스 ID

- 값 필요

#### `--identity-file`, `-i`

사용할 SSH ID(개인 키)

- 값 필요


## `environment:ssh`

```bash
magento-cloud magento-cloud ssh [--pipe] [--all] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [-i|--identity-file IDENTITY-FILE] [--] [<cmd>]...
```

SSH를 현재 환경에

### 인수

#### `cmd`

환경에서 실행할 명령입니다.

- 기본값: `[]`
- 배열

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--pipe`

SSH URL만 출력합니다.

- 기본값: `false`
- 값을 수락하지 않음

#### `--all`

모든 SSH URL을 출력합니다(모든 앱에 대해).

- 기본값: `false`
- 값을 수락하지 않음

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--app`, `-A`

원격 애플리케이션 이름

- 값 필요

#### `--worker`

작업자 이름

- 값 필요

#### `--instance`, `-I`

인스턴스 ID

- 값 필요

#### `--identity-file`, `-i`

사용할 SSH ID(개인 키)

- 값 필요


## `environment:synchronize`

```bash
magento-cloud magento-cloud sync [--rebase] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<synchronize>]...
```

환경의 코드 및/또는 데이터를 해당 상위 항목에서 동기화

```
This command synchronizes to a child environment from its parent environment.

Synchronizing "code" means there will be a Git merge from the parent to the
child. Synchronizing "data" means that all files in all services (including
static files, databases, logs, search indices, etc.) will be copied from the
parent to the child.
```

### 인수

#### `synchronize`

동기화 대상: &quot;code&quot;, &quot;data&quot; 또는 둘 다

- 기본값: `[]`
- 배열

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--rebase`

병합 대신 리베이스로 코드 동기화

- 기본값: `false`
- 값을 수락하지 않음

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--no-wait`, `-W`

작업이 완료될 때까지 기다리지 마십시오.

- 기본값: `false`
- 값을 수락하지 않음

#### `--wait`

작업이 완료될 때까지 대기(기본값)

- 기본값: `false`
- 값을 수락하지 않음


## `environment:url`

```bash
magento-cloud magento-cloud url [-1|--primary] [--browser BROWSER] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

환경의 공개 URL 가져오기

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--primary`, `-1`

기본 경로의 URL만 반환

- 기본값: `false`
- 값을 수락하지 않음

#### `--browser`

URL을 여는 데 사용할 브라우저입니다. 없음에 대해 0을 설정합니다.

- 값 필요

#### `--pipe`

URL을 stdout으로 출력합니다.

- 기본값: `false`
- 값을 수락하지 않음

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요


## `environment:xdebug`

```bash
magento-cloud magento-cloud xdebug [--port PORT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [-i|--identity-file IDENTITY-FILE]
```

환경에서 Xdebug에 대한 터널을 엽니다.

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--port`

로컬 포트

- 기본값: `9000`
- 값 필요

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--app`, `-A`

원격 애플리케이션 이름

- 값 필요

#### `--worker`

작업자 이름

- 값 필요

#### `--instance`, `-I`

인스턴스 ID

- 값 필요

#### `--identity-file`, `-i`

사용할 SSH ID(개인 키)

- 값 필요


## `integration:activity:get`

```bash
magento-cloud magento-cloud integration:activity:get [-P|--property PROPERTY] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [--] [<integration>] [<activity>]
```

단일 통합 활동에 대한 세부 정보 보기

### 인수

#### `integration`

통합 ID. 목록에서 선택하려면 비워 둡니다.


#### `activity`

활동 ID. 기본값은 가장 최근 통합 활동으로 설정됩니다.

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--property`, `-P`

볼 속성

- 값 필요

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

[사용되지 않는 옵션, 사용되지 않음]

- 값 필요

#### `--format`

출력 형식: table, csv, tsv 또는 plain

- 기본값: `table`
- 값 필요

#### `--columns`, `-c`

표시할 열입니다. % 또는 * 문자는 와일드카드로 사용될 수 있습니다. 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다.

- 기본값: `[]`
- 값 필요

#### `--no-header`

테이블 머리글 출력 안 함

- 기본값: `false`
- 값을 수락하지 않음

#### `--date-fmt`

날짜 형식(PHP 날짜 형식 문자열로서)

- 기본값: `c`
- 값 필요


## `integration:activity:list`

```bash
magento-cloud magento-cloud int:act [--type TYPE] [-x|--exclude-type EXCLUDE-TYPE] [--limit LIMIT] [--start START] [--state STATE] [--result RESULT] [-i|--incomplete] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<id>]
```

통합을 위한 활동 목록 가져오기

### 인수

#### `id`

통합 ID. 목록에서 선택하려면 비워 둡니다.

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--type`

유형별로 활동을 필터링합니다. 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다.

- 기본값: `[]`
- 값 필요

#### `--exclude-type`, `-x`

유형별로 활동을 제외합니다. 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다. % 또는 * 문자를 와일드카드로 사용하여 유형을 제외할 수 있습니다.

- 기본값: `[]`
- 값 필요

#### `--limit`

표시되는 결과 수 제한

- 기본값: `10`
- 값 필요

#### `--start`

이 날짜 이전에 만든 활동만 나열됩니다.

- 값 필요

#### `--state`

상태별로 활동을 필터링합니다. 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다.

- 기본값: `[]`
- 값 필요

#### `--result`

결과로 활동 필터링

- 값 필요

#### `--incomplete`, `-i`

완료되지 않은 활동만 나열

- 기본값: `false`
- 값을 수락하지 않음

#### `--format`

출력 형식: table, csv, tsv 또는 plain

- 기본값: `table`
- 값 필요

#### `--columns`, `-c`

표시할 열입니다. 사용 가능한 열: id*, created*, description*, type*, state*, result*, completed(* = 기본 열). &quot;+&quot; 문자는 기본 열의 자리 표시자로 사용할 수 있습니다. % 또는 * 문자는 와일드카드로 사용될 수 있습니다. 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다.

- 기본값: `[]`
- 값 필요

#### `--no-header`

테이블 머리글 출력 안 함

- 기본값: `false`
- 값을 수락하지 않음

#### `--date-fmt`

날짜 형식(PHP 날짜 형식 문자열로서)

- 기본값: `c`
- 값 필요

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

[사용되지 않는 옵션, 사용되지 않음]

- 값 필요


## `integration:activity:log`

```bash
magento-cloud magento-cloud integration:activity:log [-t|--timestamps] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<integration>] [<activity>]
```

통합 활동에 대한 로그 표시

### 인수

#### `integration`

통합 ID. 목록에서 선택하려면 비워 둡니다.


#### `activity`

활동 ID. 기본값은 가장 최근 통합 활동으로 설정됩니다.

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--timestamps`, `-t`

각 메시지 옆에 타임스탬프 표시

- 기본값: `false`
- 값을 수락하지 않음

#### `--date-fmt`

날짜 형식(PHP 날짜 형식 문자열로서)

- 기본값: `c`
- 값 필요

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

[사용되지 않는 옵션, 사용되지 않음]

- 값 필요


## `integration:add`

```bash
magento-cloud magento-cloud integration:add [--type TYPE] [--base-url BASE-URL] [--bitbucket-url BITBUCKET-URL] [--username USERNAME] [--token TOKEN] [--key KEY] [--secret SECRET] [--license-key LICENSE-KEY] [--server-project SERVER-PROJECT] [--repository REPOSITORY] [--build-merge-requests BUILD-MERGE-REQUESTS] [--build-pull-requests BUILD-PULL-REQUESTS] [--build-draft-pull-requests BUILD-DRAFT-PULL-REQUESTS] [--build-pull-requests-post-merge BUILD-PULL-REQUESTS-POST-MERGE] [--build-wip-merge-requests BUILD-WIP-MERGE-REQUESTS] [--merge-requests-clone-parent-data MERGE-REQUESTS-CLONE-PARENT-DATA] [--pull-requests-clone-parent-data PULL-REQUESTS-CLONE-PARENT-DATA] [--resync-pull-requests RESYNC-PULL-REQUESTS] [--fetch-branches FETCH-BRANCHES] [--prune-branches PRUNE-BRANCHES] [--resources-init RESOURCES-INIT] [--url URL] [--shared-key SHARED-KEY] [--file FILE] [--events EVENTS] [--states STATES] [--environments ENVIRONMENTS] [--excluded-environments EXCLUDED-ENVIRONMENTS] [--from-address FROM-ADDRESS] [--recipients RECIPIENTS] [--channel CHANNEL] [--routing-key ROUTING-KEY] [--category CATEGORY] [--index INDEX] [--sourcetype SOURCETYPE] [--protocol PROTOCOL] [--syslog-host SYSLOG-HOST] [--syslog-port SYSLOG-PORT] [--facility FACILITY] [--message-format MESSAGE-FORMAT] [--auth-mode AUTH-MODE] [--auth-token AUTH-TOKEN] [--verify-tls VERIFY-TLS] [--header HEADER] [-p|--project PROJECT] [-W|--no-wait] [--wait]
```

프로젝트에 통합 추가

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--type`

통합 유형(&#39;bitbucket&#39;, &#39;bitbucket_server&#39;, &#39;github&#39;, &#39;gitlab&#39;, &#39;webhook&#39;, &#39;health.email&#39;, &#39;health.pagerduty&#39;, &#39;health.slack&#39;, &#39;health.webhook&#39;, &#39;httplog&#39;, &#39;script&#39;, &#39;newrelic&#39;, &#39;splunk&#39;, &#39;sumologic&#39;, &#39;syslog&#39;)

- 값 필요

#### `--base-url`

서버 설치의 기본 URL

- 값 필요

#### `--bitbucket-url`

Bitbucket 서버 설치의 기본 URL

- 값 필요

#### `--username`

Bitbucket 서버 사용자 이름

- 값 필요

#### `--token`

통합을 위한 인증 또는 액세스 토큰

- 값 필요

#### `--key`

Bitbucket OAuth 소비자 키

- 값 필요

#### `--secret`

Bitbucket OAuth 소비자 암호

- 값 필요

#### `--license-key`

New Relic 로그 라이선스 키

- 값 필요

#### `--server-project`

프로젝트(예: &#39;namespace/repo&#39;)

- 값 필요

#### `--repository`

추적할 저장소(예: &quot;소유자/저장소&quot;)

- 값 필요

#### `--build-merge-requests`

GitLab: 병합 요청을 환경으로 빌드

- 기본값: `true`
- 값 필요

#### `--build-pull-requests`

모든 가져오기 요청을 환경으로 빌드

- 기본값: `true`
- 값 필요

#### `--build-draft-pull-requests`

초안 끌어오기 요청 작성

- 기본값: `true`
- 값 필요

#### `--build-pull-requests-post-merge`

병합 후 상태를 기반으로 끌어오기 요청 작성

- 기본값: `false`
- 값 필요

#### `--build-wip-merge-requests`

GitLab: WIP 병합 요청 작성

- 기본값: `true`
- 값 필요

#### `--merge-requests-clone-parent-data`

GitLab: 병합 요청에 대한 데이터 복제

- 기본값: `true`
- 값 필요

#### `--pull-requests-clone-parent-data`

끌어오기 요청에 대한 상위 환경의 데이터 복제

- 기본값: `true`
- 값 필요

#### `--resync-pull-requests`

모든 빌드에서 끌어오기 요청 환경 데이터 다시 동기화

- 기본값: `false`
- 값 필요

#### `--fetch-branches`

원격에서 비활성 환경으로 모든 분기 가져오기

- 기본값: `true`
- 값 필요

#### `--prune-branches`

원격에 존재하지 않는 분기 삭제

- 기본값: `true`
- 값 필요

#### `--resources-init`

새 서비스를 초기화할 때 사용할 리소스(&#39;minimum&#39;, &#39;default&#39;, &#39;manual&#39;, &#39;parent&#39;)

- 값 필요

#### `--url`

통합을 위한 URL 또는 API 엔드포인트

- 값 필요

#### `--shared-key`

Webhook: JWS 공유 암호 키

- 값 필요

#### `--file`

업로드할 스크립트가 포함된 로컬 파일의 이름

- 값 필요

#### `--events`

작업할 이벤트 목록(예: environment.push)

- 기본값: `*`
- 값 필요

#### `--states`

처리할 상태 목록(예: 보류 중, in_progress, 완료)

- 기본값: `complete`
- 값 필요

#### `--environments`

포함할 환경 ID

- 기본값: `*`
- 값 필요

#### `--excluded-environments`

제외할 환경 ID

- 기본값: `[]`
- 값 필요

#### `--from-address`

경고 전자 메일에 대한 [선택적] 사용자 지정 보낸 사람 주소

- 값 필요

#### `--recipients`

수신자 이메일 주소

- 기본값: `[]`
- 값 필요

#### `--channel`

Slack 채널

- 값 필요

#### `--routing-key`

PagerDuty 라우팅 키

- 값 필요

#### `--category`

필터링에 사용되는 Sumo 논리 범주

- 값 필요

#### `--index`

Splunk 인덱스

- 값 필요

#### `--sourcetype`

Splunk 이벤트 소스 유형

- 값 필요

#### `--protocol`

Syslog 전송 프로토콜(&#39;tcp&#39;, &#39;udp&#39;, &#39;tls&#39;)

- 기본값: `tls`
- 값 필요

#### `--syslog-host`

Syslog 릴레이/수집기 호스트

- 값 필요

#### `--syslog-port`

Syslog 릴레이/수집기 포트

- 값 필요

#### `--facility`

Syslog 기능

- 기본값: `1`
- 값 필요

#### `--message-format`

Syslog 메시지 형식(&#39;rfc3164&#39; 또는 &#39;rfc5424&#39;)

- 기본값: `rfc5424`
- 값 필요

#### `--auth-mode`

인증 모드(&#39;prefix&#39; 또는 &#39;structured_data&#39;)

- 기본값: `prefix`
- 값 필요

#### `--auth-token`

인증 토큰

- 값 필요

#### `--verify-tls`

HTTPS 인증서 인증을 사용하도록 설정할지 여부(권장)

- 기본값: `true`
- 값 필요

#### `--header`

POST 요청에 사용할 HTTP 헤더입니다. 콜론(:)으로 이름과 값을 구분하십시오.

- 기본값: `[]`
- 값 필요

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--no-wait`, `-W`

작업이 완료될 때까지 기다리지 마십시오.

- 기본값: `false`
- 값을 수락하지 않음

#### `--wait`

작업이 완료될 때까지 대기(기본값)

- 기본값: `false`
- 값을 수락하지 않음


## `integration:delete`

```bash
magento-cloud magento-cloud integration:delete [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] [<id>]
```

프로젝트에서 통합 삭제

### 인수

#### `id`

통합 ID. 목록에서 선택하려면 비워 둡니다.

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--no-wait`, `-W`

작업이 완료될 때까지 기다리지 마십시오.

- 기본값: `false`
- 값을 수락하지 않음

#### `--wait`

작업이 완료될 때까지 대기(기본값)

- 기본값: `false`
- 값을 수락하지 않음


## `integration:get`

```bash
magento-cloud magento-cloud integration:get [-P|--property [PROPERTY]] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [--] [<id>]
```

통합 세부 정보 보기

### 인수

#### `id`

통합 ID. 목록에서 선택하려면 비워 둡니다.

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--property`, `-P`

조회할 통합 속성

- 값을 허용합니다.

#### `--format`

출력 형식: table, csv, tsv 또는 plain

- 기본값: `table`
- 값 필요

#### `--columns`, `-c`

표시할 열입니다. % 또는 * 문자는 와일드카드로 사용될 수 있습니다. 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다.

- 기본값: `[]`
- 값 필요

#### `--no-header`

테이블 머리글 출력 안 함

- 기본값: `false`
- 값을 수락하지 않음

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요


## `integration:list`

```bash
magento-cloud magento-cloud integrations [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT]
```

프로젝트 통합 목록 보기

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--format`

출력 형식: table, csv, tsv 또는 plain

- 기본값: `table`
- 값 필요

#### `--columns`, `-c`

표시할 열입니다. 사용 가능한 열: id, 요약, 유형. % 또는 * 문자는 와일드카드로 사용될 수 있습니다. 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다.

- 기본값: `[]`
- 값 필요

#### `--no-header`

테이블 머리글 출력 안 함

- 기본값: `false`
- 값을 수락하지 않음

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요


## `integration:update`

```bash
magento-cloud magento-cloud integration:update [--type TYPE] [--base-url BASE-URL] [--bitbucket-url BITBUCKET-URL] [--username USERNAME] [--token TOKEN] [--key KEY] [--secret SECRET] [--license-key LICENSE-KEY] [--server-project SERVER-PROJECT] [--repository REPOSITORY] [--build-merge-requests BUILD-MERGE-REQUESTS] [--build-pull-requests BUILD-PULL-REQUESTS] [--build-draft-pull-requests BUILD-DRAFT-PULL-REQUESTS] [--build-pull-requests-post-merge BUILD-PULL-REQUESTS-POST-MERGE] [--build-wip-merge-requests BUILD-WIP-MERGE-REQUESTS] [--merge-requests-clone-parent-data MERGE-REQUESTS-CLONE-PARENT-DATA] [--pull-requests-clone-parent-data PULL-REQUESTS-CLONE-PARENT-DATA] [--resync-pull-requests RESYNC-PULL-REQUESTS] [--fetch-branches FETCH-BRANCHES] [--prune-branches PRUNE-BRANCHES] [--resources-init RESOURCES-INIT] [--url URL] [--shared-key SHARED-KEY] [--file FILE] [--events EVENTS] [--states STATES] [--environments ENVIRONMENTS] [--excluded-environments EXCLUDED-ENVIRONMENTS] [--from-address FROM-ADDRESS] [--recipients RECIPIENTS] [--channel CHANNEL] [--routing-key ROUTING-KEY] [--category CATEGORY] [--index INDEX] [--sourcetype SOURCETYPE] [--protocol PROTOCOL] [--syslog-host SYSLOG-HOST] [--syslog-port SYSLOG-PORT] [--facility FACILITY] [--message-format MESSAGE-FORMAT] [--auth-mode AUTH-MODE] [--auth-token AUTH-TOKEN] [--verify-tls VERIFY-TLS] [--header HEADER] [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] [<id>]
```

통합 업데이트

### 인수

#### `id`

업데이트할 통합 ID

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--type`

통합 유형(&#39;bitbucket&#39;, &#39;bitbucket_server&#39;, &#39;github&#39;, &#39;gitlab&#39;, &#39;webhook&#39;, &#39;health.email&#39;, &#39;health.pagerduty&#39;, &#39;health.slack&#39;, &#39;health.webhook&#39;, &#39;httplog&#39;, &#39;script&#39;, &#39;newrelic&#39;, &#39;splunk&#39;, &#39;sumologic&#39;, &#39;syslog&#39;)

- 값 필요

#### `--base-url`

서버 설치의 기본 URL

- 값 필요

#### `--bitbucket-url`

Bitbucket 서버 설치의 기본 URL

- 값 필요

#### `--username`

Bitbucket 서버 사용자 이름

- 값 필요

#### `--token`

통합을 위한 인증 또는 액세스 토큰

- 값 필요

#### `--key`

Bitbucket OAuth 소비자 키

- 값 필요

#### `--secret`

Bitbucket OAuth 소비자 암호

- 값 필요

#### `--license-key`

New Relic 로그 라이선스 키

- 값 필요

#### `--server-project`

프로젝트(예: &#39;namespace/repo&#39;)

- 값 필요

#### `--repository`

추적할 저장소(예: &quot;소유자/저장소&quot;)

- 값 필요

#### `--build-merge-requests`

GitLab: 병합 요청을 환경으로 빌드

- 기본값: `true`
- 값 필요

#### `--build-pull-requests`

모든 가져오기 요청을 환경으로 빌드

- 기본값: `true`
- 값 필요

#### `--build-draft-pull-requests`

초안 끌어오기 요청 작성

- 기본값: `true`
- 값 필요

#### `--build-pull-requests-post-merge`

병합 후 상태를 기반으로 끌어오기 요청 작성

- 기본값: `false`
- 값 필요

#### `--build-wip-merge-requests`

GitLab: WIP 병합 요청 작성

- 기본값: `true`
- 값 필요

#### `--merge-requests-clone-parent-data`

GitLab: 병합 요청에 대한 데이터 복제

- 기본값: `true`
- 값 필요

#### `--pull-requests-clone-parent-data`

끌어오기 요청에 대한 상위 환경의 데이터 복제

- 기본값: `true`
- 값 필요

#### `--resync-pull-requests`

모든 빌드에서 끌어오기 요청 환경 데이터 다시 동기화

- 기본값: `false`
- 값 필요

#### `--fetch-branches`

원격에서 비활성 환경으로 모든 분기 가져오기

- 기본값: `true`
- 값 필요

#### `--prune-branches`

원격에 존재하지 않는 분기 삭제

- 기본값: `true`
- 값 필요

#### `--resources-init`

새 서비스를 초기화할 때 사용할 리소스(&#39;minimum&#39;, &#39;default&#39;, &#39;manual&#39;, &#39;parent&#39;)

- 값 필요

#### `--url`

통합을 위한 URL 또는 API 엔드포인트

- 값 필요

#### `--shared-key`

Webhook: JWS 공유 암호 키

- 값 필요

#### `--file`

업로드할 스크립트가 포함된 로컬 파일의 이름

- 값 필요

#### `--events`

작업할 이벤트 목록(예: environment.push)

- 기본값: `*`
- 값 필요

#### `--states`

처리할 상태 목록(예: 보류 중, in_progress, 완료)

- 기본값: `complete`
- 값 필요

#### `--environments`

포함할 환경 ID

- 기본값: `*`
- 값 필요

#### `--excluded-environments`

제외할 환경 ID

- 기본값: `[]`
- 값 필요

#### `--from-address`

경고 전자 메일에 대한 [선택적] 사용자 지정 보낸 사람 주소

- 값 필요

#### `--recipients`

수신자 이메일 주소

- 기본값: `[]`
- 값 필요

#### `--channel`

Slack 채널

- 값 필요

#### `--routing-key`

PagerDuty 라우팅 키

- 값 필요

#### `--category`

필터링에 사용되는 Sumo 논리 범주

- 값 필요

#### `--index`

Splunk 인덱스

- 값 필요

#### `--sourcetype`

Splunk 이벤트 소스 유형

- 값 필요

#### `--protocol`

Syslog 전송 프로토콜(&#39;tcp&#39;, &#39;udp&#39;, &#39;tls&#39;)

- 기본값: `tls`
- 값 필요

#### `--syslog-host`

Syslog 릴레이/수집기 호스트

- 값 필요

#### `--syslog-port`

Syslog 릴레이/수집기 포트

- 값 필요

#### `--facility`

Syslog 기능

- 기본값: `1`
- 값 필요

#### `--message-format`

Syslog 메시지 형식(&#39;rfc3164&#39; 또는 &#39;rfc5424&#39;)

- 기본값: `rfc5424`
- 값 필요

#### `--auth-mode`

인증 모드(&#39;prefix&#39; 또는 &#39;structured_data&#39;)

- 기본값: `prefix`
- 값 필요

#### `--auth-token`

인증 토큰

- 값 필요

#### `--verify-tls`

HTTPS 인증서 인증을 사용하도록 설정할지 여부(권장)

- 기본값: `true`
- 값 필요

#### `--header`

POST 요청에 사용할 HTTP 헤더입니다. 콜론(:)으로 이름과 값을 구분하십시오.

- 기본값: `[]`
- 값 필요

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--no-wait`, `-W`

작업이 완료될 때까지 기다리지 마십시오.

- 기본값: `false`
- 값을 수락하지 않음

#### `--wait`

작업이 완료될 때까지 대기(기본값)

- 기본값: `false`
- 값을 수락하지 않음


## `integration:validate`

```bash
magento-cloud magento-cloud integration:validate [-p|--project PROJECT] [--] [<id>]
```

기존 통합의 유효성 검사

```
This command allows you to check whether an integration is valid.

An exit code of 0 means the integration is valid, while 4 means it is invalid.
Any other exit code indicates an unexpected error.

Integrations are validated automatically on creation and on update. However,
because they involve external resources, it is possible for a valid integration
to become invalid. For example, an access token may be revoked, or an external
repository may be deleted.
```

### 인수

#### `id`

통합 ID. 목록에서 선택하려면 비워 둡니다.

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요


## `local:build`

```bash
magento-cloud magento-cloud build [-a|--abslinks] [-s|--source SOURCE] [-d|--destination DESTINATION] [-c|--copy] [--clone] [--run-deploy-hooks] [--no-clean] [--no-archive] [--no-backup] [--no-cache] [--no-build-hooks] [--no-deps] [--working-copy] [--concurrency CONCURRENCY] [--lock] [--] [<app>]...
```

로컬에서 현재 프로젝트 빌드

### 인수

#### `app`

빌드할 응용 프로그램 지정

- 기본값: `[]`
- 배열

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--abslinks`, `-a`

절대 링크 사용

- 기본값: `false`
- 값을 수락하지 않음

#### `--source`, `-s`

소스 디렉토리. 기본값은 현재 프로젝트 루트입니다.

- 값 필요

#### `--destination`, `-d`

각 앱의 웹 루트가 심볼릭 링크되는 대상입니다. 기본값: _www

- 값 필요

#### `--copy`, `-c`

소스에서 심볼릭 링크 대신 빌드 디렉터리로 복사

- 기본값: `false`
- 값을 수락하지 않음

#### `--clone`

Git을 사용하여 현재 HEAD을 빌드 디렉터리에 복제

- 기본값: `false`
- 값을 수락하지 않음

#### `--run-deploy-hooks`

배포 및/또는 post_deploy 후크 실행

- 기본값: `false`
- 값을 수락하지 않음

#### `--no-clean`

이전 빌드 제거 안 함

- 기본값: `false`
- 값을 수락하지 않음

#### `--no-archive`

빌드 보관 파일을 만들거나 사용하지 마십시오.

- 기본값: `false`
- 값을 수락하지 않음

#### `--no-backup`

이전 빌드를 백업하지 않음

- 기본값: `false`
- 값을 수락하지 않음

#### `--no-cache`

캐싱 비활성화

- 기본값: `false`
- 값을 수락하지 않음

#### `--no-build-hooks`

빌드 후 후크를 실행하지 않음

- 기본값: `false`
- 값을 수락하지 않음

#### `--no-deps`

로컬에 빌드 종속성 설치 안 함

- 기본값: `false`
- 값을 수락하지 않음

#### `--working-copy`

Drush: 단순히 버전을 다운로드하는 대신 git을 사용하여 각 Drupal 모듈의 저장소를 복제합니다

- 기본값: `false`
- 값을 수락하지 않음

#### `--concurrency`

푸시: 동시에 처리될 동시 프로젝트 수를 설정합니다

- 기본값: `4`
- 값 필요

#### `--lock`

Drush: 잠금 파일 만들기 또는 업데이트(Drush 버전 7 이상에서만 사용 가능)

- 기본값: `false`
- 값을 수락하지 않음


## `local:dir`

```bash
magento-cloud magento-cloud dir [<subdir>]
```

로컬 프로젝트 루트 찾기

### 인수

#### `subdir`

찾을 하위 디렉터리(&#39;local&#39;, &#39;web&#39; 또는 &#39;shared&#39;)

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.


## `metrics:all`

```bash
magento-cloud magento-cloud metrics [-B|--bytes] [-r|--range RANGE] [-i|--interval INTERVAL] [--to TO] [-1|--latest] [-s|--service SERVICE] [--type TYPE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT]
```

BETA 환경에 대한 CPU, 디스크 및 메모리 측정 단위 표시

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--bytes`, `-B`

크기(바이트) 표시

- 기본값: `false`
- 값을 수락하지 않음

#### `--range`, `-r`

시간 범위입니다. 이 기간 동안 종료 시간(-to)까지 지표가 로드됩니다. 시간(h), 분(m) 또는 초(s)와 같은 단위를 지정할 수 있습니다. 최소 5m, 최대 8시간 이상(프로젝트에 따라 다름), 기본 10m.

- 값 필요

#### `--interval`, `-i`

시간 간격입니다. 기본값은 범위의 분할입니다. 시간(h), 분(m) 또는 초(s)와 같은 단위를 지정할 수 있습니다. 최소 1m.

- 값 필요

#### `--to`

종료 시간. 기본값은 now입니다.

- 값 필요

#### `--latest`, `-1`

최신 단일 데이터 포인트만 표시

- 기본값: `false`
- 값을 수락하지 않음

#### `--service`, `-s`

서비스 또는 애플리케이션 이름별로 필터링 % 또는 * 문자를 와일드카드로 사용할 수 있습니다.

- 기본값: `[]`
- 값 필요

#### `--type`

서비스 유형별로 필터링합니다( —service가 제공되지 않은 경우). 버전은 필수가 아닙니다. % 또는 * 문자는 와일드카드로 사용될 수 있습니다.

- 기본값: `[]`
- 값 필요

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--format`

출력 형식: table, csv, tsv 또는 plain

- 기본값: `table`
- 값 필요

#### `--columns`, `-c`

표시할 열입니다. 사용 가능한 열: timestamp*, service*, cpu_percent*, mem_percent*, disk_percent*, tmp_disk_percent*, cpu_limit, cpu_used, disk_limit, disk_used, inodes_limit, inodes_percent, inodes_used, mem_limit, mem_used, tmp_disk_limit, tmp_disk_used, tmp_inodes_limit, tmp_inodes_percent, tmp_inodes_used, 유형(* = 기본 열). &quot;+&quot; 문자는 기본 열의 자리 표시자로 사용할 수 있습니다. % 또는 * 문자는 와일드카드로 사용될 수 있습니다. 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다.

- 기본값: `[]`
- 값 필요

#### `--no-header`

테이블 머리글 출력 안 함

- 기본값: `false`
- 값을 수락하지 않음

#### `--date-fmt`

날짜 형식(PHP 날짜 형식 문자열로서)

- 기본값: `c`
- 값 필요


## `metrics:cpu`

```bash
magento-cloud magento-cloud cpu [-r|--range RANGE] [-i|--interval INTERVAL] [--to TO] [-1|--latest] [-s|--service SERVICE] [--type TYPE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT]
```

BETA 환경의 CPU 사용량 표시

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--range`, `-r`

시간 범위입니다. 이 기간 동안 종료 시간(-to)까지 지표가 로드됩니다. 시간(h), 분(m) 또는 초(s)와 같은 단위를 지정할 수 있습니다. 최소 5m, 최대 8시간 이상(프로젝트에 따라 다름), 기본 10m.

- 값 필요

#### `--interval`, `-i`

시간 간격입니다. 기본값은 범위의 분할입니다. 시간(h), 분(m) 또는 초(s)와 같은 단위를 지정할 수 있습니다. 최소 1m.

- 값 필요

#### `--to`

종료 시간. 기본값은 now입니다.

- 값 필요

#### `--latest`, `-1`

최신 단일 데이터 포인트만 표시

- 기본값: `false`
- 값을 수락하지 않음

#### `--service`, `-s`

서비스 또는 애플리케이션 이름별로 필터링 % 또는 * 문자를 와일드카드로 사용할 수 있습니다.

- 기본값: `[]`
- 값 필요

#### `--type`

서비스 유형별로 필터링합니다( —service가 제공되지 않은 경우). 버전은 필수가 아닙니다. % 또는 * 문자는 와일드카드로 사용될 수 있습니다.

- 기본값: `[]`
- 값 필요

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--format`

출력 형식: table, csv, tsv 또는 plain

- 기본값: `table`
- 값 필요

#### `--columns`, `-c`

표시할 열입니다. 사용 가능한 열: timestamp*, service*, used*, limit*,%*, type (* = 기본 열). &quot;+&quot; 문자는 기본 열의 자리 표시자로 사용할 수 있습니다. % 또는 * 문자는 와일드카드로 사용될 수 있습니다. 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다.

- 기본값: `[]`
- 값 필요

#### `--no-header`

테이블 머리글 출력 안 함

- 기본값: `false`
- 값을 수락하지 않음

#### `--date-fmt`

날짜 형식(PHP 날짜 형식 문자열로서)

- 기본값: `c`
- 값 필요


## `metrics:disk-usage`

```bash
magento-cloud magento-cloud disk [-B|--bytes] [-r|--range RANGE] [-i|--interval INTERVAL] [--to TO] [-1|--latest] [-s|--service SERVICE] [--type TYPE] [--tmp] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT]
```

환경의 디스크 사용량 표시

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--bytes`, `-B`

크기(바이트) 표시

- 기본값: `false`
- 값을 수락하지 않음

#### `--range`, `-r`

시간 범위입니다. 이 기간 동안 종료 시간(-to)까지 지표가 로드됩니다. 시간(h), 분(m) 또는 초(s)와 같은 단위를 지정할 수 있습니다. 최소 5m, 최대 8시간 이상(프로젝트에 따라 다름), 기본 10m.

- 값 필요

#### `--interval`, `-i`

시간 간격입니다. 기본값은 범위의 분할입니다. 시간(h), 분(m) 또는 초(s)와 같은 단위를 지정할 수 있습니다. 최소 1m.

- 값 필요

#### `--to`

종료 시간. 기본값은 now입니다.

- 값 필요

#### `--latest`, `-1`

최신 단일 데이터 포인트만 표시

- 기본값: `false`
- 값을 수락하지 않음

#### `--service`, `-s`

서비스 또는 애플리케이션 이름별로 필터링 % 또는 * 문자를 와일드카드로 사용할 수 있습니다.

- 기본값: `[]`
- 값 필요

#### `--type`

서비스 유형별로 필터링합니다( —service가 제공되지 않은 경우). 버전은 필수가 아닙니다. % 또는 * 문자는 와일드카드로 사용될 수 있습니다.

- 기본값: `[]`
- 값 필요

#### `--tmp`

임시 디스크 사용량 보고(열: timestamp, service, tmp_used, tmp_limit, tmp_percent, tmp_ipercent 표시)

- 기본값: `false`
- 값을 수락하지 않음

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--format`

출력 형식: table, csv, tsv 또는 plain

- 기본값: `table`
- 값 필요

#### `--columns`, `-c`

표시할 열입니다. 사용 가능한 열: timestamp*, service*, used*, limit*, percent*, tmp_percent*, limit, iused, tmp_limit, tmp_ipercent, tmp_iused, tmp_limit, tmp_used, 유형(* = 기본 열). &quot;+&quot; 문자는 기본 열의 자리 표시자로 사용할 수 있습니다. % 또는 * 문자는 와일드카드로 사용될 수 있습니다. 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다.

- 기본값: `[]`
- 값 필요

#### `--no-header`

테이블 머리글 출력 안 함

- 기본값: `false`
- 값을 수락하지 않음

#### `--date-fmt`

날짜 형식(PHP 날짜 형식 문자열로서)

- 기본값: `c`
- 값 필요


## `metrics:memory`

```bash
magento-cloud magento-cloud mem [-B|--bytes] [-r|--range RANGE] [-i|--interval INTERVAL] [--to TO] [-1|--latest] [-s|--service SERVICE] [--type TYPE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT]
```

BETA 환경의 메모리 사용량 표시

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--bytes`, `-B`

크기(바이트) 표시

- 기본값: `false`
- 값을 수락하지 않음

#### `--range`, `-r`

시간 범위입니다. 이 기간 동안 종료 시간(-to)까지 지표가 로드됩니다. 시간(h), 분(m) 또는 초(s)와 같은 단위를 지정할 수 있습니다. 최소 5m, 최대 8시간 이상(프로젝트에 따라 다름), 기본 10m.

- 값 필요

#### `--interval`, `-i`

시간 간격입니다. 기본값은 범위의 분할입니다. 시간(h), 분(m) 또는 초(s)와 같은 단위를 지정할 수 있습니다. 최소 1m.

- 값 필요

#### `--to`

종료 시간. 기본값은 now입니다.

- 값 필요

#### `--latest`, `-1`

최신 단일 데이터 포인트만 표시

- 기본값: `false`
- 값을 수락하지 않음

#### `--service`, `-s`

서비스 또는 애플리케이션 이름별로 필터링 % 또는 * 문자를 와일드카드로 사용할 수 있습니다.

- 기본값: `[]`
- 값 필요

#### `--type`

서비스 유형별로 필터링합니다( —service가 제공되지 않은 경우). 버전은 필수가 아닙니다. % 또는 * 문자는 와일드카드로 사용될 수 있습니다.

- 기본값: `[]`
- 값 필요

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--format`

출력 형식: table, csv, tsv 또는 plain

- 기본값: `table`
- 값 필요

#### `--columns`, `-c`

표시할 열입니다. 사용 가능한 열: timestamp*, service*, used*, limit*,%*, type (* = 기본 열). &quot;+&quot; 문자는 기본 열의 자리 표시자로 사용할 수 있습니다. % 또는 * 문자는 와일드카드로 사용될 수 있습니다. 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다.

- 기본값: `[]`
- 값 필요

#### `--no-header`

테이블 머리글 출력 안 함

- 기본값: `false`
- 값을 수락하지 않음

#### `--date-fmt`

날짜 형식(PHP 날짜 형식 문자열로서)

- 기본값: `c`
- 값 필요


## `mount:download`

```bash
magento-cloud magento-cloud mount:download [-a|--all] [-m|--mount MOUNT] [--target TARGET] [--source-path] [--delete] [--exclude EXCLUDE] [--include INCLUDE] [--refresh] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [-i|--identity-file IDENTITY-FILE]
```

rsync를 사용하여 마운트에서 파일 다운로드

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--all`, `-a`

모든 마운트에서 다운로드

- 기본값: `false`
- 값을 수락하지 않음

#### `--mount`, `-m`

마운트(앱 상대 경로로 사용)

- 값 필요

#### `--target`

파일을 다운로드할 디렉터리입니다. 모두 사용하는 경우 마운트 경로가 추가됩니다

- 값 필요

#### `--source-path`

—all 이 사용되는 경우 마운트 경로 대신 마운트의 소스 경로를 타겟의 하위 디렉토리로 사용합니다.

- 기본값: `false`
- 값을 수락하지 않음

#### `--delete`

대상 디렉토리에서 관련 없는 파일을 삭제할지 여부

- 기본값: `false`
- 값을 수락하지 않음

#### `--exclude`

다운로드에서 제외할 파일(패턴)

- 기본값: `[]`
- 값 필요

#### `--include`

제외하지 않을 파일(패턴)

- 기본값: `[]`
- 값 필요

#### `--refresh`

캐시 새로 고침 여부

- 기본값: `false`
- 값을 수락하지 않음

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--app`, `-A`

원격 애플리케이션 이름

- 값 필요

#### `--worker`

작업자 이름

- 값 필요

#### `--instance`, `-I`

인스턴스 ID

- 값 필요

#### `--identity-file`, `-i`

사용할 SSH ID(개인 키)

- 값 필요


## `mount:list`

```bash
magento-cloud magento-cloud mounts [--paths] [--refresh] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE]
```

마운트 목록 가져오기

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--paths`

마운트 경로만 출력(한 줄에 한 개)

- 기본값: `false`
- 값을 수락하지 않음

#### `--refresh`

캐시 새로 고침 여부

- 기본값: `false`
- 값을 수락하지 않음

#### `--format`

출력 형식: table, csv, tsv 또는 plain

- 기본값: `table`
- 값 필요

#### `--columns`, `-c`

표시할 열입니다. 사용 가능한 열: 정의, 경로. % 또는 * 문자는 와일드카드로 사용될 수 있습니다. 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다.

- 기본값: `[]`
- 값 필요

#### `--no-header`

테이블 머리글 출력 안 함

- 기본값: `false`
- 값을 수락하지 않음

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--app`, `-A`

원격 애플리케이션 이름

- 값 필요

#### `--worker`

작업자 이름

- 값 필요

#### `--instance`, `-I`

인스턴스 ID

- 값 필요


## `mount:size`

```bash
magento-cloud magento-cloud mount:size [-B|--bytes] [--refresh] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-i|--identity-file IDENTITY-FILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE]
```

마운트의 디스크 사용량 확인

```
Use this command to check the disk size and usage for an application's mounts.

Mounts are directories mounted into the application from a persistent, writable
filesystem. They are configured in the mounts key in the application configuration.

The filesystem's total size is determined by the disk key in the same file.
```

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--bytes`, `-B`

크기(바이트) 표시

- 기본값: `false`
- 값을 수락하지 않음

#### `--refresh`

캐시 새로 고침

- 기본값: `false`
- 값을 수락하지 않음

#### `--format`

출력 형식: table, csv, tsv 또는 plain

- 기본값: `table`
- 값 필요

#### `--columns`, `-c`

표시할 열입니다. 사용 가능한 열: available, max, mounts, percent_used, sizes, used % 또는 * 문자는 와일드카드로 사용될 수 있습니다. 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다.

- 기본값: `[]`
- 값 필요

#### `--no-header`

테이블 머리글 출력 안 함

- 기본값: `false`
- 값을 수락하지 않음

#### `--identity-file`, `-i`

사용할 SSH ID(개인 키)

- 값 필요

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--app`, `-A`

원격 애플리케이션 이름

- 값 필요

#### `--worker`

작업자 이름

- 값 필요

#### `--instance`, `-I`

인스턴스 ID

- 값 필요


## `mount:upload`

```bash
magento-cloud magento-cloud mount:upload [--source SOURCE] [-m|--mount MOUNT] [--delete] [--exclude EXCLUDE] [--include INCLUDE] [--refresh] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [-i|--identity-file IDENTITY-FILE]
```

rsync를 사용하여 마운트에 파일 업로드

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--source`

업로드할 파일이 포함된 디렉토리

- 값 필요

#### `--mount`, `-m`

마운트(앱 상대 경로로 사용)

- 값 필요

#### `--delete`

마운트에서 불필요한 파일을 삭제할지 여부

- 기본값: `false`
- 값을 수락하지 않음

#### `--exclude`

업로드에서 제외할 파일(패턴)

- 기본값: `[]`
- 값 필요

#### `--include`

제외하지 않을 파일(패턴)

- 기본값: `[]`
- 값 필요

#### `--refresh`

캐시 새로 고침 여부

- 기본값: `false`
- 값을 수락하지 않음

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--app`, `-A`

원격 애플리케이션 이름

- 값 필요

#### `--worker`

작업자 이름

- 값 필요

#### `--instance`, `-I`

인스턴스 ID

- 값 필요

#### `--identity-file`, `-i`

사용할 SSH ID(개인 키)

- 값 필요


## `operation:list`

```bash
magento-cloud magento-cloud ops [--full] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

환경에 대한 Beta 목록 런타임 작업

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--full`

표시할 명령의 길이를 제한하지 마십시오. 기본 제한은 24줄입니다.

- 기본값: `false`
- 값을 수락하지 않음

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--app`, `-A`

원격 애플리케이션 이름

- 값 필요

#### `--worker`

작업자 이름

- 값 필요

#### `--format`

출력 형식: table, csv, tsv 또는 plain

- 기본값: `table`
- 값 필요

#### `--columns`, `-c`

표시할 열입니다. 사용 가능한 열: 서비스*, 이름*, 시작*, 역할, 중지 (* = 기본 열). &quot;+&quot; 문자는 기본 열의 자리 표시자로 사용할 수 있습니다. % 또는 * 문자는 와일드카드로 사용될 수 있습니다. 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다.

- 기본값: `[]`
- 값 필요

#### `--no-header`

테이블 머리글 출력 안 함

- 기본값: `false`
- 값을 수락하지 않음


## `operation:run`

```bash
magento-cloud magento-cloud operation:run [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-W|--no-wait] [--wait] [--] [<operation>]
```

BETA 환경에서 작업 실행

### 인수

#### `operation`

작업 이름

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--app`, `-A`

원격 애플리케이션 이름

- 값 필요

#### `--worker`

작업자 이름

- 값 필요

#### `--no-wait`, `-W`

작업이 완료될 때까지 기다리지 마십시오.

- 기본값: `false`
- 값을 수락하지 않음

#### `--wait`

작업이 완료될 때까지 대기(기본값)

- 기본값: `false`
- 값을 수락하지 않음


## `project:clear-build-cache`

```bash
magento-cloud magento-cloud project:clear-build-cache [-p|--project PROJECT]
```

프로젝트의 빌드 캐시 지우기

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요


## `project:get`

```bash
magento-cloud magento-cloud get [-e|--environment ENVIRONMENT] [--depth DEPTH] [--build] [-p|--project PROJECT] [-i|--identity-file IDENTITY-FILE] [--] [<project>] [<directory>]
```

로컬로 프로젝트 복제

### 인수

#### `project`

프로젝트 ID


#### `directory`

복제할 디렉토리입니다. 기본값은 프로젝트 제목입니다.

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--environment`, `-e`

복제할 환경 ID입니다. 기본값은 프로젝트 기본값 또는 사용 가능한 첫 번째 환경으로 설정됩니다.

- 값 필요

#### `--depth`

약식 복제 만들기: 기록의 커밋 수 제한

- 값 필요

#### `--build`

복제 후 프로젝트 빌드

- 기본값: `false`
- 값을 수락하지 않음

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--identity-file`, `-i`

사용할 SSH ID(개인 키)

- 값 필요


## `project:info`

```bash
magento-cloud magento-cloud project:info [--refresh] [--date-fmt DATE-FMT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] [<property>] [<value>]
```

프로젝트 속성 읽기 또는 설정

### 인수

#### `property`

속성 이름


#### `value`

속성에 대한 새 값 설정

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--refresh`

캐시 새로 고침 여부

- 기본값: `false`
- 값을 수락하지 않음

#### `--date-fmt`

날짜 형식(PHP 날짜 형식 문자열로서)

- 기본값: `c`
- 값 필요

#### `--format`

출력 형식: table, csv, tsv 또는 plain

- 기본값: `table`
- 값 필요

#### `--columns`, `-c`

표시할 열입니다. % 또는 * 문자는 와일드카드로 사용될 수 있습니다. 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다.

- 기본값: `[]`
- 값 필요

#### `--no-header`

테이블 머리글 출력 안 함

- 기본값: `false`
- 값을 수락하지 않음

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--no-wait`, `-W`

작업이 완료될 때까지 기다리지 마십시오.

- 기본값: `false`
- 값을 수락하지 않음

#### `--wait`

작업이 완료될 때까지 대기(기본값)

- 기본값: `false`
- 값을 수락하지 않음


## `project:list`

```bash
magento-cloud magento-cloud projects [--pipe] [--region REGION] [--title TITLE] [--my] [--refresh REFRESH] [--sort SORT] [--reverse] [--page PAGE] [-c|--count COUNT] [--format FORMAT] [--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT]
```

모든 활성 프로젝트 목록 가져오기

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--pipe`

간단한 프로젝트 ID 목록을 출력합니다. 페이지 매김을 비활성화합니다.

- 기본값: `false`
- 값을 수락하지 않음

#### `--region`

지역으로 필터링(정확히 일치)

- 값 필요

#### `--title`

제목별 필터링(대/소문자 구분 안 함 검색)

- 값 필요

#### `--my`

소유한 프로젝트만 표시

- 기본값: `false`
- 값을 수락하지 않음

#### `--refresh`

목록 새로 고침 여부

- 기본값: `1`
- 값 필요

#### `--sort`

정렬 기준 속성

- 기본값: `title`
- 값 필요

#### `--reverse`

역순(내림차순) 정렬

- 기본값: `false`
- 값을 수락하지 않음

#### `--page`

페이지 번호. 이렇게 하면 구성이나 —count에도 불구하고 페이지 매김이 가능합니다. - 파이프가 지정된 경우 무시됩니다.

- 값 필요

#### `--count`, `-c`

페이지당 표시할 프로젝트 수입니다. 페이지 매김을 비활성화하려면 0을 사용하십시오. —page가 지정된 경우 무시됩니다.

- 값 필요

#### `--format`

출력 형식: table, csv, tsv 또는 plain

- 기본값: `table`
- 값 필요

#### `--columns`

표시할 열입니다. 사용 가능한 열: id*, title*, region*, created_at, organization_id, organization_label, organization_name, status (* = 기본 열). &quot;+&quot; 문자는 기본 열의 자리 표시자로 사용할 수 있습니다. % 또는 * 문자는 와일드카드로 사용될 수 있습니다. 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다.

- 기본값: `[]`
- 값 필요

#### `--no-header`

테이블 머리글 출력 안 함

- 기본값: `false`
- 값을 수락하지 않음

#### `--date-fmt`

날짜 형식(PHP 날짜 형식 문자열로서)

- 기본값: `c`
- 값 필요


## `project:set-remote`

```bash
magento-cloud magento-cloud set-remote [<project>]
```

현재 Git 저장소에 대한 원격 프로젝트 설정

### 인수

#### `project`

프로젝트 ID

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.


## `repo:cat`

```bash
magento-cloud magento-cloud repo:cat [-c|--commit COMMIT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] <path>
```

프로젝트 저장소의 파일 읽기

### 인수

#### `path`

파일의 경로

- 필수

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--commit`, `-c`

커밋 SHA. 상위 커밋에 대해 &quot;HEAD&quot; 및 삽입(^) 또는 물결표(~) 접미사를 사용할 수도 있습니다.

- 값 필요

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요


## `repo:ls`

```bash
magento-cloud magento-cloud repo:ls [-d|--directories] [-f|--files] [--git-style] [-c|--commit COMMIT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<path>]
```

프로젝트 저장소의 파일 나열

### 인수

#### `path`

하위 디렉터리에 대한 경로

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--directories`, `-d`

디렉터리만 표시

- 기본값: `false`
- 값을 수락하지 않음

#### `--files`, `-f`

파일만 표시

- 기본값: `false`
- 값을 수락하지 않음

#### `--git-style`

&quot;git ls-tree&quot;와 유사한 스타일 출력

- 기본값: `false`
- 값을 수락하지 않음

#### `--commit`, `-c`

커밋 SHA. 상위 커밋에 대해 &quot;HEAD&quot; 및 삽입(^) 또는 물결표(~) 접미사를 사용할 수도 있습니다.

- 값 필요

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요


## `repo:read`

```bash
magento-cloud magento-cloud read [-c|--commit COMMIT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<path>]
```

프로젝트 저장소의 디렉터리 또는 파일 읽기

### 인수

#### `path`

디렉토리 또는 파일에 대한 경로

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--commit`, `-c`

커밋 SHA. 상위 커밋에 대해 &quot;HEAD&quot; 및 삽입(^) 또는 물결표(~) 접미사를 사용할 수도 있습니다.

- 값 필요

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요


## `route:get`

```bash
magento-cloud magento-cloud route:get [--id ID] [-1|--primary] [-P|--property PROPERTY] [--refresh] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-i|--identity-file IDENTITY-FILE] [--] [<route>]
```

경로에 대한 세부 정보 보기

### 인수

#### `route`

경로의 원래 URL

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--id`

선택할 경로 ID

- 값 필요

#### `--primary`, `-1`

기본 경로 선택

- 기본값: `false`
- 값을 수락하지 않음

#### `--property`, `-P`

표시할 속성입니다

- 값 필요

#### `--refresh`

경로 캐시 우회

- 기본값: `false`
- 값을 수락하지 않음

#### `--date-fmt`

날짜 형식(PHP 날짜 형식 문자열로서)

- 기본값: `c`
- 값 필요

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--app`, `-A`

[사용되지 않는 옵션, 더 이상 사용되지 않음]

- 값 필요

#### `--identity-file`, `-i`

[사용되지 않는 옵션, 더 이상 사용되지 않음]

- 값 필요


## `route:list`

```bash
magento-cloud magento-cloud routes [--refresh] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<environment>]
```

환경의 모든 경로 나열

### 인수

#### `environment`

환경 ID

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--refresh`

경로 캐시 우회

- 기본값: `false`
- 값을 수락하지 않음

#### `--format`

출력 형식: table, csv, tsv 또는 plain

- 기본값: `table`
- 값 필요

#### `--columns`, `-c`

표시할 열입니다. 사용 가능한 열: route*, type*, to*, url (* = 기본 열). &quot;+&quot; 문자는 기본 열의 자리 표시자로 사용할 수 있습니다. % 또는 * 문자는 와일드카드로 사용될 수 있습니다. 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다.

- 기본값: `[]`
- 값 필요

#### `--no-header`

테이블 머리글 출력 안 함

- 기본값: `false`
- 값을 수락하지 않음

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요


## `self:install`

```bash
magento-cloud magento-cloud self:install [--shell-type SHELL-TYPE]
```

CLI 구성 파일 설치 또는 업데이트

```
This command automatically installs shell configuration for the Magento Cloud CLI,
adding autocompletion support and handy aliases. Bash and ZSH are supported.
```

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--shell-type`

자동 완성을 위한 셸 유형(bash 또는 zsh)

- 값 필요


## `self:update`

```bash
magento-cloud magento-cloud update [--no-major] [--unstable] [--manifest MANIFEST] [--current-version CURRENT-VERSION] [--timeout TIMEOUT]
```

CLI를 최신 버전으로 업데이트

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--no-major`

부 또는 패치 버전 사이에서만 업데이트

- 기본값: `false`
- 값을 수락하지 않음

#### `--unstable`

사용 가능한 경우 새로운 불안정 버전으로 업데이트

- 기본값: `false`
- 값을 수락하지 않음

#### `--manifest`

매니페스트 파일 위치 재정의

- 값 필요

#### `--current-version`

현재 버전 재정의

- 값 필요

#### `--timeout`

버전 검사에 대한 시간 제한

- 기본값: `30`
- 값 필요


## `service:list`

```bash
magento-cloud magento-cloud services [--refresh] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

프로젝트의 서비스 나열

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--refresh`

캐시 새로 고침 여부

- 기본값: `false`
- 값을 수락하지 않음

#### `--pipe`

서비스 이름 목록만 출력

- 기본값: `false`
- 값을 수락하지 않음

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--format`

출력 형식: table, csv, tsv 또는 plain

- 기본값: `table`
- 값 필요

#### `--columns`, `-c`

표시할 열입니다. 사용 가능한 열: 디스크, 이름, 크기, 유형 % 또는 * 문자는 와일드카드로 사용될 수 있습니다. 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다.

- 기본값: `[]`
- 값 필요

#### `--no-header`

테이블 머리글 출력 안 함

- 기본값: `false`
- 값을 수락하지 않음


## `service:mongo:dump`

```bash
magento-cloud magento-cloud mongodump [-c|--collection COLLECTION] [-z|--gzip] [-o|--stdout] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

MongoDB에서 데이터의 바이너리 아카이브 덤프 만들기

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--collection`, `-c`

덤프할 컬렉션

- 값 필요

#### `--gzip`, `-z`

gzip을 사용하여 덤프 압축

- 기본값: `false`
- 값을 수락하지 않음

#### `--stdout`, `-o`

파일 대신 STDOUT으로 출력

- 기본값: `false`
- 값을 수락하지 않음

#### `--relationship`, `-r`

사용할 서비스 관계

- 값 필요

#### `--identity-file`, `-i`

사용할 SSH ID(개인 키)

- 값 필요

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--app`, `-A`

원격 애플리케이션 이름

- 값 필요


## `service:mongo:export`

```bash
magento-cloud magento-cloud mongoexport [-c|--collection COLLECTION] [--jsonArray] [--type TYPE] [-f|--fields FIELDS] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

MongoDB에서 데이터 내보내기

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--collection`, `-c`

내보낼 컬렉션

- 값 필요

#### `--jsonArray`

데이터를 단일 JSON 배열로 내보내기

- 기본값: `false`
- 값을 수락하지 않음

#### `--type`

내보내기 유형(예: &quot;csv&quot;)

- 값 필요

#### `--fields`, `-f`

내보낼 필드

- 기본값: `[]`
- 값 필요

#### `--relationship`, `-r`

사용할 서비스 관계

- 값 필요

#### `--identity-file`, `-i`

사용할 SSH ID(개인 키)

- 값 필요

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--app`, `-A`

원격 애플리케이션 이름

- 값 필요


## `service:mongo:restore`

```bash
magento-cloud magento-cloud mongorestore [-c|--collection COLLECTION] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

데이터의 바이너리 아카이브 덤프를 MongoDB로 복원

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--collection`, `-c`

복원할 컬렉션

- 값 필요

#### `--relationship`, `-r`

사용할 서비스 관계

- 값 필요

#### `--identity-file`, `-i`

사용할 SSH ID(개인 키)

- 값 필요

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--app`, `-A`

원격 애플리케이션 이름

- 값 필요


## `service:mongo:shell`

```bash
magento-cloud magento-cloud mongo [--eval EVAL] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

MongoDB 셸 사용

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--eval`

셸에 JavaScript 조각 전달

- 값 필요

#### `--relationship`, `-r`

사용할 서비스 관계

- 값 필요

#### `--identity-file`, `-i`

사용할 SSH ID(개인 키)

- 값 필요

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--app`, `-A`

원격 애플리케이션 이름

- 값 필요


## `service:redis-cli`

```bash
magento-cloud magento-cloud redis [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--] [<args>]
```

Redis CLI 액세스

### 인수

#### `args`

Redis 명령에 추가할 인수

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--relationship`, `-r`

사용할 서비스 관계

- 값 필요

#### `--identity-file`, `-i`

사용할 SSH ID(개인 키)

- 값 필요

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--app`, `-A`

원격 애플리케이션 이름

- 값 필요


## `snapshot:create`

```bash
magento-cloud magento-cloud backup [--live] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<environment>]
```

환경의 스냅샷 만들기

### 인수

#### `environment`

환경

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--live`

라이브 백업: 환경을 중지하지 마십시오. 설정하면 백업 중에 환경이 실행되고 연결에 대해 열려 있게 됩니다. 따라서 다운타임이 줄어들어 일관성 없는 상태에서 데이터를 백업할 수 있습니다.

- 기본값: `false`
- 값을 수락하지 않음

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--no-wait`, `-W`

작업이 완료될 때까지 기다리지 마십시오.

- 기본값: `false`
- 값을 수락하지 않음

#### `--wait`

작업이 완료될 때까지 대기(기본값)

- 기본값: `false`
- 값을 수락하지 않음


## `snapshot:delete`

```bash
magento-cloud magento-cloud snapshot:delete [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<id>]
```

환경 스냅숏 삭제

### 인수

#### `id`

스냅샷의 ID입니다. 비대화형 모드에 필요합니다.

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--no-wait`, `-W`

작업이 완료될 때까지 기다리지 마십시오.

- 기본값: `false`
- 값을 수락하지 않음

#### `--wait`

작업이 완료될 때까지 대기(기본값)

- 기본값: `false`
- 값을 수락하지 않음


## `snapshot:get`

```bash
magento-cloud magento-cloud snapshot:get [-P|--property PROPERTY] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--date-fmt DATE-FMT] [--] [<id>]
```

환경 스냅숏 보기

### 인수

#### `id`

스냅샷의 ID입니다. 기본값은 가장 최근 값으로 설정됩니다.

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--property`, `-P`

표시할 속성입니다.

- 값 필요

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--date-fmt`

날짜 형식(PHP 날짜 형식 문자열로서)

- 기본값: `c`
- 값 필요


## `snapshot:list`

```bash
magento-cloud magento-cloud snapshots [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

환경의 사용 가능한 스냅샷 나열

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--format`

출력 형식: table, csv, tsv 또는 plain

- 기본값: `table`
- 값 필요

#### `--columns`, `-c`

표시할 열입니다. % 또는 * 문자는 와일드카드로 사용될 수 있습니다. 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다.

- 기본값: `[]`
- 값 필요

#### `--no-header`

테이블 머리글 출력 안 함

- 기본값: `false`
- 값을 수락하지 않음

#### `--date-fmt`

날짜 형식(PHP 날짜 형식 문자열로서)

- 기본값: `c`
- 값 필요

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요


## `snapshot:restore`

```bash
magento-cloud magento-cloud snapshot:restore [--target TARGET] [--branch-from BRANCH-FROM] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<snapshot>]
```

환경 스냅샷 복원

### 인수

#### `snapshot`

스냅샷의 이름입니다. 기본값은 가장 최근 값으로 설정됩니다.

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--target`

복원할 환경입니다. 기본값은 스냅샷의 현재 환경으로 설정됩니다.

- 값 필요

#### `--branch-from`

—target이 아직 존재하지 않는 경우 새 환경의 상위 항목을 지정합니다

- 값 필요

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--no-wait`, `-W`

작업이 완료될 때까지 기다리지 마십시오.

- 기본값: `false`
- 값을 수락하지 않음

#### `--wait`

작업이 완료될 때까지 대기(기본값)

- 기본값: `false`
- 값을 수락하지 않음


## `source-operation:list`

```bash
magento-cloud magento-cloud source-ops [--full] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

환경에 대한 소스 작업 나열

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--full`

표시할 명령의 길이를 제한하지 마십시오. 기본 제한은 24줄입니다.

- 기본값: `false`
- 값을 수락하지 않음

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--format`

출력 형식: table, csv, tsv 또는 plain

- 기본값: `table`
- 값 필요

#### `--columns`, `-c`

표시할 열입니다. 사용 가능한 열: app, command, operation. % 또는 * 문자는 와일드카드로 사용될 수 있습니다. 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다.

- 기본값: `[]`
- 값 필요

#### `--no-header`

테이블 머리글 출력 안 함

- 기본값: `false`
- 값을 수락하지 않음


## `source-operation:run`

```bash
magento-cloud magento-cloud source-operation:run [--variable VARIABLE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<operation>]
```

소스 작업 실행

### 인수

#### `operation`

작업 이름

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--variable`

작업 중에 설정할 변수(type:name=value 형식)

- 기본값: `[]`
- 값 필요

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--no-wait`, `-W`

작업이 완료될 때까지 기다리지 마십시오.

- 기본값: `false`
- 값을 수락하지 않음

#### `--wait`

작업이 완료될 때까지 대기(기본값)

- 기본값: `false`
- 값을 수락하지 않음


## `ssh-cert:load`

```bash
magento-cloud magento-cloud ssh-cert:load [--refresh-only] [--new] [--new-key]
```

SSH 인증서 생성

```
This command checks if a valid SSH certificate is present, and generates a
new one if necessary.

Certificates allow you to make SSH connections without having previously
uploaded a public key. They are more secure than keys and they allow for
other features.

Normally the certificate is loaded automatically during login, or when
making an SSH connection. So this command is seldom needed.

If you want to set up certificates without login and without an SSH-related
command, for example if you are writing a script that uses an API token via
an environment variable, then you would probably want to run this command
explicitly. For unattended scripts, remember to turn off interaction via
--yes or the MAGENTO_CLOUD_CLI_NO_INTERACTION environment variable.
```

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--refresh-only`

필요한 경우 인증서만 새로 고칩니다(SSH 구성을 작성하지 마십시오).

- 기본값: `false`
- 값을 수락하지 않음

#### `--new`

인증서를 강제로 새로 고침합니다.

- 기본값: `false`
- 값을 수락하지 않음

#### `--new-key`

[사용하지 않음] 대신 —new 사용

- 기본값: `false`
- 값을 수락하지 않음


## `ssh-key:add`

```bash
magento-cloud magento-cloud ssh-key:add [--name NAME] [--] [<path>]
```

새 SSH 키 추가

```
This command lets you add an SSH key to your account. It can generate a key using OpenSSH.

Notice:
SSH keys are no longer needed by default, as SSH certificates are supported.
Certificates offer more security than keys.

To load or check your SSH certificate, run: magento-cloud ssh-cert:load
```

### 인수

#### `path`

기존 SSH 공개 키의 경로

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--name`

키 식별을 위한 이름

- 값 필요


## `ssh-key:delete`

```bash
magento-cloud magento-cloud ssh-key:delete [<id>]
```

SSH 키 삭제

```
This command lets you delete SSH keys from your account.

Notice:
SSH keys are no longer needed by default, as SSH certificates are supported.
Certificates offer more security than keys.

To load or check your SSH certificate, run: magento-cloud ssh-cert:load
```

### 인수

#### `id`

삭제할 SSH 키의 ID

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.


## `ssh-key:list`

```bash
magento-cloud magento-cloud ssh-keys [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

계정의 SSH 키 목록 가져오기

```
This command lets you list SSH keys in your account.

Notice:
SSH keys are no longer needed by default, as SSH certificates are supported.
Certificates offer more security than keys.

To load or check your SSH certificate, run: magento-cloud ssh-cert:load
```

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--format`

출력 형식: table, csv, tsv 또는 plain

- 기본값: `table`
- 값 필요

#### `--columns`, `-c`

표시할 열입니다. 사용 가능한 열: id*, title*, path*, 지문(* = 기본 열). &quot;+&quot; 문자는 기본 열의 자리 표시자로 사용할 수 있습니다. % 또는 * 문자는 와일드카드로 사용될 수 있습니다. 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다.

- 기본값: `[]`
- 값 필요

#### `--no-header`

테이블 머리글 출력 안 함

- 기본값: `false`
- 값을 수락하지 않음


## `subscription:info`

```bash
magento-cloud magento-cloud subscription:info [-s|--id ID] [--date-fmt DATE-FMT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [--] [<property>] [<value>]
```

구독 속성 읽기 또는 수정

### 인수

#### `property`

속성 이름


#### `value`

속성에 대한 새 값 설정

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--id`, `-s`

구독 ID

- 값 필요

#### `--date-fmt`

날짜 형식(PHP 날짜 형식 문자열로서)

- 기본값: `c`
- 값 필요

#### `--format`

출력 형식: table, csv, tsv 또는 plain

- 기본값: `table`
- 값 필요

#### `--columns`, `-c`

표시할 열입니다. % 또는 * 문자는 와일드카드로 사용될 수 있습니다. 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다.

- 기본값: `[]`
- 값 필요

#### `--no-header`

테이블 머리글 출력 안 함

- 기본값: `false`
- 값을 수락하지 않음

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요


## `tunnel:close`

```bash
magento-cloud magento-cloud tunnel:close [-a|--all] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

SSH 터널 닫기

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--all`, `-a`

모든 터널 닫기

- 기본값: `false`
- 값을 수락하지 않음

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--app`, `-A`

원격 애플리케이션 이름

- 값 필요


## `tunnel:info`

```bash
magento-cloud magento-cloud tunnel:info [-P|--property PROPERTY] [-c|--encode] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

SSH 터널에 대한 관계 정보 보기

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--property`, `-P`

볼 관계 속성

- 값 필요

#### `--encode`, `-c`

base64 인코딩 JSON으로 출력

- 기본값: `false`
- 값을 수락하지 않음

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--app`, `-A`

원격 애플리케이션 이름

- 값 필요


## `tunnel:list`

```bash
magento-cloud magento-cloud tunnels [-a|--all] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

SSH 터널 나열

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--all`, `-a`

모든 터널 보기

- 기본값: `false`
- 값을 수락하지 않음

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--app`, `-A`

원격 애플리케이션 이름

- 값 필요

#### `--format`

출력 형식: table, csv, tsv 또는 plain

- 기본값: `table`
- 값 필요

#### `--columns`, `-c`

표시할 열입니다. 사용 가능한 열: 포트*, 프로젝트*, 환경*, 앱*, 관계*, url(* = 기본 열). &quot;+&quot; 문자는 기본 열의 자리 표시자로 사용할 수 있습니다. % 또는 * 문자는 와일드카드로 사용될 수 있습니다. 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다.

- 기본값: `[]`
- 값 필요

#### `--no-header`

테이블 머리글 출력 안 함

- 기본값: `false`
- 값을 수락하지 않음


## `tunnel:open`

```bash
magento-cloud magento-cloud tunnel:open [-g|--gateway-ports] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-i|--identity-file IDENTITY-FILE]
```

앱 관계에 대한 SSH 터널 열기

```
This command opens SSH tunnels to all of the relationships of an application.

Connections can then be made to the application's services as if they were
local, for example a local MySQL client can be used, or the Solr web
administration endpoint can be accessed through a local browser.

This command requires the posix and pcntl PHP extensions (as multiple
background CLI processes are created to keep the SSH tunnels open). The
tunnel:single command can be used on systems without these
extensions.
```

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--gateway-ports`, `-g`

원격 호스트가 로컬 전달 포트에 연결할 수 있도록 허용

- 기본값: `false`
- 값을 수락하지 않음

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--app`, `-A`

원격 애플리케이션 이름

- 값 필요

#### `--identity-file`, `-i`

사용할 SSH ID(개인 키)

- 값 필요


## `tunnel:single`

```bash
magento-cloud magento-cloud tunnel:single [--port PORT] [-g|--gateway-ports] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE]
```

앱 관계에 대한 단일 SSH 터널 열기

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--port`

로컬 포트

- 값 필요

#### `--gateway-ports`, `-g`

원격 호스트가 로컬 전달 포트에 연결할 수 있도록 허용

- 기본값: `false`
- 값을 수락하지 않음

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--app`, `-A`

원격 애플리케이션 이름

- 값 필요

#### `--relationship`, `-r`

사용할 서비스 관계

- 값 필요

#### `--identity-file`, `-i`

사용할 SSH ID(개인 키)

- 값 필요


## `user:add`

```bash
magento-cloud magento-cloud user:add [-r|--role ROLE] [--force-invite] [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] [<email>]
```

프로젝트에 사용자 추가

### 인수

#### `email`

사용자의 이메일 주소

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--role`, `-r`

사용자의 프로젝트 역할(&#39;admin&#39; 또는 &#39;viewer&#39;) 또는 환경 유형 역할(예: &#39;staging:contributor&#39; 또는 &#39;production:viewer&#39;). 환경 유형에서 사용자를 제거하려면 역할을 &#39;none&#39;으로 설정하십시오. % 또는 * 문자는 환경 유형에 대한 와일드카드로 사용할 수 있습니다(예: &#39;%:viewer&#39;). 사용자에게 모든 유형에서 &#39;viewer&#39; 역할을 부여할 수 있습니다. 역할은 축약될 수 있습니다(예: &#39;production:v&#39;).

- 기본값: `[]`
- 값 필요

#### `--force-invite`

초대를 이미 보냈더라도 보내기

- 기본값: `false`
- 값을 수락하지 않음

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--no-wait`, `-W`

작업이 완료될 때까지 기다리지 마십시오.

- 기본값: `false`
- 값을 수락하지 않음

#### `--wait`

작업이 완료될 때까지 대기(기본값)

- 기본값: `false`
- 값을 수락하지 않음


## `user:delete`

```bash
magento-cloud magento-cloud user:delete [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] <email>
```

프로젝트에서 사용자 삭제

### 인수

#### `email`

사용자의 이메일 주소

- 필수

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--no-wait`, `-W`

작업이 완료될 때까지 기다리지 마십시오.

- 기본값: `false`
- 값을 수락하지 않음

#### `--wait`

작업이 완료될 때까지 대기(기본값)

- 기본값: `false`
- 값을 수락하지 않음


## `user:get`

```bash
magento-cloud magento-cloud user:get [-l|--level LEVEL] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [-r|--role ROLE] [--] [<email>]
```

사용자의 역할 보기

### 인수

#### `email`

사용자의 이메일 주소

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--level`, `-l`

역할 수준(&#39;프로젝트&#39; 또는 &#39;환경&#39;)

- 값 필요

#### `--pipe`

역할을 stdout으로 출력(변경 후)

- 기본값: `false`
- 값을 수락하지 않음

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--no-wait`, `-W`

작업이 완료될 때까지 기다리지 마십시오.

- 기본값: `false`
- 값을 수락하지 않음

#### `--wait`

작업이 완료될 때까지 대기(기본값)

- 기본값: `false`
- 값을 수락하지 않음

#### `--role`, `-r`

[사용하지 않음: user:update를 사용하여 사용자의 역할을 변경합니다]

- 값 필요


## `user:list`

```bash
magento-cloud magento-cloud users [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT]
```

프로젝트 사용자 나열

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--format`

출력 형식: table, csv, tsv 또는 plain

- 기본값: `table`
- 값 필요

#### `--columns`, `-c`

표시할 열입니다. 사용 가능한 열: email*, name*, role*, id*, granted_at, updated_at (* = 기본 열). &quot;+&quot; 문자는 기본 열의 자리 표시자로 사용할 수 있습니다. % 또는 * 문자는 와일드카드로 사용될 수 있습니다. 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다.

- 기본값: `[]`
- 값 필요

#### `--no-header`

테이블 머리글 출력 안 함

- 기본값: `false`
- 값을 수락하지 않음

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요


## `user:update`

```bash
magento-cloud magento-cloud user:update [-r|--role ROLE] [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] [<email>]
```

프로젝트에서 사용자 역할 업데이트

### 인수

#### `email`

사용자의 이메일 주소

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--role`, `-r`

사용자의 프로젝트 역할(&#39;admin&#39; 또는 &#39;viewer&#39;) 또는 환경 유형 역할(예: &#39;staging:contributor&#39; 또는 &#39;production:viewer&#39;). 환경 유형에서 사용자를 제거하려면 역할을 &#39;none&#39;으로 설정하십시오. % 또는 * 문자는 환경 유형에 대한 와일드카드로 사용할 수 있습니다(예: &#39;%:viewer&#39;). 사용자에게 모든 유형에서 &#39;viewer&#39; 역할을 부여할 수 있습니다. 역할은 축약될 수 있습니다(예: &#39;production:v&#39;).

- 기본값: `[]`
- 값 필요

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--no-wait`, `-W`

작업이 완료될 때까지 기다리지 마십시오.

- 기본값: `false`
- 값을 수락하지 않음

#### `--wait`

작업이 완료될 때까지 대기(기본값)

- 기본값: `false`
- 값을 수락하지 않음


## `variable:create`

```bash
magento-cloud magento-cloud variable:create [-u|--update] [-l|--level LEVEL] [--name NAME] [--value VALUE] [--json JSON] [--sensitive SENSITIVE] [--prefix PREFIX] [--enabled ENABLED] [--inheritable INHERITABLE] [--visible-build VISIBLE-BUILD] [--visible-runtime VISIBLE-RUNTIME] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<name>]
```

변수 만들기

### 인수

#### `name`

변수 이름

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--update`, `-u`

변수가 이미 있는 경우 업데이트

- 기본값: `false`
- 값을 수락하지 않음

#### `--level`, `-l`

변수를 설정할 수준(&quot;프로젝트&quot; 또는 &quot;환경&quot;)

- 값 필요

#### `--name`

변수 이름

- 값 필요

#### `--value`

변수 값

- 값 필요

#### `--json`

변수 값이 JSON 형식인지 여부

- 기본값: `false`
- 값 필요

#### `--sensitive`

변수 값의 구분 여부

- 기본값: `false`
- 값 필요

#### `--prefix`

유형을 결정할 수 있는 변수 이름의 접두사입니다(예: &#39;env&#39;). 이름에 접두사가 없는 경우에만 적용할 수 있습니다. (예: &#39;none&#39; 또는 &#39;env:&#39;)

- 기본값: `none`
- 값 필요

#### `--enabled`

환경에서 변수를 활성화해야 하는지 여부

- 기본값: `true`
- 값 필요

#### `--inheritable`

변수가 하위 환경에 의해 상속될 수 있는지 여부

- 기본값: `true`
- 값 필요

#### `--visible-build`

빌드 시 변수를 표시할지 여부

- 값 필요

#### `--visible-runtime`

런타임 시 변수를 표시할지 여부

- 기본값: `true`
- 값 필요

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--no-wait`, `-W`

작업이 완료될 때까지 기다리지 마십시오.

- 기본값: `false`
- 값을 수락하지 않음

#### `--wait`

작업이 완료될 때까지 대기(기본값)

- 기본값: `false`
- 값을 수락하지 않음


## `variable:delete`

```bash
magento-cloud magento-cloud variable:delete [-l|--level LEVEL] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <name>
```

변수 삭제

### 인수

#### `name`

변수 이름

- 필수

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--level`, `-l`

변수 수준(&#39;project&#39;, &#39;environment&#39;, &#39;p&#39; 또는 &#39;e&#39;)

- 값 필요

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--no-wait`, `-W`

작업이 완료될 때까지 기다리지 마십시오.

- 기본값: `false`
- 값을 수락하지 않음

#### `--wait`

작업이 완료될 때까지 대기(기본값)

- 기본값: `false`
- 값을 수락하지 않음


## `variable:get`

```bash
magento-cloud magento-cloud vget [-P|--property PROPERTY] [-l|--level LEVEL] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--pipe] [--] [<name>]
```

변수 보기

### 인수

#### `name`

변수 이름

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--property`, `-P`

단일 변수 속성 보기

- 값 필요

#### `--level`, `-l`

변수 수준(&#39;project&#39;, &#39;environment&#39;, &#39;p&#39; 또는 &#39;e&#39;)

- 값 필요

#### `--format`

출력 형식: table, csv, tsv 또는 plain

- 기본값: `table`
- 값 필요

#### `--columns`, `-c`

표시할 열입니다. % 또는 * 문자는 와일드카드로 사용될 수 있습니다. 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다.

- 기본값: `[]`
- 값 필요

#### `--no-header`

테이블 머리글 출력 안 함

- 기본값: `false`
- 값을 수락하지 않음

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--pipe`

[사용되지 않는 옵션] 변수 값만 출력합니다.

- 기본값: `false`
- 값을 수락하지 않음


## `variable:list`

```bash
magento-cloud magento-cloud variables [-l|--level LEVEL] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

목록 변수

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--level`, `-l`

변수 수준(&#39;project&#39;, &#39;environment&#39;, &#39;p&#39; 또는 &#39;e&#39;)

- 값 필요

#### `--format`

출력 형식: table, csv, tsv 또는 plain

- 기본값: `table`
- 값 필요

#### `--columns`, `-c`

표시할 열입니다. 사용 가능한 열: is_enabled, level, name, value. % 또는 * 문자는 와일드카드로 사용될 수 있습니다. 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다.

- 기본값: `[]`
- 값 필요

#### `--no-header`

테이블 머리글 출력 안 함

- 기본값: `false`
- 값을 수락하지 않음

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요


## `variable:update`

```bash
magento-cloud magento-cloud variable:update [--allow-no-change] [-l|--level LEVEL] [--value VALUE] [--json JSON] [--sensitive SENSITIVE] [--enabled ENABLED] [--inheritable INHERITABLE] [--visible-build VISIBLE-BUILD] [--visible-runtime VISIBLE-RUNTIME] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <name>
```

변수 업데이트

### 인수

#### `name`

변수 이름

- 필수

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--allow-no-change`

변경 사항이 제공되지 않은 경우 성공(종료 코드 없음) 반환

- 기본값: `false`
- 값을 수락하지 않음

#### `--level`, `-l`

변수 수준(&#39;project&#39;, &#39;environment&#39;, &#39;p&#39; 또는 &#39;e&#39;)

- 값 필요

#### `--value`

변수 값

- 값 필요

#### `--json`

변수 값이 JSON 형식인지 여부

- 기본값: `false`
- 값 필요

#### `--sensitive`

변수 값의 구분 여부

- 기본값: `false`
- 값 필요

#### `--enabled`

환경에서 변수를 활성화해야 하는지 여부

- 기본값: `true`
- 값 필요

#### `--inheritable`

변수가 하위 환경에 의해 상속될 수 있는지 여부

- 기본값: `true`
- 값 필요

#### `--visible-build`

빌드 시 변수를 표시할지 여부

- 값 필요

#### `--visible-runtime`

런타임 시 변수를 표시할지 여부

- 기본값: `true`
- 값 필요

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--no-wait`, `-W`

작업이 완료될 때까지 기다리지 마십시오.

- 기본값: `false`
- 값을 수락하지 않음

#### `--wait`

작업이 완료될 때까지 대기(기본값)

- 기본값: `false`
- 값을 수락하지 않음


## `worker:list`

```bash
magento-cloud magento-cloud workers [--refresh] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

배포된 모든 작업자 목록 가져오기

### 옵션

전역 옵션에 대해서는 [전역 옵션](#global-options)을 참조하십시오.

#### `--refresh`

캐시 새로 고침 여부

- 기본값: `false`
- 값을 수락하지 않음

#### `--pipe`

작업자 이름 목록만 출력

- 기본값: `false`
- 값을 수락하지 않음

#### `--project`, `-p`

프로젝트 ID 또는 URL

- 값 필요

#### `--environment`, `-e`

환경 ID. &quot;.&quot; 사용 을 클릭하여 프로젝트의 기본 환경을 선택합니다.

- 값 필요

#### `--format`

출력 형식: table, csv, tsv 또는 plain

- 기본값: `table`
- 값 필요

#### `--columns`, `-c`

표시할 열입니다. 사용 가능한 열: 명령, 이름, 유형. % 또는 * 문자는 와일드카드로 사용될 수 있습니다. 값들은 쉼표(예를 들어, &quot;a,b,c&quot;) 및/또는 공백으로 분할될 수 있다.

- 기본값: `[]`
- 값 필요

#### `--no-header`

테이블 머리글 출력 안 함

- 기본값: `false`
- 값을 수락하지 않음
