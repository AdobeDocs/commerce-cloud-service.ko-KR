---
title: Fastly 이미지 최적화
description: Fastly 이미지 최적화를 활성화하고 구성하여 Adobe Commerce 사이트에 대한 이미지 제공을 최적화하고 이미지 관리를 간소화하는 방법에 대해 알아봅니다.
feature: Cloud, Configuration, Media
exl-id: 20f5c5c3-7c43-493b-b651-82b023cf5db5
source-git-commit: 7a181af2149eef7bfaed4dd4d256b8fa19ae1dda
workflow-type: tm+mt
source-wordcount: '1275'
ht-degree: 0%

---

# Fastly 이미지 최적화

Fastly 이미지 최적화(Fastly IO)는 실시간 이미지 조작 및 최적화를 제공하여 이미지 전달 속도를 높이고 응답형 웹 애플리케이션의 이미지 소스 세트 유지 관리를 간소화합니다. 구성된 Fastly IO는 다음과 같은 이미지 최적화 기능을 제공합니다.

- 강제 손실 변환
- 심층 이미지 최적화
- 적응 픽셀 비율
- PNG, JPEG, GIF 및 WebP와 같은 일반적인 이미지 형식 지원

Fastly IO 옵션을 활성화하고 구성하기 전에 Fastly 서비스를 설정하고 원본 차폐를 구성해야 합니다.

구성 설정에 따라 Fastly Image Optimization(Fastly IO) 스니펫은 VCL 코드를 삽입하여 상점 내 제품 이미지 제공 속도를 향상시키는 이미지 최적화를 수행합니다. Fastly IO를 구성하는 세 단계는 활성화, 구성 및 확인입니다.

## Fastly IO 활성화

Fastly IO VCL 코드 조각을 업로드하여 관리 패널에서 Fastly 이미지 최적화(Fastly IO)를 활성화합니다. 이 코드 조각은 기본 구성을 사용하여 이미지 최적기를 통해 모든 이미지를 처리하는 Fastly 구성 지침을 제공합니다.

**필수 구성 요소:**

- Fastly 모듈 버전 1.2.62 이상으로 설치 또는 업그레이드
- [Fastly Origin 실드 및 백엔드 구성](fastly-custom-cache-configuration.md#configure-back-ends-and-origin-shielding)

**빠른 IO를 사용하려면**:

1. 로컬 [관리자](../../get-started/onboarding.md#access-your-admin-panel) 패널에 관리자로 로그인합니다.

1. **스토어** > **설정** > **구성** > **고급** > **시스템**&#x200B;을 선택합니다.

1. 오른쪽 창에서 **전체 페이지 캐시**&#x200B;를 확장합니다.

1. 구성 설정을 지정하려면 **빠른 구성** > **이미지 최적화**&#x200B;를 선택하십시오.

1. _Fastly IO 코드 조각_ 필드에서 **사용/사용 안 함**&#x200B;을 선택합니다.

1. Fastly IO 코드 조각을 업로드합니다.

   - **기본 IO 구성 옵션**&#x200B;을 선택하여 이미지 최적화 기본 구성 옵션 페이지를 엽니다.
   - VCL 코드 조각을 서버에 업로드하려면 **업로드**&#x200B;를 선택하십시오.

## Fastly IO 구성

필요에 따라 이미지 최적화를 위한 기본 IO 구성 설정을 검토하고 업데이트합니다. 예를 들어 손실 형식에 대한 WebP 및 JPEG 품질 수준을 변경하거나 JPEG 이미지 제공 형식을 _점진적_ 또는 _기준선_(으)로 변경할 수 있습니다. 또한 다음과 같이 보다 세분화된 이미지 최적화 기능을 위해 Fastly IO를 사용할 수 있습니다.

- 강제 손실 변환
- 심층 이미지 최적화
- 적응 픽셀 비율

**빠른 IO를 업데이트하려면**:

1. _기본 IO 구성 옵션_ 필드의 _빠른 구성_ 페이지에서 **구성**&#x200B;을 선택합니다.

   ![Fastly IO 구성 설정 보기](../../assets/cdn/fastly-io-default-config.png)

1. _이미지 최적화 기본 구성 옵션_ 페이지에서 Fastly IO 구성 설정을 검토하고 업데이트합니다.

   ![Fastly IO 구성 검토](../../assets/cdn/fastly-io-config-options.png)

   - **자동 웹 앱** - 기본 설정(`Yes`)을 그대로 두어 지원하는 브라우저에서 이미지를 WebP 형식으로 변환합니다. 설정을 **아니요**(으)로 변경하면 Fastly에서는 이미지를 WebP 형식으로 변환하는 대신 이미지 파일 형식을 사용합니다.

   - **기본 WebP(손실) 품질**—기본 설정(`85`)을 그대로 유지하거나 손실 파일 형식의 이미지에 대한 압축 수준을 입력합니다. 1에서 100 사이의 정수를 지정할 수 있습니다.

   - **기본 JPEG 형식 컨트롤** — 기본 설정(`Auto`)을 그대로 유지하거나 이미지를 제공할 때 사용할 JPEG 형식을 선택하십시오. 값이 _Auto_(으)로 설정된 경우 Fastly에서는 출력 형식이 입력 형식과 일치하는 이미지를 전달합니다. _기준선_&#x200B;을(를) 선택하여 왼쪽 상단에서 오른쪽 하단으로 가면서 한 줄씩 이미지를 표시합니다. _점진적_&#x200B;을(를) 선택하여 로드될 때 선명해지는 흐릿한 이미지를 표시합니다.

   - **기본 JPEG 품질**—기본 설정(`85`)을 그대로 유지하거나 손실 파일 형식 품질에 대한 압축 수준을 입력합니다. 1에서 100 사이의 정수를 지정합니다.

   - **업스케일링 허용?**—기본 설정(`No`)을 그대로 두거나, 요청된 차원에 맞게 원본 소스 파일보다 큰 이미지를 반환하려면 `Yes`을(를) 선택하십시오.

   - **필터 크기 조정**—기본 설정(`Lancsoz3`)을 그대로 유지하거나 다른 설정을 선택하세요. 이 설정은 크기 조정된 이미지를 전달하는 데 사용되는 필터를 지정합니다. 선택한 필터에 따라 크기 조정된 이미지가 더 많거나 더 적은 픽셀 수를 가질 수 있습니다.

      - `Lanczos3`(기본값) - 최상의 품질 이미지를 제공합니다. 이미지 내의 가장자리와 선형 기능을 감지하는 기능이 향상되고 _[!DNL sinc]_리샘플링을 사용하여 가능한 최상의 재구성을 제공합니다.
      - `Lanczos2` - `Lancsoz3`과(와) 동일한 필터를 사용하지만 _[!DNL sinc]_리샘플링 함수의 정확도는 낮습니다.
      - `Bicubic` - 이미지를 더 작게 만들 때 자연스럽게 선명하게 합니다.
      - `Bilinear` - 이미지를 더 크게 만들 때 자연스럽게 매끄럽게 하는 효과가 있습니다.
      - `Nearest` - 픽셀 아트 크기를 조정할 때 자연스러운 픽셀 조정 효과가 있습니다.

1. Fastly 서비스에 대한 IO 구성 설정을 지정한 후 **취소**&#x200B;를 선택하여 Fastly 구성 설정으로 돌아갑니다.

1. 이미지 최적화 구성 _딥 이미지 최적화 사용_ 필드에서 **예**&#x200B;를 선택하여 딥 이미지 최적화를 켭니다.

   ![Fastly IO 딥이미지 최적화 사용](../../assets/cdn/fastly-io-deep-image-config.png)

   딥 이미지 최적화는 기본적으로 꺼져 있습니다. 이 기능이 활성화되면 Adobe Commerce의 기본 제공 크기 조정 기능이 꺼지고 크기 조정 작업이 Fastly IO 서비스로 오프로드됩니다. 이미지 최적화는 제품 이미지에만 적용됩니다. CMS 이미지의 크기가 조정되지 않습니다. [Fastly 설명서](#deep-image-optimization)를 참조하세요.

1. 딥 이미지 최적화를 활성화한 후 [적응형 픽셀 비율](#adaptive-pixel-ratios) 기능을 활성화하여 반응형 웹 사이트에서 사용하도록 최적화된 이미지를 생성합니다.

   ![Fastly IO 응용 픽셀 비율 사용](../../assets/cdn/fastly-io-config-adaptive-pixel.png)

   - _응용 장치 픽셀 비율 사용_ 필드에서 **예**&#x200B;를 선택합니다.
   - _장치 픽셀 비율_ 필드에서 기본 설정을 사용하거나 **시스템 입력** 확인란을 선택하여 설정을 제거합니다. 그런 다음 원하는 비율을 선택합니다. [장치 픽셀 비율] 설정이 높을수록 더 큰 이미지를 제공합니다.

1. **구성 저장**&#x200B;을 선택합니다.

### 강제 손실 변환

기본적으로 Fastly IO 서비스는 PNG, BMP 또는 WEBP와 같은 무손실 형식을 JPEG/WEBP 형식으로 변환합니다.

손실 변환을 강제하는 이점은 더 작은 이미지가 제공된다는 것입니다.
예를 들어 PNG 대신 JPEG 또는 WEBp 형식을 사용하면 Fastly IO 구성에 지정된 품질 수준에 따라 크기가 60~70% 줄어들 수 있습니다.

이미지 최적화를 위해 선택한 품질 수준에 따라 이미지의 시각적 차이가 감지될 수 있습니다. 예를 들어 테마의 배경색을 사용하는 딥 이미지 최적화를 사용하지 않는 한 Alpha 채널/투명도는 제거되고 흰색 배경으로 대체됩니다.

손실 변환(`WebP Auto? = No`)을 해제하면 Fastly IO는 호환 브라우저에 대해 JPEG 이미지만 WEBP 형식으로 변경합니다. 다른 이미지 유형은 변경되지 않습니다. 예를 들어 원본 이미지가 PNG인 경우 Fastly IO 서비스의 출력은 PNG입니다.

### 심층 이미지 최적화

딥 이미지 최적화는 기본적으로 꺼져 있습니다. 이 옵션을 활성화하면 내장된 Adobe Commerce 크기 조정을 해제하고 Fastly IO 서비스로 완전히 오프로드합니다.
이 기능은 _제품_ 이미지만 조정합니다. CMS 이미지의 크기가 조정되지 않습니다.

딥 이미지 최적화를 활성화하면 테마에 정의된 대로 모든 이미지에 배경색 정의가 추가됩니다. 그 결과, WebP 이미지들은 WebP 무손실로부터 WebP 손실로 전환된다. 손실 없는 이미지와 손실 있는 이미지 간의 주요 차이점 중 하나는 손실 이 훨씬 작은 이미지를 제공하는 PNG 이미지에서 알파 채널을 떨어뜨린다는 것입니다. 하지만 투명도가 있는 이미지는 다른 배경을 사용하는 제품 및 캠페인 페이지에서 이상하게 보일 수 있습니다.

예를 들어 다음 코드는 Luma 테마의 이미지에 대한 원본 소스를 나타냅니다.

```html
<img class="product-image-photo"
     src="https://mymagentosite/pub/media/catalog/product/cache/f073062f50e48eb0f0998593e568d857/m/b/mb02-gray-0.jpg"
     width="240"
     height="300"
     alt="Fusion Backpack"/>
```

Fastly IO Deep 이미지 최적화 기능이 활성화되면 다음 예와 같이 이미지에 대한 원본 소스 코드가 다시 작성됩니다.

```html
<img class="product-image-photo"
     src="https://mymagentosite/pub/media/catalog/product/m/b/mb02-gray-0.jpg?width=240&height=300&quality=80&bg-color=255,255,255&fit=bounds"
     width="240"
     height="300"
     alt="Fusion Backpack"/>
```

### 적응 픽셀 비율

적응형 픽셀 비율 기능은 점진적 웹 애플리케이션용 이미지를 최적화하는 데 유용합니다. 각 제품 이미지에 대해 `srcset`을(를) 추가하여 하나의 이미지 소스 파일에서 여러 이미지 크기와 해상도를 제공할 수 있습니다.

적응형 픽셀 비율 기능이 활성화되면 Fastly IO 서비스는 다양한 `device-pixel-ratios`에 적응할 수 있는 고정 너비 이미지를 제공합니다.
예를 들어 이 서비스는 다음 예와 같이 제품 이미지 정의를 수정합니다.

```html
<img class="product-image-photo"
     srcset="https://mymagentosite/pub/media/catalog/product/m/b/mb02-gray-0.jpg?width=240&height=300&quality=80&bg-color=255,255,255&fit=bounds&dpr=2 2x,
  https://mymagentosite/pub/media/catalog/product/m/b/mb02-gray-0.jpg?width=240&height=300&quality=80&bg-color=255,255,255&fit=bounds&dpr=3 3x"
     src="https://mymagentosite/pub/media/catalog/product/m/b/mb02-gray-0.jpg?width=240&height=300&quality=80&bg-color=255,255,255&fit=bounds"
     width="240"
     height="300"
     alt="Fusion Backpack"/>
```

`srcset` [브라우저 지원](https://caniuse.com/#feat=srcset) 및 [사양](https://html.spec.whatwg.org/multipage/embedded-content.html#attr-img-srcset)을 참조하세요.

## Fastly IO 유효성 검사

Fastly IO를 활성화하고 구성한 후 Fastly IO가 활성화된 상태에서 또는 활성화되지 않은 상태에서 웹 페이지 속도 테스트를 수행하여 구성을 확인합니다. 또한 스토어의 이미지를 검토하여 이미지 크기와 모양에서 문제가 있는지 확인하십시오.
