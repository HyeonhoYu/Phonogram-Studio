# Phonogram Studio

포노그램 타일을 누르면 AI 음성으로 소리와 키워드를 들려주는 웹앱입니다.
GitHub Pages로 무료 배포하고, 음성 파일은 Google Cloud Text-to-Speech로 한 번만 만들어 저장소에 함께 올립니다.

## 폴더 구성

```
phonogram-studio/
  index.html            앱 본체
  data.js               포노그램 목록, 키워드, 발음 기호(IPA)
  audio/                AI 음성 파일이 들어갈 폴더 (처음에는 비어 있음)
  tools/generator.html  AI 음성 생성 도구
  README.md
```

소리 재생 순서는 다음과 같습니다.
1. 선생님이 앱에서 직접 녹음한 소리 (그 브라우저에만 저장)
2. `audio/` 폴더의 AI 음성 파일
3. 둘 다 없으면 브라우저 기본 음성으로 키워드 읽기

## 1단계: API 키 준비 (둘 중 하나)

생성 도구에서 음성 엔진을 고를 수 있습니다. 가지고 있는 키에 맞춰 선택하세요.

### 방법 A: Google AI Studio 키로 Gemini TTS 사용 (가장 간단)

1. https://aistudio.google.com 에서 "Get API key"로 키를 받습니다.
2. 생성 도구에서 **Gemini TTS**를 선택하고 키를 붙여넣습니다.
3. **Find models**를 한 번 눌러 이 키로 쓸 수 있는 TTS 모델 목록을 불러옵니다.

주의: Gemini TTS는 발음 기호를 직접 지정하는 기능이 없어서, 단독 음소 소리는 "이 소리만 내라"는 지시문으로 만듭니다. 대부분 잘 나오지만 가끔 단어나 지시문 일부를 읽을 수 있으니 표에서 꼭 들어보고 Redo 하세요. 무료 키는 분당 요청 수 제한이 있어서, 제한에 걸리면 도구가 자동으로 기다렸다가 다시 시도합니다. 하루 한도에 걸리면 다음 날 **Generate missing only**로 이어서 만들면 됩니다. 만든 파일은 브라우저에 저장되어 창을 닫아도 남아 있습니다.

### 방법 B: Google Cloud 표준 API 키로 Cloud Text-to-Speech 사용 (음소 정확도 최고)

AI Studio 키는 이 방법에 쓸 수 없습니다. "API keys are not supported by this API" 오류가 나오면 키 종류가 맞지 않는다는 뜻입니다.

1. https://console.cloud.google.com 에 로그인하고 프로젝트를 고릅니다. (새로 만들어도 됩니다.)
2. 결제 계정을 연결합니다. 무료 사용량 안에서 쓰더라도 결제 계정 연결은 필요합니다. 약 200개의 짧은 음성 파일은 무료 한도 안에 들어가지만, 최신 요금은 Google Cloud 가격 페이지에서 확인하세요.
3. "API 및 서비스 > 라이브러리"에서 **Cloud Text-to-Speech API**를 검색해 사용 설정합니다.
4. "API 및 서비스 > 사용자 인증 정보 > 사용자 인증 정보 만들기 > API 키"를 누릅니다. 서비스 계정 연결 여부를 묻는 화면이 나오면 연결하지 않고 일반 API 키로 만듭니다.
5. 만든 키의 "API 제한사항"에서 Cloud Text-to-Speech API만 허용하도록 제한해 두면 안전합니다.
6. 생성 도구에서 **Google Cloud Text-to-Speech**를 선택하고 키를 붙여넣습니다.

## 2단계: AI 음성 만들기

1. `tools/generator.html`을 크롬으로 엽니다. (파일을 더블클릭해서 열면 됩니다.)
2. 음성 엔진을 고르고, API 키를 붙여넣고, 목소리와 속도를 고릅니다.
3. **Test voice**로 소리가 나는지 확인합니다.
4. **Generate all**을 누르면 모든 소리가 만들어집니다. 몇 분 걸립니다.
5. 표에서 Play를 눌러 들어보고, 어색한 소리는 IPA 칸을 고친 뒤 **Redo**를 누릅니다.
   - b, d, g, p, t, k 같은 파열음은 따로 떼어 발음하면 소리가 약하게 나올 수 있습니다. 이럴 때는 `bə`처럼 짧은 모음을 붙여보세요.
   - r은 원래 `ɝ` (/er/)로 설정되어 있습니다. 모음 없이 r만 내면 알아듣기 어렵기 때문입니다.
6. **Download zip**을 누르면 `phonogram-audio.zip`이 받아집니다. (Cloud는 mp3, Gemini는 wav 파일로 저장되며 앱은 둘 다 재생합니다.)

API 키는 코드나 파일 어디에도 저장되지 않습니다. "Remember the key" 체크 시에만 그 컴퓨터 브라우저에 남습니다. **API 키를 GitHub에 올리지 마세요.**

## 3단계: GitHub에 올리기

1. GitHub에서 새 저장소를 만들고, "Add file > Upload files"로 이 폴더의 파일들(`index.html`, `data.js`, 아이콘 파일들, `assets`, `tools`, `audio` 폴더)을 올립니다.
2. 생성 도구에서 **Download for GitHub (1 file)**을 누르면 `pack.json` 파일 하나가 받아집니다. 모든 소리가 mp3로 압축되어 이 파일 안에 들어 있습니다.
3. 저장소의 `audio` 폴더로 들어가 "Add file > Upload files"로 `pack.json` 하나만 올립니다.
4. "Settings > Pages"에서 Branch를 `main`, 폴더를 `/ (root)`로 두고 Save를 누릅니다.
5. 1~2분 뒤 `https://계정이름.github.io/저장소이름/` 으로 접속하고, 앱의 Settings에서 "AI voice files found: 숫자"를 확인합니다.

소리를 다시 만들면 새 `pack.json`을 같은 자리에 올려 덮어쓰면 됩니다.
예전 방식(파일 여러 개 + manifest.json)도 계속 동작하지만, `pack.json`이 있으면 앱은 그것을 먼저 사용합니다.

## 소리를 다시 만들고 싶을 때

generator에서 수정 후 다시 `pack.json`을 받아 `audio` 폴더의 파일을 덮어쓰면 됩니다. 브라우저 캐시 때문에 바로 안 바뀌면 새로고침(Ctrl+Shift+R)을 누르세요.

## 포노그램을 추가하거나 고치고 싶을 때

`data.js`만 수정하면 앱과 generator에 모두 반영됩니다. 형식은 다음과 같습니다.

```
["ch","ct",[["/ch/","chin","tʃ"],["/k/","school","k"]]]
  타일   그룹   소리 표기, 키워드, IPA
```

그룹: `c` 자음, `v` 모음, `ct` 자음 조합, `vt` 모음 조합, `r` r 통제 모음, `e` 어미
