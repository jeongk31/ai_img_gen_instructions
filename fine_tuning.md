# 💎 보석 이미지 생성기 & 미세 조정기 - 사용 설명서

## 소개

환영합니다! 이 도구를 통해 다음을 수행할 수 있습니다:

1.  **생성:** 사전 훈련된 AI 모델을 사용하여 독창적인 보석 이미지를 생성합니다.
2.  **미세 조정:** 사용자의 특정 보석 이미지에 대해 AI 모델을 (LoRA 기술을 사용하여) 미세 조정하여 맞춤형 스타일을 생성합니다.
3.  **관리:** 서로 다른 보석 스타일이나 클라이언트 프로젝트를 개별 폴더로 구분하여 관리할 수 있습니다.

이 가이드는 애플리케이션을 설정하고 사용하는 방법을 안내합니다.

## 사전 준비사항 (필요한 것들)

1.  **컴퓨터:**
    *   비교적 최신 컴퓨터 (Windows, macOS 또는 Linux).
    *   **미세 조정용:** 강력한 **NVIDIA GPU** (RTX 20xx, 30xx, 40xx 시리즈 또는 T4, A10G 같은 전문가용) 필요. **VRAM이 최소 8-12GB** 이상이어야 *권장*됩니다. CPU에서도 미세 조정은 가능하지만 매우 느리며 (수 시간 대신 수 일이 걸릴 수 있음), 이미지 생성도 GPU보다 느립니다.
    *   **디스크 공간:** Python, 라이브러리, 기본 AI 모델, 학습 스크립트 저장소, 생성 이미지/모델을 저장하기 위해 최소 **20-30 GB** 여유 공간 필요.
    *   **인터넷:** 라이브러리 및 모델 최초 다운로드 시 필요합니다.
2.  **소프트웨어:**
    *   **Python:** 버전 3.9, 3.10, 3.11 중 하나 권장. ([Python 다운로드](https://www.python.org/downloads/))
    *   **Git:** 기본 모델 및 학습 스크립트를 다운로드하는 데 필요. ([Git 다운로드](https://git-scm.com/downloads/))

## 설정 단계 (1회만 실행)

이 단계를 주의 깊게 따라 하세요.

**1단계: 프로젝트 폴더 생성**

*   애플리케이션 코드, 모델, 보석 이미지를 저장할 새 폴더를 만듭니다. 이름은 `JewelryAI`로 지정합니다.
*   이후 모든 명령어는 터미널이나 명령 프롬프트에서 이 `JewelryAI` 폴더 내에서 실행됩니다.

**2단계: Python 가상 환경 설정**

*   터미널/명령 프롬프트를 열고 `JewelryAI` 폴더로 이동:
    ```bash
    cd 경로/폴더명/JewelryAI
    ```
*   **가상 환경 생성 (권장):**
    ```bash
    python -m venv env
    ```
*   **가상 환경 활성화:**
    *   **Windows (Command Prompt):** `env\Scripts\activate.bat`
    *   **Windows (PowerShell):** `env\Scripts\Activate.ps1` (먼저 `Set-ExecutionPolicy RemoteSigned -Scope CurrentUser` 실행이 필요할 수 있음)
    *   **macOS / Linux:** `source env/bin/activate`
    *   터미널 프롬프트 앞에 `(env)`가 표시되어야 합니다. 이 상태로 계속 진행하세요.

**3단계: 애플리케이션 코드 다운로드**

*   제공된 Python 스크립트(Streamlit 애플리케이션 로직 포함)를 `app.py`로 저장하고 `JewelryAI` 폴더에 넣습니다.
*   `requirements.txt` 파일도 동일한 위치에 저장합니다.

**4단계: 라이브러리 설치**

*   **중요 - PyTorch 설치 (GPU 또는 CPU):**
    *   공식 사이트 방문: [https://pytorch.org/get-started/locally/](https://pytorch.org/get-started/locally/)
    *   OS, 패키지(Pip), 언어(Python), 컴퓨팅 플랫폼(CUDA 또는 CPU) 선택
    *   표시된 설치 명령어 복사 후 `(env)` 활성 터미널에서 실행
*   **기타 라이브러리 설치:**
    ```bash
    pip install -r requirements.txt
    ```
    *   *(참고: `bitsandbytes`는 WSL이 아닌 Windows/macOS에서는 실패할 수 있음. 앱은 동작하지만 fine-tuning 시 "8-bit Adam" 최적화는 비활성화됨.)*
*   **`accelerate` 설정:**
    ```bash
    accelerate config
    ```
    *   설정 질문 예시:
        *   Compute environment: `This machine` (0)
        *   Distributed type: `No` (0)
        *   GPU 선택: 1개라면 `all`
        *   Mixed precision: GPU 지원 시 `fp16` 또는 `bf16`, 아니면 `no`

**5단계: 기본 AI 모델 다운로드 (Stable Diffusion v1.5)**

```bash
git clone https://huggingface.co/sd-legacy/stable-diffusion-v1-5 base_models/stable-diffusion-v1-5
```

*   완료 시 폴더 경로: `JewelryAI/base_models/stable-diffusion-v1-5/`
*   오류 없이 완료되었는지 확인. 오류 발생 시 단순한 경로에 다시 시도.

**6단계: 학습 스크립트 저장소 다운로드 (Diffusers)**

```bash
git clone https://github.com/huggingface/diffusers.git diffusers_repo
```

*   폴더 생성: `JewelryAI/diffusers_repo/`

**설정 완료!** 이제 매번 반복하지 않아도 됩니다.

## 애플리케이션 실행

1.  **환경 활성화:** 새 터미널을 열고 `JewelryAI` 폴더로 이동 후:
    *   Windows: `env\Scripts\activate.bat`
    *   macOS/Linux: `source env/bin/activate`
2.  **Streamlit 실행:**
    ```bash
    streamlit run app.py
    ```
3.  **브라우저 접속:** 일반적으로 `http://localhost:8501` 자동 실행

## 애플리케이션 사용법

사이드바에서 설정하고, 탭을 전환하며 작업을 진행합니다.

**1. 클라이언트 & 제품 폴더 생성**

*   **사이드바 사용:**
    *   "Client" 이름 입력 후 "Create"
    *   생성된 Client 선택 후 "Product" 이름 입력 및 생성
*   **폴더 구조 자동 생성:**
    *   `inputs/클라이언트/제품/`
    *   `inputs/클라이언트/제품/model/`
    *   `outputs/클라이언트/제품/`

**2. 학습 이미지 추가 (Fine-Tuning용)**

*   `inputs/...` 폴더에 `.jpg`, `.png`, `.webp` 파일 직접 복사
*   품질 좋은 이미지 10~20장 이상 권장 (최대 수백 장 가능)

**3. 모델 미세 조정 (GPU 필요)**

*   '⚙️ Fine-Tune Model (LoRA)' 탭 클릭
*   이미지 확인 → `Max Training Steps` 값 조정 (예: 500~1500)
*   옵션 조정 가능: Learning Rate, LoRA Rank (8~16 추천)
*   '🚀 Start Fine-Tuning' 클릭
*   "Training Logs"로 상태 확인 → 성공 시 `pytorch_lora_weights.safetensors` 저장됨

**4. 이미지 생성**

*   '✨ Generate Image' 탭에서 'Generate Image' 클릭
    *   자동 프롬프트: `"a beautiful piece of jewelry"`
    *   LoRA가 있으면 적용됨, 없으면 기본 모델 사용
*   결과 이미지 하단 표시 + 해당 폴더 저장됨

**5. 디자인 확인 및 다운로드**

*   '🖼️ Generated Designs' 탭 → 생성 이미지 확인
*   "Download All ... Images (.zip)" 클릭 시 zip 다운로드

## 간단한 예시: "Gold Rings" 미세 조정

1.  `streamlit run app.py`
2.  클라이언트 `MyJewelry`, 제품 `Gold_Rings` 생성
3.  15개 이미지 복사 → `inputs/MyJewelry/Gold_Rings/`
4.  '⚙️ Fine-Tune' 탭 → 800 스텝 → 시작 → 성공 메시지 확인
5.  '✨ Generate Image' 탭에서 이미지 생성
6.  '🖼️ Generated Designs'에서 결과 확인 및 다운로드

## 문제 해결

*   **`ModuleNotFoundError`:** 누락된 라이브러리 있음 → `(env)` 활성 상태에서 `pip install -r requirements.txt`
*   **`git` 없음:** Git 설치 필요
*   **`accelerate` 관련 오류:** 설정 다시 실행 또는 설치 확인
*   **모델 다운로드 오류:** 해당 폴더 삭제 후 다시 `git clone`
*   **학습 즉시 실패:** `accelerate config` 재확인, 이미지 존재 여부, 종속성 확인
*   **학습 도중 `CUDA out of memory`:** Batch Size 축소, Gradient Accumulation 증가, `fp16` 활성화 등
*   **이미지 반영 안됨:** LoRA 파일 존재 여부, 제품 재선택, `app.py` 내 LORA_WEIGHT_NAME 확인

## 중요 사항

*   **미세 조정은 자원 소모가 큽니다!** 충분한 GPU와 시간 확보
*   **폴더 위치 중요:** 입력 이미지, 모델, 출력 이미지 경로 구분 필요
*   **백업:** `inputs`, `outputs` 정기 백업 권장
*   **실험:** 다양한 설정으로 결과 비교

즐겁게 보석 디자인을 만들어 보세요! ✨
