# Policy Cluster Manifest

Assigns all 39 source policies (`policies/*.md` in the read-only source
corpus) to one of the 5 dispatch clusters, for Wave B (one agent per
cluster). Every agent working a cluster must follow
[`DEBRANDING-SPEC.md`](DEBRANDING-SPEC.md).

Assignment is by primary regulatory/functional domain (`legal_basis` in
each file's frontmatter, cross-checked against actual body content).
Where a policy plausibly fits more than one cluster, it's assigned to the
best-fit cluster with a note explaining the runner-up.

## Cluster sizes are uneven — flagged for dispatch planning

federal-compliance is **18 of 39 files (46%)** — nearly half the corpus,
because most of these policies exist specifically to satisfy 2 CFR 200 /
FAR / federal-program requirements. AI-governance is only **2 files**.
This is a real, thematically-correct split, not an artifact of sloppy
categorization — but dispatching it as five equal-sized agent workloads
would leave one agent with 9x the work of another. **Recommend splitting
federal-compliance into two dispatch batches (A/B, ~9 files each) run by
two agents in parallel**, both still citing this one cluster name/manifest
section, rather than forcing artificial rebalancing across the other four
thematic clusters (which would blur clean domain boundaries for no good
reason). Left as your call — the file-level assignment below is correct
either way; only the dispatch batching changes.

## 1. federal-compliance (18 files)

| File | Note |
|---|---|
| `consortium-participation.md` | |
| `cost-accounting.md` | |
| `cost-sharing.md` | |
| `crada.md` | |
| `cybersecurity.md` | Legal basis is DFARS/CMMC/NIST 800-171 — federal-*contract* cyber requirements, not general infosec risk. Runner-up: risk/legal (general information-security risk applies to any org, federal contractor or not). Assigned federal-compliance because the specific controls mandated here (CMMC 2.0, DFARS 252.204-70xx) only bite because of federal contracting — that's the policy's actual reason to exist. |
| `export-control.md` | |
| `federal-award-integrity.md` | |
| `lobbying-certification.md` | |
| `mftrp.md` | |
| `pre-award-spending.md` | |
| `procurement.md` | |
| `property-equipment.md` | |
| `proposal-routing.md` | |
| `sttr-sbir-partnership.md` | |
| `subaward-monitoring.md` | |
| `suspension-debarment.md` | |
| `teaming-agreement.md` | |
| `time-effort-reporting.md` | See `DEBRANDING-SPEC.md` §4 — contains a `fiscal_year_end`-dependent worked example that needs a content decision, not just literal substitution. |

## 2. research-integrity (8 files)

| File | Note |
|---|---|
| `animal-welfare.md` | |
| `data-management.md` | Runner-up: risk/legal (data governance/cybersecurity overlap — the corpus's data-management policy discusses storage tiers and access roles, adjacent to `cybersecurity.md`). Assigned research-integrity because its primary legal basis is funder Data Management Plan requirements (NSF/NIH/DOE/DARPA DMP mandates), a research-process obligation. |
| `financial-conflict-of-interest.md` | Runner-up: risk/legal (it's a conflict-of-interest policy, same family as the board-level `conflict-of-interest.md`). Assigned research-integrity because 42 CFR Part 50 Subpart F / PHS-NIH FCOI rules are specifically an *investigator/research-funding* disclosure regime, distinct in scope and trigger from the board/officer-level conflict-of-interest policy. |
| `human-subjects-protection.md` | |
| `publication-open-access.md` | |
| `rcr-training.md` | |
| `research-misconduct.md` | |
| `research-security-conflict-of-commitment.md` | Runner-up: federal-compliance (NSPM-33 is a federal security-policy directive, and CHIPS Act 10634 is a federal-program citation, same family as several federal-compliance files). Assigned research-integrity because the operative content — foreign talent program disclosure, conflict of commitment — is a research-integrity/disclosure obligation borne by individual researchers, structurally parallel to `rcr-training.md` and `financial-conflict-of-interest.md` rather than an institutional federal-contracting control. |

## 3. HR/operations (6 files)

| File | Note |
|---|---|
| `code-of-ethics.md` | Runner-up: risk/legal (IRS Form 990 general-conduct basis overlaps with governance/legal). Assigned HR/operations because it's a conduct code applying to all staff day-to-day, structurally parallel to `non-discrimination.md` and `drug-free-workplace.md`. |
| `compensation-review.md` | |
| `drug-free-workplace.md` | |
| `non-discrimination.md` | |
| `safety-environmental.md` | Runner-up: risk/legal (OSHA/EPA carry real institutional liability exposure). Assigned HR/operations because day-to-day implementation (workplace safety practices) is an operations function, same family as `travel.md` and `drug-free-workplace.md`. |
| `travel.md` | |

## 4. risk/legal (5 files)

| File | Note |
|---|---|
| `conflict-of-interest.md` | **Exemplar for this spec — already fully de-branded, see `template/policies/conflict-of-interest.md.jinja`.** Do not re-do this file in Wave B; verify it against the spec instead. |
| `delegation-of-authority.md` | Contains all three of the harder de-branding cases in one file: proper names (Board Chair, Executive Director), the fiscal-custodian vendor name, and one-time historical facts (board-motion numbers, founding season) — see `DEBRANDING-SPEC.md` §3 and §7. Budget extra review time for this file specifically. |
| `document-retention.md` | |
| `intellectual-property.md` | Runner-up: federal-compliance (Bayh-Dole Act specifically governs IP arising from *federally funded* inventions). Assigned risk/legal because the policy's scope is broader than federal awards — it covers all IP/invention disclosure regardless of funding source — making general IP/legal governance the better primary fit. |
| `whistleblower-protection.md` | Contains proper names (Board Chair, Executive Director) — see `DEBRANDING-SPEC.md` §3. |

## 5. AI-governance (2 files)

| File | Note |
|---|---|
| `ai-governance-board.md` | Contains the AI-platform-names content note from `DEBRANDING-SPEC.md` §3 (Claude/Claude Code and two other product names hardcoded as "authorized platforms") and the fiscal-custodian/vendor-name occurrences in its role tables. Gated by `ai_agents_enabled` per spec §5. |
| `ai-responsible-use-research.md` | Gated by `ai_agents_enabled` per spec §5. |

## Cross-check

18 + 8 + 6 + 5 + 2 = 39. All source files in `policies/` accounted for,
no duplicates.
