# Роутинг и состояние

Проверяй discovery самого adapt-ai-office и маршрутизацию переносимого офиса отдельно. Explicit invocation не доказывает implicit discovery; подсказка hook не доказывает вызов нужного исполнителя.

Таблица: route_id, entrypoint, prompt_or_event, state_predicate, priority, expected_executor, required_context, write_set, result, fallback, test_ids. Сохрани действующий owner intent; не навязывай всем офисам порядок клиентского шаблона.

Типы: agent alias; явная команда/skill; смысл задачи; upload; scheduler; продолжение этапа; проект; memory write; fallback. Сверь registry с реализацией. Markdown persona не равна native subagent, карта со stub не означает доступного исполнителя.

## Значимые состояния

- Новый офис, частичный onboarding, готовый офис; последняя валидная запись важнее старой открытой строки.
- Живой агент, stub, disabled/removed; заглушку нельзя изображать доступной ролью.
- Explicit specialist против intent, файл плюс команда, два skills, два проекта.
- Незавершённая работа против нового запроса; не продолжай старый этап вопреки новому намерению.
- Fresh/stale фокус, timezone/day boundary, silence, cadence, дедуп между sessions/hosts.
- Вопрос/диагностика против изменения/отправки/деплоя.

Custom route: frontmatter и @file — контракты исходного офиса, не универсальные возможности Codex. Перенеси смысл в native механизм или явную загрузку; сохрани custom router metadata. Если hook советует маршрут, проверяй его output и фактическое решение агента.

## Тесты на каждый активный критичный route

1. Explicit и естественная формулировка.
2. Near-miss: отрицание, цитата команды, часть другого слова.
3. Два совпадения: baseline priority либо один вопрос.
4. Missing/stub executor: честный fallback, независимая полезная часть, отсутствие ложного запуска.
5. Новый/частичный/готовый, restart, повтор события и другая cwd.
6. Correct context/owner, deliverable, разрешённый write_set, memory/index update.
7. Рост: новый fixture skill/role/project в каноне становится доступным после предусмотренного reload/build без ручной правки второго списка.

Пример прогрессивной сборки: «привет» нового офиса → первый этап; частичного → актуальный checkpoint; готового → без повторного интервью. Боевая просьба во время onboarding исполняется по исходному first-win приоритету. «Атлас, набросай пост» со stub не выдумывает роль, но сохраняет рабочий fallback. «Сделай поставку» не равно «сделай пост». «Порядок в клиентах» и «в файлах офиса» могут иметь разных владельцев.

Telegram: user/chat/topic routing, allowlist, callbacks, text/voice/file только в заявленных возможностях, dedup update_id, изоляция двух пользователей. Недоступная транскрибация объявляется до обещания анализа.

## Handoff

Передавай stage ID, scope, входы, ограничения, permissions, outputs/tests, write_set, resume/rollback, путь возврата evidence. Родитель проверяет результат; sent_to_specialist не равно passed. При недоступном специалисте подготовь безопасную часть и точный blocker с ролью/операцией.
