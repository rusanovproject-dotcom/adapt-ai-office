# План и исполнение

Порядок: scope/baseline → inventory/reference graph → контракт поведения/data/ownership → план → вертикальный пилот → исправление → волны → offline automation fixtures → host preflight → авторизованный apply/cutover → E2E → observation → acceptance.

Desktop-only scope может исключать серверный перенос, но должен сохранить используемый сервером Claude. Для «весь офис, включая бота» сервер остаётся mandatory при отсутствии доступа.

## Stage record

id, objective, depends_on, executor, source_inputs, source_fingerprint, target_outputs, write_set, preconditions, actions, tests_with_expected_results, evidence_paths, approval_scope, rollback, status, next_action.

Команды проверяются под runtime/version и точный host/path. Если они неизвестны, обозначь template и необходимые значения. «Переписать конфиги» без target/acceptance непригодно для handoff.

## Checkpoint и drift

Append-only журнал: timestamp, stage, action ID, before/after hashes разрешённых файлов, result/evidence, pending work. Checkpoint обновляй атомарно. При restart сверяй реальное состояние; разрешение хранится с operation/host/scope, не универсальным флагом.

Перед live mutation или следующей волной сравни baseline с source/config/version. Drift останавливает зависимый этап: сохрани пользовательские изменения, пересобери diff, повтори связанные tests. Конфликтующие writers выполняются последовательно. Delegation ограничена scope и доступными concurrency slots.

Идемпотентность: одинаковые входы → одинаковые generated payload, без двойных registrations/cron/routes/writers. Timestamp/log evidence исключи из сравнения payload, сохрани в аудите.

## Rollback

До cutover: защищённая private backup нужного state, известный рабочий runner/config и процедура возврата; восстановление проверь на fixture. После cutover откатывай executable/config, сохраняя принятые сообщения, новые факты, delivery records и deletion tombstones. Неизвестный результат side effect → reconciliation по action ID, не повтор вслепую.

Observation window выбирается под рабочий ритм: для существующих scheduled jobs хотя бы одно реальное плановое событие, иначе соответствующий gate pending. Если jobs нет по обследованному inventory, scheduled gate — not_applicable с evidence. Ускоренная fixture проверка таймера не заменяет live tick. Legacy cleanup — после согласованного стабильного периода и разрешения удаления.
