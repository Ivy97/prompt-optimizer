# Configuration and Installation

This repository is a public, reusable prompt-optimization skill package. It does not require private files, local machine paths, API keys, or model credentials.

## 1. Clone

```bash
git clone https://github.com/Ivy97/prompt-optimizer.git
cd prompt-optimizer
```

## 2. Install as a Codex Skill

Recommended installation through Codex's skill installer:

```bash
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py \
  --repo Ivy97/prompt-optimizer \
  --path . \
  --name prompt-optimizer
```

Then restart Codex so the new skill is discovered.

If the destination already exists, update it by removing or backing up the old skill directory first:

```bash
rm -rf ~/.codex/skills/prompt-optimizer
```

For a custom Codex home, set `CODEX_HOME` before installing:

```bash
export CODEX_HOME="$HOME/.codex"
```

## 3. Install as a Claude / Claude Code Skill

Clone or copy this repository into the Claude skills directory:

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/Ivy97/prompt-optimizer.git ~/.claude/skills/prompt-optimizer
```

Restart Claude Code after installation.

## 4. Manual Local Installation

You can also copy the full repository into a skills directory:

```bash
mkdir -p ~/.codex/skills
cp -R prompt-optimizer ~/.codex/skills/prompt-optimizer
```

The repository root contains `SKILL.md`, so the full folder should be copied as the skill.

## 5. Usage Examples

```text
用 $prompt-optimizer 优化这个英文论文润色 prompt，要求保留 LaTeX 命令和数学符号。
```

```text
Use $prompt-optimizer to rewrite this tool-calling prompt for OpenAI models.
```

```text
用 $prompt-optimizer 的 evaluate 模板比较原始 prompt 和优化 prompt 的输出差异。
```

## 6. Public-Release Safety

Before publishing or updating this repository, scan for local paths and credentials:

```bash
rg -n --hidden --glob '!**/.git/**' 'E:\\|/mnt/|/Users/|C:\\Users|/home/|Desktop|token|secret|password|api[_-]?key|ghp_|github_pat_|sk-' .
```

Do not commit:

- `.env` files or local model/API configuration;
- GitHub tokens, OpenAI keys, SSH private keys, or service credentials;
- local absolute paths, personal workspace names, or manuscript-specific private data;
- unpublished reviewer comments, author identities, institution-only documents, or proprietary prompts.

## 7. Updating the Skill

When changing the skill:

1. update `SKILL.md`, `references/`, or `templates/`;
2. keep placeholders such as `{variable}`, `{{variable}}`, XML tags, JSON keys, and schemas intact;
3. validate with Codex's skill validator if available;
4. update public documentation if the invocation method or supported resources change.
