---
title: 'AnalysisXMD: A browser-based dashboard for protein–ligand molecular dynamics analysis'
tags:
  - molecular dynamics
  - GROMACS
  - AMBER
  - NAMD
  - protein–ligand interactions
  - trajectory analysis
  - data visualization
  - computational chemistry
  - JavaScript
authors:
  - name: Kevin Anthony Oré Maldonado
    orcid: 0000-0001-8863-5200
    affiliation: 1
affiliations:
  - name: Computational Chemistry Research Group, Universidad Andrés Bello (UNAB), Concepción, Chile
    index: 1
date: 2 June 2026
bibliography: paper.bib
---

# Summary

`AnalysisXMD` is a lightweight, single-file web application for the analysis and
visualization of molecular dynamics (MD) simulations of protein–ligand systems.
It runs entirely in the browser, requires no installation, server, or Python
environment, and reads the native output produced by the most widely
used MD engines — GROMACS [@abraham2015gromacs], AMBER (via `cpptraj`)
[@roe2013ptraj], and NAMD/CHARMM (PSF + DCD). From these files the tool computes and displays root-mean-square
deviation (RMSD) and fluctuation (RMSF), principal component projections,
inter-group distances, hydrogen-bond occupancy, and multi-replica comparisons,
and it renders interactive three-dimensional trajectories using NGL Viewer
[@rose2018ngl]. A distinctive feature is the automatic generation of
two-dimensional protein–ligand interaction diagrams in the style of commercial
packages, together with a panel that identifies the most statistically
representative conformation of a trajectory. All figures can be exported as
high-resolution PNG or fully editable SVG suitable for publication.

# Statement of need

Robust analysis libraries such as MDAnalysis [@michaud2011mdanalysis] and MDTraj
[@mcgibbon2015mdtraj] are powerful but require programming skills and a configured
Python environment, while interactive viewers such as VMD [@humphrey1996vmd]
focus on visualization rather than on producing publication-ready figures and
summary statistics. For many students and experimentalists, the practical
bottleneck after running an MD simulation is not the simulation itself but the
repetitive, error-prone task of plotting RMSD/RMSF curves, converting units
(ps→ns, nm→Å), comparing replicas, and characterizing the ligand binding mode —
often by switching between several heavyweight, license-restricted, or
platform-specific programs.

`AnalysisXMD` addresses this gap with a zero-install, cross-platform tool that a
researcher can open by double-clicking a single HTML file. It targets the common
protein–ligand workflow directly: automatic unit detection from XVG headers,
drag-and-drop loading of GROMACS and AMBER outputs, side-by-side replica
comparison (RMSD vs time, kernel-density distributions, and box plots),
extraction of the representative frame from the mode of the RMSD distribution,
and a binding-site interaction analysis that produces both a 3D view and a 2D
schematic. Because everything is client-side, no simulation data ever leaves the
user's machine, which is important for unpublished research. The tool is intended
for teaching, exploratory analysis, and the rapid preparation of figures for
manuscripts and theses.

# Functionality

- **Time-series analysis:** RMSD and RMSF with automatic ps→ns and nm→Å
  conversion, adjustable smoothing, and a publication mode showing the running
  mean with a ±1 SD band.
- **Distributions:** kernel-density estimates and box plots (with median, mean,
  and jittered raw points) for comparing several ligands or replicas.
- **Representative frame:** selection of the conformation closest to the mode of
  the RMSD density, downloadable as PDB or mappable to a sub-sampled trajectory.
- **3D viewer:** NGL-based [@rose2018ngl] trajectory playback with cartoon,
  surface and ball-and-stick representations, a PyMOL-like component/sequence
  explorer, and configurable binding-site, hydrogen-bond and contact display.
- **2D interaction diagrams:** automatic protein–ligand interaction maps with a
  clean ligand depiction generated with OpenChemLib [@openchemlib], colour-coded
  by interaction type.
- **Export:** high-resolution PNG and editable SVG figures (via Chart.js
  [@chartjs]) with journal-style colour palettes, plus tidy CSV export of the
  underlying data.

# Implementation

`AnalysisXMD` is implemented in vanilla HTML, CSS and JavaScript as a single
self-contained file, with charts rendered by Chart.js [@chartjs], 3D molecular
graphics by NGL Viewer [@rose2018ngl], and 2D chemical depiction by OpenChemLib
[@openchemlib]. It has no build step and no backend; the libraries are loaded
from a CDN on first use and can be bundled locally for offline operation. The
source code, documentation and example data are openly available on GitHub under
the MIT license, and each release is archived on Zenodo.

# Acknowledgements

The author thanks the Computational Chemistry Research Group at Universidad
Andrés Bello for support, and acknowledges the open-source projects Chart.js,
NGL Viewer and OpenChemLib on which this tool is built.

# References
