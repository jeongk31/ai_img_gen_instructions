# 💎 보석 이미지 생성기 & 미세 조정기 - 사용 설명서

## 소개

환영합니다! 이 도구를 통해 다음을 할 수 있습니다:

1.  **보석 이미지를 생성** – 사전 훈련된 AI 모델을 사용하여 독창적인 보석 이미지를 생성합니다.
2.  **모델 미세 조정 (Fine-tuning)** – 사용자의 특정 보석 이미지로 AI 모델을 LoRA 기술을 통해 미세 조정합니다.
3.  **클라이언트/스타일 관리** – 서로 다른 보석 스타일 또는 클라이언트 프로젝트를 별도 폴더로 관리합니다.

이 가이드는 애플리케이션을 설정하고 사용하는 방법을 단계별로 안내합니다.

## 사전 준비사항

1.  **컴퓨터:**
    *   비교적 최신의 Windows, macOS, 또는 Linux 컴퓨터.
    *   **미세 조정용:** 강력한 **NVIDIA GPU** (예: RTX 20xx, 30xx, 40xx 시리즈 또는 T4, A10G 같은 전문가용) 필요. **VRAM 최소 8~12GB 이상** 권장. CPU에서도 가능하나 매우 느립니다.
    *   **디스크 공간:** 최소 **20~30 GB** 여유 공간.
    *   **인터넷 연결:** 초기 설치 및 모델 다운로드 시 필요.
2.  **소프트웨어:**
    *   **Python:** 버전 3.9, 3.10, 3.11 중 하나 ([Python 다운로드](https://www.python.org/downloads/))
    *   **Git:** 코드 및 모델 다운로드 시 필요 ([Git 다운로드](https://git-scm.com/downloads/))

## 설정 단계 (최초 1회만)

**1단계: 프로젝트 폴더 생성**

*   `JewelryAI`라는 폴더를 새로 만듭니다. 이후 모든 작업은 이 폴더 안에서 진행됩니다.

**2단계: Python 가상환경 설정**

```bash
python -m venv env
```

활성화 방법:

- Windows: `env\Scripts\activate.bat`
- macOS/Linux: `source env/bin/activate`

**3단계: 애플리케이션 코드 다운로드**

*   제공된 Python 파일(`app.py`)과 `requirements.txt` 파일을 `JewelryAI` 폴더에 저장합니다.

**4단계: 라이브러리 설치**

1. [PyTorch 설치](https://pytorch.org/get-started/locally/) 페이지에서 환경에 맞는 명령어 복사 후 터미널에 붙여넣기
2. 나머지 라이브러리 설치:

```bash
pip install -r requirements.txt
```

3. `accelerate` 설정:

```bash
accelerate config
```

**5단계: Stable Diffusion v1.5 모델 다운로드**

```bash
git clone https://huggingface.co/sd-legacy/stable-diffusion-v1-5 base_models/stable-diffusion-v1-5
```

**6단계: 훈련 스크립트 다운로드 (Diffusers)**

```bash
git clone https://github.com/huggingface/diffusers.git diffusers_repo
```

## 애플리케이션 실행 방법

1. 가상환경 활성화 (`env` 활성화)
2. 애플리케이션 실행:

```bash
streamlit run app.py
```

3. 브라우저가 자동으로 `http://localhost:8501` 페이지를 엽니다.

## 애플리케이션 사용법

**1. 클라이언트/제품 폴더 생성**

*   사이드바에서 이름 입력 후 "Create" 클릭
*   폴더 구조 생성:
    *   `inputs/클라이언트명/제품명/` (학습 이미지)
    *   `inputs/클라이언트명/제품명/model/` (LoRA 모델)
    *   `outputs/클라이언트명/제품명/` (생성 이미지)

**2. 학습 이미지 추가**

*   `inputs/...` 폴더에 `.jpg`, `.png`, `.webp` 이미지 복사

**3. 모델 미세 조정 (GPU 필요)**

*   '⚙️ Fine-Tune' 탭 클릭 → 이미지 확인
*   매개변수 설정 (학습 단계 수 등)
*   '🚀 Start Fine-Tuning' 클릭 → 완료되면 모델 저장됨

**4. 이미지 생성**

*   '✨ Generate Image' 탭에서 'Generate Image' 클릭
*   결과 이미지는 자동 저장됨

**5. 디자인 확인 및 다운로드**

*   '🖼️ Generated Designs' 탭 → 모든 생성 이미지 보기 및 zip 다운로드

## 예시: "Gold Rings" 스타일 학습

1. `MyJewelry` → `Gold_Rings` 폴더 생성
2. 15개 `.png` 이미지 복사
3. '⚙️ Fine-Tune' → 800 스텝 → 학습 시작
4. '✨ Generate Image' 클릭 → 스타일 반영된 이미지 생성

## 문제 해결

*   라이브러리 오류: `pip install -r requirements.txt` 다시 실행
*   모델 다운로드 오류: 해당 폴더 삭제 후 재시도
*   VRAM 부족: 배치 크기 줄이기, `fp16` 설정 활용
*   이미지에 학습 반영 안 됨: LoRA 파일 존재 확인, 제품 재선택

## 주의 사항

*   미세 조정은 자원 소모가 큽니다.
*   폴더 위치 및 파일 이름에 주의하세요.
*   정기적으로 `inputs`, `outputs` 백업하세요.

즐겁게 창작하세요! ✨