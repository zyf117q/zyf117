---
name: gitee-push
description: Commit and push the current project to the Gitee remote gitee.com/whalefox/yl2. Use when the user asks to 提交项目, 提交代码到 Gitee, push or upload this project to Gitee. Do not use for other remotes, other hosts, or force-pushing.
---

# Gitee Push

Commit and push the current workspace (c:\h) to the Gitee repo `whalefox/yl2`. Shell is Windows PowerShell — do not use bash heredocs.

## Remote

- SSH (preferred): `git@gitee.com:whalefox/yl2.git`, key at `~/.ssh/id_ed25519`
- HTTPS fallback: `https://gitee.com/whalefox/yl2.git`, needs a Gitee private token as the password — ask the user for it

## Steps

1. Verify SSH access: `ssh -T git@gitee.com`. Gitee answers with a success greeting even though exit code is non-zero; a `Hi xxx` greeting means OK. If the host key is unknown, run `ssh-keyscan gitee.com >> ~/.ssh/known_hosts` first. If SSH fails entirely, switch origin to the HTTPS URL and ask the user for a token.
2. Init if needed: if not a git repo, run `git init` then `git branch -M master` (Gitee default branch is `master`, not `main`).
3. Ensure identity: `git config user.name` / `user.email` must be set (repo-local is fine). If missing, ask the user or reuse the Gitee username.
4. Stage explicit paths with `git add <paths>`; never commit secrets (`.env`, credentials, `id_ed25519`, `*.key`).
5. Commit with a PowerShell here-string:

```powershell
git commit -m @"
<message>
"@
```

6. Add remote if missing: `git remote add origin git@gitee.com:whalefox/yl2.git` (or `git remote set-url` if origin exists but points elsewhere).
7. Push: `git push -u origin master`. If rejected because the remote repo was created with an auto-initialized README, run `git pull --rebase origin master` then push again. Never force-push unless the user explicitly asks.
