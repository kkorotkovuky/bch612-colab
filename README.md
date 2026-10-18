# BCH612 Colab notebooks

In-class exercises for BCH612, Structure and Function of Proteins and Enzymes,
University of Kentucky. All notebooks run in Google Colab. Parts 1 and 2 need a GPU runtime:
open **Runtime > Change runtime type** and select **T4 GPU** before running any cell.
The AlphaFold 3 analysis notebook runs on a CPU runtime.

| Notebook | What it does |
|---|---|
| [AlphaFold_single_2026.ipynb](https://colab.research.google.com/github/kkorotkovuky/bch612-colab/blob/main/AlphaFold_single_2026.ipynb) | Fold a designed peptide from its sequence alone with AlphaFold2, with an option to fold it as a head-to-tail macrocycle |
| [MD_designed_peptide_2026.ipynb](https://colab.research.google.com/github/kkorotkovuky/bch612-colab/blob/main/MD_designed_peptide_2026.ipynb) | Put the designed structure in water and run molecular dynamics with OpenMM, then ask whether it stayed folded |
| [AlphaFold_analysis_2026.ipynb](https://colab.research.google.com/github/kkorotkovuky/bch612-colab/blob/main/AlphaFold_analysis_2026.ipynb) | Collect and compare AlphaFold 3 predictions of a protein complex: interface scores, seeds and samples, and agreement with a known structure |

Part 1 writes a PDB file; Part 2 takes that file as its input.

## Part 1: AlphaFold2, single sequence

Type an amino acid sequence, press play, and inspect the model, the mean pLDDT and the
confidence plots. Hydrophobic side chains are drawn with yellow carbons, so a buried core
can be evaluated directly. Use `/` to separate chains. Checking **cyclic_peptide** folds the
sequence as a macrocycle by replacing AlphaFold's relative-position encoding with a cyclic
one; no bond is added, and D-amino acids and N-methylation cannot be represented.

Cysteine pairs whose SG atoms fall within 3 Å are written out as standard SSBOND records and
reported in the output. AlphaFold tends to draw these contacts too short, around 1.2 to 1.6 Å;
the energy minimization at the start of Part 2 relaxes them to the usual 2.05 Å.

The exercise was designed by Sergey Ovchinnikov
([af_backprop](https://github.com/sokrypton/af_backprop)). This version runs the same model,
AlphaFold2 `model_3_ptm` with no MSA and no templates, through
[ColabDesign](https://github.com/sokrypton/ColabDesign), so that it works on the current
Colab runtime; the original notebook does not.

## Part 2: Molecular dynamics

Upload the PDB file from Part 1. The notebook adds hydrogens, solvates the peptide in water
with 0.15 M NaCl, minimizes, equilibrates with the backbone restrained, and runs unrestrained
dynamics at 300 K and 1 bar. It then plots RMSD from the AlphaFold model, secondary structure
against time, a per-residue DSSP map, and per-residue fluctuation against pLDDT.

Installation is pip only: OpenMM, PDBFixer and MDTraj. The approach follows
[Making it Rain](https://github.com/pablo-arantes/Making-it-rain) (Arantes, Poleto, Pedebos
and Ligabue-Braun, *J Chem Inf Model* 61:4852-4856, 2021,
[doi:10.1021/acs.jcim.1c00998](https://doi.org/10.1021/acs.jcim.1c00998)).

## AlphaFold 3 analysis

Upload AlphaFold Server downloads (one `.zip` per job) or a zipped local AlphaFold 3 output
folder. The notebook lists every model with its seed and sample, shows where ipTM sits in the
summary file, and computes interface scores with
[ipsae.py](https://github.com/DunbrackLab/IPSAE) (pinned commit `6174cf9`): ipSAE in both
directions, pDockQ, pDockQ2 and LIS, plus buried surface area. It plots the PAE map of one
model with its interface residues, and compares the spread of a score within a seed with the
spread between seeds.

If a structure of the complex is in the PDB, the notebook matches chains by sequence and reports
TM-score per chain and DockQ with its components (fnat, iRMSD, LRMSD) for each interface, then
lists the interface residues shared with the reference, found only in the model, or found only in
the reference. DockQ and TM-score are computed with NumPy and Biopython from the published
definitions; on the worked example they agree with DockQ 2.1.3 and US-align to within 0.001.

The worked example is NorM from *Vibrio cholerae* with the nanobody Nb17_4, compared with the
cryo-EM structure 7PHP (Bloch *et al.*, *Proc Natl Acad Sci USA* 118:e2115435118, 2021). The six
AlphaFold Server jobs will be attached to the
[norm-demo-v1 release](https://github.com/kkorotkovuky/bch612-colab/releases/tag/norm-demo-v1)
as unmodified downloads, subject to the AlphaFold Server Output Terms of Use included in each zip.
Until they are, the demo option in Step 2 will fail; uncheck `use_demo_data` and upload your own zips.
