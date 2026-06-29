---
name: seuratv5-env-requirements
description: Prepare, install, repair, and verify the Windows R/Python runtime required by the SeuratV5 full-course single-cell code. Use when the user asks to set up PATH, Rscript, Rtools, JAGS, R packages, Python/reticulate packages, or environment checks before running Seurat V5 course scripts.
---

# SeuratV5 Environment Requirements

## Purpose

Use this skill to prepare the runtime for the SeuratV5 course scripts before running any analysis module. The course code is R-first and Windows-oriented. It expects R 4.4.x, Rtools44, JAGS, many CRAN/Bioconductor/GitHub R packages, several local tar.gz packages bundled in the course, and a small Python environment for reticulate/API/scverse-compatible workflows.

## Source Course

The default source is the local F: drive SeuratV5 course folder that contains:

- `1.*`: installers for R 4.4.2, RStudio, Rtools44, and JAGS.
- `2.*`: R package installer scripts plus local archives such as `monocle_xxdchange.tar.gz`, `openai.tar.gz`, `xxdAIcelltype.tar.gz`, and `hdWGCNA-0.3.00.tar.gz`.
- Downstream scripts for matrix import, QC, clustering, annotation, doublet checks, CellChat, copykat, inferCNV, hdWGCNA, Monocle2/3, SingleR, SCINA, scPred, CIBERSORT, and MuSiC.

The bundled scripts avoid hard-coded non-ASCII directory names. When `-CourseRoot` is not provided, they search under `F:\0000-*` for the local package archive `monocle_xxdchange.tar.gz` and infer the course root from that file.

## Primary Workflow

1. Inspect whether Rscript is available and whether it points to R 4.4.x.
2. Add R, Rtools44, JAGS, Git gzip, and Conda paths to the user PATH when needed.
3. Install or update R packages from CRAN, Bioconductor, GitHub, and course-local tar.gz archives.
4. Create or repair the Conda environment `seuratv5-course-py` for Python-side packages.
5. Run an environment check and save a timestamped report.

Prefer the bundled PowerShell entrypoint:

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File `
  C:\Users\Administrator\.codex\skills\seuratv5-env-requirements\scripts\install_environment.ps1 `
  -RHome "E:\R-4.4.2" `
  -InstallRPackages `
  -InstallPythonEnv
```

Run verification only:

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File `
  C:\Users\Administrator\.codex\skills\seuratv5-env-requirements\scripts\check_environment.ps1 `
  -Rscript "E:\R-4.4.2\bin\Rscript.exe" `
  -CondaEnv "seuratv5-course-py"
```

## Installation Notes

- Add `E:\R-4.4.2\bin` or the detected R `bin` directory to the front of user PATH so `Rscript` resolves correctly.
- Add Rtools44 paths such as `E:\rtools44\usr\bin` and `E:\rtools44\x86_64-w64-mingw32.static.posix\bin` when present.
- Add JAGS `bin` paths when present; this matters for packages that link to JAGS.
- Add Git `usr\bin` when present so course/export scripts can call `gzip`.
- Do not upload matrices or clinical metadata. Python packages are installed locally through Conda/pip.
- Some GitHub packages can fail temporarily because of network, compiler, or upstream changes. Keep logs and rerun only failed package groups.
- AI annotation scripts require API keys for Moonshot/Kimi or DeepSeek and the bundled/local `openai` R package; never hard-code keys in course scripts.

## Bundled Resources

- `references/requirements.md`: package inventory and source mapping.
- `references/path-setup.md`: PATH and external software requirements.
- `scripts/install_environment.ps1`: one-command orchestrator for PATH, R packages, Python env, and checks.
- `scripts/install_r_packages.R`: CRAN/Bioc/GitHub/local R package installer.
- `scripts/install_python_env.ps1`: Conda Python environment installer.
- `scripts/check_environment.ps1`: verifies PATH, R packages, Python imports, and external tools.
- `scripts/extract_course_requirements.ps1`: re-extracts package usage from course R scripts.
