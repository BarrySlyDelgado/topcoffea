[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/TopEFT/topcoffea/tutorials?filepath=tutorials%2Fintro_coffea.hist.ipynb)
# topcoffea
Top quark analyses using the Coffea framework

### Contents
- `analysis`:
   Subfolders with different analyses: creating histograms, applying selections...
   Also including plotter scripts and/or jupyter files

- `topcoffea/cfg`:
  Configuration files (lists of samples, cross sections...)

- `topcoffea/data`:
  External inputs used in the analysis: scale factors, corrections...
  
- `topcoffea/json`:
   JSON files containing the lists of root files for each sample 

- `topcoffea/modules`:
  Auxiliar python modules and scripts

- `topcoffea/plotter`:
  Tools to produce stack plots and other plots

- `setup.py`: File for installing the `topcoffea` package

### Set up the environment 
First, download and install conda:
```
curl https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh > conda-install.sh
bash conda-install.sh
```
Next, run `unset PYTHONPATH` to avoid conflicts. Then run the following commands to set up the conda environment:    
```
conda create --name topcoffea-env python=3.8.3
conda activate topcoffea-env
conda install -y -c conda-forge ndcctools conda-pack dill xrootd coffea
```

### How to start
- This directory is set up to be installed as a python package. To install, activate your conda environment, then run this command from the top level `topcoffea` directory:
```
pip install -e .
```
The `-e` option installs the project in editable mode (i.e. setuptools "develop mode"). If you wish to uninstall the package, you can do so by running `pip uninstall topcoffea`.
- Next, set up the config file you want to use in the `topcoffea/cfg` directory. This config file should point to the JSON files for the samples that that you would like to process. There are examples in the `topcoffea/cfg` directory.
- Lastly, `cd` into `analysis/topEFT` and run the `run.py` script, passing it the path to your config: 
```
python run.py ../../topcoffea/cfg/your_cfg.cfg
```


### To run the WQ version of `run.py`:

To run with the work-queue executor, use the `work_queue_run.py` script instead of the `run.py` script. Please note that `work_queue_run.py` must be run from the directory it is located in, since the `extra-input-files` option of `executor_args` assumes the extra input will be in the current working directory. So from `analysis/topEFT`, you would run:
```
python work_queue_run.py ../../topcoffea/cfg/your_cfg.cfg
```
Next, submit some workers. Please note that the workers must be submitted from the same environment that you are running the run script from (so this will usually mean you want to activate the env in another terminal, and run the `condor_submit_workers` command from there. Here is an example `condor_submit_workers` command (remembering to activate the env prior to running the command):
```
conda activate topcoffea-env
condor_submit_workers -M ${USER}-workqueue-coffea --cores 4 --memory 4000 --disk 2000 10
```
The workers will terminate themselves after 15 minutes of inactivity.


### How to convert HistEFT to TH1EFT
- Run `source setupTH1EFT.sh` to download and compile ROOT files
- Run `python analysis/topEFT/convert3lEFT.py` to perform conversion of MET
- See `analysis/topEFT/convert3lEFT.py` for more details
