# 주얼리 디자인 생성기 - 설치 및 사용 안내서

이 안내서는 Windows 또는 Mac 컴퓨터에서 주얼리 디자인 생성기 애플리케이션을 설치하고 사용하는 방법을 설명합니다.

## 1부: 설치 (한 번만 수행)

**1. Python 설치:**

- Python이 설치되어 있지 않은 경우 공식 웹사이트에서 다운로드하세요: [https://www.python.org/downloads/](https://www.python.org/downloads/)
- **권장 버전:** Python 3.9 또는 3.10 (다른 버전에서도 작동할 수 있지만, 최상의 호환성을 위해 이 버전이 권장됩니다).
- **설치 중 (Windows):** "Add Python to PATH" 박스를 체크했는지 확인하세요.

**2. 애플리케이션 코드 열기:**

전달받은 프로젝트 파일을 컴퓨터의 원하는 위치에 압축 해제합니다.

- 프로젝트 폴더(예: `JEWERY-GENERATION-main`)로 이동합니다.
- 폴더 상단의 주소 표시줄을 클릭하고 "cmd"를 입력한 후 Enter 키를 누릅니다. 이렇게 하면 해당 폴더 위치에서 명령 프롬프트가 바로 열립니다.
- 명령 프롬프트 창이 열리면 이제 `app.py`, `requirements.txt` 등이 포함된 프로젝트 폴더 내부에 있어야 합니다.

**3. PyTorch(AI 엔진) 설치:**

- 공식 PyTorch 웹사이트로 이동합니다: [https://pytorch.org/get-started/locally/](https://pytorch.org/get-started/locally/)
- 페이지에서 다음 선택을 합니다:
  - **PyTorch 빌드:** Stable
  - **운영체제:** Windows 또는 Mac
  - **패키지:** Pip
  - **언어:** Python
  - **컴퓨팅 플랫폼:**
    - 최신 NVIDIA GPU와 CUDA 드라이버가 설치되어 있는 경우: 표시된 최신 CUDA 버전을 선택합니다.
    - 그렇지 않은 경우(또는 확실하지 않은 경우): **CPU**를 선택합니다.
- "Run this Command" 아래에 표시된 명령어를 복사합니다. 이 명령어는 (CUDA의 경우) `pip3 install torch torchvision torchaudio ...` 또는 (CPU의 경우) `pip3 install torch torchvision torchaudio`와 같은 형태일 것입니다.
- 복사한 명령어를 터미널/명령 프롬프트(프로젝트 폴더 내부에 있는 상태)에 붙여넣고 Enter를 누릅니다. 설치가 완료될 때까지 기다립니다.

**4. 기타 필요한 패키지 설치:**

- 동일한 터미널/명령 프롬프트 창에서(여전히 프로젝트 폴더 내부에 있는 상태) 다음 명령어를 실행합니다:
  ```
  pip install -r requirements.txt
  ```
- 이것은 Streamlit, Diffusers, Transformers 및 다음 단계에 필요한 `huggingface_hub`를 포함한 기타 필요한 라이브러리를 설치합니다. 모든 패키지가 다운로드되고 설치될 때까지 기다립니다.

**5. AI 모델 다운로드:**

- AI 모델은 생성에 필요하지만 GitHub에 올리기에는 너무 큽니다. 이전 단계에서 설치한 명령줄 도구를 사용하여 별도로 다운로드해야 합니다.
- 터미널/명령 프롬프트에서 여전히 프로젝트의 루트 폴더에 있는지 확인하세요.
- 다음 명령어를 하나씩 실행합니다:

  ```
  # CLIP 모델 다운로드 (시간이 걸릴 수 있습니다)
  huggingface-cli download openai/clip-vit-base-patch32 --local-dir models/clip-vit-base-patch32 --local-dir-use-symlinks False

  # Stable Diffusion 모델 다운로드 (더 오래 걸리고 더 많은 디스크 공간이 필요합니다)
  huggingface-cli download stabilityai/stable-diffusion-2-1 --local-dir models/stable-diffusion-2-1 --local-dir-use-symlinks False
  ```

- 두 다운로드가 모두 완료될 때까지 기다립니다. 모델들은 각각 `models/clip-vit-base-patch32`와 `models/stable-diffusion-2-1` 하위 디렉토리에 저장됩니다. 이 다운로드는 한 번만 수행하면 됩니다.
- **디스크 공간:** 충분한 디스크 공간이 있는지 확인하세요(모델에 약 10-15GB가 필요할 수 있습니다).

설치가 완료되었습니다!

## 2부: 애플리케이션 실행

1.  **터미널 또는 명령 프롬프트**를 엽니다(아직 열려있지 않은 경우).
2.  **애플리케이션 폴더로 이동**합니다(GitHub에서 복제한 폴더, 예: `cd C:\Projects\JEWERY-GENERATION-main`).
3.  **앱 실행:** 다음 명령어를 입력하고 Enter를 누릅니다:
    ```
    streamlit run app.py
    ```
4.  웹 브라우저가 자동으로 주얼리 디자인 생성기 인터페이스와 함께 열립니다. 그렇지 않은 경우, 터미널에 URL(예: `http://localhost:8501`)이 표시됩니다 - 이를 복사하여 브라우저에 붙여넣으세요.

## 3부: 애플리케이션 사용하기

애플리케이션 인터페이스는 설정을 위한 왼쪽의 **사이드바**와 이미지 및 결과를 보기 위한 오른쪽의 **메인 영역**, 두 가지 주요 영역으로 구성되어 있습니다.

1.  **클라이언트 폴더 생성:**

    - 사이드바의 "Create New Client" 아래에 클라이언트+프로덕트 이름을 입력합니다(예: "client1_productA").
    - "Create Client Folder" 버튼을 클릭합니다.

2.  **클라이언트 선택:**

    - 사이드바의 "Select Client" 아래에서 드롭다운 메뉴에서 원하는 클라이언트+프로덕트 폴더를 선택합니다.

3.  **입력 이미지 업로드:**

    - 클라이언트가 선택되면 사이드바에 "Upload Images for [Client Name]"이 표시됩니다.
    - "Browse files"를 클릭하고 컴퓨터에서 하나 이상의 주얼리 이미지(.jpg, .jpeg, .png)를 선택합니다. 이 이미지들은 AI가 영감을 얻기 위해 사용할 이미지입니다.
    - 업로드된 이미지는 메인 영역의 "Input Images" 탭에 표시됩니다.

4.  **디자인 생성:**

    - 메인 영역에서 "Generated Designs" 탭을 클릭합니다.
    - **생성 방식 선택:**
      - `Standard`: _첫 번째_ 업로드된 이미지를 주요 형태/구조로 사용하고, 모든 이미지에서 스타일을 적용합니다. 특정 작품의 변형을 만들 때 좋습니다.
      - `Batch Process`: _각_ 업로드된 이미지를 기본으로 하여 여러 디자인을 생성합니다. 가장 많은 디자인을 생성합니다.
    - **매개변수 조정:**
      - `Style Transfer Strength`: AI가 기본 이미지의 스타일을 얼마나 변경하는지 제어합니다(값이 높을수록 더 많이 변경됨). 0.6-0.8 정도에서 시작하세요.
      - `Number of Designs`: 원하는 디자인 수를 설정합니다(방식에 따라 총 수량 또는 기본 이미지당 수량).
    - **"✨ Generate Designs" 버튼을 클릭합니다.**

5.  **처리 대기:**

    - 디자인 생성은 시간이 걸립니다. 특히 강력한 GPU(NVIDIA 그래픽 카드)가 없는 경우에는 더 오래 걸립니다. CPU에서 실행하는 경우 **디자인당 몇 분**이 걸릴 수 있습니다.
    - 스피너와 진행 메시지가 표시됩니다. 브라우저 탭이나 터미널 창을 닫지 말고 기다려 주세요.

6.  **결과 보기 및 다운로드:**
    - 완료되면 생성된 디자인이 "Generated Designs" 탭의 "Previously Generated Designs" 아래에 표시됩니다.
    - "Download All [Number] Designs as ZIP" 버튼을 클릭하여 선택한 클라이언트에 대해 생성된 모든 이미지가 포함된 zip 파일을 컴퓨터의 Downloads 폴더에 저장합니다.

**문제 해결:**

- **오류 `git: command not found`:** Git이 설치되지 않았거나 시스템 PATH에 없습니다. [https://git-scm.com/downloads](https://git-scm.com/downloads)에서 설치하세요.
- **오류 `huggingface-cli: command not found`:** `huggingface_hub` 라이브러리가 제대로 설치되지 않았습니다. `pip install --force-reinstall huggingface_hub`를 실행해 보고 5단계가 오류 없이 완료되었는지 확인하세요.
- **오류 "Model files not found":** 모델이 설치 단계 6에서 올바르게 다운로드되지 않았거나 예상된 `models/clip-vit-base-patch32` 및 `models/stable-diffusion-2-1` 하위 디렉토리에 없습니다. 폴더가 존재하고 파일이 들어 있는지 확인하세요. 필요한 경우 `huggingface-cli download` 명령을 다시 실행하세요.
- **오류 "CUDA out of memory":** GPU의 메모리가 부족합니다. 한 번에 더 적은 디자인을 생성해 보거나 GPU를 사용하는 다른 프로그램을 닫아보세요. PyTorch 설치 중 CUDA를 선택했지만 적합한 GPU가 없는 경우, CPU 옵션을 선택하여 PyTorch를 다시 설치해야 할 수 있습니다.
- **매우 느린 생성 속도:** PyTorch가 CPU용으로 설치된 경우 예상되는 현상입니다. 이미지 생성은 계산 집약적인 작업입니다.
- **기타 오류:** 앱이나 터미널 창에 표시되는 오류 메시지를 기록해 두세요. 문제 진단에 도움이 될 수 있습니다. 애플리케이션을 다시 시작해 보세요(터미널을 닫은 다음 2부를 반복). 프로젝트의 GitHub 저장소 페이지에서 문제를 보고할 수도 있습니다.
