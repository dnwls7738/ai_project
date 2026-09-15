# AI Mask Detection Project

이 프로젝트는 Stable Diffusion으로 합성 이미지를 생성하고, RetinaFace로 얼굴 위치를 검출한 뒤, YOLOv8 모델로 마스크 착용 여부를 탐지하는 컴퓨터 비전 실험 프로젝트입니다.

![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?logo=python&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-2.4.0-EE4C2C?logo=pytorch&logoColor=white)
![YOLOv8](https://img.shields.io/badge/YOLOv8-Ultralytics-111F68)
![CUDA](https://img.shields.io/badge/CUDA-11.x-76B900?logo=nvidia&logoColor=white)

> Stable Diffusion, RetinaFace, YOLOv8을 연결한 마스크 착용 여부 탐지 파이프라인입니다.

## Contents

- [Overview](#overview)
- [Features](#features)
- [Visuals](#visuals)
- [Repository Structure](#repository-structure)
- [Workflow](#workflow)
- [Installation](#installation)
- [Usage](#usage)
- [Outputs](#outputs)
- [Project Status](#project-status)
- [Contributing](#contributing)
- [Support](#support)
- [License](#license)

## Overview

프로젝트의 목표는 마스크를 쓴 얼굴과 안 쓴 얼굴을 구분할 수 있는 데이터셋과 탐지 모델을 만드는 것입니다.

- 데이터 생성: Stable Diffusion
- 얼굴 탐지: RetinaFace
- 데이터 라벨링: RetinaFace 결과를 YOLO 형식으로 변환
- 학습 및 추론: YOLOv8

## Features

- 합성 이미지 기반 데이터셋 생성
- 얼굴 bounding box 추출
- YOLO 학습용 라벨 생성 및 정리
- YOLO 모델 학습 및 검증
- 마스크 착용 여부 추론
- 학습 및 추론 결과 저장

## Visuals

학습 곡선, 생성 이미지, 검출 결과를 GitHub에서 보여주려면 결과 이미지를 `docs/` 폴더에 추가한 뒤 README에 연결할 수 있습니다.

```text
docs/
├── training-results.png
├── detection-example.png
└── generated-samples.png
```

예시:

```markdown
![검출 결과](docs/detection-example.png)
```

현재 저장소에 별도의 `docs/` 이미지가 없다면 결과 이미지 추가 후 이 섹션을 활성화하면 됩니다.

## Repository Structure

```text
ai_project/
├── README.md
├── RetinaFace.ipynb              # 얼굴 검출
├── labels.ipynb                  # 라벨 생성 및 정리
├── YOLO.ipynb                    # YOLO 학습 및 검증
├── detect_mask.ipynb             # 마스크 감지 추론
├── mask_val_predict.ipynb        # 검증/예측 결과 확인
├── stableDiffusion.ipynb         # 합성 이미지 생성
├── yolov8m.pt                    # YOLOv8 초기 가중치
├── labels/                       # 얼굴 라벨 데이터
├── mask_images/                  # 생성/가공 이미지
├── masked_man/                   # 생성된 인물 이미지
├── mask_yolo_dataset/            # YOLO 학습용 데이터셋
├── Pytorch_Retinaface/           # RetinaFace 지원 코드
├── results/                      # 추론 결과
├── runs/                         # 학습 및 예측 결과
├── .vscode/                      # VS Code 설정
└── .ipynb_checkpoints/           # Jupyter 체크포인트
```

## Workflow

1. `stableDiffusion.ipynb`에서 마스크 착용/비착용 인물 이미지를 생성합니다.
2. `RetinaFace.ipynb`에서 이미지 속 얼굴 위치와 bounding box를 검출합니다.
3. `labels.ipynb`에서 검출 결과를 YOLO 학습용 라벨로 변환합니다.
4. `YOLO.ipynb`에서 `mask_yolo_dataset/`을 사용해 YOLOv8을 학습하고 검증합니다.
5. `detect_mask.ipynb` 또는 `mask_val_predict.ipynb`에서 새 이미지의 마스크 착용 여부를 추론합니다.

## Installation

### Requirements

- Windows 10/11
- Python 3.10+
- NVIDIA GPU 및 CUDA 환경
- CUDA 11.x 호환 cuDNN
- Hugging Face 계정 및 Access Token
- 라이브러리 빌드가 필요한 경우 VS Build Tools와 CMake

### Clone the repository

```bash
git clone https://github.com/dnwls7738/ai_project.git
cd ai_project
```

### Prepare the environment

```bash
conda activate yolo_env01
cd C:\ai_project01
```

### Install dependencies

```bash
pip install numpy==1.26.4
pip install torch==2.4.0 torchvision==0.19.0 torchaudio==2.4.0 --index-url https://download.pytorch.org/whl/cu118
pip install -U xformers --index-url https://download.pytorch.org/whl/cu118
pip install --upgrade diffusers transformers accelerate safetensors
pip install ultralytics
```

### Configure Hugging Face

Stable Diffusion 모델을 내려받으려면 Hugging Face Access Token이 필요합니다. 토큰을 README나 Git 저장소에 직접 기록하지 말고, 로컬 환경 변수 또는 안전한 비밀 저장소를 사용하세요.

## Usage

Jupyter Notebook을 실행합니다.

```bash
jupyter notebook
```

권장 실행 순서는 다음과 같습니다.

1. `stableDiffusion.ipynb`: 합성 이미지 생성
2. `RetinaFace.ipynb`: 얼굴 위치 검출
3. `labels.ipynb`: YOLO 라벨 생성
4. `YOLO.ipynb`: 모델 학습 및 검증
5. `detect_mask.ipynb` 또는 `mask_val_predict.ipynb`: 추론

학습된 모델을 사용하는 기본 예시는 다음과 같습니다.

```python
from ultralytics import YOLO

model = YOLO("best.pt")
results = model("path/to/image.jpg")
results[0].show()
```

> 실제 클래스 이름과 데이터셋 설정은 `YOLO.ipynb`의 학습 설정을 기준으로 확인하세요.

## Outputs

- `best.pt`: 학습된 YOLO 모델 가중치
- `runs/`: 학습 로그와 예측 결과
- `results/`: 추론 결과
- `labels/`: 얼굴 라벨 데이터
- `masked_man/`: Stable Diffusion 생성 이미지

## Notes

- 이미지 생성 품질은 프롬프트, 랜덤 시드, 배경, 자세, 조명 조건에 영향을 받습니다.
- 모델 성능은 데이터 다양성, 라벨 품질, GPU 및 CUDA 환경에 따라 달라질 수 있습니다.
- 데이터 다양성을 높이려면 의상, 자세, 인종, 배경, 조명 조건을 다양하게 설정하는 것이 좋습니다.

## Project Status

현재는 합성 데이터 생성, 얼굴 검출, 라벨링, YOLO 학습 및 추론을 검증하는 실험 단계입니다. 정량적인 성능 지표는 실험별로 별도 기록할 예정입니다.

## Contributing

개선 아이디어와 버그 수정 제안을 환영합니다.

1. 저장소를 Fork합니다.
2. 변경 사항을 별도 브랜치에서 작업합니다.
3. 실행 방법과 테스트 결과를 Pull Request에 작성합니다.

프롬프트, 데이터셋 또는 모델을 변경한 경우 변경 전후 결과를 함께 남겨 주세요.

## Support

문제나 개선 제안은 [GitHub Issues](https://github.com/dnwls7738/ai_project/issues)에 등록해 주세요. 운영체제, Python/CUDA 버전, 실행한 노트북, 오류 메시지를 함께 작성하면 문제를 재현하는 데 도움이 됩니다.

## License

현재 저장소에 별도의 라이선스 파일이 없으므로 사용, 수정, 배포 조건이 명시되어 있지 않습니다. 오픈소스로 배포할 경우 프로젝트 목적에 맞는 라이선스를 선택하고 `LICENSE` 파일을 추가하세요.
