---
name: qwenwork-context-migration
name_en: QwenWork Context Migration
name_zh: QwenWork 跨设备上下文迁移
description: Move QwenWork's accumulated context — long-term memory, user profile, conversation history, and project files — from one machine to another while signed in with the same account. Use when the user wants to continue on a new or different computer, carry their setup across devices, transfer memory/profile, or back up QwenWork data. Produces no secrets.
description_en: Move QwenWork's accumulated context — long-term memory, user profile, conversation history, and project files — from one machine to another while signed in with the same account. Use when the user wants to continue on a new or different computer, carry their setup across devices, transfer memory/profile, or back up QwenWork data. Produces no secrets.
description_zh: 在同一账号登录的前提下，把 QwenWork 积累下来的上下文——长期记忆、用户画像、对话历史和项目文件——从一台设备迁移到另一台设备。当用户想在新电脑上继续、跨设备同步环境、转移记忆/画像或备份 QwenWork 数据时使用。不产生任何机密信息。
argument-hint: Describe the migration, e.g. "carry my memory and chats to my work PC"
argument-hint-en: Describe the migration, e.g. "carry my memory and chats to my work PC"
argument-hint-zh: 描述迁移需求，例如“把我的记忆和聊天记录带到办公电脑”
user-invocable: true
---

# QwenWork Context Migration

Guide for carrying a QwenWork user's working context from one machine to another so the assistant "remembers" them on the new device. Applies when someone installs QwenWork on a second computer (e.g. a work or home PC) and wants to pick up where they left off, or wants a backup/transfer of their setup.

## What travels with the account — and what does not

Signing in with the same account on another machine carries over the *identity* and the *shared credit balance*, plus account-level preferences. It does **not** sync the local transcript or the accumulated memory automatically. Those live as files under the QwenWork resource directory on each machine and must be copied or synced deliberately.

| Item | Where it lives | Synced by account login? |
|------|----------------|--------------------------|
| Identity + shared credits | account (server side) | Yes |
| Long-term memory / user profile | `awareness/main/` (MEMORY.md, USER.md, AGENTS.md, memory/*.md) | No — local files |
| Conversation history / transcripts | QwenWork app data under the resource dir | No — local |
| Installed skills | `skills/` | No — local |
| Project deliverables the work refers to | the user's own folders (e.g. Documents, a project dir) | No — separate |

## Where QwenWork stores data

The resource directory is `{home}/.qwenwork`.

| Environment | Path |
|-------------|------|
| macOS / Linux | `~/.qwenwork/` |
| Windows | `%USERPROFILE%\.qwenwork\` |
| VM / container | `/root/.qwenwork/` |

Within it: `awareness/main/` holds memory and profile, `skills/` holds skills, and app/session data holds transcripts. Ephemeral per-session scratch directories (e.g. `workspace/<chatId>/`) are machine-specific and are not expected to transfer.

## Migration procedure

1. Confirm the source and destination machines and the destination's user-name path (it usually differs from the source).
2. On the destination, install QwenWork and **sign in with the same account** so identity + credits line up. Then fully quit the app before touching files.
3. Copy the context folders from source → destination into the *same relative location* in the destination resource dir:
   - Memory + profile: `awareness/main/`
   - Skills (optional, for tooling parity): `skills/`
   - Transcripts (only if the user wants the chat log too): copy the whole resource dir's app/session data, or at least its sessions store.
4. Bring the actual project files separately — the memory usually references real deliverables that live outside `.qwenwork` (a project folder, Documents, etc.). Those are not inside the resource dir and must be copied on their own.
5. Relaunch QwenWork on the destination. Awareness mode reloads `MEMORY.md`, `USER.md`, `AGENTS.md`, so the assistant resumes with the same knowledge.

## Merge instead of overwrite

If the destination already has its own `.qwenwork`, do **not** overwrite the whole folder — that destroys the other machine's memory. Copy file-by-file and merge. For `awareness/main/`, the daily journals (`memory/*.md`) are safe to union by filename; `MEMORY.md` / `USER.md` / `AGENTS.md` are single curated files — merge their sections manually rather than replacing, or keep the richer copy and reconcile deltas.

## Security rules (read before any cloud step)

These come first because the memory files are the most sensitive things a user owns.

- `MEMORY.md` and `USER.md` frequently contain internal hostnames, IP addresses, system topology, and — worst case — plaintext credentials. Treat them as secrets.
- **Never** upload the awareness/memory files to a public repository, shared drive, pastebin, or third-party sync without redaction. If a sync destination is required, it must be **private**.
- Before publishing anything derived from memory, redact credentials and internal identifiers, and prefer copying generic procedure over copying raw contents.
- Back up the destination's existing files before merging (copy them aside), and verify a moved file actually arrived before reporting success.

## Verification checklist

- [ ] Destination signed in with the same account; credits visible.
- [ ] `awareness/main/MEMORY.md`, `USER.md`, `AGENTS.md` present at the correct path.
- [ ] QwenWork restarted and the assistant demonstrably recalls prior context.
- [ ] Any referenced project files exist on the destination.
- [ ] No secrets pushed to any shared/public location.
