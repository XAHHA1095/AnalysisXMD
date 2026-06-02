# 🔥 AnalysisXMD — Protein–Ligand MD Analysis Dashboard

A lightweight, **single-file** web dashboard for analyzing and visualizing **Molecular Dynamics (MD)** simulations of protein–ligand systems. Runs entirely in your browser — **no installation, no server, no Python environment required**. Just open the HTML file.

Built to read native output from **GROMACS** and **AMBER** (cpptraj), produce **publication-quality figures (300–600 DPI)**, and interactively visualize trajectories.

---

## ✨ Features

| Module | What it does |
|--------|--------------|
| **RMSD** | Root-mean-square deviation vs time, with automatic ps→ns and nm→Å unit detection |
| **Representative frame** | Finds the most statistically probable conformation (mode of the RMSD density) and downloads it as PDB — works even with a subsampled trajectory (maps time → nearest available frame) |
| **RMSF** | Per-residue fluctuation, multiple files at once, automatic detection of the most flexible residues |
| **PCA** | Principal component projection (PC1 × PC2) with optional cluster coloring |
| **Distances** | Inter-domain / inter-group distance, multi-replica support, threshold lines |
| **H-Bonds** | Protein–ligand hydrogen bond count over time **and** per-residue occupancy detail (identifies relevant interacting residues) |
| **Multi-Replica** | Overlay multiple RMSD replicas **+ RMSD density distribution (KDE)** plots like Seaborn/matplotlib, with Å/nm units and manual axis controls |
| **Interactions / 3D** | Discovery-Studio-style 3D viewer: protein cartoon/surface, ligand sticks (custom carbon/CPK/single color), binding-site residues, H-bonds/contacts, pocket surface, PyMOL-like component & sequence explorer (multi-select highlight, hide water/ions/hetero/residues) |
| **2D interaction diagram** | Real 2D ligand structure (skeletal or ball & stick) with per-atom interactions to each residue, **or** a flat schematic — colour-coded by interaction type, exportable as SVG / high-res PNG |
| **Trajectory Viewer** | Interactive 3D visualization with VMD-style playback (play/pause, frame stepping, speed control) using NGL Viewer |
| **Export** | High-resolution **PNG (up to 600 DPI / 4800 px)** and **editable vector SVG** with journal color palettes |

---

## 🚀 Quick Start

1. **Download** the file `md_analysis_dashboard.html`.
2. **Double-click it** (or right-click → Open with → your browser).
3. Drag & drop your MD output files into each tab. Done.

> No internet connection is needed for the analysis itself — but the first load fetches Chart.js and NGL Viewer from a CDN, so **keep an internet connection on first open** (see [Offline use](#-offline-use) to bundle the libraries).

---

## 💻 Requirements

- **A modern web browser**: Google Chrome, Microsoft Edge, Firefox, or Brave (recent version recommended).
- **Internet connection on first launch** (to load Chart.js and NGL Viewer from CDN).
- That's it. No Python, no Node.js, no compilation.

**Tested on:** Chrome / Edge (Windows, macOS, Linux).

---

## 📂 Supported Input Formats

The dashboard parses plain-text data files. Comment lines starting with `#`, `@`, or `;` are ignored automatically (so native GROMACS `.xvg` and cpptraj `.dat` work directly).

### RMSD
```
# GROMACS:  gmx rms -f traj.xtc -s topol.tpr -o rmsd.xvg
@ xaxis label "Time (ps)"     ← auto-converted to ns
@ yaxis label "RMSD (nm)"     ← auto-converted to Å
0.000   0.0021
1000    0.1345

# cpptraj / AMBER (.dat .txt) — already in Å
0.0   0.00
0.1   1.23

# Generic CSV
time_ns,rmsd_A
0.0,0.00
```

### RMSF (multiple files supported)
```
# GROMACS:  gmx rmsf -f traj.xtc -s topol.tpr -o rmsf.xvg -res
1   0.245
2   0.312

# cpptraj:  atomicfluct out rmsf.dat byres
1   1   0.245
2   5   0.312
```

### PCA
```
# GROMACS:
#   gmx covar  -f traj.xtc -s topol.tpr -o eigenval.xvg -v eigenvec.trr
#   gmx anaeig -v eigenvec.trr -f traj.xtc -s topol.tpr -2d proj2d.xvg

# CSV with optional cluster column:
pc1,pc2,cluster
-12.3,5.6,0
45.1,2.3,2
```

### Distances (multi-replica)
```
# GROMACS:  gmx distance -f traj.xtc -s topol.tpr -n index.ndx -oall dist.xvg
# cpptraj:  distance :1-200 :201-400 out dist.dat

# Multi-replica CSV:
time_ns,r1,r2,r3
0,38.2,37.5,38.8
```

### H-Bonds
```
# Count vs time:
#   gmx hbond -f traj.xtc -s topol.tpr -n index.ndx -num hbnum.xvg
0.0   3
1000  2

# Per-residue detail (cpptraj avgout):
#   hbond Hbonds ... avgout hbavg.dat
#Acceptor      DonorH        Donor        Frames  Frac    AvgDist  AvgAng
:LIG@O3        :SER205@HG    :SER205@OG   892     0.8920  2.7953   158.22
:LIG@N1        :LYS43@HZ1    :LYS43@NZ    450     0.4500  2.9123   162.14
```
> In the H-Bonds tab, type your **ligand residue name** (e.g. `LIG`, `MOL`, `UNK`) to automatically filter and highlight the protein residues interacting with it.

---

## 🎬 Trajectory Viewer

Open the **"Molecular Dynamics Trajectory Viewer"** panel in the RMSD tab.

1. **Load Topology / Structure**: `.gro`, `.pdb`, `.prmtop`, `.psf`, `.mol2`.
2. **Load Trajectory** and animate.

### Recommended: convert your trajectory to a multi-frame PDB
Browser libraries cannot stream binary `.xtc` / `.nc` reliably without a dedicated server. The most robust path is a multi-frame PDB:

**GROMACS**
```bash
gmx trjconv -f traj.xtc -s topol.tpr -o traj_anim.pdb -dt 100
```
**AMBER (cpptraj)**
```bash
cpptraj -p topol.prmtop -y traj.nc -x traj_anim.pdb
# or inside cpptraj:  trajout traj_anim.pdb pdb
```
Then load `traj_anim.pdb` as the trajectory.

### Playback controls (VMD-style)
- ▶ / ⏸ Play / Pause
- ◀ / ▶▶ step one frame
- ⏮ / ⏭ jump to first / last frame
- **Speed selector**: 5 → 60 fps
- **Representation**: cartoon, licorice, ball+stick, surface, ribbon, backbone
- **Color scheme**: chain, residue index, B-factor, electrostatic, hydrophobicity, element
- 📷 4× screenshot export

---

## 📊 Density Distribution (KDE)

In the **Multi-Replica** tab, switch *Analysis* to **"RMSD Density Distribution (KDE)"** to produce smooth probability-density curves (like Seaborn `kdeplot`) for each loaded file — ideal for comparing protein vs ligand stability.

- **Units**: switch between Å and nm (density rescales automatically so the area under each curve = 1).
- **Bandwidth**: Silverman's rule by default, adjustable.
- **Manual axis controls**: set X/Y max and tick step, or leave blank for auto (uses the 99.5th percentile so outliers don't stretch the axis).

---

## 🖼️ Exporting Figures

Every chart has an **Export figure** button:

- **PNG**: 300 / 400 / 600 / 800 DPI equivalents (up to 4800 × 3200 px), rendered at native resolution (not upscaled — equivalent to `matplotlib dpi=600`).
- **SVG**: fully editable vector for Inkscape / Illustrator.
- **Backgrounds**: white (publication), dark (presentation), transparent.
- **Color palettes**: Matplotlib default, Nature Publishing, Colorblind-safe (Wong 2011), Monochrome.

Clean **CSV export** is also available to share tidy, unit-normalized data.

---

## 🎨 Grayscale Mode

The interface ships in grayscale by default. To restore the original cyan theme, open the HTML in a text editor and remove `filter:grayscale(1)` from the `.app` and `.modal-bg` CSS rules.

---

## 🔌 Offline Use

To run with **zero internet dependency**, download these two libraries next to the HTML and update the two `<script src="...">` lines at the top of the file to point to the local copies:

- `chart.umd.js` — [Chart.js 4.x](https://www.chartjs.org/)
- `ngl.js` — [NGL Viewer](https://nglviewer.org/)

---

## 🗂️ Repository Structure (suggested)

```
AnalysisXMD/
├── md_analysis_dashboard.html   # the whole app (open this)
├── README.md                    # this file
├── LICENSE                      # see below
└── examples/                    # (optional) sample .xvg / .dat files
```

---

## 🐛 Troubleshooting

| Problem | Solution |
|---------|----------|
| Charts are empty | Make sure your file has at least 2 numeric columns; comment lines are fine. |
| X-axis looks compressed | Your first column may be **frame number**, not time. Use a file with time in column 1. |
| RMSD values look 10× too small | The tool expects nm in GROMACS `.xvg`; if your `.dat` is already in Å it is kept as-is. |
| Trajectory won't load | Convert to **multi-frame PDB** (see above). Binary `.xtc`/`.nc` need a server to stream. |
| Density X-axis too wide | Use the **manual X max** control in Multi-Replica, or it auto-clamps to the 99.5th percentile. |
| Nothing loads at all | Check your internet connection (first load needs the CDN), or set up [offline use](#-offline-use). |

---

## 📜 Citation

If this tool contributes to your research, please cite it:

> Oré Maldonado, K. A. (2026). *AnalysisXMD: A browser-based dashboard for protein–ligand molecular dynamics analysis.* Computational Chemistry Research Group, Universidad Andrés Bello, Chile.

*(Add a Zenodo DOI here once you archive the repository — GitHub integrates with Zenodo to mint one automatically.)*

---

## 🤝 Contributing

Contributions are very welcome! Bug reports, feature requests, new analysis modules, and documentation improvements all help.

- Read the **[CONTRIBUTING.md](CONTRIBUTING.md)** guide before opening a pull request.
- Please follow the **[Code of Conduct](CODE_OF_CONDUCT.md)**.
- Open an **Issue** to report a bug or propose a feature.
- For scientific questions or collaboration proposals, contact the author directly (see below).

---

## 👤 Author

**Kevin Anthony Oré Maldonado**
PhD candidate — Computational Physical Chemistry
Universidad Andrés Bello (UNAB), Chile
Computational Chemistry Research Group

- 📧 **Email:** [k.ormaldonado@uandresbello.edu](mailto:k.ormaldonado@uandresbello.edu)
- 🔬 **Scientific profile:** [sciprofiles.com/profile/KevinAnthonyOreMaldonado](https://sciprofiles.com/profile/KevinAnthonyOreMaldonado)

---

## 🙌 Credits

- **Concept, design & scientific data:** Kevin Anthony Oré Maldonado
- **Development:** built with the assistance of **Claude (Anthropic)**
- **Libraries:** [Chart.js](https://www.chartjs.org/) · [NGL Viewer](https://nglviewer.org/)

---

## ⚖️ License

Released under the **MIT License** — free to use, modify, and distribute, including for academic and commercial purposes, with attribution.

```
MIT License

Copyright (c) 2026 Kevin Anthony Oré Maldonado

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

> You can change this to another license if you prefer (e.g. GPL-3.0, CC-BY-4.0). MIT is the most permissive and common for academic tools.
