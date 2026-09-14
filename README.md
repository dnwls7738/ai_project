# ai_project

RetinaFace를 이용한 얼굴 검출과 Stable Diffusion 인페인팅을 결합하여, 얼굴에 마스크를 합성한 이미지를 생성하는 프로젝트입니다.

> ⚠️ 이 저장소에는 별도의 프로젝트 설명이 없어, 노트북 파일명과 폴더 구조를 바탕으로 목적을 추정해 작성했습니다. 실제 의도와 다른 부분이 있다면 내용을 수정해 주세요.

## 프로젝트 소개

이 프로젝트는 다음과 같은 파이프라인으로 구성되어 있는 것으로 보입니다.

1. **얼굴 검출**: `RetinaFace.ipynb`에서 RetinaFace 모델로 이미지 속 얼굴 위치(및 랜드마크)를 검출합니다.
2. **라벨링**: `labels.ipynb`에서 검출된 얼굴에 대한 라벨(바운딩 박스 등) 데이터를 생성/가공하여 `labels/` 폴더에 저장합니다.
3. **마스크 이미지 합성**: `stableDiffusion.ipynb`에서 Stable Diffusion(인페인팅)을 활용해 검출된 얼굴 영역에 마스크를 합성하고, 결과 이미지를 `mask_images/`, `masked_man/` 폴더에 저장합니다.

## 폴더 구조

```
ai_project/
├── RetinaFace.ipynb        # 얼굴 검출(RetinaFace) 노트북
├── labels.ipynb            # 라벨 생성/가공 노트북
├── stableDiffusion.ipynb   # Stable Diffusion 기반 마스크 합성 노트북
├── labels/                 # 얼굴 검출 결과 라벨 데이터
├── mask_images/            # 합성에 사용되는 마스크 이미지
├── masked_man/             # 마스크 합성이 완료된 결과 이미지
├── .vscode/                # VS Code 설정
└── .ipynb_checkpoints/     # 주피터 노트북 자동 저장 체크포인트
```

## 요구사항

- Python 3.x
- Jupyter Notebook / JupyterLab
- PyTorch (RetinaFace, Stable Diffusion 모델 실행용)
- RetinaFace 관련 라이브러리 (예: `retina-face` 또는 자체 구현 모델)
- Diffusers / Stable Diffusion 관련 라이브러리 (예: `diffusers`, `transformers`)
- OpenCV, NumPy, Pillow 등 이미지 처리 라이브러리

> 실제 사용 라이브러리와 버전은 각 노트북 상단의 import 구문을 참고해 `requirements.txt`로 정리하는 것을 권장합니다.

## 사용 방법

1. 저장소를 클론합니다.
   ```bash
   git clone https://github.com/dnwls7738/ai_project.git
   cd ai_project
   ```
2. 필요한 패키지를 설치합니다.
   ```bash
   pip install -r requirements.txt
   ```
3. 노트북을 순서대로 실행합니다.
   1. `RetinaFace.ipynb` — 원본 이미지에서 얼굴 검출
   2. `labels.ipynb` — 검출 결과를 라벨 데이터로 정리
   3. `stableDiffusion.ipynb` — 검출된 얼굴에 마스크 합성 및 결과 저장

## 참고 사항

- `mask_images/`는 합성용 마스크 소스 이미지, `masked_man/`은 합성이 완료된 결과물로 추정됩니다.
- 프로젝트의 정확한 목적(예: 마스크 착용 데이터셋 생성, 얼굴 익명화 등)에 맞게 소개 문구를 보완해 주세요.

## 라이선스

별도로 명시된 라이선스가 없습니다. 필요 시 라이선스를 추가해 주세요.
