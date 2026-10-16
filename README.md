# BCH612 Colab notebooks

In-class exercises for BCH612, Structure and Function of Proteins and Enzymes,
University of Kentucky. Both notebooks run in Google Colab and need a GPU runtime:
open **Runtime > Change runtime type** and select **T4 GPU** before running any cell.

| Notebook | What it does |
|---|---|
| [AlphaFold_single_2026.ipynb](https://colab.research.google.com/github/kkorotkovuky/bch612-colab/blob/main/AlphaFold_single_2026.ipynb) | Fold a designed peptide from its sequence alone with AlphaFold2, with an option to fold it as a head-to-tail macrocycle |
| [MD_designed_peptide_2026.ipynb](https://colab.research.google.com/github/kkorotkovuky/bch612-colab/blob/main/MD_designed_peptide_2026.ipynb) | Put the designed structure in water and run molecular dynamics with OpenMM, then ask whether it stayed folded |

Part 1 writes a PDB file; Part 2 takes that file as its input.

## Part 1: AlphaFold2, single sequence

Type an amino acid sequence, press play, and inspect the model, the mean pLDDT and the
confidence plots. Hydrophobic side chains are drawn with yellow carbons, so a buried core
can be judged directly. Use `/` to separate chains. Ticking **cyclic_peptide** folds the
sequence as a macrocycle by replacing AlphaFold's relative-position encoding with a cyclic
one; no bond is added, and D-amino acids and N-methylation cannot be represented.

The exercise was designed by Sergey Ovchinnikov
([af_backprop](https://github.com/sokrypton/af_backprop)). This version runs the same model,
AlphaFold2 `model_3_ptm` with no MSA and no templates, through
[ColabDesign](https://github.com/sokrypton/ColabDesign), so that it works on the current
Colab runtime; the original notebook does not.

## Part 2: molecular dynamics

Upload the PDB file from Part 1. The notebook adds hydrogens, solvates the peptide in water
with 0.15 M NaCl, minimizes, equilibrates with the backbone restrained, and runs unrestrained
dynamics at 300 K and 1 bar. It then plots RMSD from the AlphaFold model, secondary structure
against time, a per-residue DSSP map, and per-residue fluctuation against pLDDT.

Installation is pip only: OpenMM, PDBFixer and MDTraj. The approach follows
[Making it Rain](https://github.com/pablo-arantes/Making-it-rain) (Arantes, Poleto, Pedebos
and Ligabue-Braun, *J Chem Inf Model* 61:4852-4856, 2021,
[doi:10.1021/acs.jcim.1c00998](https://doi.org/10.1021/acs.jcim.1c00998)).

## Background reading

Hollingsworth SA and Dror RO, Molecular Dynamics Simulation for All, *Neuron* 99:1129-1143,
2018, [doi:10.1016/j.neuron.2018.08.011](https://doi.org/10.1016/j.neuron.2018.08.011).
Free to read at the publisher under Elsevier's open-archive licence, and in PubMed Central as
[PMC6209097](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6209097/).
