# 🎤 면접 연습실

학생회 · 동아리 · 생기부 · 대입 면접을 혼자서 연습하는 웹앱입니다. 서버 없이 정적 파일(`index.html`) 하나로 동작합니다.

## 기능
- 면접 유형 선택 → 질문 유형(인성/동기/상황/심화) · 난도(하/중/상) 선택
- 난도·질문 유형에 따라 달라지는 타이머
- 카메라·마이크 분석 피드백(목소리 크기·억양·속도·자세), 권한이 없으면 키보드 답변
- 연습 기록 localStorage 저장, 연습 횟수·점수 변화 그래프, 유형/설정별 필터
- 꼬리 질문

## 파일 구성
```
├─ index.html                      # 앱 전체 (HTML + CSS + JS)
├─ README.md
├─ LICENSE                         # MIT (이름 수정)
├─ .nojekyll                       # GitHub Pages가 파일을 그대로 서비스하도록
├─ .gitignore
└─ .github/workflows/pages.yml     # main 푸시 시 자동 배포
```

## GitHub Pages 배포
1. GitHub에서 새 저장소를 만들고 이 파일들을 올립니다.
   ```bash
   git init
   git add .
   git commit -m "init"
   git branch -M main
   git remote add origin https://github.com/<아이디>/<저장소>.git
   git push -u origin main
   ```
2. 저장소 **Settings → Pages → Build and deployment → Source**를 **GitHub Actions**로 선택합니다.
3. 푸시하면 Actions가 실행되고 `https://<아이디>.github.io/<저장소>/` 에서 열립니다.

(워크플로 없이 쓰려면 Source를 *Deploy from a branch → main / (root)* 로 선택해도 됩니다.)

## 로컬 실행
카메라·마이크는 `https` 또는 `localhost`에서만 동작합니다.
```bash
python3 -m http.server 8000   # http://localhost:8000
```

## ⚠️ Claude 연동 기능에 대해
원본은 Claude 아티팩트 환경의 `window.claude`(sample) 기능으로 아래 기능을 구현했습니다.
- AI 질문 출제 · 꼬리 질문 · 답변 내용 평가

GitHub Pages 등 일반 호스팅에서는 이 기능이 없어서 다음처럼 동작합니다.
- 질문은 내장된 100개 질문 은행에서 중복을 피해 무작위 출제
- 꼬리 질문은 만들어지지 않음, 답변 평가는 "받을 수 없어요" 안내 후 전달력·시간 점수만 반영

AI 기능을 쓰려면 **직접 만든 서버(또는 서버리스 함수)** 를 거쳐 Anthropic API를 호출하도록 `pickQ`, `evaluate`, `followUp` 함수를 수정하세요. API 키는 절대 `index.html`에 넣지 마세요(공개 저장소에 그대로 노출됩니다).

## 개인정보
영상·음성은 앱이 저장하거나 전송하지 않고 브라우저 안에서만 분석합니다. 다만 음성 인식(Web Speech API)은 브라우저 제공 기능이라 브라우저 업체 서버로 음성이 전송될 수 있습니다.
연습 기록은 사용자의 브라우저 localStorage에만 저장됩니다.
