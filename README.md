# Market Wiki

뷰티·스킨케어(국내·글로벌) 시장·경쟁사 리서치 위키. [Andrej Karpathy의 LLM Wiki 패턴](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)을 따른다.

- **사람:** 자료를 고르고 질문한다.
- **Claude Code:** 요약·교차참조·인덱스·로그를 관리한다. 규칙은 [CLAUDE.md](CLAUDE.md)에 있다.
- **시작점:** [index.md](index.md)

## 사용법

이 폴더에서 `claude`를 실행한 뒤:

| 하고 싶은 일 | Claude에게 말하기 |
|---|---|
| 자료 추가 | `ingest raw/articles/파일명.md` |
| 질문 | `query: 올리브영에서 뜨는 더마 브랜드는?` |
| 점검 | `lint` |
| 저장 | `커밋하고 push해줘` |

자료 수집은 [Obsidian Web Clipper](https://obsidian.md/clipper)로 `raw/articles/`에 저장하면 편하다.

> 이 위키는 공개 출처만 담는다. 모든 내용은 사용 전에 담당자가 검토해야 한다.
