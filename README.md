# QwenWork Skills — Context Migration

A reusable [QwenWork](https://docs.qwenwork.ai) **agent skill** that walks through carrying a user's accumulated context — long-term memory, user profile, conversation history, and project files — from one machine to another while signed in with the same account.

## Contents

```
skills/
└── qwenwork-context-migration/
    ├── SKILL.md                # the skill (generic procedure, no secrets)
    └── .skill-metadata.yaml    # recommended prompts shown in the QwenWork UI
```

## What the skill covers

- Which parts of a QwenWork setup travel with the account (identity, shared credits) and which stay local (memory, transcripts, skills).
- Where QwenWork stores data per platform (`~/.qwenwork`, `%USERPROFILE%\.qwenwork`, `/root/.qwenwork`).
- A step-by-step migration procedure, plus **merge-not-overwrite** guidance for machines that already have their own data.
- Security rules: the memory files often contain internal hostnames, IPs, and credentials, so they must **never** be pushed to a public/shared destination without redaction.

## Install on another machine

Copy the skill folder into your QwenWork skills directory:

- macOS / Linux: `~/.qwenwork/skills/`
- Windows: `%USERPROFILE%\.qwenwork\skills\`
- VM / container: `/root/.qwenwork/skills/`

```bash
git clone <this-repo-url>
cp -r qwenwork-skills/skills/qwenwork-context-migration ~/.qwenwork/skills/
```

Relaunch QwenWork; the skill is then available by name `qwenwork-context-migration` (or invoked when you ask to migrate/carry over your setup).

## No secrets

This repository intentionally contains only the generic procedure. It does **not** include any `awareness/` memory files, transcripts, credentials, hostnames, or IP addresses.
