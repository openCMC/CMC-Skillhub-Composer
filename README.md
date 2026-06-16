# CMC Skillhub Composer

This repository contains the agent-facing skill for using CMC Crypto Skill Hub through MCP.

The skill teaches an agent how to:

- discover CMC Crypto Skill Hub capabilities with `find_skill`;
- validate parameters against each returned `input_schema`;
- execute selected capabilities with `execute_skill`;
- parse wrapped `raw_output` responses;
- handle blocked, partial, stale, and failed tool results;
- render concise Markdown research summaries for crypto market questions, in the user's own language.

## Repository Layout

The installable skill is in the `cmc-skillhub-composer/` subdirectory:

```text
cmc-skillhub-composer/
  SKILL.md
  reference/
    template-overview.md
    template-macro.md
    template-scanner.md
    template-comparison.md
    template-attribution.md
```

The directory name matches the `name` field in `SKILL.md`.

## Compatibility

This skill is compatible with Codex and Claude Code skill layouts when installed as the `cmc-skillhub-composer` skill directory.

| Host | Install path |
|---|---|
| Codex | `${CODEX_HOME:-$HOME/.codex}/skills/cmc-skillhub-composer/` |
| Claude Code | `$HOME/.claude/skills/cmc-skillhub-composer/` |

Install the contents of this directory into the skill folder:

```text
cmc-skillhub-composer/
```

The installed folder must remain:

```text
cmc-skillhub-composer
```

Do not install the repository root as the skill. The repository root contains project documentation and is not the skill directory.

`SKILL.md` is the entry point; the `reference/` directory holds the output report templates the skill loads on demand (overview, macro, scanner, comparison, attribution). Both hosts load the skill from `SKILL.md`. Configure the CMC Crypto Skill Hub MCP server separately in the host so that its `find_skill` and `execute_skill` tools are available; the skill resolves whatever id the connected server is registered under and does not require a fixed server name.

## Codex Skill Installer

Use the GitHub directory URL:

```text
$skill-installer install https://github.com/openCMC/CMC-Skillhub-Composer/tree/main/cmc-skillhub-composer
```

Equivalent explicit installer arguments:

```bash
python install-skill-from-github.py \
  --repo openCMC/CMC-Skillhub-Composer \
  --path cmc-skillhub-composer
```

## Send To Agent

Copy the prompt below into Codex, Claude Code, or another local coding agent:

```text
Install the CMC Skillhub Composer skill from https://github.com/openCMC/CMC-Skillhub-Composer/tree/main/cmc-skillhub-composer.

Use the `cmc-skillhub-composer/` directory as the skill source.

Steps:

1. Clone the repository into a temporary directory.
2. Verify that `cmc-skillhub-composer/SKILL.md` exists and that its frontmatter name is `cmc-skillhub-composer`.
3. Install `cmc-skillhub-composer/` as folder `cmc-skillhub-composer`.
4. If you are running as Codex, install to `${CODEX_HOME:-$HOME/.codex}/skills/cmc-skillhub-composer/`.
5. If you are running as Claude Code, install to `$HOME/.claude/skills/cmc-skillhub-composer/`.
6. If both `$HOME/.codex` and `$HOME/.claude` exist, install to both locations.
7. Copy only the contents of `cmc-skillhub-composer/`, preserving the `reference/` subdirectory.
8. If an existing `cmc-skillhub-composer` folder is present, move it to a timestamped backup before installing.
9. Do not install the repository root as the skill.
10. After installation, report the installed path or paths and remind me to restart the host agent so it can load the skill.

Use shell commands where appropriate. Stop and explain the error if the repository cannot be cloned or the expected skill directory is missing.
```

## Manual Install

For Codex:

```bash
tmp_dir="$(mktemp -d)"
git clone --depth 1 https://github.com/openCMC/CMC-Skillhub-Composer "$tmp_dir"
install_dir="${CODEX_HOME:-$HOME/.codex}/skills/cmc-skillhub-composer"
if [ -e "$install_dir" ]; then
  mv "$install_dir" "${install_dir}.bak.$(date +%Y%m%d%H%M%S)"
fi
mkdir -p "$install_dir"
cp -R "$tmp_dir/cmc-skillhub-composer/." "$install_dir/"
```

For Claude Code:

```bash
tmp_dir="$(mktemp -d)"
git clone --depth 1 https://github.com/openCMC/CMC-Skillhub-Composer "$tmp_dir"
install_dir="$HOME/.claude/skills/cmc-skillhub-composer"
if [ -e "$install_dir" ]; then
  mv "$install_dir" "${install_dir}.bak.$(date +%Y%m%d%H%M%S)"
fi
mkdir -p "$install_dir"
cp -R "$tmp_dir/cmc-skillhub-composer/." "$install_dir/"
```

## License

Licensed under the Apache License, Version 2.0. See [LICENSE](LICENSE) and [NOTICE](NOTICE) for details. The skill bundle in `cmc-skillhub-composer/` carries its own copy of the license so it stays licensed when installed standalone.
