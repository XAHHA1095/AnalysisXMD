# Contributing to AnalysisXMD

First off — thank you for taking the time to contribute! 🎉
AnalysisXMD is an open tool for the molecular dynamics community, and contributions of all kinds are welcome: bug reports, new analysis modules, format support, documentation, and examples.

---

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [How can I contribute?](#how-can-i-contribute)
- [Reporting bugs](#reporting-bugs)
- [Suggesting features](#suggesting-features)
- [Submitting changes (Pull Requests)](#submitting-changes-pull-requests)
- [Project structure & technical notes](#project-structure--technical-notes)
- [Style guidelines](#style-guidelines)
- [Questions & contact](#questions--contact)

---

## Code of Conduct

This project follows a [Code of Conduct](CODE_OF_CONDUCT.md). By participating, you are expected to uphold it. Please be respectful and constructive.

---

## How can I contribute?

There are many ways to help, even without writing code:

- 🐛 **Report bugs** you encounter.
- 💡 **Suggest features** or new analysis types (e.g. SASA, radius of gyration, secondary structure, contact maps).
- 📁 **Add example datasets** (small `.xvg` / `.dat` files) so new users can try the tool immediately.
- 📖 **Improve documentation** — fix typos, clarify instructions, translate the README.
- 🧪 **Test** with real GROMACS / AMBER outputs and report what works or breaks.
- 🎨 **Improve the UI** or accessibility.

---

## Reporting bugs

Before opening an issue, please:

1. **Search existing issues** to avoid duplicates.
2. Use the **Bug Report** issue template.

A good bug report includes:

- What you did (which tab, which file).
- What you expected vs what happened.
- The **type of file** and the **tool that generated it** (GROMACS `gmx`, AMBER `cpptraj`, custom CSV…).
- The **first ~5 lines** of the data file (remove anything confidential).
- Your **browser and OS**.
- A screenshot if it's a visual issue.

> ⚠️ **Do not upload confidential or unpublished research data.** A few anonymized example lines are enough to reproduce most parsing issues.

---

## Suggesting features

Use the **Feature Request** issue template and describe:

- The scientific use case (what analysis, why it matters).
- An example of the expected input/output.
- If possible, a reference figure or paper showing the analysis.

---

## Submitting changes (Pull Requests)

1. **Fork** the repository.
2. **Create a branch** with a descriptive name:
   ```bash
   git checkout -b feature/sasa-analysis
   # or
   git checkout -b fix/rmsf-unit-detection
   ```
3. **Make your changes** (see [technical notes](#project-structure--technical-notes) below).
4. **Test locally**: open `md_analysis_dashboard.html` in a browser and verify your change works and nothing else broke.
5. **Commit** with a clear message:
   ```
   feat: add SASA analysis tab
   fix: correct ps→ns conversion when xvg header missing
   docs: clarify trajectory conversion command
   ```
6. **Open a Pull Request** against the `main` branch. Describe what you changed and why, and reference any related issue (`Closes #12`).

PRs are reviewed by the maintainer. Small, focused PRs are merged faster than large ones.

---

## Project structure & technical notes

AnalysisXMD is intentionally a **single self-contained HTML file** — no build step, no framework, no dependencies to install. This keeps it easy to share and run anywhere.

```
md_analysis_dashboard.html   ← everything lives here
├── <style>   CSS theme + layout
├── <body>    HTML: tabs, upload zones, controls, footer
└── <script>  vanilla JavaScript: parsing, charts, KDE, NGL viewer, export
```

**Key technical points for contributors:**

- **No frameworks.** Plain HTML/CSS/JS only. External libraries are loaded from CDN: [Chart.js](https://www.chartjs.org/) (plots) and [NGL Viewer](https://nglviewer.org/) (3D trajectories).
- **Data parsing** lives in the `parseFile`, `parseLines`, and `parseXVGHeader` functions. Unit auto-detection (ps→ns, nm→Å) is centralized in `normalizeTimeValues` and `normalizeRMSDValues`.
- **Charts** are built per tab in their `renderX()` functions. Reuse the shared helpers `linearXAxis`, `yAxisOpts`, and the color constants.
- **Adding a new analysis tab**: add (1) a `<button>` in the `<nav>`, (2) a `<div id="tab-name" class="section">`, (3) a parser branch, and (4) a `renderName()` function.
- **High-resolution export** is handled by `exportHighDPI` (PNG, renders at native target size) and `exportSVG` (vector). New charts automatically work if registered in `CHART_CANVAS_MAP`.
- Keep everything **client-side** — no data should ever be uploaded to a server. User privacy and data confidentiality are core principles of this tool.

---

## Style guidelines

- **JavaScript:** keep the existing compact style; use `const`/`let`, descriptive function names, and short comments for non-obvious logic.
- **CSS:** use the existing CSS variables (`--cyan`, `--bg2`, etc.) instead of hard-coded colors where possible.
- **Commit messages:** use [Conventional Commits](https://www.conventionalcommits.org/) prefixes (`feat:`, `fix:`, `docs:`, `refactor:`, `style:`).
- **Scientific correctness first:** unit conversions, statistics, and axis labels must be physically/mathematically correct. When in doubt, cite a source in the PR.

---

## Questions & contact

- For **technical discussion**, open a [GitHub Issue](../../issues) or [Discussion](../../discussions).
- For **scientific questions or research collaboration**, contact the author:

**Kevin Anthony Oré Maldonado**
📧 [k.ormaldonado@uandresbello.edu](mailto:k.ormaldonado@uandresbello.edu)
🔬 [Scientific profile (SciProfiles)](https://sciprofiles.com/profile/KevinAnthonyOreMaldonado)
🏛️ Computational Chemistry Research Group, Universidad Andrés Bello (UNAB), Chile

Thank you for helping improve AnalysisXMD! 🔥🧬
