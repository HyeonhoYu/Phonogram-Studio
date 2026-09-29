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

## 1단계: Google Cloud API 키 만들기

1. https://console.cloud.google.com 에 로그인하고 새 프로젝트를 만듭니다.
2. 결제 계정을 연결합니다. 무료 사용량 안에서 쓰더라도 결제 계정 연결은 필요합니다. 약 200개의 짧은 음성 파일(소리 103개 x 소리용, 키워드용)은 무료 한도 안에 들어가지만, 최신 요금은 Google Cloud 가격 페이지에서 한 번 확인하세요.
3. "API 및 서비스 > 라이브러리"에서 **Cloud Text-to-Speech API**를 검색해 사용 설정합니다.
4. "API 및 서비스 > 사용자 인증 정보 > 사용자 인증 정보 만들기 > API 키"로 키를 만듭니다.
5. 만든 키의 "API 제한사항"에서 Cloud Text-to-Speech API만 허용하도록 제한해 두면 안전합니다.

## 2단계: AI 음성 만들기

1. `tools/generator.html`을 크롬으로 엽니다. (파일을 더블클릭해서 열면 됩니다.)
2. API 키를 붙여넣고 목소리와 속도를 고릅니다.
3. **Test voice**로 소리가 나는지 확인합니다.
4. **Generate all**을 누르면 모든 소리가 만들어집니다. 몇 분 걸립니다.
5. 표에서 Play를 눌러 들어보고, 어색한 소리는 IPA 칸을 고친 뒤 **Redo**를 누릅니다.
   - b, d, g, p, t, k 같은 파열음은 따로 떼어 발음하면 소리가 약하게 나올 수 있습니다. 이럴 때는 `bə`처럼 짧은 모음을 붙여보세요.
   - r은 원래 `ɝ` (/er/)로 설정되어 있습니다. 모음 없이 r만 내면 알아듣기 어렵기 때문입니다.
6. **Download zip**을 누르면 `phonogram-audio.zip`이 받아집니다.

API 키는 코드나 파일 어디에도 저장되지 않습니다. "Remember the key" 체크 시에만 그 컴퓨터 브라우저에 남습니다. **API 키를 GitHub에 올리지 마세요.**

## 3단계: GitHub에 올리기

1. GitHub에서 새 저장소를 만듭니다. (예: `phonogram-studio`)
2. "Add file > Upload files"로 `index.html`, `data.js`, `README.md`, `tools` 폴더를 올립니다.
3. zip을 풀면 `audio` 폴더가 나옵니다. 그 안의 mp3 파일들과 `manifest.json`을 모두 선택해서 저장소의 `audio` 폴더에 올립니다.
   - 웹 업로드 화면에 `audio` 폴더째 끌어다 놓으면 폴더 구조가 그대로 올라갑니다.
4. "Settings > Pages"에서 Branch를 `main`, 폴더를 `/ (root)`로 두고 Save를 누릅니다.
5. 1~2분 뒤 `https://계정이름.github.io/phonogram-studio/` 로 접속합니다.

`manifest.json`이 꼭 함께 올라가야 앱이 AI 음성 파일을 인식합니다. 앱의 Settings를 열면 "AI voice files found: 숫자"로 확인할 수 있습니다.

## 소리를 다시 만들고 싶을 때

generator에서 수정 후 다시 zip을 받아 `audio` 폴더의 파일을 덮어쓰면 됩니다. 브라우저 캐시 때문에 바로 안 바뀌면 새로고침(Ctrl+Shift+R)을 누르세요.

## 포노그램을 추가하거나 고치고 싶을 때

`data.js`만 수정하면 앱과 generator에 모두 반영됩니다. 형식은 다음과 같습니다.

```
["ch","ct",[["/ch/","chin","tʃ"],["/k/","school","k"]]]
  타일   그룹   소리 표기, 키워드, IPA
```

그룹: `c` 자음, `v` 모음, `ct` 자음 조합, `vt` 모음 조합, `r` r 통제 모음, `e` 어미
