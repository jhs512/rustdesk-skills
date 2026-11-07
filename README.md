# rustdesk-skills

`jhs512-rust-desk:listen` — 휴대폰에서 말하는 음성을 입력받도록 파세오 수신 통화를 수락하고, 3초 후 오디오 입력 메뉴에서 ‘시스템 소리’를 선택합니다.

## 필수 의존성

**macos-mcp(맥 MCP)에 의존합니다.** 각 호스트에 MCP를 별도로 연결해야 합니다. 필수 도구는 `Snapshot`, `App`, `Click`, `Wait`이며 macOS 접근성·화면 기록 권한이 필요합니다. 스킬의 `agents/openai.yaml`에도 MCP 의존성을 선언했습니다. 이 선언은 MCP 자동 설치 설정이 아닙니다.

macOS의 Paseo 앱과 휴대폰에서 걸려온 수신 통화 또는 진행 중인 통화가 필요합니다. 저장소 이름과 호출 네임스페이스에 rustdesk가 들어가지만, 현재 스킬이 조작하는 앱은 Paseo입니다.

## 호출

`jhs512-rust-desk:listen`

이름만 논의하거나 사용법을 묻는 요청은 실행하지 않습니다. 통화가 없으면 발신하지 않고 상태를 알립니다. 이미 ‘시스템 소리’가 선택되어 있으면 그대로 둡니다.

## 설치

Claude Code 플러그인:

```text
/plugin marketplace add jhs512/rustdesk-skills
/plugin install jhs512-rust-desk@rustdesk-skills
```

플러그인 명령은 `/jhs512-rust-desk:listen`입니다.

Codex 전역 스킬은 `~/.codex/skills/<skill-folder>/`, Antigravity 전역 스킬은 `~/.gemini/config/skills/<skill-folder>/`에 설치합니다. 각 폴더에 이 저장소의 `skills/listen/` 내용을 넣습니다. 독립 스킬로 설치할 때 콜론 호출어는 자연어 트리거이며 호스트 명령어 등록과는 다릅니다. 독립 설치의 명시적 호출 식별자는 SKILL.md의 name 값입니다.

Antigravity의 MCP는 `~/.gemini/config/mcp_config.json`에 별도로 등록합니다. 이 파일에 기존 서버가 있으면 항목을 병합하세요. macos-mcp의 실제 실행 경로를 사용합니다.

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

설치 후 호스트에서 스킬 및 MCP 설정을 다시 로드하거나 새 대화를 시작하세요. MCP 도구 연결을 확인한 다음 실행합니다.

[Antigravity 스킬 문서](https://antigravity.google/docs/skills) · [MCP 문서](https://antigravity.google/docs/mcp)

## 구조

- `skills/listen/SKILL.md`: 실행 지침과 필수 의존성
- `skills/listen/agents/openai.yaml`: 표시 이름, 호출 예와 MCP 의존성
- `plugin.json`: 포터블 플러그인 메타데이터
- `.claude-plugin/`: Claude Code 플러그인 및 마켓플레이스 메타데이터

현재 검증 범위는 스킬 형식·패키지 메타데이터·MCP 초기화 및 도구 목록 확인입니다. 실제 수신 통화를 사용한 종단 간 동작은 별도 실행이 필요합니다.
