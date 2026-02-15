# Ralph Audit Commands

> **Disclaimer:** I made these for my own use. Sharing in case others find them useful.
> There might be bugs. Feel free to open issues or fork and customize.

Iterative security audit skills for AI coding agents. Works with **Claude Code**, **OpenClaw**, and any **[AgentSkills](https://agentskills.io)-compatible** platform.

## Skills

| Skill | Iterations | Duration | Use Case |
|-------|------------|----------|----------|
| `/ralph-quick` | 10 | ~5-10 min | Quick pre-deploy check |
| `/ralph-security` | 100 | ~30-60 min | Standard audit |
| `/ralph-ultra` | 1,000 | ~4-8 hours | Deep dive |
| `/ralph-promax` | 10,000 | ~2-5 days | Maximum paranoia |

## Installation

### Claude Code (Skills)

```bash
git clone https://github.com/dorukardahan/ralph-audit-commands.git
cp -r ralph-audit-commands/ralph-* ~/.claude/skills/
```

Or place them in your project's `.claude/skills/` directory.

### Claude Code (Legacy Commands)

If you prefer the old flat-file format, copy from `archive/`:

```bash
cp ralph-audit-commands/archive/ralph-*.md ~/.claude/commands/
```

### OpenClaw

```bash
git clone https://github.com/dorukardahan/ralph-audit-commands.git
cp -r ralph-audit-commands/ralph-* ~/.openclaw/skills/
# Or into your workspace: <workspace>/skills/
```

Skills are picked up on the next session.

### ClawHub (coming soon)

```bash
clawhub install ralph-quick
clawhub install ralph-security
clawhub install ralph-ultra
clawhub install ralph-promax
```

## How It Works

Each skill includes an **Execution Engine** that forces iterative behavior:

```
STATE → PHASE → ACTION → VERIFY → REPORT → SAVE → INCREMENT → CONTINUE
```

**Key principles:**
- **One check per iteration** (not all at once)
- **Evidence-based verification** before reporting FAIL
- Progress shown as `[X/N]`
- Findings saved to `.ralph-report.md`
- Checkpoint & resume for long audits

## Skill Structure

Each skill follows the [AgentSkills](https://agentskills.io) open standard:

```
ralph-security/
├── SKILL.md              # Main instructions (required)
└── references/           # Loaded on-demand (progressive disclosure)
    └── severity-guide.md # Triage, confidence, false positive guide
```

Larger skills (ultra, promax) include additional reference files:

```
ralph-promax/
├── SKILL.md
└── references/
    ├── severity-guide.md   # Triage & confidence
    ├── personas.md         # 8 expert personas
    ├── attack-patterns.md  # OWASP payloads & checklists
    └── phase-details.md    # Full 16-phase breakdown
```

This uses **progressive disclosure** — SKILL.md stays focused on core instructions, detailed references are loaded only when needed by the agent.

## Example Output

```
╔══════════════════════════════════════════════════════════════╗
║ [SEC-42/100] Phase 2: OWASP Top 10 Analysis                ║
║ Check: SQL Injection in user input handlers                 ║
╠══════════════════════════════════════════════════════════════╣
║ Target: src/api/users.py:127                                ║
║ Result: FAIL                                                ║
║ Confidence: VERIFIED                                        ║
║ Severity: HIGH                                              ║
╠══════════════════════════════════════════════════════════════╣
║ Finding: User input directly concatenated in SQL query      ║
║ Fix: Use parameterized queries or ORM                       ║
╠══════════════════════════════════════════════════════════════╣
║ Progress: [████████████████░░░░░░░░░░░░░░] 42%              ║
╚══════════════════════════════════════════════════════════════╝
```

## What It Checks

- **OWASP Top 10** — SQLi, XSS, IDOR, SSRF, etc.
- **Secrets** — Hardcoded API keys, passwords, tokens
- **Containers** — Docker security, non-root, capabilities
- **Supply Chain** — Dependency CVEs, outdated packages
- **Auth & JWT** — Token security, session management
- **Infrastructure** — Ports, firewall, TLS/SSL
- **CI/CD** — Pipeline security, secret management
- **AI/RAG** — Prompt injection, context poisoning (promax)

## Parameters

| Parameter | Description | Example |
|-----------|-------------|---------|
| `--iterations` | Override iteration count | `--iterations=50` |
| `--focus` | Target specific area | `--focus=owasp` |
| `--phase` | Start from specific phase | `--phase=3` |
| `--resume` | Continue from checkpoint | `--resume` |

**Note:** Parameters are interpreted by the AI agent as instructions, not parsed as formal CLI arguments.

## Checkpoint & Resume

| Skill | Save Frequency |
|-------|----------------|
| ralph-quick | End only |
| ralph-security | Every 10 iterations |
| ralph-ultra | Every 50 iterations |
| ralph-promax | Every 100 iterations |

## Reading Your Report

### Triage Priority
1. **CRITICAL + VERIFIED** — Fix immediately
2. **HIGH + VERIFIED/LIKELY** — Fix before deployment
3. **Any + PATTERN_MATCH** — Verify manually
4. **NEEDS_REVIEW** — Low priority

### Common False Positives
- Library-handled concerns (jose, bcrypt, passport)
- Environment-gated (dev-only, not production)
- Database-protected (UNIQUE constraints handle race conditions)
- Pattern match only (keyword, not actual vulnerability)

## Compatibility

| Platform | Install Path | Status |
|----------|-------------|--------|
| Claude Code (Skills) | `~/.claude/skills/ralph-*` | ✅ Full support |
| Claude Code (Commands) | `~/.claude/commands/` | ✅ Legacy (`archive/`) |
| OpenClaw | `~/.openclaw/skills/ralph-*` | ✅ Full support |
| ClawHub | `clawhub install ralph-*` | 🔜 Coming soon |
| AgentSkills-compatible | Any skills directory | ✅ Standard format |

## Migration from v1 (Commands)

If you were using the old flat `.md` files in `~/.claude/commands/`:

1. Remove old files: `rm ~/.claude/commands/ralph-*.md`
2. Copy new skills: `cp -r ralph-*/  ~/.claude/skills/`
3. Everything works the same — same `/ralph-*` slash commands

Old files are preserved in `archive/` for reference.

## When to Use Which

| Scenario | Recommended |
|----------|-------------|
| Before deploying to production | `/ralph-quick` |
| Weekly security check | `/ralph-security` |
| New project onboarding | `/ralph-security` |
| Before major release | `/ralph-ultra` |
| Compliance audit preparation | `/ralph-ultra` |
| Security incident investigation | `/ralph-promax` |
| Maximum paranoia mode | `/ralph-promax` |

## Security

See [SECURITY.md](SECURITY.md) for vulnerability reporting guidelines.

## Contributing

- Found a bug? Open an issue
- Have improvements? Send a PR
- Want to customize? Fork and make it yours

## License

MIT
