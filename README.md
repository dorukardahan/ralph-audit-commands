# Ralph Audit Commands

> **Disclaimer:** I made these for my own use. Sharing in case others find them useful.
> There might be bugs. Feel free to open issues or fork and customize.

Iterative security audit commands for [Claude Code](https://docs.anthropic.com/en/docs/claude-code).

## Commands

| Command | Iterations | Duration | Use Case |
|---------|------------|----------|----------|
| `/ralph-quick` | 10 | ~5-10 min | Quick pre-deploy check |
| `/ralph-security` | 100 | ~30-60 min | Standard audit |
| `/ralph-ultra` | 1,000 | ~4-8 hours | Deep dive |
| `/ralph-promax` | 10,000 | ~2-5 days | Maximum paranoia |

## Installation

Copy the files to `~/.claude/commands/`:

```bash
git clone https://github.com/dorukardahan/ralph-audit-commands.git
cp ralph-audit-commands/*.md ~/.claude/commands/
```

## How It Works

Each command includes an **Execution Engine** that forces iterative behavior:

```mermaid
flowchart TB
    A[STATE] --> B[PHASE]
    B --> C[ACTION]
    C --> D[REPORT]
    D --> E[SAVE]
    E --> F{iter ≤ N?}
    F -->|Yes| A
    F -->|No| G[FINAL]
```

**Key principles:**
- **One check per iteration** (not all at once)
- Progress shown as **[X/N]**
- Findings saved to **`.ralph-report.md`**

## Command Hierarchy

```mermaid
flowchart TB
    Q["/ralph-quick<br/>10 iter • ~5-10 min"]
    S["/ralph-security<br/>100 iter • ~30-60 min"]
    U["/ralph-ultra<br/>1,000 iter • ~4-8 hours"]
    P["/ralph-promax<br/>10,000 iter • ~2-5 days"]

    Q -->|more depth| S
    S -->|deeper| U
    U -->|max paranoia| P
```

## What It Checks

- **OWASP Top 10** - SQLi, XSS, IDOR, SSRF, etc.
- **Secrets** - Hardcoded API keys, passwords, tokens
- **Containers** - Docker security, non-root, capabilities
- **Supply Chain** - Dependency CVEs, outdated packages
- **Auth & JWT** - Token security, session management
- **Infrastructure** - Ports, firewall, TLS/SSL
- **CI/CD** - Pipeline security, secret management

## Parameters

Each command supports customization flags:

| Parameter | Description | Example |
|-----------|-------------|---------|
| `--iterations` | Override iteration count | `/ralph-security --iterations=50` |
| `--focus` | Target specific area | `/ralph-ultra --focus=owasp` |
| `--phase` | Start from specific phase | `/ralph-promax --phase=3` |
| `--resume` | Continue from checkpoint | `/ralph-security --resume` |

**Focus areas:** `recon`, `owasp`, `auth`, `secrets`, `infra`, `code`, `supply-chain`, `compliance`, `all`

## Checkpoint & Resume

Long audits save progress automatically to `.ralph-report.md`:

| Command | Save Frequency |
|---------|----------------|
| `/ralph-quick` | End only |
| `/ralph-security` | Every 10 iterations |
| `/ralph-ultra` | Every 50 iterations |
| `/ralph-promax` | Every 100 iterations |

**If interrupted or context limit reached:**
```bash
# Resume from last checkpoint
/ralph-ultra --resume

# Or start specific phase
/ralph-promax --phase=5
```

## Auto-Detection

Commands automatically detect your environment on first iteration:

- **Stack**: `package.json`, `requirements.txt`, `go.mod`, `pyproject.toml`
- **Infrastructure**: `Dockerfile`, `docker-compose.yml`, Kubernetes manifests
- **CI/CD**: GitHub Actions, GitLab CI, etc.

No configuration needed — just run the command in your project directory.

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

## Contributing

- Found a bug? Open an issue
- Have improvements? Send a PR
- Want to customize? Fork and make it yours

## License

MIT
