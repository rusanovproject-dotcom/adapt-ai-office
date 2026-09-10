# Synthetic existing office (no real client data)
Owner: synthetic consultant Mira; runtime Claude Code; target Codex, same owner.
Layout: workspace/CLAUDE.md imports @crew/registry.md; crew/personas/director/core.md is INLINE director, never spawned. crew/registry.md: Director active; Atlas stub. work/projects/CLAUDE.md forbids publishing drafts.
state/build-log.md latest valid event: stage 4 open; stage 1-3 complete. profile says owner ready; knowledge/summary.md has safe fact "prefers concise Russian". Personal data path people-store is a symlink outside root: only redacted pointer permitted.
.claude/skills/office-firstwin/SKILL.md frontmatter route: "напиши пост|сделай черновик => first-win". Writes results/ and updates INDEX.
.claude/hooks/route.sh reads .claude/skills/*/SKILL.md route metadata using CLAUDE_PROJECT_DIR or cwd. Explicit active persona beats intent; stub does not suppress useful intent. "сделай поставку" must not match "пост".
Onboarding: greeting resumes stage 4; production request during onboarding has first-win priority.
session-load hook writes state/session-counter; installer reads .claude/hooks/pre-push.sh, installs into git configured hooksPath. Existing custom pre-push must be preserved. Two session-start handlers can run concurrently.
update-office updates .claude/* and root CLAUDE.md while preserving owner block; no Codex regeneration exists.
No declared server for this fixture. Do not read actual private files, call external services or run startup hooks.
