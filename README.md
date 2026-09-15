# AI Mask Detection Project

이 프로젝트는 Stable Diffusion으로 합성 이미지를 생성하고, RetinaFace로 얼굴 위치를 검출한 뒤, YOLOv8 모델을 학습시켜 마스크 착용 여부를 탐지하는 파이프라인입니다.

## Overview

- 얼굴 탐지: RetinaFace
- 데이터 생성: Stable Diffusion
- 데이터 라벨링: RetinaFace 결과를 정제하여 YOLO 형식으로 변환
- 학습/추론: YOLOv8 기반 마스크 여부 검출

목표는 단순한 얼굴 검출이 아니라, 마스크를 쓴 얼굴과 안 쓴 얼굴을 구분할 수 있는 모델을 학습하고 추론하는 것입니다.

---

## Features

- 합성 이미지 기반 데이터셋 생성
- 얼굴 바운딩 박스 추출
- 라벨 데이터 생성 및 정리
- YOLO 학습 및 검증
- 추론 결과 시각화 및 저장

---

## Repository Structure

```text
ai_project/
├── README.md
├── RetinaFace.ipynb
├── labels.ipynb
├── YOLO.ipynb
├── detect_mask.ipynb
├── mask_val_predict.ipynb
├── stableDiffusion.ipynb
├── yolov8m.pt
├── labels/
├── mask_images/
├── masked_man/
├── mask_yolo_dataset/
├── Pytorch_Retinaface/
├── results/
├── runs/
├── .vscode/
├── .ipynb_checkpoints/
└── .git/
```

---

## Workflow

1. `stableDiffusion.ipynb` 실행
   - Hugging Face 토큰을 사용해 이미지 생성
   - 마스크 착용/비착용 인물 이미지를 생성

2. `RetinaFace.ipynb` 실행
   - 생성된 이미지에서 얼굴 좌표를 탐지
   - bounding box 정보를 추출

3. `labels.ipynb` 실행
   - RetinaFace 결과를 YOLO 학습용 라벨 형식으로 정리

4. `YOLO.ipynb` 실행
   - `mask_yolo_dataset/`을 학습 데이터로 사용
   - `yolov8m.pt`를 초기 가중치로 활용
   - 최종 모델은 `best.pt`로 저장

5. `detect_mask.ipynb` 또는 `mask_val_predict.ipynb` 실행
   - 새로운 이미지에 대해 마스크 착용 여부 추론

---

## Requirements

### Hardware
- Windows 10/11
- NVIDIA GPU required
- CUDA-enabled environment

### Software
- Python 3.10+
- CUDA 11.2 recommended
- cuDNN compatible with CUDA 11.x
- VS Build Tools / CMake if needed for library compilation

### Account
- Hugging Face account
- Access token for model download

---

## Environment Setup

```bash
conda activate yolo_env01
cd C:\ai_project01
jupyter notebook
```

---

## Dependencies

```bash
pip install numpy==1.26.4
pip install torch==2.4.0 torchvision==0.19.0 torchaudio==2.4.0 --index-url https://download.pytorch.org/whl/cu118
pip install -U xformers --index-url https://download.pytorch.org/whl/cu118
pip install --upgrade diffusers
pip install --upgrade transformers
pip install --upgrade accelerate
pip install --upgrade safetensors
pip install ultralytics
```

---

## Quick Start

```bash
git clone https://github.com/dnwls7738/ai_project.git
cd ai_project
```

1. CUDA 환경 준비
2. Hugging Face 토큰 발급
3. `conda activate yolo_env01` 실행
4. `stableDiffusion.ipynb` → `RetinaFace.ipynb` → `labels.ipynb` → `YOLO.ipynb` 순서로 실행
5. 추론 단계에서 `detect_mask.ipynb` 또는 `mask_val_predict.ipynb` 사용

---

## Notes

- 이미지 생성 품질은 프롬프트, 랜덤 시드, 배경, 자세, 조명 조건에 큰 영향을 받습니다.
- GPU 사양과 CUDA 환경에 따라 학습 속도와 성능이 달라질 수 있습니다.
- `huggingface_token`은 본인 계정 토큰으로 교체해 사용해야 합니다.
- 데이터 다양성을 높이기 위해 프롬프트와 생성 조건을 다양하게 실험하는 것을 권장합니다.

---

## Outputs

- `best.pt`: 훈련된 YOLO 모델 가중치
- `runs/`: 학습 로그 및 시각화 결과
- `results/`: 추론 결과 저장 폴더
- `labels/`: 얼굴 라벨 데이터
- `masked_man/`: Stable Diffusion 생성 이미지 저장 폴더