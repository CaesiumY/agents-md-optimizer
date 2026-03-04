# agents-md-optimizer

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

[English](README.md)

에이전트 컨텍스트 파일(`AGENTS.md`, `CLAUDE.md`, `.cursorrules`, `.windsurfrules` 등)을 [Addy Osmani의 agents-md 방법론](https://addyosmani.com/blog/agents-md/)으로 최적화하는 [에이전트 스킬](https://skills.sh)입니다.

## 기능

에이전트 컨텍스트 파일에 **발견 가능성 필터(discoverability filter)**를 적용합니다:

1. **기준선 분석** — 모든 섹션을 `discoverable`, `operational`, `verbose`로 분류
2. **Gotcha 마이닝** — 소스 코드를 스캔하여 문서에 빠진 비명시적 운영 지식 발굴
3. **최적화** — 중복 콘텐츠 제거, 장황한 섹션 압축, 발견된 gotcha 추가
4. **검증** — 최적화 전후 통계 비교

연구에 따르면 중복 컨텍스트는 에이전트 성능을 15-20% 저하시키고, 집중된 운영 지식(gotcha, landmine)은 런타임을 약 28% 단축시킵니다.

## 설치

```bash
npx skills add CaesiumY/agents-md-optimizer
```

또는 로컬 클론에서 설치:

```bash
npx skills add ~/path/to/agents-md-optimizer
```

## 사용법

설치 후 AI 코딩 에이전트에서 다음 문구로 트리거할 수 있습니다:

- `CLAUDE.md 최적화`
- `CLAUDE.md 줄이기`
- `CLAUDE.md 다이어트`
- `optimize CLAUDE.md`
- `streamline CLAUDE.md`
- `agents-md`
- `discoverability filter`
- `add gotchas`
- `optimize AGENTS.md`
- `optimize context file`
- `optimize .cursorrules`

### 플래그

| 플래그 | 효과 |
|--------|------|
| `--dry-run` | 파일 수정 없이 분석 결과와 diff만 표시 |
| `--report-only` | 통계와 분류 테이블만 출력 |
| `--path <경로>` | 대상 파일 경로 (미지정 시 자동 감지) |
| `--help` | 사용법 표시 후 종료 |

### 자동 감지

`--path`를 지정하지 않으면 다음 순서로 첫 번째 존재하는 파일을 자동 탐색합니다:

`AGENTS.md` → `CLAUDE.md` → `.cursorrules` → `CURSOR.md` → `.github/copilot-instructions.md` → `.windsurfrules` → `codex.md`

### 예시

```
CLAUDE.md 최적화
CLAUDE.md 최적화 --dry-run
CLAUDE.md 최적화 --report-only
optimize AGENTS.md --path ./AGENTS.md
```

## 호환 플랫폼

스킬을 지원하는 모든 AI 코딩 에이전트에서 사용 가능합니다:

Claude Code · Cursor · GitHub Copilot · Windsurf · Cline · Gemini CLI · OpenAI Codex · 그 외 다수

## 작동 원리

### 발견 가능성 필터

컨텍스트 파일의 각 줄에 대해 다음을 판단합니다:

> "에이전트가 Glob, Grep, Read를 사용해 10초 이내에 이 정보를 발견할 수 있는가?"

- **예** → 제거 (디렉토리 트리, 데이터 플로우 다이어그램, 기술 스택 설명)
- **아니오** → 유지 (타이밍 제약, 암묵적 의미론, 플랫폼 gotcha)
- **부분적** → 비명시적 함의만 남기고 압축

### Gotcha 마이닝

코드베이스를 체계적으로 스캔하여 8가지 범주의 숨겨진 운영 지식을 발굴합니다:

1. **타이밍 & 순서** — 숨겨진 시간 예산, 순서 요구사항
2. **암묵적 의미론** — 예상과 다른 의미를 가진 값
3. **플랫폼 감지** — 비명시적 플랫폼 동작
4. **필수 계약** — 특정 출력을 반드시 생성해야 하는 진입점
5. **에러 종료 정책** — 비표준 에러 처리
6. **숨겨진 설정 규칙** — 스키마에서 명확하지 않은 설정 동작
7. **쿨다운 & 중복 제거** — 비명시적 범위의 속도 제한
8. **예약된 기능** — 구현 없는 정의된 인터페이스

### 최적화 전후

실제 프로젝트에서의 전형적인 최적화 결과:

| 지표 | 이전 | 이후 | 변화 |
|------|------|------|------|
| 총 줄 수 | 182 | 90 | **-51%** |
| 발견 가능한 줄 | 80 | 0 | 제거됨 |
| 운영 지식 줄 | 60 | 55 | 유지됨 |
| Gotcha 항목 | 0 | 25 | **추가됨** |

## 참고 자료

- [Addy Osmani — agents-md: the developer's guide to AGENTS.md](https://addyosmani.com/blog/agents-md/)
- [Lulla et al. (2026)](https://arxiv.org/abs/2501.02896) — 사람이 작성한 AGENTS.md가 런타임을 28.64% 단축
- [ETH Zurich](https://arxiv.org/abs/2502.13138) — LLM이 생성한 컨텍스트 파일은 성공률을 2-3% 저하

## 라이선스

MIT
