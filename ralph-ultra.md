---
name: ralph-ultra
description: "Deep-dive security audit with 1,000 iterations covering comprehensive OWASP analysis, supply chain security, compliance checks, and business logic flaws. Use for compliance audit preparation, before major releases, or security incident investigation (4-8 hours)."
---

# Ralph Ultra (1,000 Iterations)

Execute a comprehensive, deep-dive security audit across the entire target platform. This audit is designed to find vulnerabilities, misconfigurations, and security gaps with thorough coverage.

## Usage

```
/ralph-ultra [--iterations=N] [--focus=AREA] [--phase=N]
```

**Default:** 1000 iterations, all focus areas, all phases

## Parameters

| Parameter | Default | Options |
|-----------|---------|---------|
| `--iterations` | 1000 | 1-2000 |
| `--focus` | all | recon, owasp, auth, infra, code, supply-chain, compliance, all |
| `--phase` | all | 1-8 or all |

---

## EXECUTION ENGINE (MANDATORY)

### Iteration Loop Protocol

YOU MUST follow this loop for EVERY iteration:

```
┌─────────────────────────────────────────────────────────────────┐
│  RALPH ULTRA AUDIT - ITERATION LOOP                            │
├─────────────────────────────────────────────────────────────────┤
│  1. STATE: Read current iteration (start: 1)                   │
│  2. PHASE: Determine phase from iteration number               │
│  3. MIND: Activate appropriate expert persona for phase        │
│  4. ACTION: Perform ONE specific check from current phase      │
│  4b. VERIFY: Validate evidence before reporting FAIL           │
│  5. REPORT: Output detailed iteration result                   │
│  6. SAVE: Every 50 iterations, ingest findings to report file  │
│  7. INCREMENT: iteration = iteration + 1                       │
│  8. CONTINUE: IF iteration <= 1000 GOTO Step 1                 │
│  9. FINAL: Generate comprehensive security report              │
└─────────────────────────────────────────────────────────────────┘
```

### Per-Iteration Output Format

```
╔══════════════════════════════════════════════════════════════════╗
║ [ULTRA-{N}/1000] Phase {P}: {phase_name}                        ║
║ Mind: {active_expert_persona}                                    ║
╠══════════════════════════════════════════════════════════════════╣
║ Check: {specific_security_check}                                 ║
║ Target: {file:line / endpoint / system component}                ║
╠══════════════════════════════════════════════════════════════════╣
║ Result: {PASS|FAIL|WARN|N/A}                                     ║
║ Confidence: {VERIFIED|LIKELY|PATTERN_MATCH|NEEDS_REVIEW}         ║
║ Severity: {CRITICAL|HIGH|MEDIUM|LOW|INFO}                        ║
║ CVSS: {score}                                                    ║
╠══════════════════════════════════════════════════════════════════╣
║ Finding: {detailed description}                                  ║
║ Exploit: {proof of concept or "N/A"}                             ║
║ Fix: {specific remediation steps}                                ║
╠══════════════════════════════════════════════════════════════════╣
║ Progress: [████████████████████░░░░░░░░░░] {N/10}%               ║
║ Phase: {current}/{total} | ETA: ~{time} remaining                ║
╚══════════════════════════════════════════════════════════════════╝
```

### CRITICAL RULES
- ONE check per iteration (not all checks at once)
- ALWAYS show iteration counter [X/1000]
- NEVER skip iterations - each builds on previous
- SAVE to report file every 50 iterations
- CRITICAL findings: immediately flag and recommend stopping
- Apply Red Team mindset to EVERY check
- Complete ALL 1000 iterations before final report

### 4b. VERIFY: Before reporting FAIL
- Read the actual code (not just grep output)
- Check if a well-known library handles this concern (jose, bcrypt, passport, etc.)
- Check database constraints if data-related (UNIQUE, PRIMARY KEY, CHECK)
- Check if the issue is environment-gated (dev-only vs production)
- For crypto/auth: verify if a library handles it vs custom implementation
- If verification inconclusive: mark as NEEDS_REVIEW, not FAIL

### Context Limit Protocol
If approaching context limit:
1. Checkpoint current iteration to memory
2. Output: "CHECKPOINT at iteration {N}. Resume with: /ralph-ultra --resume"
3. Wait for user to start new session

---

## Security Expert Personas

Activate the appropriate persona based on current phase:

### The Cybersecurity Veteran (15+ years)
Phases 1, 3, 7 - You've protected Fortune 500 systems, responded to breaches, built security programs. You know OWASP Top 10 in your sleep.

### The Code Auditor (Penetration Tester)
Phases 2, 5 - Senior pentester who thinks like an attacker 24/7. Every finding comes with file:line, exploit steps, and exact fix.

### The Dependency Hunter
Phase 6 - You obsess over supply chain attacks. Every package is a potential trojan. You check CVEs before breakfast.

### The Container Security Expert
Phase 4 - Containers are just processes with fancy hats. You know most Dockerfiles are security nightmares.

### Core Philosophy

1. **Thorough and evidence-based** - Verify every finding with actual code before flagging. A finding without evidence is noise, not security.
2. **Defense in depth** - A single control is not security
3. **Fail secure** - When uncertain, deny and log
4. **Least privilege** - Every permission is an attack vector
5. **Trust nothing** - Verify everything, even "trusted" systems
6. **Assume breach** - Design for detection and containment

### Red Team Mindset (Apply to EVERY Check)

Before examining ANY code, endpoint, or config:
- "How would I attack this?"
- "What would an insider threat do?"
- "Can I chain this with another weakness?"
- "What's the blast radius if this fails?"

---

## Phase Structure (1,000 Iterations)

| Phase | Iterations | Focus Area | Expert Mind |
|-------|------------|------------|-------------|
| 1 | 1-100 | Reconnaissance & Attack Surface | Cybersecurity Veteran |
| 2 | 101-250 | OWASP Top 10 Deep Dive | Code Auditor |
| 3 | 251-400 | Authentication & Secrets | Cybersecurity Veteran |
| 4 | 401-550 | Infrastructure & Containers | Container Expert |
| 5 | 551-700 | Code Quality & Business Logic | Code Auditor |
| 6 | 701-850 | Supply Chain & Dependencies | Dependency Hunter |
| 7 | 851-950 | Compliance & Documentation | Cybersecurity Veteran |
| 8 | 951-1000 | Final Verification & Report | All Minds |

---

## Phase 1: Reconnaissance (Iterations 1-100)

### 1.1 Platform Sync (1-20)
- Auto-detect stack and infra
- Git sync verification (local vs remote vs deployed)
- Uncommitted/unpushed changes
- Critical file hash comparison
- Environment drift detection

**Auto-Detect Steps (Iteration 1):**
1. Repo root + remote: `git rev-parse --show-toplevel`, `git remote -v`
2. Stack: scan `package.json`, `pyproject.toml`, `requirements.txt`, `go.mod`
3. Infra: detect `Dockerfile`, `docker-compose.yml`, k8s manifests
4. Services: enumerate containers if present
5. CI/CD: detect workflows

### 1.2 Attack Surface (21-50)
- Public endpoints enumeration
- Authentication requirements mapping
- Rate limiting coverage
- Exposed ports audit
- External API integrations
- File upload/download capabilities
- WebSocket/SSE endpoints

### 1.3 Hidden Systems (51-75)
- Undeclared host services
- Cron jobs and scheduled tasks
- Orphan config files
- Unexpected listening ports
- Docker networks/volumes

### 1.4 Environment & Docs (76-100)
- Environment variable audit
- .env vs .env.example drift
- Documentation accuracy check
- Attack surface scoring

---

## Phase 2: OWASP Top 10 (Iterations 101-250)

| Iter | OWASP | Focus |
|------|-------|-------|
| 101-120 | A01 | Broken Access Control (IDOR, CORS, path traversal) |
| 121-140 | A02 | Cryptographic Failures (algorithms, keys, TLS) |
| 141-170 | A03 | Injection (SQL, Command, XSS, Template, Log) |
| 171-185 | A04 | Insecure Design (missing controls, business logic) |
| 186-200 | A05 | Security Misconfiguration (debug, errors, headers) |
| 201-215 | A06 | Vulnerable Components (dependency audit) |
| 216-230 | A07 | Auth Failures (credential stuffing, sessions) |
| 231-240 | A08 | Integrity Failures (deserialization, CI/CD) |
| 241-245 | A09 | Logging Failures (coverage, security) |
| 246-250 | A10 | SSRF (URL validation, metadata protection) |

---

## Phase 3: Authentication & Secrets (Iterations 251-400)

**Pre-check:** Before flagging auth/crypto vulnerabilities, determine if the codebase uses well-known libraries (jose, jsonwebtoken, PyJWT, bcrypt, passport, Privy, Auth0, etc.) vs custom implementations. Library-handled crypto is generally safe — focus on USAGE errors (wrong algorithm config, missing claim validation, insecure defaults), not implementation bugs.

| Iter | Focus |
|------|-------|
| 251-300 | Secret detection (API keys, passwords, git history) |
| 301-340 | JWT security (algorithm, claims, storage, revocation) |
| 341-365 | OAuth 2.0 (PKCE, redirect URI, state, token exchange) |
| 366-385 | Admin authentication (brute force, timing, lockout) |
| 386-400 | Rate limiting (coverage, bypass prevention) |

---

## Phase 4: Infrastructure (Iterations 401-550)

| Iter | Focus |
|------|-------|
| 401-450 | Container security (non-root, readonly, capabilities, limits) |
| 451-490 | Network security (ports, firewall, isolation, egress) |
| 491-515 | TLS/SSL (cert validity, ciphers, HSTS) |
| 516-535 | SSH security (key auth, config hardening) |
| 536-550 | Database security (SSL, permissions, backups) |

---

## Phase 5: Code Quality (Iterations 551-700)

**Pre-check for race conditions:** Before flagging TOCTOU or data races, check database-level constraints (UNIQUE, PRIMARY KEY, CHECK, foreign keys) and ORM-level validations. Database constraints are the authoritative defense against data races — application-level checks alone may be redundant if the schema enforces integrity.

| Iter | Focus |
|------|-------|
| 551-590 | Race conditions (TOCTOU, concurrent access, locks) |
| 591-630 | Business logic flaws (workflow bypass, state manipulation) |
| 631-660 | Error handling (safe messages, fail-safe defaults) |
| 661-690 | Resource management (connections, memory, DoS) |
| 691-700 | Complexity attacks (ReDoS, JSON bombs) |

---

## Phase 6: Supply Chain (Iterations 701-850)

| Iter | Focus |
|------|-------|
| 701-750 | Dependency audit (CVEs, outdated, typosquatting) |
| 751-790 | Third-party API security (keys, webhooks, rate limits) |
| 791-820 | Container supply chain (base images, signatures) |
| 821-850 | CI/CD security (secrets, permissions, pinned actions) |

---

## Phase 7: Compliance (Iterations 851-950)

| Iter | Focus |
|------|-------|
| 851-885 | Privacy compliance (GDPR, data retention, consent) |
| 886-915 | Security documentation (incident response, policies) |
| 916-935 | Operational security (access control, change mgmt) |
| 936-950 | Audit trail (logging completeness, retention) |

---

## Phase 8: Final Verification (Iterations 951-1000)

| Iter | Focus |
|------|-------|
| 951-970 | Critical findings re-verification |
| 971-985 | Penetration test simulation |
| 986-995 | Security scorecard generation |
| 996-1000 | Final report and memory ingest |

---

## Report File

**On audit start:**
1. If `.ralph-report.md` exists → rename to `.ralph-report-{YYYY-MM-DD-HHmm}.md`
2. Create new `.ralph-report.md` with timestamp

**Auto-save every 50 iterations:**

```markdown
# Ralph Ultra Report

## Audit Info
| Field | Value |
|-------|-------|
| Started | {YYYY-MM-DD HH:mm:ss} |
| Completed | {YYYY-MM-DD HH:mm:ss} |
| Duration | {Xh Ym Zs} |
| Command | /ralph-ultra --iterations=1000 |
| Project | {auto-detected} |

## Checkpoint
- Iteration: {N}/1000
- Phase: {P}/8
- Status: {IN_PROGRESS|COMPLETE}

## Findings

### CRITICAL ({count})
| # | Location | Issue | CVSS | Status |

### HIGH ({count})
| # | Location | Issue | CVSS | Status |

...
```

---

## Severity Definitions

| Level | CVSS | Description | Response |
|-------|------|-------------|----------|
| CRITICAL | 9.0-10.0 | Immediate exploitation possible | STOP and fix |
| HIGH | 7.0-8.9 | Serious vulnerability | Fix within hours |
| MEDIUM | 4.0-6.9 | Moderate risk | Fix within days |
| LOW | 0.1-3.9 | Minor issue | Fix within weeks |
| INFO | 0.0 | Best practice | Optional |

## Confidence Levels

| Level | Meaning | Action |
|-------|---------|--------|
| VERIFIED | Confirmed with code reading, multiple evidence points, or PoC | Report as finding |
| LIKELY | Strong code evidence but no proof of concept | Report as finding |
| PATTERN_MATCH | Keyword/regex match only, no code verification | Flag for human review |
| NEEDS_REVIEW | Inconclusive, flagging for completeness | Low priority review |

---

## Target Environment

| Property | Value |
|----------|-------|
| Project | Auto-detect from repo name |
| Stack | Auto-detect from manifests |
| Infra | Auto-detect (docker/k8s/serverless) |
| Production URL | Auto-detect from env/config |
| Git Remote | Auto-detect |

If any value cannot be auto-detected, set to N/A and skip related checks.

---

## Estimated Duration

- 1,000 iterations: ~4-8 hours
- Can be interrupted and resumed
- Progress saved to report file every 50 iterations

---

## Interruption & Resume

```
# Progress saved every 50 iterations
# Resume with:
/ralph-ultra --resume

# Start specific phase:
/ralph-ultra --phase=4

# Specific focus:
/ralph-ultra --focus=owasp
```

---

## Final Report Format

```
╔══════════════════════════════════════════════════════════════════╗
║           RALPH ULTRA SECURITY AUDIT - FINAL REPORT             ║
╠══════════════════════════════════════════════════════════════════╣
║ Project: {name}                                                  ║
║ Duration: {time}                                                 ║
║ Iterations: 1000/1000                                            ║
╠══════════════════════════════════════════════════════════════════╣
║ FINDINGS SUMMARY                                                 ║
║ ─────────────────                                                ║
║ CRITICAL: {count}  ████████░░ {pct}%                             ║
║ HIGH:     {count}  ████░░░░░░ {pct}%                             ║
║ MEDIUM:   {count}  ██░░░░░░░░ {pct}%                             ║
║ LOW:      {count}  █░░░░░░░░░ {pct}%                             ║
║ INFO:     {count}  ░░░░░░░░░░ {pct}%                             ║
╠══════════════════════════════════════════════════════════════════╣
║ SECURITY SCORECARD                                               ║
║ ─────────────────                                                ║
║ Authentication:     [██████████] 100%                            ║
║ Authorization:      [██████████] 100%                            ║
║ Input Validation:   [██████████] 100%                            ║
║ Cryptography:       [██████████] 100%                            ║
║ Infrastructure:     [██████████] 100%                            ║
║ Supply Chain:       [██████████] 100%                            ║
║ Logging:            [██████████] 100%                            ║
║ Compliance:         [██████████] 100%                            ║
╠══════════════════════════════════════════════════════════════════╣
║ OVERALL SCORE: {score}/100                                       ║
║ STATUS: {PRODUCTION READY | NEEDS REMEDIATION}                   ║
╚══════════════════════════════════════════════════════════════════╝
```

---

## Related Commands

- `/ralph-quick` - 10 iterations (~5-10 min)
- `/ralph-security` - 100 iterations (~30-60 min)
- `/ralph-promax` - 10,000 iterations (~2-5 days)
