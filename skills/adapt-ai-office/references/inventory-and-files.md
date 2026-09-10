# Инвентаризация и преобразование файлов

## Безопасное обследование

1. Установи source root, root каждого repo и применимые инструкции. Сначала список путей через rg --files --hidden с исключениями .git, caches, dependencies, histories, secrets и private payload; проверяй игнорируемые automation paths отдельно. Не делай глобальный content grep по home.
2. Определи symlinks и targets без рекурсивного перехода. Внешняя ссылка требует отдельного scope/data решения. Учитывай регистр .Codex/.codex, OS, пробелы, кириллицу, executable bit, line endings, shell/interpreter и cwd. Ссылка в candidate допустима только на разрешённый существующий target внутри него; внешний target блокирует упаковку.
3. Классифицируй файл до чтения: safe instruction/code; internal; private payload; secret; machine-local/cache. .env, auth stores, histories, raw logs, transcripts и client cards не печатай. Из конфигов получай allowlisted metadata безопасным парсером, не dump целиком. Private payload обследуй по redacted locator/count/owner; содержимое только при необходимости и подходящей авторизации, отдельно от переносимого пакета.
4. Снимай git status и fingerprints разрешённых файлов. Чужие изменения не прячь и не сбрасывай. Проверь другие worktrees/repos и generated files; tracked не означает publishable.

## Что искать

| Объект | Проверяемые связи |
|---|---|
| инструкции | root/nested rules, imports, priority, budget, cwd, safety и personality |
| роли | registry → body → knowledge/memory → skills/tools; orchestrator, native subagent, inline persona, skill-agent, autonomous service, stub/disabled/archive |
| skills/commands/plugins | discovery roots, duplicate names, custom route metadata, scripts/assets, global dependencies |
| hooks | registration → script → interpreter → paths → exit/output semantics; local/user/plugin duplicates |
| память/onboarding | profile, summary, build log, open threads, indexes, cadence; writers/readers |
| обновление | upstream, installers, self-update scripts, generated wrappers; способность перезаписать адаптеры |
| сервер | systemd/PM2/Docker/cron/timers/launchd/CI/n8n, transport, runner, volumes, jobs, auth bindings |

Сопоставь entrypoints, registry, local deployment docs и доступную redacted runtime metadata. Local docs могут устареть: указывай дату и источник. Remote read-only обследование выполняй на host в согласованном scope; если доступа нет, запроси metadata export и продолжай локальную ветвь.

## Manifest и reference closure

Для каждого элемента: id, source_locator, source_hash_if_safe, role, active_state, data_class, owner, decision, target_locator, transformation, incoming_refs, outgoing_refs, evidence, verification_id.

Переписывай структурно: Markdown links/инструкции, YAML/JSON/TOML paths, launcher cwd, volume mounts, arguments, env dependencies и search roots. Глобальная замена Claude→Codex ломает разные сущности: историческое описание, model alias, provider name и executable.

После преобразования проверь:

- обязательные ссылки разрешаются из фактического cwd;
- runtime instructions сохраняют смысл и приоритет;
- source→target уникален; collisions имён/регистра/writers разрешены;
- config fragments не заменили пользовательские настройки целиком;
- scripts/assets доступны без авторских абсолютных путей;
- root-started задача в nested project получает его ограничения;
- updater/installer не возвращает старые vendor paths;
- оставшиеся Claude references классифицированы как control/historical/unresolved.

State переносится отдельно от шаблона: counters и build progress существующего владельца сохраняются или мигрируют по схеме. Новый владелец не наследует чужие private payload. Отсутствующая папка может быть нормой незавершённого onboarding; не создавай native agents из заглушек автоматически.

Отдельно проследи enforcement chain runtime lifecycle → installer → фактический Git hooksPath → pre-push. Учитывай worktrees и чужой pre-push: не перезаписывай его без безопасной композиции. Runtime deny, shell/Git gates и service permissions не взаимозаменяемы.
