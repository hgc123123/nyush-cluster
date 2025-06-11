# Introducation

Boltz is a family of models for biomolecular interaction prediction. Boltz-1 was the first fully open source model to approach AlphaFold3 accuracy. Our latest work Boltz-2 is a new biomolecular foundation model that goes beyond AlphaFold3 and Boltz-1 by jointly modeling complex structures and binding affinities, a critical component towards accurate molecular design. Boltz-2 is the first deep learning model to approach the accuracy of physics-based free-energy perturbation (FEP) methods, while running 1000x faster — making accurate in silico screening practical for early-stage drug discovery.

| Cluster | Version| Module                  |
|:--------|:-------|:-----------------------:|
| nyushc  | 2.0.3  | boltz/2.0.3-torch-2.7.1 |

## Example
```
git clone https://github.com/jwohlwend/boltz.git
cd ./boltz
[hpc@hpclogin1 boltz]$ tree  examples/
examples/
├── affinity.yaml
├── cyclic_prot.yaml
├── ligand.fasta
├── ligand.yaml
├── msa
│   ├── seq1.a3m
│   └── seq2.a3m
├── multimer.yaml
├── pocket.yaml
├── prot_custom_msa.yaml
├── prot.fasta
├── prot_no_msa.yaml
└── prot.yaml
```

## Submit Inference Jobs

**protenix.slurm**
```
#!/bin/bash
#SBATCH --job-name=boltz2
#SBATCH --partition=sfscai
#SBATCH -N 1
#SBATCH --ntasks-per-node=8
#SBATCH --gres=gpu:1
#SBATCH --output=%j.out
#SBATCH --error=%j.err

module load singularity boltz
boltz predict ligand.fasta --cache=/root/.boltz/mols
```

```
sbatch boltz.slurm
```

## Protenix Prediction Result

```
$ tree boltz_results_ligand/
boltz_results_ligand/
├── lightning_logs
│   └── version_0
│       └── hparams.yaml
├── msa
├── predictions
│   └── ligand
│       ├── confidence_ligand_model_0.json
│       ├── ligand_model_0.cif
│       ├── pae_ligand_model_0.npz
│       ├── pde_ligand_model_0.npz
│       └── plddt_ligand_model_0.npz
└── processed
    ├── constraints
    │   └── ligand.npz
    ├── manifest.json
    ├── mols
    │   └── ligand.pkl
    ├── msa
    │   └── ligand_0.npz
    ├── records
    │   └── ligand.json
    ├── structures
    │   └── ligand.npz
    └── templates

12 directories, 12 files
```
