---
description: "整理记忆 — 从日志提炼记忆，合并冲突，整理已有记忆。像大脑在睡眠中整理记忆一样。"
allowed-tools: "Read Write Edit Bash AskUserQuestion"
---

Organize memories — extract from logs, detect conflicts, merge intelligently, confirm before writing. Manages both memory.md (facts & patterns) and persona.md (behavioral profile).

0. Resolve data directory — read `~/.claude/me-config.json` to get `data_dir`. If not found, tell user to run `/me:init` first. Set `DATA_DIR` from config value.

1. Check data initialization. If not initialized, tell user to run `/me:init` first.

2. Collect素材 — read both files + extract ALL from CSV:

```bash
python3 ${CLAUDE_PLUGIN_ROOT}/skills/me/tools/memory_manager.py --action read --data-dir DATA_DIR
python3 ${CLAUDE_PLUGIN_ROOT}/skills/me/tools/memory_manager.py --action read --target-file persona --data-dir DATA_DIR
python3 ${CLAUDE_PLUGIN_ROOT}/skills/me/tools/memory_manager.py --action extract-from-csv --file work_log --format summary --data-dir DATA_DIR
python3 ${CLAUDE_PLUGIN_ROOT}/skills/me/tools/memory_manager.py --action extract-from-csv --file work_log --impact-threshold 3 --format summary --data-dir DATA_DIR
python3 ${CLAUDE_PLUGIN_ROOT}/skills/me/tools/memory_manager.py --action extract-from-csv --file work_log --format patterns --data-dir DATA_DIR
python3 ${CLAUDE_PLUGIN_ROOT}/skills/me/tools/memory_manager.py --action extract-from-csv --file life_log --format summary --data-dir DATA_DIR
python3 ${CLAUDE_PLUGIN_ROOT}/skills/me/tools/memory_manager.py --action extract-from-csv --file life_log --format patterns --data-dir DATA_DIR
```

3. Read the dream prompt: `Read ${CLAUDE_PLUGIN_ROOT}/skills/me/prompts/dream.md`

4. Follow the dream prompt strictly — three phases, for both files:

   **Phase A — Extract**: From CSV data, extract new memories for BOTH files:
   - memory.md: core events, patterns, habits, decisions (with source tags)
   - persona.md: behavioral traits, emotional patterns, decision style (with source tags)
   Tag each extraction with source date `<!-- [来源于 {log} {date}] -->`.

   **Phase B — Merge**: Compare extractions against BOTH existing files. For each:
   - New info (no overlap) → mark for append
   - Confirmed info (already recorded) → skip
   - Conflicting info → flag with `⚠️ 冲突` for user decision
   - Strengthened evidence → mark with `[证据加强 {date}]`
   - Duplicate/stale entries → mark for cleanup

   **Phase C — Confirm**: Show a structured change summary for BOTH files using AskUserQuestion. Include: new additions, evidence upgrades, conflicts needing decision, cleanup operations. Wait for user confirmation before any write.

5. After user confirms (and resolves any conflicts), write results to both files:

```bash
# memory.md
python3 ${CLAUDE_PLUGIN_ROOT}/skills/me/tools/memory_manager.py --action update-section --section '{section_name}' --content '{reorganized_content}' --data-dir DATA_DIR
python3 ${CLAUDE_PLUGIN_ROOT}/skills/me/tools/memory_manager.py --action append-section --section '{section_name}' --content '{new_content}' --data-dir DATA_DIR

# persona.md
python3 ${CLAUDE_PLUGIN_ROOT}/skills/me/tools/memory_manager.py --action update-section --target-file persona --section '{section_name}' --content '{reorganized_content}' --data-dir DATA_DIR
python3 ${CLAUDE_PLUGIN_ROOT}/skills/me/tools/memory_manager.py --action append-section --target-file persona --section '{section_name}' --content '{new_content}' --data-dir DATA_DIR
```

6. Backup:

```bash
python3 ${CLAUDE_PLUGIN_ROOT}/skills/me/tools/version_manager.py --action backup --data-dir DATA_DIR
```
