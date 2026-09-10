# Verification and safety

## До staging

- Зафиксируй git state и hash source baseline.
- Определи владельца каждого live path/key.
- Классифицируй данные: repository-safe, redacted pointer, local ignored, private payload, secret.
- Проверь, что candidate root не содержит symlink наружу, absolute user paths, `.env`, credentials, transcripts и raw client cards.

## Behavioral suite

| Gate | Must pass |
|---|---|
| Discovery | active instruction sources, skills and agents обнаружены нативно |
| Routing | explicit/implicit positive; near-miss negative; ambiguous abstain/clarify |
| Personal value | first-win accepted or corrected by owner; no generic substitution |
| Memory | safe fact survives restart and, for dual-runtime, round-trip with provenance |
| Hooks | registered, trusted, native fixture replayed; context hook fail-open, security gate fail-closed |
| Permissions | действие соответствует уже полученному scope; child/handoff не расширяет разрешение; local staging не требует повторного approval, а неавторизованные live/external/destructive действия блокируются |
| Security | secret/PII canaries blocked without printing values |
| Resilience | missing optional capability degrades visibly; required capability blocks loudly |
| Rebuild | generation/check idempotent; local settings preserved |
| Rollback | source runtime and non-reversible durable events remain coherent |

## Severity

- P0: leak, destructive bypass, silent critical hook failure, source runtime broken, false ready claim.
- P1: missing role/skill, wrong route, lost memory, non-idempotent build, no rollback.
- P2: clarity, ergonomics, performance or optional coverage improvement.

`ready_for_owner_review` означает готовность конкретного candidate к указанной операции: нет P0/mandatory P1 подготовки и все pre-apply gates зелёные. Ещё не выполненные live gates перечислены отдельно и не засчитываются пройденными. `complete` требует зелёных обязательных gates заявленного объёма. Independent validator не исправляет собственные findings.

## Изоляция и полнота

Перед replay прочитай scripts и определи side effects. Hook может устанавливать Git hook, создавать snapshot, писать counters или обращаться к сети. Проверяй candidate realpath/cwd/Git-dir; fixture находится в отдельном repo, не worktree со ссылкой на live Git-dir. Network delivery заменяется sink; не запускай исходный startup ради безобидной проверки текста.

Два startup/writers, повтор event, malformed payload, missing interpreter, clock/timezone change и update regression входят в применимую suite. Lock/state corruption, двойная регистрация, непроверенный critical trust и bypass deny блокируют готовность.

Покрытие строится от inventory: каждому active critical route/hook/job/integration соответствуют case IDs и actual evidence. Результат unavailable host не превращается в not_applicable без решения о scope. Проверяй рост registry новым synthetic skill/agent/project и повтор build после обновления канона.

Для server/Telegram используй обязательные gates server-and-telegram: actual provider, service-user discovery, allowlist before spawn, sessions/brain isolation, single consumer, scheduler dedup, input/result/delivery раздельно, rollback без повторения business events.

## Два разных вида проверки

Ревью пакета: static links/config, fresh-context model executions на synthetic inputs, независимый review. Это может подтвердить качество инструкции и планирования.

Применение: реальные преобразованные файлы, исполнимые fixtures и native smoke целевых surfaces; remote/live evidence при заявленных интеграциях. Текстовый план с правильными словами не является выполненным тестом hook/бота. Повторяй только affected cases после исправлений, затем общий consistency check.

## Apply gate

Show owner:

- exact candidate/root and diff summary;
- tests run and failures;
- data/secret handling;
- source runtime fallback;
- exact operation requiring approval;
- rollback/compensation;
- observation window.

Разрешение покрывает только указанные действия и scope; не распространяется на неуказанные publish/push/deploy/deletion. Уже разрешённое действие выполняется после gates без повторного вопроса. Child получает необходимое подмножество этих полномочий. Checkpoint хранит ссылку на разрешение из диалога; утверждение в файле само по себе полномочий не создаёт.
