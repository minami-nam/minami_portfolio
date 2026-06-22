# Computer Vision

본 문서는 Computer Vision 및 Image Processing 관련 프로젝트를 정리하기 위한 문서입니다.
주요 관심사는 **Image Enhancement**, **CNN 기반 영상 처리**, **Low Light Enhancement**, **Model 경량화** 입니다.

---

## Overview

Computer Vision 분야에서는 이미지 품질 개선과 CNN 연산 최적화를 중심으로 프로젝트를 진행하고 있습니다.

현재 정리 대상 프로젝트는 다음과 같습니다.

| Project                                                   | Description                                         | Main Topics                                                    | Status              |
| --------------------------------------------------------- | --------------------------------------------------- | -------------------------------------------------------------- | ------------------- |
| Image Enhancement based on CNN using Gaussian Filter      | Gaussian Filter와 CNN을 활용한 이미지 개선 실험                 | Image Enhancement, CNN, Gaussian Filtering                     | Completed Prototype |
| Flat Region Convolution Reusing System in High Resolution | 고해상도 이미지의 평탄 영역에서 convolution 결과를 재사용하여 연산량을 줄이는 구조 | CNN Acceleration, Convolution Reuse, PSNR, SSIM, MAC Reduction | Research / Planning |

---

# 1. Image Enhancement based on CNN using Gaussian Filter

## Project Summary

**Image Enhancement based on CNN using Gaussian Filter** 프로젝트는 Gaussian Filter 기반 전처리와 CNN을 이용하여 이미지 품질을 개선하는 것을 목표로 진행한 프로젝트입니다.

이미지의 noise 또는 local variation을 완화하기 위해 Gaussian Filter를 적용하고, 이후 CNN 기반 모델을 통해 enhancement 결과를 생성하는 구조를 실험했습니다. 이 프로젝트는 Computer Vision 분야에서 CNN 기반 image-to-image processing을 학습하고, 이미지 품질 평가 지표를 다루기 위한 기반 프로젝트로 활용할 수 있습니다.

---

## Motivation

전통적인 이미지 개선 작업에서는 단순한 filter 기반 처리만으로는 복잡한 조명 변화, contrast 저하, local detail 손실을 충분히 보정하기 어렵고,       
Low Light 환경에서는 특히 Noise로 인하여 다른 환경 대비 자연스러운 밝기 조정이 어렵습니다.         
CNN 및 Gaussian Filter를 이용하면, 입력 이미지의 spatial feature를 학습하여 보다 유연한 enhancement 결과를 만들 수 있습니다.

이 프로젝트에서는 Gaussian Filter를 활용한 전처리와 CNN 기반 보정 구조를 결합하여 다음을 확인하고자 했습니다.

* Gaussian Filter가 CNN 입력에 미치는 영향
* CNN 기반 image enhancement 구조의 기본 동작
* 이미지 품질 평가를 위한 PSNR / SSIM 지표 활용 가능성
* 향후 hardware-aware CNN acceleration 프로젝트로 확장 가능한 구조

---

## Main Idea

전체 처리 흐름은 다음과 같이 정리할 수 있습니다.

```text
Input Image
    ↓
Gaussian Filter-based Preprocessing
    ↓
CNN-based Image Enhancement Model
    ↓
Enhanced Output Image
    ↓
Quality Evaluation
```

Gaussian Filter는 입력 이미지의 local noise를 완화하고, CNN은 전처리된 이미지로부터 개선된 출력을 생성합니다.

---


## Result Summary

현재 프로젝트는 prototype 수준으로 구현되었으며, 주요 결과는 다음 항목을 중심으로 정리할 예정입니다.

| Evaluation Item     | Description                             |
| ------------------- | --------------------------------------- |
| Visual Comparison   | 입력 이미지와 enhancement 결과 비교               |
| PSNR                | 원본 또는 target image 대비 pixel-level 품질 평가 |
| SSIM                | 구조적 유사도 기반 이미지 품질 평가                    |
| Inference Time      | 모델 추론 시간 측정                             |
| Limitation Analysis | blur, edge 손상, detail loss 여부 분석        |


---

# 2. Flat Region Convolution Reusing System in High Resolution

## Project Summary

**Flat Region Convolution Reusing System in High Resolution** 프로젝트는 고해상도 이미지에서 발생하는 CNN convolution 연산의 spatial redundancy를 줄이기 위한 연구형 프로젝트입니다.

고해상도 이미지에는 하늘, 벽, 도로, 배경과 같이 주변 pixel 또는 local patch 값이 매우 유사한 평탄 영역이 자주 등장합니다. 이러한 영역에서는 인접한 convolution patch가 유사하므로 convolution 결과 역시 유사할 가능성이 높다고 판단했습니다. 

본 프로젝트는 이러한 특성을 이용하여, 평탄 영역에서는 모든 위치에서 convolution을 수행하지 않고 일부 대표 위치의 convolution 결과를 재사용함으로써 MAC 연산량과 latency를 줄이는 것을 목표로 합니다.

---

## Motivation

일반적인 CNN convolution은 입력 feature map의 모든 위치에 대해 동일한 kernel 연산을 수행합니다.

```text
Every output position
→ Generate local patch
→ Apply convolution kernel
→ Store output feature
```

그러나 고해상도 이미지의 평탄 영역에서는 인접한 patch 간 차이가 작기 때문에, 모든 위치에서 convolution을 독립적으로 수행하는 것은 중복 연산을 유발할 수 있습니다.

이 프로젝트의 핵심 질문은 다음과 같습니다.

> 고해상도 이미지의 평탄 영역에서 일부 대표 위치의 convolution 결과를 재사용하면, PSNR과 SSIM 저하를 제한하면서 MAC 연산량과 latency를 줄일 수 있는가?

---

## Main Hypothesis

고해상도 이미지의 평탄 영역에서는 인접 local patch 간 차이가 작습니다.

```text
P(i, j) ≈ P(i+1, j)
P(i, j) ≈ P(i, j+1)
```

동일한 convolution kernel을 적용하면 출력 feature도 유사해질 가능성이 높습니다.

```text
Conv(P(i, j), W) ≈ Conv(P(i+1, j), W)
```

따라서 평탄 영역에서는 모든 위치에서 convolution을 수행하는 대신, 대표 위치에서 계산한 convolution 결과를 주변 위치에 재사용할 수 있습니다.

---

## Proposed Method

제안하는 기본 구조는 다음과 같습니다.

```text
High-Resolution Input Image / Feature Map
    ↓
Block-wise Flatness Detection
    ↓
Flat Block / Non-flat Block Classification
    ↓
Flat Block:
    Representative Convolution + Result Reuse
    ↓
Non-flat Block:
    Full Convolution
    ↓
Output Feature Map
    ↓
PSNR / SSIM / MAC Reduction / Latency Evaluation
```

---

## Flatness Detection

입력 이미지 또는 feature map을 block 단위로 나누고, 각 block의 평탄도를 계산합니다.

예상 후보는 다음과 같습니다.

| Method                  | Description                    | Hardware Friendliness |
| ----------------------- | ------------------------------ | --------------------- |
| Local Variance          | block 내부 pixel 분산 계산           | Medium                |
| Gradient Magnitude      | Sobel filter 등을 이용해 edge 강도 계산 | Medium                |
| Adjacent Difference Sum | 인접 pixel 간 절댓값 차이의 합 계산        | High                  |
| Patch Similarity        | local patch 간 L2 distance 계산   | Low ~ Medium          |

초기 구현에서는 hardware-friendly한 구조를 고려하여 **Adjacent Difference Sum** 기반 방식을 우선 검토합니다.

---

## Convolution Reuse Strategy

block이 충분히 평탄하다고 판단되면, block 내부 모든 위치에서 convolution을 수행하지 않고 대표 위치의 결과를 재사용합니다.

예를 들어 `4×4 block` 기준으로 다음 mode를 고려할 수 있습니다.

| Mode                         | Description              | Conv Positions | Expected MAC Reduction |
| ---------------------------- | ------------------------ | -------------: | ---------------------: |
| Full Conv                    | 모든 위치에서 convolution 수행   |        16 / 16 |                     0% |
| 2×2 Representative Reuse     | 대표 위치 4개만 convolution 수행 |         4 / 16 |                    75% |
| 1-point Representative Reuse | 대표 위치 1개만 convolution 수행 |         1 / 16 |                 93.75% |

실제 프로젝트에서는 품질 손상을 줄이기 위해 다음과 같은 adaptive mode를 고려합니다.

```text
Highly flat block      → 1-point reuse
Moderately flat block  → 2×2 representative reuse
Edge / texture block   → full convolution
```

---

## Evaluation Plan

본 프로젝트는 image enhancement 또는 denoising task를 기준으로 평가할 예정입니다.

| Metric              | Purpose                               |
| ------------------- | ------------------------------------- |
| PSNR                | pixel-level reconstruction quality 평가 |
| SSIM                | structural similarity 평가              |
| MAC Reduction Ratio | convolution 연산량 감소율 평가                |
| Skip Ratio          | convolution을 생략한 위치 비율 평가             |
| Latency             | 실제 추론 시간 또는 cycle-level latency 평가    |
| Visual Comparison   | edge 손상, blur, block artifact 확인      |

---


## Expected Contribution

이 프로젝트의 예상 contribution은 다음과 같습니다.

1. 고해상도 이미지의 평탄 영역에서 발생하는 convolution 연산 중복성 분석
2. block-wise flatness detection 기반 convolution result reuse 방법 제안
3. PSNR / SSIM / MAC reduction / latency 기반 품질-성능 trade-off 분석
4. 향후 CNN accelerator 또는 NPU 구조로 확장 가능한 hardware-aware processing 방식 제안


---

## Current Status

현재 단계는 research planning 및 algorithm design 단계입니다.

진행 예정 항목은 다음과 같습니다.

* baseline CNN 선정
* image enhancement dataset 또는 sample image 구성
* flatness detector prototype 구현
* convolution reuse simulation
* PSNR / SSIM evaluation pipeline 구축
* MAC reduction 계산식 정리
* latency 측정 또는 cycle-level estimation

---

# Summary

Computer Vision 프로젝트는 현재 두 방향으로 구성됩니다.

| Direction                     | Description                                                 |
| ----------------------------- | ----------------------------------------------------------- |
| Image Enhancement CNN         | CNN 기반 이미지 개선 및 품질 평가 실험                                    |
| Flat Region Convolution Reuse | 고해상도 이미지에서 convolution 중복 연산을 줄이기 위한 research-oriented 프로젝트 |

첫 번째 프로젝트는 CNN 기반 image enhancement의 기초 실험으로 정리하고, 두 번째 프로젝트는 향후 논문 또는 hardware-aware CNN acceleration 주제로 확장하는 것을 목표로 합니다.
