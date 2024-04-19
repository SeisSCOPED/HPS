# NoisePy tutorial with Coiled

__Prerequisite__
0. A laptop with command line tools
1. Have a running anaconda/miniconda on your laptop.
2. Have your AWS credential configured on your laptop.
3. Your credential should at least have read access to the SCEDC data archive at `s3://scedc-pds/` in the `us-west-2` region.


## 1. Create the Working Environment 
It is not recommended to install coiled in a pre-existing working environment. The dependencies will be too complicated for coiled to resolve and mirror on the remote server. We suggest creating a new empty environment and only install required packages.
```bash
    conda create --name coiled python=3.10
```

After activating the new environment, we install NoisePy and coiled using `pip`. Moreover, we also install JupyterLab here since we will run a jupyter notebook on the remove server. 
```bash
    conda activate coiled

    pip install noisepy-seis coiled jupyterlab
```

## 2. Launching the Jupyter Notebook
Specify your CPU/RAM requirement accordingly and coiled will select and launch an instance that fulfills this requirement. 
```bash
    coiled notebook start --cpu 16 --memory '64 GB'
```
Then, the coiled will scan and duplicate your local environment on the remote server. After this, you should be able to see your Jupyter notebook address, instance type, uptime, and up-to-date cost.
```
╭─────────────────────── Notebook notebook-XXXXXXXX... ────────────────────────╮
│                                                                              │
│ Jupyter: https://cluster-txorc.dask.host/jupyter/lab?token=XXXX  │
│ Details: https://cloud.coiled.io/clusters/XXXX?account=XXXXX            │
│                                                                              │
│ Ready  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━      │
│                                                                              │
│ Region:   us-west-2                 Uptime:                           2m 48s │
│ VM Type:  m6i.4xlarge               Approx cloud cost:              $0.80/hr │
│                                     Total cost:                        $0.04 │
│                                                                              │
│ Use Control-C to stop this notebook server                                   │
╰──────────────────────────────────────────────────────────────────────────────╯
```

You can now access the Jupyter server by copying the URL to any of your web browser.

## 3. Running the NoisePy tutorial
Get the tutorial from the NoisePy GitHub repository, e.g., [SCEDC tutorial here](https://github.com/noisepy/NoisePy/blob/main/tutorials/noisepy_scedc_tutorial.ipynb). 
Please download the raw file on your local computer and upload to the Jupyter server. The rest should be the same as running the notebook on local.

## 4. Post-tutorial Checking
Be sure to shut down the Jupyter server after finishing the processing. Shut down the `coiled` command will automatically and safely shut down the remote server. 

__Reference__
* Coiled: http://coiled.io
* NoisePy tutorials: https://github.com/noisepy/NoisePy/tree/main/tutorials