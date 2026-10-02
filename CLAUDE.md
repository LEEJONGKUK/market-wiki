# Market Wiki — 스키마 (LLM 운영 규약)

이 저장소는 Andrej Karpathy의 "LLM Wiki" 패턴을 따르는 **뷰티·스킨케어 시장·경쟁사 리서치 위키**다.
사람은 자료를 고르고 질문을 던지며, Claude는 위키를 쓰고 교차참조·인덱스·로그를 유지한다.

## 1. 핵심 맥락

- **도메인:** 뷰티·스킨케어 (화장품, 더마, 뷰티 디바이스)
- **시장 범위:** 국내(KR) + 글로벌(US, JP, CN, SEA, EU 등)
- **위키가 반복해서 답해야 할 4가지 질문**
  1. **경쟁사** — 누가 무엇을 하고 있나? (신제품, 가격, 프로모션, 캠페인, 포지셔닝)
  2. **트렌드** — 시장은 어디로 가나? (성분, 소비자 니즈, 카테고리 성장, 규제)
  3. **채널** — 채널별 동향은 어떤가? (올리브영, 쿠팡, 자사몰, 아마존, 틱톡샵, 오프라인 등)
  4. **기회** — 우리 기회는 어디 있나? (화이트스페이스, 신제품·마케팅 아이디어) → **로컬 전용(internal/)**

## 2. 디렉터리 구조

```
market-wiki/
├── CLAUDE.md          # 이 스키마 (운영 규약)
├── index.md           # 공개 위키 전체 목차 (매 ingest마다 갱신)
├── log.md             # 작업 기록 (append-only)
├── raw/               # 원본 자료 — 불변. Claude는 절대 수정하지 않음
│   ├── articles/      # 웹 아티클·블로그 (Obsidian Web Clipper .md)
│   ├── reports/       # PDF·논문·리포트 (PDF 파일은 git 제외, 저작권)
│   ├── transcripts/   # 유튜브·팟캐스트 스크립트
│   └── assets/        # 이미지
├── wiki/              # Claude가 소유·관리하는 공개 위키
│   ├── sources/       # 원본 1건당 요약 1페이지
│   ├── competitors/   # 경쟁 브랜드/기업 엔티티 페이지
│   ├── trends/        # 성분·니즈·카테고리·규제 개념 페이지
│   ├── channels/      # 유통·마케팅 채널 페이지
│   ├── markets/       # 국가/지역 시장 페이지
│   └── analyses/      # 질의 결과로 남긴 비교·분석·종합 페이지
├── templates/         # 페이지 템플릿
└── internal/          # ⚠️ 로컬 전용 — .gitignore로 GitHub 업로드 차단
    ├── raw/           # 회의록·메모 원본
    ├── wiki/opportunities/  # "우리 기회" 분석
    ├── wiki/notes/    # 내부 자료 요약
    ├── index.md
    └── log.md
```

## 3. 보안 규칙 (최우선 — 이 저장소는 Public)

1. **회의록·메모·사내 자료는 반드시 `internal/` 아래에만** 저장한다. 공개 `wiki/`에 옮기지 않는다.
2. 공개 `wiki/` 페이지는 **공개 출처(raw/articles, raw/reports, raw/transcripts)만** 근거로 쓴다.
   `internal/` 내용을 인용·요약·암시하지 않는다.
3. 다음은 어떤 공개 파일에도 쓰지 않는다: 원가, 거래처 조건, 미공개 매출, 인사 정보, 내부 전략·로드맵, 실명·이메일·전화번호.
4. "우리 기회"(Q4) 분석은 `internal/wiki/opportunities/`에만 쓴다. 공개 위키 페이지는 internal 페이지로 링크하지 않는다 (internal → 공개 링크는 허용).
5. 커밋 전에 `git status`로 `internal/` 파일이나 민감 정보가 스테이징되지 않았는지 확인한다.
6. 유료 리포트 원문은 공개 저장소에 올리지 않는다(PDF는 git 제외). 요약과 출처 표기만 공개한다.

## 4. 페이지 규칙

- 파일명: 소문자 kebab-case 영문 (`olive-young.md`, `pdrn.md`). 제목·본문은 한국어.
- 모든 페이지는 YAML frontmatter를 가진다 (`templates/` 참고):
  `type`, `title`, `tags`, `created`, `updated`, `sources`(근거 source 페이지 목록), `confidence`(high/medium/low).
- 링크는 Obsidian 위키링크 `[[competitors/brand-name|브랜드명]]` 형식을 쓴다.
- **모든 사실 주장에는 출처**를 단다: `([[sources/2026-10-02-article-slug]])`.
  출처 없는 수치는 쓰지 않는다. 추론·해석은 `> 💡 해석:` 블록으로 사실과 구분한다.
- 날짜가 중요한 정보(가격, 순위, 점유율)는 "(2026-10 기준)"처럼 시점을 명시한다.
- 새 정보가 기존 내용과 충돌하면 덮어쓰지 말고 `## ⚠️ 상충 정보` 섹션에 양쪽 출처를 남긴다.

## 5. 작업 흐름

### Ingest (자료 추가) — "ingest raw/articles/xxx.md"
1. 원본을 읽고 핵심 내용 3~5개를 사용자와 짧게 공유한다.
2. `wiki/sources/YYYY-MM-DD-slug.md` 요약 페이지를 만든다 (`templates/source.md`).
3. 관련 competitors/trends/channels/markets 페이지를 새로 만들거나 갱신한다. 없던 엔티티는 새 페이지로 만든다.
4. 관련 페이지 간 교차 링크를 추가한다.
5. `index.md`에 새/변경 페이지를 한 줄 요약과 함께 반영한다.
6. `log.md`에 기록을 추가한다.
7. 내부 자료(`internal/raw/`) ingest는 같은 절차를 `internal/` 안에서만 수행한다.

### Query (질문) — "query: 올리브영에서 성장 중인 더마 브랜드는?"
1. `index.md`에서 관련 페이지를 찾아 읽고, 출처를 달아 답한다.
2. 재사용 가치가 있는 답은 `wiki/analyses/`에 페이지로 남기고 index·log를 갱신한다 (사용자 확인 후).
3. 위키에 근거가 없으면 "위키에 근거 없음"이라고 밝히고 필요한 자료를 제안한다. 추측으로 채우지 않는다.

### Lint (점검) — "lint"
- 페이지 간 모순, 오래된 주장(새 자료로 대체된 것), 인바운드 링크 없는 고아 페이지,
  index에 없는 페이지, 출처 없는 주장, 공개 위키의 민감 정보·internal 참조를 찾아 보고하고 수정을 제안한다.
- 다음에 조사할 질문과 부족한 자료를 제안한다.

## 6. log.md 형식

```
## [YYYY-MM-DD] ingest | 자료 제목
- 생성: [[...]]
- 갱신: [[...]], [[...]]
```
작업 유형: `ingest` | `query` | `lint` | `schema` (이 규약 변경 시).

## 7. 커밋 규칙

- 작업 단위로 커밋: `ingest: <자료 제목>`, `query: <질문 요약>`, `lint: <요약>`.
- 사용자가 요청할 때만 push한다.
- 모든 위키 내용은 사용자가 검토한 뒤 외부 공유·의사결정에 사용한다 (Human-in-the-loop).
