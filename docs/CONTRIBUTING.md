# Contributing to the Shared KiCad Library

## 1. Purpose

This document describes the required workflow for contributing symbols, footprints, 3D models, and documentation to the shared KiCad library.

The goal is to keep `main` stable and production-ready while allowing multiple team members to add and improve components safely.

---

## 2. Before You Begin

You need:

- A GitHub account with access to the repository.
- Git installed.
- SSH authentication configured for GitHub.
- KiCad 10 installed.
- The shared repository cloned locally.
- The KiCad path variable `MY_KICAD_LIB` configured.

Repository:

```text
git@github.com:zehel15/kicad-library.git
```

---

## 3. Never Start by Editing an Outdated Copy

Before starting work:

```bash
cd ~/Documents/kicad-library
git switch main
git pull
```

On Windows PowerShell, use the local Windows repository path.

Verify:

```bash
git status
```

You should normally see:

```text
On branch main
Your branch is up to date with 'origin/main'.
nothing to commit, working tree clean
```

---

## 4. Create a Branch

Do not make routine library changes directly on `main`.

### Adding a component

```bash
git switch -c add/tps62133
```

### Fixing a component

```bash
git switch -c fix/tps62133-footprint
```

### Updating documentation or general library content

```bash
git switch -c update/library-documentation
```

Branch naming:

```text
add/<part-number>
fix/<part-number>-<issue>
update/<description>
```

---

## 5. Create or Modify the KiCad Item

Make the required changes in KiCad.

For a new component, this may include:

- Symbol.
- Footprint.
- 3D model.
- Datasheet link.
- Manufacturer field.
- MPN field.
- Description.
- Footprint assignment.

Follow `docs/LIBRARY_STANDARD.md`.

---

## 6. Verify the Component Before Committing

At minimum, compare the item against the manufacturer datasheet.

### Symbol check

- [ ] Part number is correct.
- [ ] Pin numbers are correct.
- [ ] Pin names are correct.
- [ ] Electrical pin types are reasonable.
- [ ] Power pins are handled correctly.
- [ ] Datasheet URL is correct.
- [ ] Manufacturer is correct.
- [ ] MPN is correct.
- [ ] Footprint assignment is correct.

### Footprint check

- [ ] Pad count is correct.
- [ ] Pad numbers are correct.
- [ ] Pad dimensions are correct.
- [ ] Pad pitch is correct.
- [ ] Package dimensions are correct.
- [ ] Pin 1 is correct.
- [ ] Silkscreen is clear.
- [ ] Fabrication outline is present.
- [ ] Courtyard is present.
- [ ] Exposed pad is correct, if applicable.
- [ ] 3D model alignment is correct.

---

## 7. Inspect Git Changes

Run:

```bash
git status
```

Review what changed before staging anything.

If useful:

```bash
git diff
```

Do not commit unrelated temporary files.

---

## 8. Stage Changes

Stage the intended files:

```bash
git add .
```

Then verify:

```bash
git status
```

If unrelated files were staged accidentally, remove them from the staging area before committing.

---

## 9. Commit

Use a clear commit message.

Examples:

```bash
git commit -m "Add TPS62133 symbol and footprint"
```

```bash
git commit -m "Fix EFR32FG25 pad numbering"
```

```bash
git commit -m "Update Molex 43045 3D model alignment"
```

Avoid vague messages such as:

```text
update
changes
fix
stuff
```

---

## 10. Push the Branch

For the first push of a new branch:

```bash
git push -u origin add/tps62133
```

After the upstream branch has been established:

```bash
git push
```

---

## 11. Create a GitHub Pull Request

Open the repository on GitHub.

Create a Pull Request from your branch into:

```text
main
```

The pull request should contain:

### Part information

```text
Manufacturer:
MPN:
Datasheet:
Package:
```

### What changed

Briefly describe the symbol, footprint, 3D model, or documentation changes.

### Validation performed

State what was checked.

Example:

```text
- Verified all pin numbers against datasheet Table 5-1.
- Verified QFN pad pitch and body dimensions against package drawing.
- Verified exposed pad size.
- Verified pin 1 orientation.
- Checked 3D model alignment in KiCad 3D Viewer.
```

### Known limitations

Document anything that still requires attention.

---

## 12. Request a Second-Person Review

A second team member should independently review production library items before merge.

The reviewer should not simply verify that the part "looks right."

Critical checks include:

- Symbol pin numbers.
- Footprint pad numbers.
- Pin 1.
- Package dimensions.
- Exposed pad.
- Footprint orientation.
- 3D model orientation.

---

## 13. Reviewer Checklist

The reviewer should confirm:

- [ ] Manufacturer and MPN match the intended component.
- [ ] Datasheet is authoritative.
- [ ] Pin numbers match the datasheet.
- [ ] Pin names are correct.
- [ ] Pin electrical types have been reviewed.
- [ ] Footprint pad numbers match the symbol.
- [ ] Pad dimensions and pitch match the package documentation.
- [ ] Pin 1 is correct.
- [ ] Silkscreen is reasonable.
- [ ] Fabrication outline is reasonable.
- [ ] Courtyard is reasonable.
- [ ] 3D model alignment is correct.
- [ ] No machine-specific paths were introduced.
- [ ] Changes do not unintentionally break released components.

---

## 14. Address Review Comments

If the reviewer requests changes:

Make the changes locally on the same branch.

Then:

```bash
git add .
git commit -m "Address library review comments"
git push
```

The existing Pull Request will update automatically.

---

## 15. Merge

Once review is complete, merge the Pull Request into:

```text
main
```

Recommended policy:

- Do not merge unreviewed production library items.
- Do not force-push `main`.
- Do not bypass review for critical footprint corrections unless there is an approved emergency process.

---

## 16. Update Your Local Main Branch

After the Pull Request is merged:

```bash
git switch main
git pull
```

Delete the local working branch if it is no longer needed:

```bash
git branch -d add/tps62133
```

The remote branch may also be deleted after merge.

---

## 17. Standard Daily Workflow

### Start

```bash
git switch main
git pull
git status
```

### Create branch

```bash
git switch -c add/<part-number>
```

### Make KiCad changes

Use KiCad 10 and follow `LIBRARY_STANDARD.md`.

### Check

```bash
git status
git diff
```

### Commit

```bash
git add .
git commit -m "Describe the change"
```

### Push

```bash
git push -u origin add/<part-number>
```

### Review

Create a Pull Request and request a second-person review.

### Finish

After merge:

```bash
git switch main
git pull
```

---

## 18. Handling a Conflict

If `git pull` or a merge reports a conflict, do not guess.

First:

```bash
git status
```

Identify the conflicting files.

For KiCad symbol files, conflicts can be difficult because multiple symbols may be stored in the same `.kicad_sym` file.

If two people edited the shared symbol library at the same time:

1. Coordinate with the other contributor.
2. Decide which changes need to be preserved.
3. Resolve the conflict carefully.
4. Open the resulting library in KiCad.
5. Verify both components after resolution.
6. Commit the resolved version.

Do not automatically accept "ours" or "theirs" unless you are certain that no required changes will be lost.

---

## 19. Avoiding Symbol-Library Conflicts

Because many symbols can exist in one `.kicad_sym` file:

- Pull before starting work.
- Keep branches short-lived.
- Avoid having multiple users edit the symbol library simultaneously when practical.
- Communicate when working on commonly modified components.
- Merge completed work promptly.

Footprints are less conflict-prone because each `.kicad_mod` file is normally a separate file.

---

## 20. Do Not Commit Secrets

Never commit:

- SSH private keys.
- GitHub personal access tokens.
- Passwords.
- API keys.
- KWallet exports.
- Credential files.

Safe to share:

```text
id_ed25519.pub
```

Never share:

```text
id_ed25519
```

---

## 21. Portable Path Requirement

Shared library content must use:

```text
${MY_KICAD_LIB}
```

Example:

```text
${MY_KICAD_LIB}/3dmodels/Connectors/Molex_43045-0200.step
```

Do not commit:

```text
/home/sergio/Documents/...
```

or:

```text
C:\Users\Sergio\Documents\...
```

The same repository must work on Linux and Windows.

---

## 22. Fixing an Existing Released Component

Create a fix branch:

```bash
git switch main
git pull
git switch -c fix/<part-number>-<issue>
```

Example:

```bash
git switch -c fix/efr32fg25-pin1
```

Make the correction, validate it, and commit:

```bash
git add .
git commit -m "Fix EFR32FG25 pin 1 orientation"
git push -u origin fix/efr32fg25-pin1
```

In the Pull Request, clearly state whether the correction can affect existing PCB designs.

---

## 23. Breaking Changes

Examples of breaking changes:

- Changing pin numbers.
- Changing pad numbers.
- Renaming symbols.
- Renaming footprints.
- Removing library items.
- Changing footprint assignments.
- Moving 3D models without preserving the old reference.
- Changing library path conventions.

Breaking changes require explicit review and should be clearly labeled in the Pull Request.

---

## 24. Library Releases

When a stable set of changes is ready, an authorized maintainer may create a version tag.

Example:

```bash
git switch main
git pull
git tag -a v1.1.0 -m "KiCad library release 1.1.0"
git push origin v1.1.0
```

Production projects should preferably record the library version used at release time.

---

## 25. Recommended Pull Request Template

```markdown
## Component

Manufacturer:
MPN:
Datasheet:
Package:

## Change

Describe what was added or modified.

## Symbol Verification

- [ ] Pin numbers verified
- [ ] Pin names verified
- [ ] Electrical pin types reviewed
- [ ] Datasheet link verified
- [ ] Footprint assignment verified

## Footprint Verification

- [ ] Pad count verified
- [ ] Pad numbers verified
- [ ] Pad sizes verified
- [ ] Pitch verified
- [ ] Package dimensions verified
- [ ] Pin 1 verified
- [ ] Courtyard reviewed
- [ ] Silkscreen reviewed
- [ ] Fabrication layer reviewed

## 3D Model

- [ ] Model present
- [ ] Alignment verified
- [ ] Orientation verified
- [ ] Z-height verified

## Compatibility

- [ ] No breaking change
- [ ] Breaking change documented below

## Notes

Add any review notes or known limitations here.
```

---

## 26. Final Rule

The shared library is production infrastructure.

If there is uncertainty about a pin number, pad number, dimension, package orientation, or datasheet interpretation, stop and verify it before merging.
