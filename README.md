# ai_project

Stable Diffusion를 이용하여 학습용 이미지를 생성후 RetinaFace를 이용한 얼굴 검출과 YOLO를 통해 마스크쓴 얼굴과 안 쓴 얼굴을 구별하는 프로젝트입니다.

## 프로젝트 소개

1. **얼굴 영역 검출**: `RetinaFace.ipynb`에서 RetinaFace 모델로 이미지 속 얼굴 위치를 검출하는 테스트입니다.
2. **라벨링**: `RetinaFace.ipynb`를 통해서 `labels.ipynb`에서 검출된 얼굴에 대한 라벨(바운딩 박스 등) 데이터를 생성/가공하여 `labels/` 폴더에 저장합니다.
3. **마스크 이미지 합성**: `stableDiffusion.ipynb`에서 Stable Diffusion(인페인팅)을 활용해 마스크를 쓴 얼굴과 안쓴 얼굴의 이미지를 생성하고 결과 이미지를 `mask_images/`, `masked_man/` 폴더에 저장합니다.

## 폴더 구조

```
ai_project/
├── RetinaFace.ipynb        # 얼굴 검출(RetinaFace) 노트북
├── labels.ipynb            # 라벨 생성/가공 노트북
├── stableDiffusion.ipynb   # Stable Diffusion 기반 마스크 이미지 생성 노트북
├── labels/                 # 얼굴 검출 결과 라벨 데이터
├── mask_images/            # 라벨링/검출 과정에 사용되는 이미지
├── masked_man/             # Stable Diffusion으로 생성된 마스크 착용 인물 이미지
├── .vscode/                # VS Code 설정
└── .ipynb_checkpoints/     # 주피터 노트북 자동 저장 체크포인트
```

## 요구사항

### 하드웨어 / OS
- Windows 10/11
- NVIDIA GPU (CUDA 연산 필수)
- 작업 디렉토리: `C:\ai_project01` (해당 경로 아래 `masked_man` 폴더가 생성됨)

### GPU 드라이버 & CUDA 스택
| 항목 | 버전 |
|---|---|
| GPU 드라이버 확인 | `nvidia-smi` 명령으로 확인 (CUDA 11.2가 아니면 기존 드라이버 삭제 후 재설치) |
| CUDA Toolkit | 11.2 |
| cuDNN | v8.1.0 (for CUDA 11.0, 11.1, 11.2) |
| VS Build Tools | Microsoft C++ Build Tools (xFormers가 CUDA/C++ 코드를 컴파일할 때 MSVC 컴파일러 필요) |
| CMake | xFormers 빌드 시 필요 |

### 계정
- Hugging Face 회원가입 및 Access Token 발급 (Read 권한, 모델 다운로드 인증용)

### Python 개발 환경
```bash
conda activate yolo_env01
cd c:\ai_project01
jupyter notebook
```
※ 기존 `yolo_env01` 가상환경을 재사용합니다 (RetinaFace/YOLO 계열 노트북과 환경 공유).

### 주요 Python 패키지
```bash
pip install numpy==1.26.4
pip install torch==2.4.0 torchvision==0.19.0 torchaudio==2.4.0 --index-url https://download.pytorch.org/whl/cu118
pip install -U xformers --index-url https://download.pytorch.org/whl/cu118   # xformers 0.0.27.post2+cu118
pip install --upgrade diffusers
pip install --upgrade transformers
pip install --upgrade accelerate
pip install --upgrade safetensors
```

## 사용 방법

1. 저장소를 클론합니다.
   ```bash
   git clone https://github.com/dnwls7738/ai_project.git
   cd ai_project
   ```
2. 위 "요구사항"에 따라 CUDA/cuDNN/VS Build Tools/CMake 등 환경을 구성하고, Hugging Face 토큰을 발급받습니다.
3. `conda activate yolo_env01` 후 필요한 패키지를 설치합니다.
4. 노트북을 순서대로 실행합니다.
   1. `stableDiffusion.ipynb` — 마스크 착용 인물 이미지 생성 → `masked_man/`
      - `huggingface_token` 값을 본인의 토큰으로 교체해야 합니다.
      - 모델: `Lykon/dreamshaper-8` (`StableDiffusionPipeline`, `torch_dtype=torch.float16`, `.to("cuda")`)
      - 생성 옵션: `num_images=20`, `batch_size=8`, `num_inference_steps=15`, `guidance_scale=7.0`, `height=width=640`
   2. `RetinaFace.ipynb` — 생성된 이미지에서 얼굴 검출
   3. `labels.ipynb` — 검출 결과를 라벨 데이터로 정리

## 참고 사항

- 현재 스크립트는 비슷한 구도/의상의 이미지만 생성되므로 **다양한 의상·자세·인종의 이미지가 생성되도록 프롬프트/스크립트 수정이 필요**하다고 명시되어 있습니다.
- `huggingface_token`, CUDA 버전 등은 실습 환경(강의 기준 CUDA 11.2, PyTorch 2.4.0+cu118)에 맞춘 값이며, 실제 사용 환경(GPU, 드라이버 버전)에 따라 조정이 필요할 수 있습니다.

