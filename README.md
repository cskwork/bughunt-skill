# bughunt — an agent skill that plays your app until it breaks

[![Skill](https://img.shields.io/badge/Claude_Code-Skill-6b5bd6)](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview)
[![License](https://img.shields.io/badge/license-MIT-blue)](LICENSE)

Unit tests and a fresh-game autoplay pass can both be green while a real player hits a bug in the first minute: the first command after "Continue" is rejected, an AI acts before its screen is visible, a finished player's save comes back to life. These bugs live on **transitions**, and only show up when someone plays the way people actually play.

`bughunt` makes an agent do that playing for you: seeded **personas** drive the shipped build through its real input paths, **invariant oracles** check every step, and every failure becomes a replayable bundle, a regression test, and a root-cause fix.

## What it does

1. Build the target **as shipped** (exported / simulator build, not only the editor).
2. Run seeded personas: new player, button masher, save → quit → resume at every phase, background / foreground interruptions, slow player who lets timers expire, speed changer, random tapper, other languages, small and large screens, Reduce Motion.
3. Check oracles at every step: no errors or crashes, every enabled command accepted on the first try, no AI action before its screen is ready, UI inside the safe area with no overlap, sane state (no negative currency, no vanished units), deterministic replay of the same seed, no softlock, persisted state round-trips and stays eligible.
4. On failure: save seed + action log + screenshot + state dump → minimise → regression test → root-cause fix → replay the whole matrix.
5. Stop when every persona × seed passes twice in a row; report the evidence.

Adapters: **Godot** (in-game QA runner driven by command-line flags), **native iOS** (XCUITest / Maestro), **web** (Playwright). Templates for personas, invariants, failure reports, and a per-project `.bughunt.md` override are included. Worked examples: [`examples/transition-misses.md`](examples/transition-misses.md).

## Install

```sh
npx skills@latest add cskwork/bughunt-skill
# or
git clone https://github.com/cskwork/bughunt-skill ~/.claude/skills/bughunt
```

Then ask your agent to "run bughunt" before a release candidate, after a big feature, or when a user reports "it only happens when I do X".

## Files

| Path | What |
|---|---|
| `SKILL.md` | When to use it and the loop |
| `personas.md`, `oracles.md` | Persona catalogue and invariant oracles |
| `adapters/` | Godot, native iOS, web |
| `templates/` | Persona, invariants, failure report, project override |
| `examples/transition-misses.md` | Three real transition misses, generalised |
| `references.md` | Sources (monkey testing, stateful property testing, deterministic replay, game QA bots, tool docs) |

Also part of the skill collection [cskwork/skills](https://github.com/cskwork/skills).

## 한국어 요약

`bughunt`는 에이전트가 앱이나 게임을 사람처럼 직접 플레이하며 버그를 찾고 고치게 하는 스킬입니다. 여러 성격의 가상 플레이어(처음 하는 사람, 연타, 저장 후 이어하기, 앱 전환, 느린 플레이, 무작위 터치 등)가 같은 조건을 재현할 수 있는 방식으로 실제 빌드를 플레이하고, "절대 일어나면 안 되는 일"을 매 단계 검사합니다. 실패하면 재현 자료를 남기고 회귀 테스트와 근본 원인 수정까지 진행합니다. Godot, 네이티브 iOS, 웹을 지원합니다.

## License

MIT
