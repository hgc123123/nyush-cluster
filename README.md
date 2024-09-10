# NYUSH HPC Cluster Documentation

You can find the built documentation here:

- https://ood.shanghai.nyu.edu/index.html



## Building the Document Locally
```bash
module load miniconda3
conda env list
source  activate hpc
pip install pipenv
pipenv install
pipenv shell
cd nyush-cluster/
pipenv run mkdocs build

```
