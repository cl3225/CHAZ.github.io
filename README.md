# CHAZ
## (Columbia Tropical Cyclone Hazard Model)

## Overview 


The Columbia Tropical Cyclone Hazard Model (CHAZ) is a statistical-dynamical downscaling tropical cyclone hazard model. CHAZ consists of three components: a genesis model, a track model, and an intensity model. 

The genesis model relies on the Tropical Cyclone Genesis Index (TCGI, developed by Tippett et al, 2011). Using the TCGI, the seeding rate, which describes the rate at which weak vortices are formed throughout the domain is found. The seeding rate is then passed to the remaining two components, the track and intensity models, within which the storm evolves beyond genesis. 

The track model moves the storm forward via a Beta-Advection Model. From the Beta-Advection Model, predictors are calculated. 

The intensity model evolves the storms beyond genesis using an Autoregressive Model that involves a deterministic element and a stochastic forcing element. 


#### 3 Components of CHAZ


Genesis | Track | Intensity
------------ | ------------- | -------------
seeds domain with weak vortices  | further evolution of storm beyond genesis | further evolution of storm beyond genesis 
seeding rate, found via TCGI, depends on environmental conditions | moves storm by a beta advection model | 2 components: empirical linear regression model and stochastic element
for each seed, genesis location and date are chosen randomly on a 1 km resolution within the a selected month | predictors found based on beta-advection model|  empirical multiple linear regression model advances TC in time along track based on surrounding large scale environment 
&nbsp;|&nbsp;|  stochastic element, which only depends on the storm's current state and recent history, accounts for internal storm dynamics 
&nbsp;|&nbsp;|  *intensity at landfall is separate and uses separate regression model that takes into account proximity to land and other environmental conditions



![first_flow_chart](https://user-images.githubusercontent.com/46905677/93243910-bb8f0700-f73d-11ea-80ad-3ae64ea87326.jpg)

## Getting Started with CHAZ

### Getting the code

TBD 

### System dependencies 

#### Operating System

* Linux

#### Text Editor 

In the below instructions, vi is used, but any text editor will do.

#### Python

* Python 2

* [NumPy](https://numpy.org/)

* [pandas](https://pandas.pydata.org/)

The remaining portion of **Getting Started with CHAZ** assumes that the [Anaconda Python Distribution](https://www.anaconda.com/) is installed, which is a simple method to install the above. The below steps can be done without Anaconda installed, so in that case, change accordingly. 

### Building CHAZ

1. Confirm you are using bash shell: `echo $0`

-bash

2.  in your .bashrc (in your home directory) add the following lines

`$ vi .bashrc`

<img width="455" alt="Screen Shot 2020-09-16 at 5 47 54 PM" src="https://user-images.githubusercontent.com/46905677/93406675-1a8b7380-f845-11ea-9afd-432c766396b7.png">

3. Check if you can run python.

`$ ipython -pylab`

## Running CHAZ 

### Changing Global Variables in `Namelist.py`

Prior to calculating genesis, intensity, and track, the data must be preprocessed. After preprocessing data, CHAZ returns genesis, intensity, and track information that can be passed to numerous other models, such as storm surge models and a wind compenent model.

To run CHAZ, change the values of the global variables outlined below contained in `Namelist.py`. Below the global variables that can be changed are outlined below.

`Model = 'ERAInterim'` - model that provides reanalysis data (str), default is European Center for Medium Range Weather Forecasts interim reanalysis

`ENS = 'r1i1p1'` - global model (str)

`TCGIinput = 'TCGI_CRH_SST'` - environmental parameters used as input for TCGI (str)

`CHAZ_ENS = 1` - number of ensemble members per year (int)

`CHAZ__Int_ENS = 40` - number of ensemble members for intensity model (int)

`seedN = 1000` - annual seeding rate for random seeding (int)

`landmaskfile = 'landmask.nc'` - land mask file (NETCDF)

`ipath = ''` - location of environmental data for seeding ratio (str)

`opath = ''` - location of pik file with observed best track global predictors (str)

`obs_bt_path = ''` - location of observed best track data (str)

`Year1 = ` - first year (int)

`Year2 = ` - last year (int)

`runPreprocess = ` - if `True`, preprocessing will run, if `False` preprocessing will not run (bool)

`runCHAZ = ` - if `True`, CHAZ will run, if `False` CHAZ will not run (bool)

`calGen = ` - if `True`, genesis will  be calculated, if `False` genesis will not be calculated (bool)

`calBam = ` - if `True`, track will  be calculated, if `False` track will not be calculated (bool)

`calInt = ` - if `True`, intensity will  be calculated, if `False` intensity will not be calculated (bool)

### Running CHAZ.py

1. Copy the reanalysis data to your home directory in a new folder.

2. Now go to that folder and run the following command:`$ ./CHAZ.py`

![second_flow_chart](https://user-images.githubusercontent.com/46905677/93244535-be3e2c00-f73e-11ea-80b5-2f59f6d62a63.jpg)

