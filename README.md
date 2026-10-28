# Ligand QM refinement skill

This repository contains the Codex skill
[`gromacs-ligand-qm-refinement`](skills/gromacs-ligand-qm-refinement/SKILL.md).
It guides chemistry validation, CHARMM/CGenFF penalty triage, targeted QM
charge/angle/torsion fitting, and reproducible ligand GRO/ITP/TOP delivery for
ligand-only or receptor-ligand GROMACS simulations.

The skill distinguishes an accepted physical refit from an unchanged CGenFF
analogy penalty. It requires evidence for each parameter decision and flags
remaining uncertainty. It does not contain ligand coordinates, private QM
outputs, or a universal force-field parameter set.

To install on Windows with PowerShell after cloning this repository:

```powershell
Copy-Item -LiteralPath .\skills\gromacs-ligand-qm-refinement -Destination (Join-Path $env:USERPROFILE '.codex\skills') -Recurse -Force
```

Then invoke `$gromacs-ligand-qm-refinement` with a chemically identified ligand
structure and the intended force-field/simulation context.
