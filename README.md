# llm-as-judge

An independent model scores the agent's last turn on what code cannot check: whether the answer follows the request, whether the tone fits, and whether a diff does what the request asked. Below 7 the agent iterates on the feedback. At 7 or above it moves on. `scripts/llm-judge.sh` calls the `claude` CLI with Claude Fable 5.1 and falls back to Opus 5.5. It runs only after `quality-gate` passed for the same turn.

## Install

For Claude Code, copy the skill folders into your personal skills directory:

```bash
git clone https://github.com/orenatobr/llm-as-judge.git
mkdir -p ~/.claude/skills
cp -R llm-as-judge/skills/* ~/.claude/skills/
mkdir -p ~/.claude/scripts
cp llm-as-judge/scripts/* ~/.claude/scripts/
chmod +x ~/.claude/scripts/*.sh
```

Start a new Claude Code session. Skills load from the `description` in each `SKILL.md`, so they trigger on their own and can also be called by name.

Other agents: each skill is a folder with a `SKILL.md` (YAML frontmatter plus Markdown). Tools that read the Agent Skills format can load `skills/` as it is. For the rest, paste the body of the `SKILL.md` into the agent's instruction file.

## Usage

Run `/quality-gate` first, then `/llm-as-judge`. The skill passes `--state`, `--turn` and `--transcript` to the script. A Stop hook can automate both, but this repo does not ship one.

## Requirements

`claude` CLI on PATH, `jq`, `git` and `python3`. Install [quality-gate](https://github.com/orenatobr/quality-gate) as well, because the judge refuses to run until the gate has passed.

## Related skills

[quality-gate](https://github.com/orenatobr/quality-gate) (must run first).

## What is in this repo

| Path | Purpose |
|------|---------|
| `skills/llm-as-judge/` | The `llm-as-judge` skill. |
| `scripts/` | Helper scripts that the skill calls. |
| `CHANGELOG.md` | Notable changes, in Keep a Changelog format. |
| `AGENTS.md` | Rules for AI agents that edit this repo. |

## Contributing

`main` is protected: changes land through a pull request with one approval. See [AGENTS.md](AGENTS.md) for the conventions.

## License

MIT, see [LICENSE](LICENSE).
