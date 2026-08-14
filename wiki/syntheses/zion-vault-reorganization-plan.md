# Zion Vault Reorganization Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Reorganize every Zion Vault note into the approved domain hierarchy while preserving content, attachments, old-name discoverability, and all resolvable Obsidian links.

**Architecture:** Use a manifest-driven, eight-batch migration. A checked-in CSV is the sole authority for moves and renames; each batch validates destinations and link rewrites before changing files, then runs a full integrity gate before the next batch. Baseline hashes protect existing `raw/` sources and attachments.

**Tech Stack:** Obsidian Markdown, Wiki links, PowerShell 5.1+, Git, SHA-256 file hashes.

## Global Constraints

- Follow [[AGENTS]] and [[wiki/syntheses/zion-vault-reorganization-design]].
- Preserve the existing numbered domain directories and English technical naming style.
- Keep Chinese explanatory prose unchanged unless a link path must change.
- Do not modify, delete, or move files that already existed under `raw/` before migration.
- Do not move or rename attachment files under `00 - Assets/`.
- Do not touch `.obsidian/`, `.git/`, `.trash/`, `.agents/`, or `.codex/`.
- Never overwrite a destination; stop the batch on case-insensitive path collision.
- Preserve the previous basename as a frontmatter alias whenever a basename changes.
- Use complete-path Wiki links whenever a basename is ambiguous.
- Do not expose the contents of local configuration notes in reports or logs.
- Each batch must pass verification before the next batch begins.
- Existing unrelated workspace changes must remain untouched.

---

## Planned Artifacts

- Create: `wiki/syntheses/zion-vault-reorganization-baseline.csv` — immutable pre-migration path, size, and SHA-256 inventory.
- Create: `wiki/syntheses/zion-vault-reorganization-map.csv` — authoritative move/rename manifest.
- Create: `wiki/syntheses/zion-vault-reorganization-report.md` — batch counts, ambiguous cases, link results, and final verification.
- Modify: `index.md` — rebuild final domain navigation.
- Modify: `log.md` — append one concise entry per completed batch.
- Move/rename: Markdown notes listed in the manifest only.
- Modify: Markdown notes whose Wiki links, embeds, or aliases must change because of the manifest.

## Manifest Interface

`zion-vault-reorganization-map.csv` must use these exact columns:

```csv
Batch,Domain,Source,Destination,OldBasename,NewBasename,Reason,Status
```

Allowed `Batch` values:

```text
cpp-language-oop
cpp-memory-build
cpp-stl-containers
cpp-stl-algorithms
d3d11
math
other-domains
root-loose-notes
```

Allowed `Status` values:

```text
approved
moved
verified
blocked
```

Every source and destination is a Vault-relative path using `/`. The migration command must process only rows whose `Status` is `approved` and whose `Batch` exactly matches the requested batch.

---

### Task 1: Checkpoint Current Approved Smart Pointer Work

**Files:**

- Move set already present: `01 - C++/CPP.SmartPointers.*.md` → `01 - C++/Smart Pointers/*.md`
- Modify: `index.md`
- Modify: `log.md`
- Exclude: `.obsidian/plugins/colored-tags/data.json`

**Interfaces:**

- Consumes: the four Smart Pointer moves completed before the full-Vault design.
- Produces: a clean Git checkpoint from which the full migration can be isolated and reviewed.

- [ ] **Step 1: Verify the pending change scope**

Run:

```powershell
git status --short
git diff --check
```

Expected: only the four Smart Pointer source deletions, `01 - C++/Smart Pointers/`, `index.md`, `log.md`, and the unrelated colored-tags file appear. `git diff --check` exits `0`.

- [ ] **Step 2: Stage only the approved knowledge changes**

Run:

```powershell
git add -- '01 - C++/CPP.SmartPointers.Shared Pointers.md' `
  '01 - C++/CPP.SmartPointers.Smart Pointer.md' `
  '01 - C++/CPP.SmartPointers.Unique Pointers.md' `
  '01 - C++/CPP.SmartPointers.Weak Pointers.md' `
  '01 - C++/Smart Pointers' `
  'index.md' `
  'log.md'
git diff --cached --check
```

Expected: cached diff contains the four moves plus index/log changes and excludes `.obsidian/`.

- [ ] **Step 3: Commit the checkpoint**

Run:

```powershell
git commit -m "docs: organize C++ smart pointer notes"
```

Expected: commit succeeds and `.obsidian/plugins/colored-tags/data.json` remains unstaged.

---

### Task 2: Capture the Immutable Baseline

**Files:**

- Create: `wiki/syntheses/zion-vault-reorganization-baseline.csv`
- Modify: `wiki/syntheses/zion-vault-reorganization-report.md`

**Interfaces:**

- Consumes: the checkpointed Vault state.
- Produces: CSV columns `Path,Extension,Length,SHA256,Protected`, used by every later verification task.

- [ ] **Step 1: Generate the baseline inventory**

Run from the Vault root:

```powershell
$vaultRoot = (Resolve-Path '.').Path
$excluded = '\\(.git|.obsidian|.trash|.agents|.codex)(\\|$)'
Get-ChildItem -LiteralPath $vaultRoot -Recurse -File |
  Where-Object { $_.FullName -notmatch $excluded } |
  ForEach-Object {
    $relative = $_.FullName.Substring($vaultRoot.Length + 1).Replace('\', '/')
    [PSCustomObject]@{
      Path      = $relative
      Extension = $_.Extension
      Length    = $_.Length
      SHA256    = (Get-FileHash -LiteralPath $_.FullName -Algorithm SHA256).Hash
      Protected = ($relative -like 'raw/*' -or $relative -like '00 - Assets/*')
    }
  } |
  Sort-Object Path |
  Export-Csv -LiteralPath 'wiki/syntheses/zion-vault-reorganization-baseline.csv' -NoTypeInformation -Encoding UTF8
```

Expected: the CSV contains every non-excluded file; all existing `raw/*` and `00 - Assets/*` rows have `Protected=True`.

- [ ] **Step 2: Record baseline counts without sensitive content**

Create `wiki/syntheses/zion-vault-reorganization-report.md` with:

```markdown
# Zion Vault Reorganization Report

## Baseline

- Markdown notes before migration: 299
- PNG attachments before migration: 11
- Wiki links observed during design audit: 277
- Existing raw sources: protected by SHA-256 baseline

## Batch Results

## Ambiguous Cases

## Final Verification
```

- [ ] **Step 3: Verify baseline uniqueness**

Run:

```powershell
$baseline = Import-Csv -LiteralPath 'wiki/syntheses/zion-vault-reorganization-baseline.csv'
$duplicates = $baseline | Group-Object Path | Where-Object Count -gt 1
if ($duplicates) { throw "Duplicate baseline paths detected" }
if (($baseline | Where-Object Extension -eq '.md').Count -ne 299) { throw "Unexpected Markdown baseline count" }
if (($baseline | Where-Object Extension -eq '.png').Count -ne 11) { throw "Unexpected PNG baseline count" }
```

Expected: command exits `0`.

- [ ] **Step 4: Commit the baseline**

Run:

```powershell
git add -- 'wiki/syntheses/zion-vault-reorganization-baseline.csv' 'wiki/syntheses/zion-vault-reorganization-report.md'
git commit -m "docs: capture Zion migration baseline"
```

---

### Task 3: Build and Validate the Authoritative Manifest

**Files:**

- Create: `wiki/syntheses/zion-vault-reorganization-map.csv`
- Modify: `wiki/syntheses/zion-vault-reorganization-report.md`

**Interfaces:**

- Consumes: current file inventory and the approved taxonomy in the design note.
- Produces: one exact `approved` row for every file that will move or change basename.

- [ ] **Step 1: Enumerate candidate notes**

Run:

```powershell
Get-ChildItem -LiteralPath '01 - C++','03 - D3D11','04 - Math','animation','rendering','thesis','wiki' -Recurse -File -Filter '*.md' |
  Sort-Object FullName |
  Select-Object FullName
```

Expected: every domain note is visible for classification; control files and protected existing raw sources are absent.

- [ ] **Step 2: Create the manifest using the exact taxonomy rules**

Populate the required CSV columns. Apply these deterministic transformations:

```text
CPP.<topic>.md                         → 01 - C++/<approved category>/<topic>.md
CPP.<container>.<member>.md            → 01 - C++/STL/Containers/<container>/<container>.<member>.md
CPP.Algorithm.<name>.md                → 01 - C++/STL/Algorithms/<name>.md
CPP.Iterators.<name>.md                → 01 - C++/STL/Iterators/<name>.md
D3D11.<topic>.md                       → 03 - D3D11/<approved category>/<topic>.md
MP.Vec2.<member>.md                    → 04 - Math/Vectors/Vec2.<member>.md
MP.AABB2.<member>.md                   → 04 - Math/Geometry/AABB/AABB2.<member>.md
MP.OBB2.<member>.md                    → 04 - Math/Geometry/OBB/OBB2.<member>.md
MP.Disc.<member>.md                    → 04 - Math/Geometry/Disc/Disc.<member>.md
MP.MathUtils.<member>.md               → 04 - Math/Utilities/<member>.md
Collision.<topic>.md                   → 04 - Math/Collision/<topic>.md
Physics.<topic>.md                     → 04 - Math/Physics/<topic>.md
```

Apply exact spelling corrections:

```text
ClassMemberAccessModifires → Class Member Access Modifiers
Deferencing                → Dereferencing
PriorityQueue              → priority_queue
Range-Base for             → Range-Based For
const _iterator            → const_iterator
```

Root loose-note destinations:

```text
LLM Wiki.md  → raw/articles/LLM-Wiki.md
2026-07-11.md → raw/notes/Networking-Packets.md
Perforce.md   → raw/notes/Perforce-Local-Configuration.md
```

Rows that do not change path must not be added to the manifest.

- [ ] **Step 3: Validate every manifest row before migration**

Run:

```powershell
$vaultRoot = (Resolve-Path '.').Path
$map = Import-Csv -LiteralPath 'wiki/syntheses/zion-vault-reorganization-map.csv'

if ($map.Count -eq 0) { throw "Manifest is empty" }
if (($map | Where-Object Status -ne 'approved').Count -gt 0) { throw "Manifest contains non-approved rows" }
if (($map | Group-Object Source | Where-Object Count -gt 1).Count -gt 0) { throw "Duplicate source path" }
if (($map | Group-Object Destination | Where-Object Count -gt 1).Count -gt 0) { throw "Duplicate destination path" }

foreach ($row in $map) {
  $source = [IO.Path]::GetFullPath((Join-Path $vaultRoot $row.Source))
  $destination = [IO.Path]::GetFullPath((Join-Path $vaultRoot $row.Destination))
  if (-not $source.StartsWith($vaultRoot, [StringComparison]::OrdinalIgnoreCase)) { throw "Source escapes Vault: $($row.Source)" }
  if (-not $destination.StartsWith($vaultRoot, [StringComparison]::OrdinalIgnoreCase)) { throw "Destination escapes Vault: $($row.Destination)" }
  if (-not (Test-Path -LiteralPath $source)) { throw "Missing source: $($row.Source)" }
  if (Test-Path -LiteralPath $destination) { throw "Destination already exists: $($row.Destination)" }
}
```

Expected: command exits `0`; no source is missing, no destination exists, and there are no duplicate paths.

- [ ] **Step 4: Record ambiguous notes without moving them**

Append only genuinely uncertain notes to `## Ambiguous Cases` in the report. Each entry must contain current path, candidate categories, and why the design rules do not decide between them. Such notes remain absent from the manifest.

- [ ] **Step 5: Commit the validated manifest**

Run:

```powershell
git add -- 'wiki/syntheses/zion-vault-reorganization-map.csv' 'wiki/syntheses/zion-vault-reorganization-report.md'
git commit -m "docs: define Zion vault migration manifest"
```

---

### Task 4: Execute One Manifest Batch Safely

**Files:**

- Move: rows in `wiki/syntheses/zion-vault-reorganization-map.csv` for one batch.
- Modify: all Markdown files containing affected Wiki links or embeds.
- Modify: moved files whose basename changes, adding the old basename as an alias.
- Modify: manifest row `Status` from `approved` to `moved`, then `verified`.

**Interfaces:**

- Consumes: one validated manifest batch.
- Produces: moved notes, rewritten links, aliases, and a verified batch state.

- [ ] **Step 1: Select exactly one batch**

Set `$batchName` to one allowed batch value. Run:

```powershell
$batchName = 'cpp-language-oop'
$mapPath = 'wiki/syntheses/zion-vault-reorganization-map.csv'
$rows = Import-Csv -LiteralPath $mapPath | Where-Object { $_.Batch -eq $batchName -and $_.Status -eq 'approved' }
if ($rows.Count -eq 0) { throw "No approved rows for $batchName" }
```

For later executions, replace only the literal batch value in the first line.

- [ ] **Step 2: Re-run source, destination, and Vault-boundary preflight**

Run the validation loop from Task 3 Step 3 against `$rows`. Expected: exit `0` before any move occurs.

- [ ] **Step 3: Move the selected rows**

Run:

```powershell
$vaultRoot = (Resolve-Path '.').Path
foreach ($row in $rows) {
  $source = [IO.Path]::GetFullPath((Join-Path $vaultRoot $row.Source))
  $destination = [IO.Path]::GetFullPath((Join-Path $vaultRoot $row.Destination))
  $destinationDirectory = Split-Path -Parent $destination
  if (-not (Test-Path -LiteralPath $destinationDirectory)) {
    New-Item -ItemType Directory -Path $destinationDirectory | Out-Null
  }
  Move-Item -LiteralPath $source -Destination $destination
}
```

Expected: every selected source disappears and every destination exists.

- [ ] **Step 4: Add aliases for changed basenames**

For each row where `OldBasename != NewBasename`, add `OldBasename` under YAML `aliases`. If the file has no frontmatter, add this minimal block at the top:

```yaml
---
aliases:
  - Exact OldBasename From Manifest
---
```

When frontmatter already exists, merge the alias without replacing existing properties or aliases.

- [ ] **Step 5: Rewrite affected links and embeds**

For each row, update exact occurrences of:

```text
[[SourceWithoutExtension]]
[[SourceWithoutExtension|Display Text]]
![[SourceWithoutExtension]]
![[SourceWithoutExtension|SizeOrText]]
[[OldBasename]]
[[OldBasename|Display Text]]
```

Use `DestinationWithoutExtension` for path-qualified replacements. Preserve headings (`#Heading`) and block IDs (`#^block-id`). If `OldBasename` is shared by more than one note, replace only path-qualified links and record the ambiguous bare links in the report.

- [ ] **Step 6: Mark selected rows as moved**

Update only the selected rows from `approved` to `moved`; preserve all other CSV fields and row order.

- [ ] **Step 7: Run the batch gate**

Verify:

```powershell
$vaultRoot = (Resolve-Path '.').Path
foreach ($row in $rows) {
  if (Test-Path -LiteralPath (Join-Path $vaultRoot $row.Source)) { throw "Source still exists: $($row.Source)" }
  if (-not (Test-Path -LiteralPath (Join-Path $vaultRoot $row.Destination))) { throw "Destination missing: $($row.Destination)" }
}
git diff --check
```

Also search the Vault for every old full path. Expected: no old path remains outside the manifest, baseline, report, or historical log text.

- [ ] **Step 8: Mark selected rows as verified and report counts**

Change the selected rows from `moved` to `verified`. Append the batch name, moved count, link replacements, alias additions, and verification result under `## Batch Results`.

- [ ] **Step 9: Commit the batch**

Stage only the selected batch moves, affected link files, manifest, report, index, and log. Exclude `.obsidian/`. Run:

```powershell
git diff --cached --check
$commitMessage = "docs: reorganize $batchName notes"
git commit -m $commitMessage
```

Expected: the commit message contains the exact selected batch value.

---

### Task 5: Run the Eight Migration Batches

**Files:**

- Consume and update: `wiki/syntheses/zion-vault-reorganization-map.csv`
- Modify/move: notes selected by each batch.
- Modify: `wiki/syntheses/zion-vault-reorganization-report.md`
- Modify: `log.md`

**Interfaces:**

- Consumes: Task 4 batch procedure.
- Produces: all manifest rows in `verified` state.

- [ ] **Step 1: Run `cpp-language-oop`**

Use Task 4 with `$batchName = 'cpp-language-oop'`. Expected: Language Fundamentals, Functions, and Classes & OOP destinations verify successfully.

- [ ] **Step 2: Run `cpp-memory-build`**

Use Task 4 with `$batchName = 'cpp-memory-build'`. Expected: Pointers & References, Memory Management, Templates, and Build & Preprocessor destinations verify successfully. Move the existing `01 - C++/Smart Pointers/` subtree to `01 - C++/Memory Management/Smart Pointers/` through manifest rows.

- [ ] **Step 3: Run `cpp-stl-containers`**

Use Task 4 with `$batchName = 'cpp-stl-containers'`. Expected: every container note is under its exact container subdirectory.

- [ ] **Step 4: Run `cpp-stl-algorithms`**

Use Task 4 with `$batchName = 'cpp-stl-algorithms'`. Expected: algorithms and iterators are separated into their approved directories.

- [ ] **Step 5: Run `d3d11`**

Use Task 4 with `$batchName = 'd3d11'`. Expected: all unambiguous D3D11 notes are under Initialization, Pipeline, Resources, Shaders, Drawing, or HLSL.

- [ ] **Step 6: Run `math`**

Use Task 4 with `$batchName = 'math'`. Expected: all unambiguous Math notes are under Vectors, Geometry, Collision, Physics, Rotation & Transforms, or Utilities.

- [ ] **Step 7: Run `other-domains`**

Use Task 4 with `$batchName = 'other-domains'`. Expected: animation, rendering, thesis, and wiki pages remain in place unless the manifest documents a clear correction.

- [ ] **Step 8: Run `root-loose-notes`**

Use Task 4 with `$batchName = 'root-loose-notes'`. Expected: the three approved root notes move to their exact raw destinations; their contents are not copied into the report.

- [ ] **Step 9: Verify manifest completion**

Run:

```powershell
$map = Import-Csv -LiteralPath 'wiki/syntheses/zion-vault-reorganization-map.csv'
$incomplete = $map | Where-Object Status -ne 'verified'
if ($incomplete) { $incomplete | Format-Table Batch,Source,Destination,Status; throw "Manifest is incomplete" }
```

Expected: command exits `0` and every manifest row is `verified`.

---

### Task 6: Rebuild Index and Run Full-Vault Verification

**Files:**

- Modify: `index.md`
- Modify: `log.md`
- Modify: `wiki/syntheses/zion-vault-reorganization-report.md`
- Verify: all Markdown notes and attachments.

**Interfaces:**

- Consumes: fully verified manifest and immutable baseline.
- Produces: final navigation, final log entry, and evidence-backed completion report.

- [ ] **Step 1: Rebuild `index.md` from the final directory structure**

Update C++, D3D11, Math, animation, rendering, thesis, and wiki sections with complete-path Wiki links. Remove every obsolete path. Keep Control Plane links at the end.

- [ ] **Step 2: Append the final log entry**

Record the eight completed batches, total moved notes, total renamed basenames, total rewritten links, alias count, and ambiguous notes left in place. Do not include local configuration contents.

- [ ] **Step 3: Verify protected hashes**

Run:

```powershell
$vaultRoot = (Resolve-Path '.').Path
$baseline = Import-Csv -LiteralPath 'wiki/syntheses/zion-vault-reorganization-baseline.csv'
$protected = $baseline | Where-Object Protected -eq 'True'
foreach ($row in $protected) {
  $path = Join-Path $vaultRoot $row.Path
  if (-not (Test-Path -LiteralPath $path)) { throw "Protected file missing: $($row.Path)" }
  $hash = (Get-FileHash -LiteralPath $path -Algorithm SHA256).Hash
  if ($hash -ne $row.SHA256) { throw "Protected file changed: $($row.Path)" }
}
```

Expected: all pre-existing raw sources and attachments remain present with identical SHA-256 hashes.

- [ ] **Step 4: Verify file-count accounting**

Calculate:

```powershell
$markdown = Get-ChildItem -LiteralPath '.' -Recurse -File -Filter '*.md' |
  Where-Object { $_.FullName -notmatch '\\(.git|.obsidian|.trash|.agents|.codex)(\\|$)' }
$png = Get-ChildItem -LiteralPath '00 - Assets' -Recurse -File -Filter '*.png'
Write-Output "Markdown=$($markdown.Count)"
Write-Output "PNG=$($png.Count)"
```

Expected: PNG is `11`. Markdown count equals baseline `299` plus the report Markdown file, for a total of `300`; CSV artifacts do not affect the Markdown count.

- [ ] **Step 5: Run full old-path and whitespace checks**

Run:

```powershell
$map = Import-Csv -LiteralPath 'wiki/syntheses/zion-vault-reorganization-map.csv'
foreach ($row in $map) {
  if (Test-Path -LiteralPath $row.Source) { throw "Old path remains: $($row.Source)" }
  if (-not (Test-Path -LiteralPath $row.Destination)) { throw "New path missing: $($row.Destination)" }
}
git diff --check
```

Expected: all source paths are absent, all destination paths exist, and `git diff --check` exits `0`.

- [ ] **Step 6: Resolve Wiki links and embeds**

Extract every `[[...]]` target, remove aliases (`|...`), headings (`#...`), and `.md`, then resolve against exact Vault paths and unique basenames. Record unresolved and ambiguous targets in the report. Completion requires zero newly introduced unresolved or ambiguous targets; pre-existing issues must be identified separately.

- [ ] **Step 7: Verify Markdown structure**

Check all changed Markdown files for:

```text
paired triple-backtick code fences
at most one YAML frontmatter block at the top
no duplicate alias entries
valid complete-path Wiki links for renamed notes
```

Expected: zero structural failures.

- [ ] **Step 8: Complete the report**

Under `## Final Verification`, record actual counts and pass/fail results for protected hashes, file accounting, mapping completion, links, embeds, Markdown structure, and `git diff --check`.

- [ ] **Step 9: Commit final navigation and verification**

Run:

```powershell
git add -- 'index.md' 'log.md' 'wiki/syntheses/zion-vault-reorganization-map.csv' 'wiki/syntheses/zion-vault-reorganization-report.md'
git diff --cached --check
git commit -m "docs: finalize Zion vault reorganization"
```

Expected: commit succeeds; `.obsidian/` remains unstaged.

---

## Completion Evidence

Do not claim completion until the final report contains actual values for every verification gate and the last run of each command exits `0`. The handoff must link the final `index.md`, migration manifest, and report, and must list any ambiguous notes intentionally left in place.
