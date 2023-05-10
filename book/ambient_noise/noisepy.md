# NoisePy


We will test NoisePy, a python package for ambient noise seismology.
While the software is currently under active development, we will test its new functionality for cross correlation on the AWS SCEDC data.


## Clone the Repository

```bash
git clone https://github.com/mdenolle/NoisePy
```

```bash
conda create -n noisepy python=3.8 pip
conda activate noisepy
conda install -c conda-forge openmpi
pip install  jupyter
pip install noisepy-seis -e ."[dev]"
```