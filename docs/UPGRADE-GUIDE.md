# Upgrade Guide

## What `copier update` does

When the `golden-records` commons template improves — a policy is
clarified, a compliance checklist is added, boilerplate language is fixed —
adopters pull those improvements into their already-rendered repo with:

```bash
copier update --trust
```

This is **not** a re-render from scratch. Copier performs a **3-way merge**:

1. It reads your repo's `.copier-answers.yml` (or whatever
   `_copier_conf.answers_file` was set to — the commons uses the default)
   to find out which template version you last updated from, and what
   answers you gave.
2. It re-renders the template at that *old* version with your saved
   answers — this is the "base."
3. It renders the template at the *new* version with the same answers —
   this is "theirs."
4. It diffs base → theirs, and applies that diff on top of your actual
   current files ("yours," which may include manual edits) — a standard
   3-way merge, same concept as a git merge.

If you kept a rendered file exactly as generated, this update is usually
clean. If you (or your counsel) hand-edited a rendered file, Copier tries
to merge the upstream change around your edits, same as git would — and,
same as git, it can produce conflict markers when both sides touched the
same lines.

## ⚠️ The dirty-tree gotcha

**`copier update` requires a clean git working tree to run.** This isn't a
soft recommendation — it's enforced. If you have uncommitted changes when
you run it, Copier refuses outright:

```
Destination repository is dirty; cannot continue. Please commit or stash your local changes and retry.
```

(Verified against Copier 9.16.0; the message may vary slightly by version,
but the refusal itself is a stable, documented behavior.)

**Before every `copier update`:**

```bash
git status              # confirm working tree is clean
git commit -am "..."    # commit any pending local changes first
copier update --trust
```

**After the update:**

```bash
git status               # review what changed
git diff                 # review the actual diff
```

**Do not try to work around the dirty-tree check by stashing "just to see
what happens" and forgetting to reconcile the stash afterward.** If the
merge goes sideways, you can end up with local edits, upstream changes,
and merge artifacts tangled together with no clean way to tell them apart.
Commit first, always — a real commit, not a stash, so the 3-way merge has
an honest "yours" to work from and your history shows exactly what state
you updated from.

## Worked example: a clean merge and a conflicted one

Both examples below were run for real against this repo's own template,
to confirm the mechanism and the exact output before documenting it —
not reconstructed from memory of how Copier is supposed to behave.

### Clean merge

Say the commons ships a wording clarification to `policies/travel.md` —
adding a sentence about receipt substantiation — in a part of the file an
adopter hasn't touched locally. Running `copier update --trust` on a clean
tree produces:

```
Updating to template version 0.1.1
Update complete. Please review the diff carefully — `copier update` does a
3-way merge and may have introduced conflict markers in files you've
customized locally. Resolve any `<<<<<<<` / `=======` / `>>>>>>>` blocks
before committing.
```

`git diff` shows a normal, single-sided change to `policies/travel.md` —
no conflict markers, because the adopter's local edits (elsewhere in the
same file) and the upstream change (a different line) didn't overlap. This
merges exactly like an uneventful `git merge` where two people edited
different parts of the same file.

### Conflicted merge

Now say the commons *and* the adopter both edit the **same sentence** —
the commons broadens a conflict-of-interest policy's scope to cover major
donors, while the adopter's counsel had already edited that same sentence
locally to cover directors' immediate family members. `copier update`
still completes (it does not abort on a conflict — it writes conflict
markers into the file and reports success at the update-mechanism level),
but the affected paragraph now looks like this:

```
<<<<<<< before updating
The purpose of this policy is to protect the interests of Example Org
("the Organization") ... a director, officer, key employee, or *immediate
family member of any of the foregoing*. ...
||||||| last update
The purpose of this policy is to protect the interests of Example Org
("the Organization") ... a director, officer, or key employee. ...
=======
The purpose of this policy is to protect the interests of Example Org
("the Organization") ... a director, officer, key employee, **or major
donor**. ...
>>>>>>> after updating
```

Note the exact marker set Copier uses — it's a **diff3-style** conflict,
with three sections rather than git's usual two:

- `<<<<<<< before updating` … the content **you had, immediately before this update** (your customization).
- `||||||| last update` … the **common ancestor** — what the file looked like the last time you updated, before either side changed it.
- `=======` … divider.
- `>>>>>>> after updating` … the **new upstream content** from the version you just updated to.

Having the ancestor section (`|||||||`) is genuinely useful here: it tells
you which words are actually new on *each* side, rather than leaving you to
diff two full paragraphs by eye. Resolve the conflict the same way you'd
resolve a `git merge` conflict — by hand, deciding what the reconciled
sentence should say (here, most likely: cover both immediate family members
*and* major donors), deleting all four marker lines, and committing the
result.

**Search for `<<<<<<<` across the whole diff after every update**, not
just in files you remember editing — it's easy to forget which files
counsel touched six months ago.

## Reviewing an upgrade as a governance change

Don't run `copier update` directly against `main`. Treat it like any other
proposed change under your rendered `GOVERNANCE.md`:

1. Create a branch — `admin/YYYY-MM-commons-update` is the natural prefix
   for a routine version bump with no substantive policy change; use
   `board/YYYY-MM-commons-update` instead if the upstream diff includes a
   substantive policy or bylaws-adjacent change that your quorum rules
   would otherwise require review for.
2. Run `copier update --trust` on that branch (tree clean, per above).
3. Resolve any conflict markers, then open the pull/merge request. The
   PR/MR description should summarize what changed upstream (the commons'
   own release notes or changelog between the old and new tag are the
   source for this) and flag anything that touches policy substance versus
   pure boilerplate/typo fixes.
4. Let it go through the same review and approval your branch prefix
   requires. A commons update that only fixes a typo in boilerplate still
   goes through `admin/` review; a commons update that changes what a
   policy actually requires should be treated as the substantive amendment
   it is, reviewed with the same scrutiny your board would give a
   hand-drafted amendment.
5. Merge = adoption, same as any other governance change.

This is the same discipline as reviewing a dependency upgrade in software:
routine and low-risk most of the time, but the review step exists
specifically to catch the time it isn't.

## Pinning vs. tracking

Adopters can either:

- **Pin to a specific semver tag** (e.g. `v0.2.0`) and upgrade
  deliberately by running `copier update --trust` when ready, letting
  Copier walk forward one tagged release at a time. This is the
  recommended default — predictable, reviewable upgrades, and it's what
  `copier copy` gives you by default (it checks out the latest tag, not
  `main`).
- **Track `main`** for the latest unreleased changes, via
  `copier update --trust --vcs-ref HEAD`. Not recommended for most
  adopters; only for those actively co-developing the commons, since
  `main` has no stability guarantee between commits.

## Getting help

If an update produces a merge result you don't understand, open an issue on
`USCouncil/golden-records` with your Copier version (`copier --version`),
the template version you're updating from/to, and the specific file where
the merge looks wrong.
