---
title: "Stampede2 setup"
collection: devlogs
date: 2023-05-01
permalink: /dev-logs/2023/05/stampede2-setup/
excerpt: "Environment setup for stampede2 supercomputer."
tags:
  - devlogs
  - stampede2
  - rfdiffusion
---

[Stampede2](https://www.tacc.utexas.edu/systems/stampede2/) is a supercomputers cluster at the Texas Advanced Computing Center (TACC).
Our group has resource allocation to use the supercomputing resource.

## Set up RFdiffusion to run on Stampede2

### git clone repo

```bash
git clone https://github.com/RosettaCommons/RFdiffusion.git
```

### install models

```bash
mkdir models && cd models
wget http://files.ipd.uw.edu/pub/RFdiffusion/6f5902ac237024bdd0c176cb93063dc4/Base_ckpt.pt
wget http://files.ipd.uw.edu/pub/RFdiffusion/e29311f6f1bf1af907f9ef9f44b8328b/Complex_base_ckpt.pt
wget http://files.ipd.uw.edu/pub/RFdiffusion/60f09a193fb5e5ccdc4980417708dbab/Complex_Fold_base_ckpt.pt
wget http://files.ipd.uw.edu/pub/RFdiffusion/74f51cfb8b440f50d70878e05361d8f0/InpaintSeq_ckpt.pt
wget http://files.ipd.uw.edu/pub/RFdiffusion/76d00716416567174cdb7ca96e208296/InpaintSeq_Fold_ckpt.pt
wget http://files.ipd.uw.edu/pub/RFdiffusion/5532d2e1f3a4738decd58b19d633b3c3/ActiveSite_ckpt.pt
wget http://files.ipd.uw.edu/pub/RFdiffusion/12fc204edeae5b57713c5ad7dcb97d39/Base_epoch8_ckpt.pt

# Optional:
wget http://files.ipd.uw.edu/pub/RFdiffusion/f572d396fae9206628714fb2ce00f72e/Complex_beta_ckpt.pt

# original structure prediction weights
wget http://files.ipd.uw.edu/pub/RFdiffusion/1befcb9b28e2f778f53d47f18b7597fa/RF_structure_prediction_weights.pt
```


### install miniconda

``` bash
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh

# install to work2 directory instead of home
# yes to init, which modifies bashrc, but stampede2 sources bash_profile so that doesn't really do anything
```

### install SE3Transformer environment 

```bash

conda env create -f env/SE3nv.yml

conda activate SE3nv
cd env/SE3Transformer
pip install --no-cache-dir -r requirements.txt
python3 setup.py install
cd ../.. # change into the root directory of the repository
pip install -e . # install the rfdiffusion module from the root of the repository
```

### running RFdifussion jobs

```bash
#!/bin/bash

#SBATCH -J rfdiffusion

#SBATCH -p skx-dev
#SBATCH -N 1
#SBATCH -n 4
#SBATCH -t 02:00:00

#SBATCH -A Allocation
#SBATCH --mail-user=email
#SBATCH --mail-type=all

cd $SLURM_SUBMIT_DIR

conda activate SE3nv

./scripts/run_inference.py \
inference.output_prefix=temp/example_design_enzyme \
inference.input_pdb=examples/input_pdbs/5an7.pdb \
'contigmap.contigs=[10-100/A1083-1083/10-100/A1051-1051/10-100/A1180-1180/10-100]' \
potentials.guide_scale=1 \
'potentials.guiding_potentials=["type:substrate_contacts,s:1,r_0:8,rep_r_0:5.0,rep_s:2,rep_r_min:1"]' \
potentials.substrate=LLK inference.ckpt_override_path=./models/ActiveSite_ckpt.pt \
>& log_example_enzyme_design.txt < /dev/null
```

Note:
tried 4 MPI and 48 MPI tasks, same speed
