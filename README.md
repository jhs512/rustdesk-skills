# rustdesk-skills

`jhs512-rust-desk:listen` — 휴대폰에서 말하는 음성을 입력받도록 RustDesk 수신 통화를 수락하고 오디오 입력 메뉴(`Audio input`)에서 ‘시스템 소리’를 선택합니다.

## 동작 순서 (고속 실행)

1. `Click(loc=[1248, 400])` — RustDesk 연결 창 빈 공간 클릭(창 포커스 활성화)
2. `Move(loc=[1248, 458])` — `수락` 버튼 위치로 이동
3. `Wait(duration=1)` — 1초 대기
4. `Click(loc=[1248, 458])` — `수락` 클릭
5. `Wait(duration=2)` — 2초 대기
6. `Click(loc=[1248, 458])` — `Audio input`으로 변한 버튼 다시 클릭
7. `Wait(duration=1)` — 1초 대기
8. `Move(loc=[1300, 448])` — 팝오버의 `시스템 소리` 항목으로 이동
9. `Wait(duration=1)` — 1초 대기
10. `Click(loc=[1300, 448])` — `시스템 소리` 클릭

## 필수 의존성

**macos-mcp(맥 MCP)에 의존합니다.** 각 호스트에 MCP를 별도로 연결해야 합니다. 필수 도구는 `Move`, `Click`, `Wait`이며 macOS 접근성·화면 기록 권한이 필요합니다. 스킬의 `agents/openai.yaml`에도 MCP 의존성을 선언했습니다. 이 선언은 MCP 자동 설치 설정이 아닙니다.

macOS의 RustDesk 연결 창과 휴대폰에서 걸려온 수신 통화가 필요합니다.

## 호출

`jhs512-rust-desk:listen`

## 설치

Claude Code 플러그인:

```text
/plugin marketplace add jhs512/rustdesk-skills
/plugin install jhs512-rust-desk@rustdesk-skills
```

플러그인 명령은 `/jhs512-rust-desk:listen`입니다.

Codex 전역 스킬은 `~/.codex/skills/<skill-folder>/`, Antigravity 전역 스킬은 `~/.gemini/config/skills/<skill-folder>/`에 설치합니다. 각 폴더에 이 저장소의 `skills/listen/` 내용을 넣습니다.

Antigravity의 MCP는 `~/.gemini/config/mcp_config.json`에 별도로 등록합니다:

```json
{
  "mcpServers": {
    "macos-mcp": {
      "command": "/absolute/path/to/macos-mcp",
      "args": ["serve", "--transport", "stdio"]
    }
  }
}
```

## 구조

- `skills/listen/SKILL.md`: 실행 지침과 필수 의존성
- `skills/listen/agents/openai.yaml`: 표시 이름, 호출 예와 MCP 의존성
- `plugin.json`: 포터블 플러그인 메타데이터
- `.claude-plugin/`: Claude Code 플러그인 및 마켓플레이스 메타데이터
