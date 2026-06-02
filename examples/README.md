# Example data

Small **synthetic** datasets to try AnalysisXMD without running a simulation.
They are not real trajectories — they only reproduce the column format the tool
expects so you can verify that every tab works.

| File | Use it in | Columns |
|------|-----------|---------|
| `rmsd_protein.dat`   | **RMSD** tab | `time(ns)  RMSD(Å)` |
| `rmsd_ligand_A.dat`  | **Multi-Replica** tab | `time(ns)  RMSD(Å)` |
| `rmsd_ligand_B.dat`  | **Multi-Replica** tab | `time(ns)  RMSD(Å)` |
| `rmsf_protein.dat`   | **RMSF** tab | `residue  RMSF(Å)` |

## Quick test (≈1 minute)

1. Open `md_analysis_dashboard.html` in your browser.
2. **RMSD tab** → drag `rmsd_protein.dat`. You should see the RMSD vs time curve
   (equilibrating to ~2 Å). Try the *Publication style* and *Distribution box plot*
   options, then **Export figure**.
3. **RMSF tab** → drag `rmsf_protein.dat`. Two flexible loops (residues ~40–55 and
   ~120–135) should stand out.
4. **Multi-Replica tab** → drag `rmsd_ligand_A.dat` and `rmsd_ligand_B.dat`
   together. Switch *Analysis* between **RMSD vs Time**, **Density (KDE)** and
   **Box plot** to compare the two compounds.

These files contain comment lines starting with `#`, exactly like native GROMACS
`.xvg` / cpptraj `.dat` output, so they also demonstrate the automatic
header/comment handling.
