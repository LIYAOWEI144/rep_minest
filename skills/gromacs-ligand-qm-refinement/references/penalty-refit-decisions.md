# Deciding Whether a QM Refit Is Defensible

This reference applies to CGenFF/CHARMM ligand charge, angle, and torsion
penalties. Treat each parameter family as a separate hypothesis. A high
analogy penalty identifies uncertainty; it is not an objective function that
Gaussian or GROMACS can numerically minimize. Preserve the original penalty
and source topology for audit.

## Before Computing

1. Validate the molecule's identity, bond orders, stereochemistry,
   protonation/tautomer state, total charge, and atom map from a chemical
   source such as SDF/MOL2. Do not infer these from PDB coordinates alone.
2. Inventory original penalties by family and exact topology atom indices.
   Map each suspect term to its chemical environment and symmetry mates.
   Record the original parameter value, units, analogy source, and penalty.
3. Select a QM model that retains the local electronic and steric environment.
   If truncation or capping changes conjugation, formal charge, or a nearby
   hydrogen bond, enlarge the model or use the full ligand.
4. Record Gaussian/other QM program version, method, basis, solvent model,
   cap definition, atom map, input coordinates, and convergence evidence.
   An optimized geometry needs a frequency or equivalent stationary-point
   check before it becomes a bonded target.

## Charge Decision

- Use a force-field-consistent electrostatic fitting target: relevant
  ligand-water interaction orientations and distances/energies, dipole, or
  electrostatic potential as appropriate to the selected CHARMM protocol.
  An isolated-molecule ESP fit alone need not reproduce condensed-phase
  contacts. Check multiple chemically distinct sites, not one favorable
  contact.
- Fit the smallest justified charge group subject to total-charge and
  chemical-symmetry constraints. Compare both the original and proposed
  charges against held-out water orientations or equivalent checks.
- Reject or defer a change that pushes charges to arbitrary bounds, breaks
  symmetry without evidence, fits one probe while worsening others, or
  depends strongly on one target definition. A high charge penalty can
  remain unresolved; report it rather than forcing a visually lower score.
- A changed protonation state is a new chemical species. Rebuild and validate
  its complete topology; do not patch charges onto the old species.

## Bond and Angle Decision

- Verify the QM minimum and compare local geometry and vibrational/Hessian
  information with an MM-minimized structure using the same capped model.
  In a fused or conjugated core, a QM angle need not equal the force-field
  equilibrium angle because coupled terms shift the relaxed geometry.
- Fit equilibrium values and force constants together where the chosen
  CHARMM functional form and bonded optimizer permit it. Test symmetry-related
  occurrences together. Do not insert a raw projected Hessian diagonal as an
  independent angle or bond force constant.
- Accept only if MM geometry and local response improve, parameter values
  remain physical, and the complete ligand still minimizes with the intended
  topology. Record before/after values and residuals. If an attempted fit
  improves one metric slightly but worsens the geometry or creates an
  implausible equilibrium value, retain the original term.

## Torsion Decision

- For a genuine rotatable bond, run a relaxed constrained QM scan with
  sufficient angular coverage to distinguish relevant minima and barriers.
  For fused/aromatic ring distortions, define the coupled coordinate rather
  than treating an internal ring dihedral as a free 360-degree rotor.
- Confirm each requested restraint in the optimized output; discard failed,
  unconverged, or restraint-ignored points. Use relative QM energies and fit
  the corresponding CHARMM periodic terms while accounting for contributions
  from the rest of the MM model. Validate barriers and minima, including
  points withheld from fitting if practical.
- Keep the original torsion when the new fit gives negligible improvement or
  merely shifts error to an unscanned region. Scope a justified correction
  to the intended atoms; do not change a shared atom type globally.

## Acceptance Record and Handoff

For every proposed term, keep a machine-readable or tabular record of:
original penalty, topology atom indices, QM target and convergence, original
and candidate parameters, comparable before/after residuals, acceptance or
rejection reason, and where the evidence files live. Distinguish three states:
accepted refit, retained original after testing, and untested/unresolved.

Deliver the validated source SDF/MOL2, matched ligand GRO/ITP/TOP and
force-field includes, atom map, immutable original topology, QM inputs/outputs,
fit reports, and a concise limitations note. `gmx grompp -maxwarn 0` and an
energy-minimization check establish technical consistency, not charge-model
accuracy or production readiness. Solvent, ions, temperature, pH-dependent
chemical state, equilibrium, and trajectory sampling remain separate MD
decisions. Do not claim that a published CGenFF analogy penalty was reduced
unless the parameterizer itself was rerun under a specified revised model;
report physical fit improvements under their own metrics instead.
