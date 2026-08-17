# KiCad Library Standard

## 1. Purpose

This document defines the standards for creating, reviewing, and maintaining shared KiCad symbols, footprints, and 3D models in the `kicad-library` repository.

The goals are:

- Prevent schematic and PCB errors caused by incorrect library data.
- Keep naming and organization consistent.
- Make the library portable between Linux and Windows.
- Make every released library item easy to review and trace back to a manufacturer datasheet.
- Maintain a controlled, repeatable ECAD library process for the team.

---

## 2. Repository Structure

The repository should use the following structure:

```text
kicad-library/
├── symbols/
│   └── zehel.kicad_sym
├── footprints/
│   └── zehel.pretty/
├── 3dmodels/
│   ├── Connectors/
│   ├── IC/
│   ├── Passives/
│   └── Mechanical/
├── datasheets/
├── docs/
│   ├── LIBRARY_STANDARD.md
│   └── CONTRIBUTING.md
├── README.md
└── .gitignore
```

The KiCad path variable used on all computers shall be:

```text
${MY_KICAD_LIB}
```

Example Linux path:

```text
/home/<username>/Documents/kicad-library
```

Example Windows path:

```text
C:\Users\<username>\Documents\kicad-library
```

Shared KiCad files shall not contain user-specific absolute paths.

---

## 3. Library Names

Use the following library names unless the team intentionally changes them:

### Symbol library

```text
zehel.kicad_sym
```

KiCad nickname:

```text
zehel
```

### Footprint library

```text
zehel.pretty
```

KiCad nickname:

```text
zehel
```

Example symbol reference:

```text
zehel:TPS62133RGTR
```

Example footprint reference:

```text
zehel:TI_RGT_VQFN-16_3x3mm_P0.5mm
```

---

## 4. Symbol Naming

### 4.1 Manufacturer-specific parts

Use the manufacturer part number as the symbol name whenever the symbol represents a specific purchasable component.

Examples:

```text
EFR32FG25B220F1920IM56
TPS62133RGTR
SN74LVC1G17DBVR
43045-0200
```

Do not use ambiguous names such as:

```text
Regulator1
MCU_New
Connector2
U1_Part
```

### 4.2 Generic parts

Generic symbols may use functional names.

Examples:

```text
R
C
LED
TestPoint
MountingHole
Fuse
```

### 4.3 Multi-unit devices

For multi-unit parts:

- Use KiCad units consistently.
- Group pins logically.
- Keep power pins in a dedicated unit when appropriate.
- Do not renumber or rearrange pins merely for visual convenience if it makes datasheet comparison harder.

---

## 5. Required Symbol Fields

Production symbols should contain the following fields where applicable:

| Field | Requirement |
|---|---|
| Reference | Required |
| Value | Required |
| Footprint | Required when known |
| Datasheet | Required |
| Description | Required |
| Manufacturer | Required for manufacturer-specific parts |
| MPN | Required for manufacturer-specific parts |

Example:

```text
Reference:       U
Value:           TPS62133RGTR
Footprint:       zehel:TI_RGT_VQFN-16_3x3mm_P0.5mm
Datasheet:       https://...
Description:     Step-down converter
Manufacturer:    Texas Instruments
MPN:             TPS62133RGTR
```

Optional fields may include:

```text
Supplier
Supplier Part Number
Lifecycle
RoHS
Voltage Rating
Tolerance
Package
Internal Part Number
```

---

## 6. Symbol Quality Requirements

Before a symbol is approved:

- Every pin number shall match the manufacturer datasheet.
- Every pin name shall match the datasheet unless a documented naming simplification is used.
- Pin electrical types shall be assigned correctly.
- Power pins shall not be incorrectly marked as passive.
- No-connect pins shall be handled intentionally.
- Hidden pins should be avoided unless there is a clear reason.
- Pin 1 location should be visually clear.
- Symbol units shall be logically organized.
- The symbol shall remain readable at normal schematic zoom.
- Fields shall not overlap the symbol body or pins.
- The datasheet link shall point to the manufacturer or another approved authoritative source.

For ICs, review the symbol against the datasheet pin table line by line.

---

## 7. Footprint Naming

Footprint names should identify the package clearly.

### 7.1 Generic package examples

```text
QFN-56_7x7mm_P0.4mm
SOT-23-5
TSSOP-16_4.4x5mm_P0.65mm
0603_1608Metric
```

### 7.2 Manufacturer-specific footprints

For custom or manufacturer-defined land patterns, include the manufacturer and part/package identifier.

Examples:

```text
Molex_43045-0200
TE_1-1734592-0
TI_RGT_VQFN-16_3x3mm_P0.5mm
SiliconLabs_EFR32FG25_QFN56
```

Avoid names such as:

```text
Connector1
IC_Footprint
New_QFN
U1
```

---

## 8. Footprint Quality Requirements

Every production footprint shall be checked for:

- Correct pad count.
- Correct pad numbering.
- Correct pad dimensions.
- Correct pad pitch.
- Correct package dimensions.
- Correct exposed-pad size and numbering, if applicable.
- Correct pin-1 indication.
- Correct package orientation.
- Appropriate solder mask settings.
- Appropriate paste mask settings.
- Silkscreen clearance from pads.
- Fabrication outline.
- Courtyard outline.
- Reference designator placement.
- Value field placement.
- Correct footprint anchor/origin.
- Correct 3D model placement and orientation.

Pad numbering is a critical item. Never approve a footprint solely because it visually resembles the package.

---

## 9. Datasheet and Land-Pattern Sources

Preferred source order:

1. Manufacturer datasheet.
2. Manufacturer package drawing.
3. Manufacturer recommended land pattern.
4. IPC-derived land pattern.
5. Trusted component documentation.

Do not rely on an unverified third-party footprint as the only source for dimensions.

When manufacturer and IPC recommendations differ, document which source was followed.

---

## 10. Silkscreen Standard

Silkscreen should:

- Clearly indicate component orientation where needed.
- Include a visible pin-1 indicator for polarized or orientation-sensitive packages.
- Avoid overlapping copper pads.
- Avoid running through exposed solderable areas.
- Remain useful after assembly whenever practical.

Do not use silkscreen as the only orientation indicator. Fabrication layers should also communicate package orientation.

---

## 11. Fabrication Layer Standard

The fabrication layer should contain enough information to identify the package body and orientation.

Recommended items:

- Package body outline.
- Pin-1 indicator.
- Reference designator.
- Accurate component body dimensions where practical.

---

## 12. Courtyard Standard

All production footprints should include a courtyard unless there is a documented reason not to.

The courtyard should:

- Fully enclose the component body and assembly keepout area.
- Use consistent clearance rules.
- Not overlap adjacent courtyards in a properly placed design unless intentionally approved.

---

## 13. 3D Model Standard

3D models shall be stored inside:

```text
${MY_KICAD_LIB}/3dmodels/
```

Use logical subdirectories such as:

```text
3dmodels/Connectors/
3dmodels/IC/
3dmodels/Passives/
3dmodels/Mechanical/
```

Footprints shall reference shared models using:

```text
${MY_KICAD_LIB}/3dmodels/<category>/<model>.step
```

Example:

```text
${MY_KICAD_LIB}/3dmodels/Connectors/Molex_43045-0200.step
```

Do not use user-specific paths such as:

```text
/home/user/Documents/...
C:\Users\user\Documents\...
```

Before approval, verify:

- Model body dimensions are reasonable.
- Model orientation matches the footprint.
- Model origin aligns with the footprint.
- Pin 1 orientation is correct.
- Z-height is correct.
- The model is not mirrored.

STEP models are preferred for mechanical interoperability.

---

## 14. Datasheets

Datasheet URLs should normally point to the manufacturer website.

Datasheet PDF files may be stored in the repository only when there is a clear team need.

Avoid committing large numbers of manufacturer PDFs because they can increase repository size significantly.

If local datasheets are used, organize them consistently.

Example:

```text
datasheets/TI/TPS62133.pdf
datasheets/Molex/43045-0200.pdf
```

---

## 15. Library Item Review Checklist

A new production component should pass the following review sequence:

```text
CREATE
  ↓
DATASHEET CHECK
  ↓
SYMBOL CHECK
  ↓
FOOTPRINT CHECK
  ↓
3D MODEL CHECK
  ↓
SECOND-PERSON REVIEW
  ↓
MERGE TO MAIN
  ↓
RELEASED
```

### Required checks

- [ ] Manufacturer and MPN verified.
- [ ] Datasheet verified.
- [ ] Symbol pin numbers verified.
- [ ] Symbol pin names verified.
- [ ] Pin electrical types reviewed.
- [ ] Footprint pad numbers verified.
- [ ] Footprint dimensions verified.
- [ ] Pin 1 verified.
- [ ] Courtyard reviewed.
- [ ] Silkscreen reviewed.
- [ ] Fabrication layer reviewed.
- [ ] 3D model alignment verified.
- [ ] Footprint linked to symbol.
- [ ] No local absolute paths present.
- [ ] Second-person review completed.

---

## 16. Git Branch Naming

Do not perform routine library development directly on `main`.

Use short-lived branches.

### New components

```text
add/<part-number>
```

Examples:

```text
add/tps62133
add/molex-43045
```

### Fixes

```text
fix/<part-number>-<issue>
```

Examples:

```text
fix/efr32fg25-pad-numbering
fix/molex-43045-silkscreen
```

### General updates

```text
update/<description>
```

Example:

```text
update/testpoint-symbols
```

---

## 17. Commit Message Standard

Commit messages should explain what changed.

Good examples:

```text
Add TPS62133 symbol and footprint
Add Molex 43045-0200 connector
Fix EFR32FG25 pad numbering
Update TPS62133 3D model alignment
Correct pin names on SN74LVC1G17DBVR
```

Avoid:

```text
update
changes
fix stuff
new files
test
```

---

## 18. Versioning

Known-good library states should be tagged.

Recommended semantic versioning pattern:

```text
v1.0.0
v1.1.0
v1.2.0
v2.0.0
```

Suggested interpretation:

- PATCH: corrections that do not intentionally break existing designs.
- MINOR: new approved components or non-breaking improvements.
- MAJOR: intentional breaking changes to library structure, naming, or component behavior.

Example:

```bash
git tag -a v1.1.0 -m "KiCad library release 1.1.0"
git push origin v1.1.0
```

---

## 19. Breaking Changes

Changes that can affect existing projects require additional caution.

Examples:

- Renaming a symbol.
- Renaming a footprint.
- Changing pin numbers.
- Changing footprint pad numbers.
- Changing a symbol-to-footprint assignment.
- Moving or renaming a 3D model.
- Changing the `MY_KICAD_LIB` structure.
- Deleting a previously released library item.

Breaking changes should be clearly described in the pull request.

When possible, preserve compatibility instead of deleting or renaming released items.

---

## 20. File and Path Rules

Allowed shared references:

```text
${MY_KICAD_LIB}/symbols/zehel.kicad_sym
${MY_KICAD_LIB}/footprints/zehel.pretty
${MY_KICAD_LIB}/3dmodels/IC/example.step
```

Do not commit references containing:

```text
/home/<specific-user>/
C:\Users\<specific-user>\
Desktop
Downloads
Temporary folders
Network paths that are unavailable to the team
```

---

## 21. Release Standard

A library version is considered released when:

- Required reviews are complete.
- The branch has been merged into `main`.
- CI or automated checks pass, if configured.
- The repository is in a clean state.
- A release tag is created when appropriate.

Production PCB projects should preferably record which library release/tag was used.

---

## 22. Ownership and Responsibility

The author of a new component is responsible for the first-pass validation.

The reviewer is responsible for independently checking critical information, especially:

- Pin numbers.
- Pad numbers.
- Package dimensions.
- Pin 1.
- Footprint orientation.
- Exposed pad implementation.

Approval should not be treated as a visual-only review.

---

## 23. Golden Rule

If a library item cannot be confidently verified against an authoritative source, it should not be merged into `main` as a production-ready component.
