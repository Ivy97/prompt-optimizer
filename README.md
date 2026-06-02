# Prompt Optimizer Skill

A bilingual Codex skill for optimizing, rewriting, diagnosing, iterating, evaluating, and creating prompts.

It supports:

- system/developer prompts;
- user/task prompts;
- variable templates;
- multi-message prompts;
- tool/function prompts;
- coding-agent prompts;
- image-generation prompts;
- prompt evaluation and compare-tests;
- model adaptation for OpenAI/GPT, Claude, Gemini, DeepSeek, Qwen, Llama/Mistral, and local models.

The skill merges the local prompt-optimizer methodology based on `linshenkx/prompt-optimizer` with the broader Codex `$prompt-optimizer` workflow.

## Structure

```text
prompt-optimizer/
├── SKILL.md
├── CONFIGURATION.md
├── agents/openai.yaml
├── references/
│   ├── methodology.md
│   ├── model-adaptation.md
│   └── playbook.md
└── templates/
    ├── evaluate.md
    ├── iterate.md
    ├── optimize-system.md
    └── optimize-user.md
```

## Installation

See [CONFIGURATION.md](./CONFIGURATION.md) for Codex and Claude / Claude Code installation, update, and public-release safety instructions.

## Example

```text
用 $prompt-optimizer 优化这个英文论文润色 prompt，要求保留 LaTeX 命令和数学符号。
```

```text
Use $prompt-optimizer to rewrite this system prompt for an OpenAI coding agent.
```
