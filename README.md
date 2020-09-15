# CHAZ
## (Columbia Tropical Cyclone Hazard Model)

### Overview 

The Columbia Tropical Cyclone Hazard Model (CHAZ) is a statistical-dynamical downscaling tropical cyclone hazard model. CHAZ consists of three components: a genesis model, a track model, and an intensity model. 

The genesis model relies on the Tropical Cyclone Genesis Index (TCGI, developed by Tippett et al, 2011). Using the TCGI, the seeding rate, which describes the rate at which weak vortices are formed throughout the domain is found. The seeding rate is then passed to the remaining two components, the track and intensity models, within which the storm evolves beyond genesis. 

The track model moves the storm forward via a Beta-Advection Model. From the Beta-Advection Model, predictors are calculated. 

The intensity model evolves the storms beyond genesis using an Autoregressive Model that involves a deterministic element and a stochastic forcing element. 

##### 3 Components of CHAZ
Genesis | Track | Intensity
------------ | ------------- | -------------
seeds domain with weak vortices  | further evolution of storm beyond genesis | further evolution of storm beyond genesis 
seeding rate, found via TCGI, depends on environmental conditions | moves storm by a beta advection model | 2 components: empirical linear regression model and stochastic element
for each seed, genesis location and date are chosen randomly on a 1 km resolution within the a selected month | predictors found based on beta-advection model|  empirical multiple linear regression model advances TC in time along track based on surrounding large scale environment 
 | |  |   stochastic element, which only depends on the storm's current state and recent history, accounts for internal storm dynamics 
 | |  |   *intensity at landfall is separate and uses separate regression model that takes into account proximity to land and other environmental conditions

![first_flow_chart](https://user-images.githubusercontent.com/46905677/93243910-bb8f0700-f73d-11ea-80ad-3ae64ea87326.jpg)

### Getting Started with CHAZ

Prior to calculating genesis, intensity, and track, the data must be preprocessed. After preprocessing data, CHAZ returns genesis, intensity, and track information that can be passed to numerous other models, such as storm surge models and a wind compenent model.

![second_flow_chart](https://user-images.githubusercontent.com/46905677/93244535-be3e2c00-f73e-11ea-80b5-2f59f6d62a63.jpg)

### Running CHAZ 
