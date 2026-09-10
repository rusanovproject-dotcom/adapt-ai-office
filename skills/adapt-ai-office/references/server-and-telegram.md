# Серверные автоматизации и существующий Telegram-бот

Загружай при любом заявленном или найденном внешнем исполнении. Адаптер владеет inventory, планом, handoff и проверкой сквозного результата; deployment может выполнять отдельный специалист. Тестирование самого скилла не даёт разрешения подключаться к действующим клиентским серверам.

## 1. Найди всю цепочку

Заполни automation inventory из assets/contracts.md. Разделяй transport (Telegram polling/webhook), runner (CLI/SDK/app-server/custom), provider (Claude/Codex), scheduler и process manager. Они могут находиться в разных repos, hosts, containers и user contexts.

Обследуй по применимости systemd units/timers, PM2, Docker Compose, cron пользователей, launchd, CI, app scheduler, n8n, watchdog и ручные wrappers. Для каждого перехода проверь cwd, mounted brain, executable, аргументы без секретов, config/runtime home, input/output protocol и state owner. Не печатай env, pm2 jlist, docker inspect, process argv или логи целиком: в них бывают credentials; выбирай allowlisted metadata.

Отметь прямые вызовы Claude, косвенные shell/tmux wrappers, fallback, stop hooks, scheduled prompts, reboot startup и updater. Local deployment README подтверждает намерение, не live state. Недоступный обязательный host = not_inspected, а не «интеграций нет».

## 2. Проверь контракт целевого runner

- Установленная версия/интерфейс на target host; Codex CLI, SDK и app-server имеют разные event/output/session contracts. Не заменяй имя executable в Claude argv.
- Service user, рабочая директория, runtime home, PATH, interpreter, auth method и права; interactive login владельца не доказывает доступ сервиса. Сохрани выбранный subscription/API способ; новая платная зависимость или смена auth — отдельное решение.
- Config, agents, skills, MCP доступны именно процессу сервиса. Не копируй auth store владельца или весь environment; secret bindings остаются host-local.
- Expected input: prompt, attachments, safe context, cwd. Expected output: final result, progress, structured tool events, error, exit. В CLI JSONL progress events не должны стать текстом ответа пользователю. Empty output, malformed stream и nonzero exit — ошибки, даже если процесс стартовал.
- Timeout, cancellation и cleanup дочерних процессов; bounded retries только для повторяемых операций. После неизвестного результата внешнего действия требуется reconcile.
- Effective sandbox/approval проверяй на реальном headless пути. Невозможный интерактивный approval возвращается как needs_owner_action, не обходится широкими permissions.
- Для Codex-only требуй actual_provider evidence на каждом пути. При missing/expired Codex — контролируемая ошибка, не silent Claude fallback. Dual-runtime fallback должен быть явно согласован, наблюдаем и не считаться Codex parity.

Если backend не поддерживает нужный protocol/tools/streaming, добавь отдельную engineering stage с контрактом/tests. Отсутствие адаптера нельзя закрыть переименованием файлов инструкций.

## 3. Память, сессии и получатели

Различай durable memory, provider session IDs, queue offsets, dedup state и delivery journal. Claude session ID не передаётся в Codex resume. Создай новую native session и разрешённую handoff capsule; raw transcripts автоматически не мигрируют. Сохрани соответствие bot/user/chat/topic → office/brain → runtime session. Проверяй два пользователя/две services на отсутствие пересечения state.

Allowlist и authorization проверяются до spawn/tool. Сохраняй bot identity, адресатов, topics и transport по умолчанию; смена движка не требует нового bot token. Callback, voice/document и MCP checks выполняются для возможностей из baseline. Optional unsupported объясняй; required unsupported блокирует ветвь.

## 4. Расписания

Каждое задание: trigger, timezone/DST, owner, command chain, cwd/config, enabled state, missed-run policy, lock, queue, retry, timeout, idempotency key, destination. Сверь external cron с in-process scheduler, чтобы не перенести одно задание дважды. Зафиксируй restart/reboot и отсутствие revival старого Claude runner через watchdog.

## 5. Безопасный пилот и cutover

1. Offline fixtures с synthetic input, stub transport и outbound sink; никакого polling/webhook registration или отправок реальному боту. Hook fixtures не запускают live jobs и не пишут state исходного офиса.
2. Target-host preflight в согласованном scope под service identity: runtime/version/auth status без credentials, files/capabilities, permission behavior, pinned release/config. Smoke без внешних side effects.
3. Зафиксируй release refs, защищённую state backup, pending work, rollback owner/commands, stop thresholds, observation window и точное разрешение live cutover.
4. Согласованно pause old scheduler/consumer, drain или reconcile in-flight, сохранить offsets/dedup/durable events, предотвратить auto-respawn.
5. Переключить runner/config, начать один target consumer, затем возобновить один scheduler. Для webhook проверить ownership/endpoint и retry policy; для polling — единственного владельца updates. Shadow не получает реальные updates и не отправляет сообщения от того же бота.
6. Авторизованный canary: input accepted → correct brain/provider → expected result → delivery acknowledged правильным recipient/topic. Ответ модели и receipt доставки — отдельные gates; outbound failure не green.
7. Наблюдать выбранное окно, включая реальный scheduled tick и restart при разрешённой проверке. Rollback: остановить target/children и scheduler, вернуть pinned runner/config, сохранить новые durable events, восстановить ровно одного consumer. Уже обработанное действие не повторять.

Защитись от shared checkout: изменение revision для одного бота не должно незаметно переключить соседние services; используй изолированные release dirs или согласованный общий rollout.

## Acceptance

Все обязательные integration IDs закрыты evidence целевого уровня. Offline pass доказывает только fixture. При отсутствии remote access/разрешения/scheduled observation оставь точный blocker и готовые operator steps; статус всего заявленного переноса не complete.
