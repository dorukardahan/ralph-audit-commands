---
name: ralph-quick
description: "Fast security spot-check with 10 iterations for pre-deployment verification. Covers secrets scanning, OWASP basics, authentication, rate limiting, and container security. Use for quick pre-deployment checks or daily security spot-checks (5-10 minutes)."
---

# Ralph Quick (10 Iterations)

Fast security spot-check for pre-deployment verification or daily security hygiene. Covers the most critical security checks in a condensed format.

## Usage

```
/ralph-quick [--focus=AREA]
```

**Default:** 10 iterations, all focus areas

## Parameters

| Parameter | Default | Options |
|-----------|---------|---------|
| `--iterations` | 10 | 1-20 |
| `--focus` | all | sync, secrets, owasp, infra, all |

---

## EXECUTION ENGINE (MANDATORY)

### Iteration Loop Protocol

YOU MUST follow this loop for EVERY iteration:

```
┌─────────────────────────────────────────────────────────┐
│  RALPH QUICK - ITERATION LOOP                          │
├─────────────────────────────────────────────────────────┤
│  1. STATE: Read current iteration (start: 1)           │
│  2. ACTION: Perform ONE check from current phase       │
│  2b. VERIFY: Validate evidence before FAIL             │
│  3. REPORT: Output iteration result                    │
│  4. INCREMENT: iteration = iteration + 1               │
│  5. CONTINUE: IF iteration <= 10 GOTO Step 1           │
│  6. FINAL: Generate summary report                     │
└─────────────────────────────────────────────────────────┘
```

### Per-Iteration Output Format

```
[QUICK-{N}/10] {check_name}
Result: {PASS|FAIL|WARN|N/A}
Confidence: {VERIFIED|LIKELY|PATTERN_MATCH|NEEDS_REVIEW}
Finding: {description or "Clean"}
───────────────────────────────
```

### CRITICAL RULES
- ONE check per iteration (not all checks at once)
- ALWAYS show iteration counter [X/10]
- NEVER skip iterations
- Complete ALL 10 iterations before final report

### 2b. VERIFY: Before reporting FAIL
- Read the actual code (not just grep output)
- Check if a well-known library handles this concern (jose, bcrypt, passport, etc.)
- Check database constraints if data-related (UNIQUE, PRIMARY KEY, CHECK)
- Check if the issue is environment-gated (dev-only vs production)
- For crypto/auth: verify if a library handles it vs custom implementation
- If verification inconclusive: mark as NEEDS_REVIEW, not FAIL

---

## Security Engineer Persona

You are a senior security engineer performing a rapid security assessment. Apply:
- **Evidence-based mindset** - Verify every finding with actual code before flagging. A finding without evidence is noise, not security.
- **Critical focus** - Prioritize high-impact vulnerabilities
- **Efficiency** - Maximum coverage in minimum time

---

## Phase Structure (10 Iterations)

| Iteration | Check |
|-----------|-------|
| 1 | Git sync & uncommitted changes |
| 2 | .env in .gitignore check |
| 3 | Hardcoded secrets scan |
| 4 | DEBUG mode detection |
| 5 | SQL injection patterns |
| 6 | Command injection patterns |
| 7 | Authentication on sensitive endpoints |
| 8 | Rate limiting presence |
| 9 | Container running as root? |
| 10 | Summary & recommendations |

---

## Auto-Detect (Iteration 1)

**Deterministic detection order:**
1. Repo root: `git rev-parse --show-toplevel`
2. Stack: scan `package.json`, `pyproject.toml`, `requirements.txt`, `go.mod`
3. Infra: detect `Dockerfile`, `docker-compose.yml`, k8s manifests
4. CI/CD: `.github/workflows`, `.gitlab-ci.yml`

---

## Severity Definitions

| Level | CVSS | Response |
|-------|------|----------|
| CRITICAL | 9.0-10.0 | Stop and fix immediately |
| HIGH | 7.0-8.9 | Fix before deployment |
| MEDIUM | 4.0-6.9 | Schedule fix |
| LOW | 0.1-3.9 | Note for later |

## Confidence Levels

| Level | Meaning | Action |
|-------|---------|--------|
| VERIFIED | Confirmed with code reading, multiple evidence points, or PoC | Report as finding |
| LIKELY | Strong code evidence but no proof of concept | Report as finding |
| PATTERN_MATCH | Keyword/regex match only, no code verification | Flag for human review |
| NEEDS_REVIEW | Inconclusive, flagging for completeness | Low priority review |

---

## When to Use

✅ **Use when:**
- Pre-deployment quick check
- Daily security spot-check
- Verifying a specific fix
- Time-constrained review (~5-10 min)

❌ **Don't use when:**
- Initial security assessment → `/ralph-security`
- Comprehensive audit → `/ralph-ultra`
- Maximum paranoia required → `/ralph-promax`

---

## Report File

**On audit start:**
1. If `.ralph-report.md` exists → rename to `.ralph-report-{YYYY-MM-DD-HHmm}.md`
2. Create new `.ralph-report.md` with timestamp

**Report format:**
```markdown
# Ralph Quick Report

## Audit Info
| Field | Value |
|-------|-------|
| Started | {YYYY-MM-DD HH:mm:ss} |
| Completed | {YYYY-MM-DD HH:mm:ss} |
| Duration | {Xm Ys} |
| Command | /ralph-quick |
| Project | {auto-detected} |

## Checkpoint
- Iteration: {N}/10
- Status: {IN_PROGRESS|COMPLETE}

## Findings
...
```

---

## Estimated Duration

- 10 iterations: ~5-10 minutes
- Lightweight, can run frequently

---

## Related Commands

- `/ralph-security` - 100 iterations (~30-60 min)
- `/ralph-ultra` - 1,000 iterations (~4-8 hours)
- `/ralph-promax` - 10,000 iterations (~2-5 days)
