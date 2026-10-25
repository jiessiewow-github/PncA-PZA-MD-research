# PncA–pyrazinamide molecular dynamics

Each system is an isolated PncA monomer (residues 1–185), one catalytic Fe²⁺, and one pyrazinamide, in explicit water (residue name WAT) and sodium. Mutant models were built from the wild-type complex `wt/build/complex.pdb`. The Amber topology of that complex is the parameter file (`wt/build/complex.prmtop`; modified Bondi radii). The experimental PDB accession and the tleap input were not recovered in the preparation notes, so neither is stated here. Residue numbering matches the file of record. The catalytic set used later is Asp8, Lys96, and Cys138, and the iron ligands Asp49, His51, His57, and His71 are those of the crystal structure with the same numbering.

Simulations were run in NAMD 3. Bonds to hydrogen were constrained (SHAKE) and the timestep was 2 fs. Nonbonded interactions used a 12 Å cutoff, a switching function from 10 Å, and a 13.5 Å pair list. Electrostatics used particle-mesh Ewald with a 1.0 Å grid spacing. One–four interactions were scaled by 1/1.2, as specified by the Amber topology. Temperature was controlled by Langevin dynamics at 300 K with a damping coefficient of 5 ps⁻¹, applied to non-hydrogen atoms. Pressure was controlled by a Nosé–Hoover Langevin piston at 1.01325 bar (period 50 fs, decay 25 fs), with isotropic cell fluctuations. Water and all solute clusters were wrapped into the periodic cell.

Minimization (`eq0`) applied harmonic restraints that were halved over ten blocks of 1,000 steps, followed by 1,000 unrestrained steps. Equilibration (`eq1`) was 1×10⁶ steps (2 ns) of the NPT protocol above, started from the minimized restart. Production was five consecutive segments (`sim1`–`sim5`). Each segment was 3×10⁷ steps (60 ns), so each replica is 300 ns. Coordinates, including the unit cell, were saved every 100,000 steps (200 ps). Fifty replicas were run for each mutant under this protocol. The wild-type ensemble used the same protocol; one wild-type replica has no receptor trajectory, so exposure averages that require it use 49 replicas.

## Files

The inputs below are the wild-type build and one copy of the NAMD configuration used for the analysed trajectories. Every replica used the same settings. Restarts, trajectories, and velocities are not in this repository.

| Path | Role |
|---|---|
| `wt/build/complex.pdb` | Wild-type coordinates. Protein residues 1–185, Fe²⁺ as residue 186 (`FE2`), pyrazinamide as residue 187 (`PZA`), explicit WAT and Na⁺. |
| `wt/build/complex.prmtop` | Amber topology and parameters read by every NAMD input (`amber yes`). |
| `wt/build/cons.pdb` | Positional-restraint reference for minimization. Occupancy column holds the force constants. |
| `wt/replicas/rep1/equilibration/eq0.conf` | Minimization. |
| `wt/replicas/rep1/equilibration/eq1.conf` | 2 ns NPT equilibration. Restarts from `eq0`. |
| `wt/replicas/rep1/simulation/sim1.conf` | First 60 ns production segment. Restarts from `../equilibration/eq1`. |
| `wt/replicas/rep1/simulation/sim2.conf` … `sim5.conf` | The next four 60 ns segments. Each restarts from the previous segment. |

Paths inside the configuration files are relative to those directories (`../../../build/complex.prmtop` and so on) and resolve to `wt/build/` in this tree. Launch each file from its own directory.

`SHA256SUMS` lists the checksum of every file. The coordinate-file checksum is `ae1d4639d0bceb6f661a80cc67c076eb93b0dedb5dd5cf685742b68767770ce8`.

## Provenance

`eq1.conf` and `sim1.conf`–`sim5.conf` are the inputs that were executed for one analysed replica (`models/S88A/replicas/rep1`). Each production segment is `run 30000000`. A later copy of the wild-type template, `models/WT/replica-confs/sim1.conf`, instead specifies `run 50000000` (100 ns per segment). That later template is not the input of the trajectories described above. `eq0.conf` is the shared minimization input stored with that replica’s configuration set; the per-replica copy was not kept next to the equilibration restart.
