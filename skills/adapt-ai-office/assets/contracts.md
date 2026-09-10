# Контракты результатов

Work root конкретной адаптации отделён от исходного офиса. Markdown tables допустимы вместо JSON при тех же полях. Для отсутствующих подсистем not_applicable с evidence вместо пустых реестров.

## adaptation-report.md

- Scope: source roots/hosts, runtime/surface/version, owner, authorized actions, data boundaries.
- 12-layer matrix: layer / item ID / observed-declared-inferred-unknown / keep-adapt-drop-unknown / owner / evidence / target / test ID.
- Coverage: discovered/represented/unresolved counts active agents, skills, hooks, jobs, capabilities, entrypoints; необследованные hosts.
- Dependency graph entry→delivery/memory, findings с evidence/affected stages.
- Приложения: file-map.md, routing-matrix.md, automation-inventory.md по применимости.

## adaptation-plan.md

Инварианты, first-win acceptance, baseline fingerprint, candidate/apply roots, dependency-ordered stage records, первый пилот, команды/кейсы и expected results, local/remote gates, approvals, rollback/compensation, observation.

## automation-inventory.md

На service/job/bot: id, host_alias, evidence_time, declared_or_observed, required_or_optional, service_manager/name, enabled_state, service_user, cwd, runtime_surface/version, executable_locator, runtime_home_ref, provider_selection, fallback_policy, config_locator, secret_binding_ref_redacted, inbound_transport, session_store_owner, queue/offset_owner, schedule/timezone, retry/timeout/missed_run, lock/dedup, outgoing_delivery, capabilities, health_check, migration_action, test_ids, blocker.

Cron wrapper — отдельный узел с переходом к provider. «Бот есть» без цепочки недостаточно.

## verification-report.md

Case: case_id, stage_id, scenario, input_fixture, preconditions, expected, observed, evidence_level, command_or_invocation, exit_or_result, evidence_locator, runtime_host_version, status, limitation.

- pass | fail | blocked | not_run | not_applicable; blocked/not_run не засчитываются pass.
- Доли отдельно static, fixture, fresh-model, native-local, remote-live; знаменатель — все mandatory применимые cases.
- Critical safety/route/mutation требуют 100%; mandatory P1 блокирует затронутый release scope.
- Independent findings/resolution; package readiness отдельно от readiness адаптации.

## execution-log.md и checkpoint.md

Stage/action ID, fingerprint, фактическая операция, evidence, dirty changes, pending mutation ID, next_action, permissions scope. Checkpoint указывает последний согласованный шаг и журнал.

## handoff.md

Что проверенно работает; scope/package version; completed/pending/blocked; точный next step; чего ждём от кого; evidence/rollback; ограничения результата. Установка скилла, локальная адаптация и production switching — разные результаты.
