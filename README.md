# CHAZ
## (Columbia Tropical Cyclone Hazard Model)

## Overview 


The Columbia HAZard model (CHAZ) is a statistical-dynamical downscaling model for estimating tropical cyclone hazard. The CHAZ model three primary components for describing tropical cyclone activity from genesis to lysis, they are genesis, track, and intensity modules. The genesis module uses Tropical Cyclone Genesis Index (TCGI, developed by Tippett et al, 2011) to estimate the seeding rate of the storm precursors. The seeds are then passed to the a  Beta-Advection Model (BAM) that moves the storm forward with giving environmental sterring flow. The intensity module then evolves storms beyond genesis using an Autoregressive Model that involves a deterministic and a stochastic forcing elements. The underlying science of the CHAZ model can be found at Lee et al. (2018, https://doi.org/10.1002/2017MS001186).

The model is structured into CHAZ and its preprocessing (CHAZ-pre). 

## Getting Started with CHAZ

### Getting the code

TBD 

### System dependencies 

The CHAZ model was developed in python2 and has only been running on linux machines. Here we provide our experience. However, it is by no means that CHAZ can only be run on a linux system. We welcome users' contribution in terms of operationg system. We also appreciate any comments, bug-discovery, and improvment of the CHAZ model.

#### Operating System

* Linux

#### Text Editor (REMOVE)

In the below instructions, vi is used, but any text editor will do.

#### Python v2

* [NumPy](https://numpy.org/)

* [pandas](https://pandas.pydata.org/)

* [SciPy](https://scipy.org/)

* Pygrib

The remaining portion of **Getting Started with CHAZ** assumes that the [Anaconda Python Distribution](https://www.anaconda.com/) is installed. The below steps can be done without Anaconda installed, so in that case, change accordingly. 


## Basic CHAZ structure

In CHAZ, Namelist.py controls all global variables such as the source of the global model forcing, the ensemble member of that global model, the version of genesis module, numbers of track and intensity ensemble, and whether you are doing CHAZ-pre or CHAZ downscaling. In theory, Namelist.py is the only file you need to modify. CHAZ.py reads in Namelist.py and is the script that call for all the modules/subroutines. Below we describe input varables that need to be modified when conducting CHAZ-pre and CHAZ downscaling.  


`input`: the directory containing best track data from NHC and JTWC

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `bt_*.nc`: inputs for CHAZ
       
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `bt_global_predictors`: CHAZ predictors for historical events (from `bt_*.nc`). The predictors is calculated using monthly ERA-Interim reanalysis data. This file is used for calculating the seeding rate ratio for genesis model and for getting the regression stochastic error terms for intensity model

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `landmask.nc`: landmask data

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `MPI_ESM_MR`: folder with model specific data necessart for finding genesis

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `coefficient_meanstd.nc`: containing the mean and standard deviations of the predictors for historical events (i.e., those saved in bt_global_predictors.nc)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `result_l.nc`: regression parameters for near land cases

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `result_w.nc`: regression parameters for cases over open water


&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `bt_global_predictos.nc`, `coefficient_meanstd.nc`, `result_l.nc` and `result_w.nc` 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  are used for Lee et al. 2018 JAMES paper (doi:https://agupubs.onlinelibrary.wiley.com/doi/full/10.1002/2017MS001186)

Below, we describe source code that can also be modified if ____

`src`: contains all of the source code necessary to run CHAZ


&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  `calA.py`: part of preprocessing


&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  `getMeanStd.py`: not used in example, but used to calculate data in coefficient_meanstd.nc if user does not have file


&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  `module_riskModel.py`: contains functions necessary to calculate genesis, track, and predictors


&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  `caldetsto_track_lysis.py`: return intensities based on calGenisi.py and module_GenBamPred.py output


&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  `global_variable.py`: defines some global variables universal to running CHAZ


&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  `preprocess.py`: preprocess data


&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  `calGenisi.py`: use TCGI seeding to find genesis points


&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  `module_GenBamPred.py`: calculates tracks via Beta-Advection Model


&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  `calWindCov.py`: calculate wind mean and covariance


`tools`: contains all of the files with tools  necessary to run the source code of CHAZ.


&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  `globalvariable.py`: global variables independent of run


&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  `module_ecmwf_daily.py`


&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  `module_stochastic.py`


&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  `regression3.py`: regression tools


&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  `regression4.py`: regression tools


&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  `ships.py`


&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  `util.py`


## CHAZ-pre
### Changing Global Variables in Namelist.py
in preprocess. (expected outcome)


`Model = 'ERAInterim'` - model that provides reanalysis data (str), default is European Center for Medium Range Weather Forecasts interim reanalysis

`ENS = 'r1i1p1'` - global model (str)

`TCGIinput = 'TCGI_CRH_SST'` - environmental parameters used as input for TCGI (str)

`Year1 = ` - the beginning year (int)

`Year2 = ` - tne end inner year (int)

`runPreprocess = ` - if `True`, preprocessing will run, if `False` preprocessing will not run (bool)

Adding others.

### getting TCGI and PI 
in preprocess.(expected outcome)


&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  `calA.py`: part of preprocessing

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  `preprocess.py`


### get mean and standard deviations of the predictors
in preprocess. (expected outcome)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  `getMeanStd.py`: not used in example, but used to calculate data in coefficient_meanstd.nc if user does not have file

### Checking Initial conditions
Prior to conducting CHAZ downsclinag, we should check the initial conditions those are nessary for CHAZ runs. They are XXXXX 

## Running CHAZ 

#### 3 Components of CHAZ


Genesis | Track | Intensity
------------ | ------------- | -------------
seeds domain with weak vortices  | further evolution of storm beyond genesis | further evolution of storm beyond genesis 
seeding rate, found via TCGI, depends on environmental conditions | moves storm by a beta advection model | 2 components: empirical linear regression model and stochastic element
for each seed, genesis location and date are chosen randomly on a 1 km resolution within the a selected month | predictors found based on beta-advection model|  empirical multiple linear regression model advances TC in time along track based on surrounding large scale environment 
&nbsp;|&nbsp;|  stochastic element, which only depends on the storm's current state and recent history, accounts for internal storm dynamics 
&nbsp;|&nbsp;|  *intensity at landfall is separate and uses separate regression model that takes into account proximity to land and other environmental conditions

CHAZ consists of three main computations: Genesis, Track, and Intensity. 

The genesis component seeds the domain with weak vortices; the seeding rate, found via TCGI, depends on environmental conditions. For each seed, genesis location and date are chosen randomly on a 1 km resolution within the a selected month.

The track component involves the further evolution of the TC beyond genesis. Through a beta-advection model, this component moves the storm and then finds predictors based on the model.  

Like the track component, the intensity component involves the further evolution of the TC beyond genesis. The instensity computation consists of an empirical linear regression model and a stochastic element. The empirical multiple linear regression model advances TC in time along track based on surrounding large scale environment, and the stochastic element, which only depends on the storm's current state and recent history, accounts for internal storm dynamics; *Note intensity at landfall is separate and uses separate regression model that takes into account proximity to land and other environmental conditions.

![first_flow_chart](https://user-images.githubusercontent.com/46905677/93243910-bb8f0700-f73d-11ea-80ad-3ae64ea87326.jpg)

### Changing Global Variables in `Namelist.py`

`Model = 'ERAInterim'` - model that provides reanalysis data (str), default is European Center for Medium Range Weather Forecasts interim reanalysis

`ENS = 'r1i1p1'` - global model (str)

`TCGIinput = 'TCGI_CRH_SST'` - environmental parameters used as input for TCGI (str)

`CHAZ_ENS = 1` - number of track ensemble members per year (int)

`CHAZ__Int_ENS = 40` - number of intensity ensemble members (int)

`seedN = 1000` - annual seeding rate for random seeding (int). This function is currrent under development.

`landmaskfile = 'landmask.nc'` - land mask file (NETCDF)

`ipath = ''` - location of environmental data TCGI, and mean/std of the predictors (str)

`opath = ''` - location of pik file with observed best track global predictors (str)

`obs_bt_path = ''` - location of observed best track data (str)

`Year1 = ` - the beginning year (int)

`Year2 = ` - tne end inner year (int)

`runCHAZ = ` - if `True`, CHAZ will run, if `False` CHAZ will not run (bool)

`calGen = ` - if `True`, genesis will  be calculated, if `False` genesis will not be calculated (bool)

`calBam = ` - if `True`, track will  be calculated, if `False` track will not be calculated (bool)

`calInt = ` - if `True`, intensity will  be calculated, if `False` intensity will not be calculated (bool)

### Running CHAZ.py

1. Make sure all the inputs are in the ipath, opath, and obs_bt_path 

2. Check that `output` is empty before running CHAZ

3. Simply type the following command:`$ ./CHAZ.py`

![second_flow_chart](https://user-images.githubusercontent.com/46905677/93244535-be3e2c00-f73e-11ea-80b5-2f59f6d62a63.jpg)

###  Output of CHAZ

The output of CHAZ gives a set of netCDF files with names `[model name]_[year]_ens[ensemble number].nc` where "model name" is the model used (also specified in `Namelist.py`), "year" is the year of the simulation, and "ensemble number" is the ensemble number represented with three digits. 

The dimensions of each netCDF file are lifelength, stormID, and ensembleNum (the specific size of the dimensions can be found using the command `$ ncinfo [filename]`. "lifelength" is the amount length of time of the storm's life. "stormID" represents how many storms have been run at this year.  "ensembleNum" is the number of members in the intensity ensemble.

## Example 1: Running CHAZ without Preprocessing

This example runs one 40 intensity ensemble from the years 2000 through 2002. 
Download the Github repository. In the repository, there are `README.txt`, `Namelist.py`, `CHAZ.py`, and the following directories each containing a `README.txt`. 

Navigate to the downloaded repository on the Terminal. 

Run the following command:`$ ./CHAZ.py`

After running CHAZ, the directory `output` will contain the following files:


&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  `MPI_ESM_MR_2000_ens000.nc`: netCDF containing latitide, longitude, maximum wind speed, and time of resulting ensemble

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  `MPI_ESM_MR_2001_ens000.nc`: netCDF containing latitide, longitude, maximum wind speed, and time of resulting ensemble


&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  `MPI_ESM_MR_2002_ens000.nc`: netCDF containing latitide, longitude, maximum wind speed, and time of resulting ensemble

