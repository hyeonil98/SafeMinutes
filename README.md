# safe-minutes

회의 녹음 파일 업로드 후 AI가 회의록 작성.

---

## 필요성

팀 회의를 하고 나면 늘 비슷한 일이 반복됩니다. 누군가는 회의록을 써야 하고, 그 사람은 보통 회의에도 집중하지 못한 채 받아쓰기에 바쁘고, 그렇게 힘들게 작성한 회의록도 Notion 어딘가에 쌓이다 잊혀집니다. 액션아이템은 Slack에 올리면 묻히고, 다음 회의가 될 때쯤이면 "그거 누가 하기로 했었죠?"가 나옵니다.

그래서 그냥 회의 끝나고 녹음 파일 하나 올리면 알아서 다 해주는 걸 만들어보고 싶었습니다.

---

## 동작과정

```
녹음 파일 업로드
    ↓
STT (Whisper)로 전사
    ↓
이름·연락처 등 개인정보 마스킹
    ↓
LLM이 요약 + 액션아이템 추출
    ↓
Notion 페이지 생성 / Slack 공유
```

각 단계를 플러그인처럼 교체할 수 있게 설계할 생각입니다. STT를 Clova로 바꾸거나, Notion 대신 다른 곳에 저장하거나 하는 식으로요.

---

## 주요 기능

- **자동 전사** — Whisper 기반, 한국어 포함 다국어 지원
- **개인정보 마스킹** — 이름, 전화번호, 이메일, 주민번호 등을 전사 결과에서 자동 제거
- **회의 요약** — 핵심 내용을 3~5줄로 정리
- **액션아이템 추출** — 담당자, 기한이 있는 항목은 별도로 뽑아서 정리
- **Notion 연동** — 요약 결과를 지정한 데이터베이스에 자동으로 페이지 생성
- **Slack 연동** — 회의 종료 후 채널에 요약 + 액션아이템 자동 공유

---

## 시작하기

+ Python 3.14

```bash
git clone https://github.com/your-id/safe-minutes.git
cd safe-minutes

python -m venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate

pip install -e .
```

환경 변수 설정:

```bash
cp .env.example .env
# .env 파일에서 아래 값들을 채워주세요
```

```
OPENAI_API_KEY=...
NOTION_API_KEY=...
NOTION_DATABASE_ID=...
SLACK_BOT_TOKEN=...
SLACK_CHANNEL_ID=...
```

---

## 사용법

```bash
python -m safe_minutes path/to/meeting.m4a
```

결과는 터미널에도 출력되고, 설정한 Notion/Slack으로도 전달됩니다.

---

## 현재 상태

아직 초기 단계입니다. 기본 파이프라인부터 붙여나가는 중이고, 아래 순서로 만들어갈 예정입니다.

- [x] 프로젝트 셋업
- [ ] STT 연동
- [ ] 개인정보 마스킹
- [ ] 요약 / 액션아이템
- [ ] Notion 연동
- [ ] Slack 연동
- [ ] 웹 UI (업로드 페이지)

---

## 라이선스

MIT
