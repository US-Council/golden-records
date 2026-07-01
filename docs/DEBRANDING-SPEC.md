# De-Branding Spec — Policy Corpus → Copier Template

This is the authoritative, exhaustive rulebook for turning the source
policy corpus (39 adopted policies of a real 501(c)(3) research
institute, read-only at the path given to each dispatched agent) into the
generic, parameterized `template/policies/*.md.jinja` payload of this
commons.

**Every agent de-branding a policy cluster must conform to this document.**
If a branded pattern you encounter is not covered here, stop and flag it
rather than improvising — inconsistent de-branding across clusters is
worse than a slower, uniform pass.

## 0. Meta-rule: this spec must itself contain zero real org data

This file is tracked in a public commons repo and is **not** exempted from
`leak-guard` (it is not on the `.gitleaks.toml` / `leak-guard.yml`
allowlist, and it should not be added to it — see rationale in
`docs/ARCHITECTURE.md#why-the-leak-guard-exists-at-the-architecture-level-not-just-as-a-ci-afterthought`).
Copier's own precedent (`copier.yml`'s EIN allowlist) only ever exempts
**fake, canonical placeholder values** (`12-3456789`, `Example Community
Foundation`-style names) — never the real source organization's actual
identifiers. This spec follows the same rule: it never spells out the real
legal name, EIN, charter number, address, or any real person's name from
the source corpus. Instead:

- Real literal strings are described **structurally** (which frontmatter
  key, which line pattern, which table column) so you can find them
  yourself in the read-only source with a grep, exactly as this spec's
  author did.
- The authoritative list of forbidden literal strings is
  `.gitleaks-denylist.txt` at this repo's root — treat every line in that
  file as "must not appear, in any casing, in any file you commit."
  **The fiscal-custodian vendor's literal name was found during this audit
  and has been added to that file** (see §8) — it was a real gap; the
  entry already present for the IT vendor's name only covers the *other*
  vendor, not this one.
- Where an illustrative example is useful, this spec uses the same fake
  canonical placeholders `copier.yml` already uses (e.g. `Example
  Community Foundation`, `12-3456789`) or a bracketed token like
  `«ORG_NAME»` / `«EIN»` / `«STATE»` — never a real value.

## 1. Frontmatter convention

Every policy in the source corpus carries this exact YAML frontmatter key
set, in this order:

```yaml
---
type: policy
organization: «ORG_NAME»
title: <Policy Title>
status: adopted
effective_date: on-merge
legal_basis: <citation(s) — regulatory/statutory, not org-specific, leave verbatim>
id: POL-0XX
version: "1.0"
last_updated: <YYYY-MM-DD>
adopted_meeting: <meeting tag, e.g. 2026-Q2-board>
supersedes: null
superseded_by: null
review_cycle: annual
next_review: <YYYY-Qn>
---
```

De-branding rule for frontmatter:

| Key | Treatment |
|---|---|
| `organization` | Replace the literal value with `{{ org_name }}`. This is the only frontmatter key that carries organization identity. |
| `title` | Verbatim — policy titles are generic (never org-branded). |
| `legal_basis` | Verbatim — these are regulatory/statutory citations (CFR, USC, IRS form line items, NIST publications, etc.) shared by every nonprofit. **They require zero de-branding.** Do not touch this field beyond what §6 says about conditional sections. |
| `id` | Verbatim (`POL-0XX`). This is a sequential policy-corpus number, not an organization identifier — safe to keep as-is in the commons template. |
| `status`, `effective_date`, `version`, `supersedes`, `superseded_by`, `review_cycle` | Verbatim — these are template mechanics, not org identity, and describe the intended state of *this template's* policy language, not any adopter's actual adoption history. |
| `last_updated`, `adopted_meeting`, `next_review` | These encode the **source org's real adoption history** (a specific board meeting, a specific date). Replace with a generic placeholder that clearly signals "the adopter re-dates this on their own adoption," e.g. `last_updated: <to be set on adoption>` / `adopted_meeting: <to be set on adoption>` / `next_review: <to be set on adoption>`. Do not carry forward the source org's real dates — they are meaningless (and mildly misleading) in a template nobody has adopted yet. |

## 2. Masthead / byline convention

Immediately after the `# <Title>` H1, every policy has a 2-line masthead
naming the org, its tax status, its state of incorporation, and its EIN.
**Four textual variants exist in the source corpus** — treat all four as
equivalent and replace tokens regardless of exact wording/punctuation:

- Variant A (3 files): `**«ORG_NAME»** — EIN «EIN»` / `A 501(c)(3) nonprofit research institute incorporated in «STATE».`
- Variant B (majority of files): `**«ORG_NAME»** — a 501(c)(3) nonprofit research institute incorporated in «STATE»` / `EIN: «EIN»`
- Variant C (3 files, uses `--` instead of an em-dash): `**«ORG_NAME»** -- a 501(c)(3) nonprofit research institute incorporated in «STATE» (EIN: «EIN»)`
- Variant D (7 files): `**«ORG_NAME»** — 501(c)(3) Nonprofit Research Institute` / `EIN: «EIN» | Incorporated in «STATE»`

Replacement rule: swap `«ORG_NAME»` → `{{ org_name }}`, `«EIN»` →
`{{ ein }}`, `«STATE»` → `{{ state }}`, **preserving each file's exact
existing wording/punctuation/line-break variant**. Do not normalize all
four variants into one — that's a content-style decision outside this
de-branding pass's scope, and collapsing them would produce a larger,
harder-to-review diff for no de-branding benefit. "501(c)(3)" and
"nonprofit research institute" are generic descriptors — leave as prose
(do not parameterize "research institute"; it is not a `copier.yml`
variable and every adopter using this commons is, definitionally, running
some kind of mission org).

## 3. Token mapping table

| Token category | Where it appears | Jinja replacement |
|---|---|---|
| Full legal organization name (denylist: the corporate name string, appears in `.gitleaks-denylist.txt`) | Frontmatter `organization:`; masthead (all 4 variants); recurring inline prose, typically once near the top of "Article I / Section 1 — Purpose" introducing the defined term, e.g. `«ORG_NAME» ("the Organization")` | `{{ org_name }}` |
| Short/acronym form | **Not observed as a distinct token in the policies/ corpus.** The corpus consistently uses the full legal name once, then the defined term "the Organization" for every subsequent reference within a document — already generic, do not touch. `org_short` exists in `copier.yml` for use elsewhere in the template tree (letters, capability statements, etc., outside this cluster's scope) — do not introduce it into policy bodies where the source didn't have a distinct short form. |
| EIN (also caught by the `us-ein` gitleaks regex `\d{2}-\d{7}`) | Masthead (all 4 variants) | `{{ ein }}` |
| State of incorporation | Masthead ("incorporated in «STATE»") | `{{ state }}` |
| Incorporation statute | **Not found verbatim in the policies/ corpus** (no "Ohio Revised Code" / "ORC 1702" citations inside `policies/`; that citation lives in `bylaws.md`, out of this cluster's scope). No action needed in policy files. If a downstream agent finds one, map it to `{{ incorp_statute }}`. |
| Charter number | **Not found in the policies/ corpus** (only in `bylaws.md`, `COPYRIGHT.md`, `capability-statement.md`, `sttr-ri-qualification.md` — all outside `policies/`, out of scope for Wave B). No action needed. |
| Principal address / city+state | **Not found in the policies/ corpus.** (Present in `bylaws.md`, `capability-statement.md`, `sttr-ri-qualification.md`, and a letter template — out of scope.) No action needed. |
| `public_charity_classification` (509(a)(1)/170(b)(1)(A)(vi)) | **Not found in the policies/ corpus.** No action needed within this cluster. |
| Director/officer proper names (denylist: 3 real names) | Inline prose in `whistleblower-protection.md` and `non-discrimination.md` ("Contact ⟨Name⟩, Chairman of the Board, ..." / "Contact ⟨Name⟩, Executive Director, ..."); a role-assignment table in `delegation-of-authority.md` (columns: Role \| Name \| Authority basis) | **Not a copier variable.** Strip the proper name and its comma-appositive entirely; keep only the role title. This matches the style the source corpus *already* uses elsewhere in the same cluster — e.g. `conflict-of-interest.md` Article IV already says "the Board Chair (or, if the Chair has the conflict, to the Vice-Chair)" with no name at all. Transform pattern: `"Contact ⟨Name⟩, Chairman of the Board, in writing"` → `"Contact the Board Chair in writing"`. In `delegation-of-authority.md`'s table, replace the Name column value with a bracketed fillable placeholder, e.g. `[current Board Chair]`, `[current Executive Director]` — board composition is adopter-specific and changes over time independent of org identity, so it is deliberately **not** a `copier.yml` variable (see §7 for the full rationale). |
| Fiscal custodian vendor name ("the finance vendor," referenced ~12 files) | Inline prose across `ai-governance-board.md`, `consortium-participation.md`, `cost-sharing.md`, `cost-accounting.md`, `document-retention.md`, `delegation-of-authority.md`, `federal-award-integrity.md`, `pre-award-spending.md`, `proposal-routing.md`, `sttr-sbir-partnership.md`, `subaward-monitoring.md`, `time-effort-reporting.md`; also a role table in `ai-governance-board.md` and `delegation-of-authority.md` | **Not a copier variable.** Replace with the generic role phrase already used for this same role elsewhere in the corpus: "the fiscal custodian" (lowercase mid-sentence, "Fiscal Custodian" if it's a table-header/role-label position). Drop the parenthetical/appositive vendor name entirely, e.g. `"«VENDOR_NAME» shall maintain..."` → `"The fiscal custodian shall maintain..."`. See §7 for why this is a role phrase, not a variable. |
| IT vendor name (denylist: the IT vendor string) | One file, `data-management.md`: `"The IT Administrator (currently ⟨Vendor⟩)"` | **Not a copier variable.** Drop the parenthetical entirely: `"The IT Administrator (currently ⟨Vendor⟩) must perform..."` → `"The IT Administrator must perform..."`. |
| One-time historical facts (a specific founding season, specific numbered board motions) | `delegation-of-authority.md` table, "Authority basis" column (e.g. "Board appointment, ⟨season⟩ ⟨year⟩" / "Board Motion ⟨N⟩, ⟨season⟩ ⟨year⟩") | **Not a copier variable** — these are one-time facts specific to the source org's founding, meaningless (and misleading if left as-is) in a template nobody has adopted. Replace with a bracketed fillable placeholder, e.g. `[board action establishing this role]`. |
| AI platform/tool names (`Claude / Claude Code`, and two other agent-orchestration product names) | `ai-governance-board.md`, §3 "Authorized AI Platforms" table | **Not org-identifying — not a leak-guard concern, no action required for de-branding purposes.** Flagged as a content-quality note, not a de-branding requirement: this table hardcodes one org's specific AI tooling choices as if every adopter uses the same stack. The AI-governance cluster agent should consider softening this into an illustrative "example platforms — replace with your organization's actual authorized tools" framing, gated (like the rest of this cluster) behind `{% if ai_agents_enabled %}`. This is a judgment call for that agent, not a hard requirement of this spec. |

## 4. `fiscal_year_end`-dependent example — flag, don't silently fix

`time-effort-reporting.md` contains a worked reporting-period example tied
to a **calendar fiscal year** ("Period 2 \| July 1 – December 31 \|
February 15"). This is correct only when `fiscal_year_end` is
"December 31" (the `copier.yml` default) — it will be wrong prose for an
adopter with a non-calendar fiscal year. This is a content-logic issue,
not a pure literal-substitution one. Recommended treatment: wrap the
worked example in a comment noting it assumes a calendar fiscal year, or
convert it to describe the mechanism generically ("two six-month periods
aligned to your fiscal year") rather than hardcoding July 1/December 31.
Flagging for the agent assigned `time-effort-reporting.md` (federal-compliance
cluster) to decide; not solved in this spec or its exemplar.

## 5. Conditional-rendering rules

Two clusters are entirely gated, per the plan:

- **AI-governance cluster** (`ai-governance-board.md`,
  `ai-responsible-use-research.md`) renders only when `ai_agents_enabled`
  is true.
- **Federal-compliance cluster** (see manifest — 18 files, all citing
  2 CFR 200 / FAR / federal-program-specific statutes) renders only when
  `federal_grants_focus` is true.

Wrapping convention — wrap at the **file level**, using Jinja's
whitespace-control block delimiters so the rendered output has no stray
blank line where the tag was, and use the file's very first and very last
line as the tag boundary (i.e. the `{% if %}` is the first line of the
file, the `{% endif %}` is the last line, nothing else on either line):

```jinja
{%- if ai_agents_enabled %}
---
type: policy
...
---

# <Title>
...(full policy body, byline through last section)...
{%- endif %}
```

Do **not** wrap individual sections/paragraphs within an otherwise-always-
rendered file for these two flags — the plan calls for whole-policy
gating (a policy either belongs to the org's operating posture or it
doesn't). If a future cluster needs partial/section-level conditionals for
a different reason, that's a separate decision this spec doesn't cover;
raise it rather than improvising a new pattern.

The other three clusters (research-integrity, HR/operations, risk/legal)
are **not** gated by any `copier.yml` boolean — they render
unconditionally for every adopter.

## 6. Hard rule: de-brand identifiers, never soften normative language

AI-governance policy text, and any language that reads as
CLAUDE.md-derived operational instruction (e.g. the "Speed-First Operating
Principle" and "Human Oversight Model" sections of the AI-governance
policy), must be carried **verbatim in normative force**. De-brand
organization identifiers only (org name, EIN, state, addresses, proper
names, vendor names — per §3). Never weaken "must" to "should," never
remove a "shall," never soften an absolute rule into a recommendation,
even where the surrounding prose is being lightly touched for other
reasons. If a sentence mixes a normative rule with a branded identifier
inline, edit only the identifier and leave every modal verb exactly as
written.

## 7. Why proper names and vendor names are placeholders, not variables

`copier.yml`'s variable set is deliberately for facts that are (a) stable
at organization-identity level and (b) knowable/decided once, at
render-time, by whoever runs `copier copy` (typically a founding director
or ED setting up the repo). Who currently holds the Board Chair seat, or
which accounting firm currently serves as fiscal custodian, fails both
tests — boards turn over, vendors get replaced, and neither fact is part
of what makes an organization *the same organization* the way its EIN or
state of incorporation is. Baking a specific person's name into a
Jinja-rendered file that isn't easily bulk-updated later (unlike a
`copier update` picking up a genuine template improvement) would produce
stale, wrong governance documents the first time that seat changes hands.
Bracketed placeholders (`[current Board Chair]`) keep this fillable at the
adopter's own pace, exactly like `templates/`-directory fillable blanks in
the source corpus's own repo conventions.

## 8. Leak-guard gap found and fixed

`.gitleaks-denylist.txt` already contained an entry for the IT vendor
name (a two-word string), but had **no entry for the fiscal-custodian
vendor name** (a distinct three-word string, appearing in ~12 files, far
more prevalent than the IT vendor mention). This is a real gap: had any
future contributor pasted raw source material containing that name, CI
would not have caught it. Fixed as part of this change —
`.gitleaks-denylist.txt` now has both vendor names as explicit lines. See
the diff in that file; no other leak-guard config changed.

## 9. File naming — corrected

The dispatch brief for this work assumed source files were named
`POL-0XX-<slug>.md`. **They are not.** `ls` of the source `policies/`
directory confirms every file is named `<slug>.md` with no `POL-0XX`
prefix — the ID lives only in the frontmatter `id:` key (see §1). This
spec's naming convention mirrors the source exactly, with no added
prefix, to keep the transformation minimal and to keep filenames
consistent with `POLICY-INDEX.md`-style file-column references:

```
policies/<slug>.md   (source)  →  template/policies/<slug>.md.jinja  (this commons)
```

Example: `policies/conflict-of-interest.md` →
`template/policies/conflict-of-interest.md.jinja`.

## 10. Cross-reference hazard check — none found

Checked for: (a) references to other policies by their `POL-0XX` number
(none found — the corpus never cross-references a *different* policy by
ID; the only `POL-0XX` occurrence in each file is that file's own
frontmatter `id:` key), and (b) references to bylaws by article/section
number (none found in `policies/` — bylaws are mentioned only generically,
e.g. "the Organization's articles of incorporation, bylaws, or legal
filings," never "Bylaws Article IV Section 2"). The corpus does
cross-reference other policies **by title**, not number, e.g. "per the
Travel Policy," "governed by the Subaward and Subrecipient Monitoring
Policy" (found in `delegation-of-authority.md`, `proposal-routing.md`,
`teaming-agreement.md`). Titles are stable across this de-branding pass
(§1: `title` is left verbatim), so these cross-references remain valid
with no special handling required. Flagging only so no downstream agent
"fixes" a title-based cross-reference into an ID-based one or vice versa —
leave the citation style exactly as found in each file.
