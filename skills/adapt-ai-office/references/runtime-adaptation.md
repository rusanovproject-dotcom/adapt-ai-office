# Runtime adaptation

## Главный принцип

Добивайся semantic parity, не file parity. Сначала опиши, что элемент делает, затем материализуй его нативно для target runtime.

## Mapping

| Семантика | Claude Code | Codex | Проверка |
|---|---|---|---|
| постоянные project rules | `CLAUDE.md` и nested rules | `AGENTS.md`/`AGENTS.override.md` root→cwd | спросить runtime, какие инструкции загружены; проверить budget/precedence |
| reusable workflow | `.claude/skills/<name>` | `.agents/skills/<name>` | explicit + implicit positive/negative prompts |
| custom role | `.claude/agents/*.md`/project registry | `.codex/agents/*.toml` | schema + explicit invocation + boundary case |
| lifecycle automation | `.claude/settings*.json` + hooks | `.codex/hooks.json` или inline config | native payload replay, trust, timeout, blocking behavior |
| external capability | MCP/config | MCP/config/app/plugin по native contract | required/optional, auth redacted, fallback |
| durable office memory | repository-safe/private declared owners | тот же канон через adapter | cross-session/runtime recall with provenance |
| permission boundary | Claude permissions + deterministic gates | Codex sandbox/approval + hooks/rules | реальный deny/ask/HITL smoke |

Перед apply перепроверь актуальную официальную документацию target runtime: форматы hooks, agents, skills и config меняются.

## Codex-specific checks

- `AGENTS.md` автоматически обнаруживается только по documented chain; `@file` не считать import-механизмом.
- Общий instruction budget ограничен: корень держать компактным, специализированные правила класть ближе к target path.
- Repo skills должны иметь один канонический `SKILL.md` с точным `description`; одноимённые skills не сливаются.
- Project custom agent — отдельный `.codex/agents/<name>.toml` с `name`, `description`, `developer_instructions`; model/sandbox задавать только осознанно.
- Hook sources суммируются. Проверять дубли, race и порядок независимо; matching command hooks могут стартовать одновременно.
- Non-managed hook должен быть trusted по текущему hash. Файл на диске без trust/registration не считается активным.
- Использовать release behavior page как контракт; schema из main может опережать установленную версию.
- Native memories, imports и session transcripts не являются единственным каноном офиса.
- Machine-local auth/provider/telemetry settings не перезаписывать repo config.

## Обязательные уточнения mapping

- Custom role в таблице означает native subagent. Inline persona, Директор, skill-agent, autonomous service и stub требуют собственных mapping: не превращай каждую папку в TOML.
- Discovery проверяется отдельно на Desktop, CLI, headless runner и сервисном host. Зафиксируй версии и capabilities каждого; local Codex 0.150.1 в ревью 2026-09-10 не свидетельствует о версии клиента.
- Переводи literal tool names, model aliases, callback/event payloads и approval semantics по контракту целевой версии. При отсутствии native возможности используй проверенный adapter или пометь блокер.
- Hooks проверяются в фактическом launch mode. Неподдерживаемые handler types, matcher/tool-name mismatch и непринятый trust не закрываются синтаксическим parse.
- Custom route metadata, parsers и runtime-specific env/cwd/read paths мигрируют совместно. Проверь state machine и upstream updater через соседние playbooks.
- Shell/Git/service enforcement отдельно от model instructions. Не заменяй работающий deny обычной фразой в AGENTS.md.

## Источники контрактов

Сверены 2026-09-10; перед применением проверяй установленный runtime и свежую документацию. Документация задаёт ожидаемый механизм, host smoke — фактическую поддержку.

- https://learn.chatgpt.com/docs/agent-configuration/agents-md
- https://learn.chatgpt.com/docs/build-skills
- https://learn.chatgpt.com/docs/agent-configuration/subagents
- https://learn.chatgpt.com/docs/hooks
- https://learn.chatgpt.com/docs/non-interactive-mode

Не используй копии документации из сторонних зеркал как доказательство совместимости релиза.

## Migration order

1. inventory и signed baseline;
2. semantic canon/ownership;
3. one real critical route and its existing applicable agent/skill/hook/memory dependencies; absent subsystems are evidence-backed not_applicable, not new infrastructure;
4. native smoke и correction;
5. остальные элементы волнами;
6. shadow/opt-in;
7. owner acceptance;
8. cutover и observation;
9. cleanup только после выбранного observation window, проверенного повторного build и отдельной авторизации удаления.

Source runtime остаётся контрольным, пока target не проходит critical journeys. Import может ускорить bootstrap, но не заменяет inventory, classification и parity verification.
