# Not Legal Advice

## Plain statement

Nothing in this repository — the commons template, the rendered output of
`copier copy`, any policy language, bylaws example, resolution template, or
documentation — is legal, tax, or accounting advice. It is not a substitute
for advice from a licensed attorney, CPA, or other qualified professional
admitted to practice in your jurisdiction.

## Why this exists

The Golden Records commons exists to give nonprofit organizations a strong,
well-structured **starting point** for governance-as-code: bylaws
skeletons, board resolution templates, compliance checklists, and policy
language that reflects common nonprofit practice in the United States.
"Common practice" is not the same as "correct for your organization." Every
nonprofit is different — different state law, different IRS classification,
different board culture, different risk tolerance, different funder
requirements.

Templates in this repository are necessarily generic. They are written to
be *parameterized* (via [Copier](https://copier.readthedocs.io/)) and to be
a reasonable default, not a legal opinion about what your organization
specifically needs.

## What you should do before relying on any rendered document

1. **Have a licensed attorney in your state review** any bylaws, resolution,
   or governance policy before your board adopts it. State nonprofit
   corporation law varies significantly — a document that is fully
   compliant in one state may be invalid or incomplete in another.
2. **Have a CPA or tax professional review** anything touching your
   501(c)(3) status, public charity classification, unrelated business
   income, or federal grant compliance (2 CFR 200) before you rely on it.
3. **Do not treat a passing `leak-guard` CI check, a successful
   `copier copy`, or the existence of a document in this repo as legal
   sign-off.** These are engineering safeguards against accidental data
   leakage and template drift — they say nothing about legal sufficiency.
4. **Keep a paper trail.** If your counsel modifies a rendered document,
   that modification is now specific to your organization and should be
   maintained by your organization going forward, independent of upstream
   template updates (see `UPGRADE-GUIDE.md` for how `copier update` handles
   local customizations).

## No attorney-client relationship

Use of this repository, its templates, or its documentation does not create
an attorney-client relationship between you and the maintainers of this
commons, US-Council, or any contributor. The maintainers are not acting as
your counsel.

## Liability

This repository is provided "as is," without warranty of any kind, to the
extent permitted by the applicable license (see `LICENSE-CONTENT` for the
CC-BY-4.0 terms governing this document and other policy/template content,
and `LICENSE` for the Apache-2.0 terms governing scripts and CI logic).
Neither the maintainers nor contributors are liable for any damages arising
from the use of, or reliance on, this material.

## Questions

If you are unsure whether something in a rendered repository requires legal
review, the safe default is: **assume it does, and ask a lawyer.** This
document itself is not a checklist for what does or does not need review.
