# Synthetic redacted deployment manifest
Declared, not live inspected. Host alias client-vps. Target Codex-only. Existing identity bot-A stays.
systemd bot.service: service user officebot; cwd /srv/office/releases/r17/brain; runner /srv/office/releases/r17/bin/bot-runner; runtime home /var/lib/officebot/runtime-profile (different from interactive owner).
Transport polling, owner bot.service, allowlist ref allowlist-A, topic routing map-A. Two conversations chat-A and chat-B; state keyed by bot/chat/topic; Claude session IDs c-A/c-B. Durable memory safe fact in brain/knowledge; state /var/lib/officebot/state; credentials reference auth-binding-A, do not read values.
Runner parser expects Claude stream JSON. choose_engine prefers configured engine then falls back to any installed CLI; both CLIs declared installed, versions/auth unknown. It spawns MCP writer on permitted requests. Telegram output delivery acknowledgement may fail after engine reply.
digest.timer 08:30 Europe/Berlin -> digest.service -> /srv/office/releases/r17/bin/digest-wrapper -> claude -p -> delivery sink bot-A topic digest. In-process daily scheduler may ALSO be enabled (unknown). Job pending job-41; durable processed ledger exists.
watchdog.service restarts bot.service pointing at r17 after reboot. Another bot-B shares r17 checkout but uses own brain.
No remote access granted. No new bot token, API billing, server or transport requested. Can prepare fixture-based plan only.
