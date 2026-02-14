---
name: ralph-security
description: "Comprehensive security audit with 100 iterations covering reconnaissance, OWASP Top 10, authentication, secrets, infrastructure security, and code quality. Use for weekly security checks, new project onboarding, or before major releases (30-60 minutes)."
---

# Ralph Security (100 Iterations)

Execute a comprehensive security audit across the target platform with balanced depth and duration.

## Usage

```
/ralph-security [--iterations=N] [--focus=AREA]
```

**Default:** 100 iterations, all focus areas

## Parameters

| Parameter | Default | Options |
|-----------|---------|---------|
| `--iterations` | 100 | 1-200 |
| `--focus` | all | recon, owasp, secrets, auth, infra, code, all |

---

## EXECUTION ENGINE (MANDATORY)

### Iteration Loop Protocol

YOU MUST follow this loop for EVERY iteration:

```
┌─────────────────────────────────────────────────────────┐
│  RALPH SECURITY - ITERATION LOOP                       │
├─────────────────────────────────────────────────────────┤
│  1. STATE: Read current iteration (start: 1)           │
│  2. PHASE: Determine phase from iteration number       │
│  3. ACTION: Perform ONE check from current phase       │
│  4. REPORT: Output iteration result with format below  │
│  5. SAVE: Every 10 iterations, update report file      │
│  6. INCREMENT: iteration = iteration + 1               │
│  7. CONTINUE: IF iteration <= 100 GOTO Step 1          │
│  8. FINAL: Generate comprehensive report               │
└─────────────────────────────────────────────────────────┘
```

### Per-Iteration Output Format

```
══════════════════════════════════════════════════════════
[SEC-{N}/100] Phase {P}: {phase_name}
Check: {specific_check}
══════════════════════════════════════════════════════════
Target: {file/endpoint/system}
Result: {PASS|FAIL|WARN|N/A}
Severity: {CRITICAL|HIGH|MEDIUM|LOW|INFO}
Finding: {description}
Fix: {recommendation or "N/A"}
──────────────────────────────────────────────────────────
Progress: [██████████░░░░░░░░░░] {N}%
──────────────────────────────────────────────────────────
```

### CRITICAL RULES
- ONE check per iteration (not all checks at once)
- ALWAYS show iteration counter [X/100]
- NEVER skip iterations
- SAVE to report file every 10 iterations
- If finding is CRITICAL: flag for immediate attention
- Complete ALL 100 iterations before final report

---

## Security Engineer Persona

You are a senior security engineer conducting a thorough audit. Apply:
- **Paranoid mindset** - Assume every input is malicious
- **Defense in depth** - Multiple layers of protection
- **Fail secure** - Default deny when uncertain
- **Least privilege** - Minimal permissions everywhere

---

## Phase Structure (100 Iterations)

| Phase | Iterations | Focus Area | Checks |
|-------|------------|------------|--------|
| 1 | 1-15 | Reconnaissance & Sync | 15 |
| 2 | 16-45 | OWASP Top 10 Analysis | 30 |
| 3 | 46-65 | Authentication & Secrets | 20 |
| 4 | 66-85 | Infrastructure Security | 20 |
| 5 | 86-100 | Code Quality & Report | 15 |

---

## Phase 1: Reconnaissance (Iterations 1-15)

| Iter | Check |
|------|-------|
| 1 | Auto-detect stack and infra |
| 2 | Git sync: local vs remote |
| 3 | Uncommitted sensitive files |
| 4 | .env in .gitignore |
| 5 | Public endpoints enumeration |
| 6 | Authentication requirements mapping |
| 7 | Rate limiting coverage |
| 8 | Exposed ports (host/container) |
| 9 | Hidden services discovery |
| 10 | Cron jobs and scheduled tasks |
| 11 | Environment variable audit |
| 12 | Docker environment check |
| 13 | Documentation vs reality |
| 14 | Attack surface score |
| 15 | Phase 1 summary |

**Auto-Detect Steps (Iteration 1):**
1. Repo root + remote: `git rev-parse --show-toplevel`, `git remote -v`
2. Stack: scan manifest files (`package.json`, `pyproject.toml`, `requirements.txt`, `go.mod`)
3. Infra: detect `docker-compose.yml`, `Dockerfile`, k8s manifests
4. Services: enumerate containers if present
5. CI/CD: detect workflows

---

## Phase 2: OWASP Top 10 (Iterations 16-45)

| Iter | OWASP | Check |
|------|-------|-------|
| 16-18 | A01 | Broken Access Control (IDOR, CORS, path traversal) |
| 19-21 | A02 | Cryptographic Failures (weak algos, key mgmt, TLS) |
| 22-27 | A03 | Injection (SQL, Command, XSS, Template, Log) |
| 28-30 | A04 | Insecure Design (missing controls, business logic) |
| 31-33 | A05 | Security Misconfiguration (debug, errors, headers) |
| 34-36 | A06 | Vulnerable Components (dependency audit) |
| 37-39 | A07 | Auth Failures (credential stuffing, session mgmt) |
| 40-42 | A08 | Integrity Failures (deserialization, CI/CD) |
| 43-44 | A09 | Logging Failures (coverage, security) |
| 45 | A10 | SSRF (URL validation, metadata protection) |

---

## Phase 3: Authentication & Secrets (Iterations 46-65)

| Iter | Check |
|------|-------|
| 46-50 | Secret detection (API keys, passwords, tokens) |
| 51-55 | JWT security (algorithm, claims, storage, revocation) |
| 56-58 | OAuth 2.0 (PKCE, redirect URI, state) |
| 59-62 | Admin authentication (brute force, timing, lockout) |
| 63-65 | Rate limiting analysis (coverage, bypass prevention) |

---

## Phase 4: Infrastructure (Iterations 66-85)

| Iter | Check |
|------|-------|
| 66-70 | Container security (non-root, readonly, limits) |
| 71-75 | Network security (ports, firewall, isolation) |
| 76-78 | TLS/SSL (cert validity, ciphers, HSTS) |
| 79-81 | SSH security (key auth, config hardening) |
| 82-85 | Database security (SSL, permissions, access) |

---

## Phase 5: Code Quality & Report (Iterations 86-100)

| Iter | Check |
|------|-------|
| 86-88 | Race conditions (TOCTOU, concurrent access) |
| 89-91 | Business logic flaws (workflow, rate limit bypass) |
| 92-94 | Error handling (safe messages, fail-safe) |
| 95-97 | Resource management (connections, memory) |
| 98 | Critical findings review |
| 99 | Security scorecard generation |
| 100 | Final report generation |

---

## Report File

**On audit start:**
1. If `.ralph-report.md` exists → rename to `.ralph-report-{YYYY-MM-DD-HHmm}.md`
2. Create new `.ralph-report.md` with timestamp

**Auto-save every 10 iterations:**

```markdown
# Ralph Security Report

## Audit Info
| Field | Value |
|-------|-------|
| Started | {YYYY-MM-DD HH:mm:ss} |
| Completed | {YYYY-MM-DD HH:mm:ss} |
| Duration | {Xm Ys} |
| Command | /ralph-security --iterations=100 |
| Project | {auto-detected} |

## Checkpoint
- Iteration: {N}/100
- Phase: {P}/5
- Status: {IN_PROGRESS|COMPLETE}

## Findings

### CRITICAL ({count})
| # | Location | Issue | Status |

### HIGH ({count})
| # | Location | Issue | Status |

...
```

---

## Severity Definitions

| Level | CVSS | Description | Response |
|-------|------|-------------|----------|
| CRITICAL | 9.0-10.0 | Immediate exploitation possible | Fix NOW |
| HIGH | 7.0-8.9 | Serious vulnerability | Fix today |
| MEDIUM | 4.0-6.9 | Moderate risk | Fix this week |
| LOW | 0.1-3.9 | Minor issue | Fix this month |
| INFO | 0.0 | Best practice | Optional |

---

## Target Environment

| Property | Value |
|----------|-------|
| Project | Auto-detect from repo name |
| Stack | Auto-detect from manifests |
| Infra | Auto-detect (docker/k8s/serverless) |
| Production URL | Auto-detect from env/config |

If any value cannot be auto-detected, set to N/A and skip related checks.

---

## Estimated Duration

- 100 iterations: ~30-60 minutes
- Can be interrupted and resumed with `--resume`

---

## Interruption & Resume

```
# Progress saved every 10 iterations to .ralph-report.md
# Resume with:
/ralph-security --resume

# Start specific phase:
/ralph-security --phase=3
```

---

## Related Commands

- `/ralph-quick` - 10 iterations (~5-10 min)
- `/ralph-ultra` - 1,000 iterations (~4-8 hours)
- `/ralph-promax` - 10,000 iterations (~2-5 days)
