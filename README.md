# NextGen-Info

**Description**:  This repo serves as the central location for information on the Next Generation Water Resources Modeling Framework (NextGen) and its related tools, software, and datasets.

**Status**: Documentation and references will continue to evolve as the NextGen ecosystem grows!

## NextGen Framework

NextGen is a language- and model-agnostic framework that takes a data-centric approach to hydrologic modeling. This enables various models to use the same spatial discretizations, forcing data, hydrofabric, and other standard features. NextGen development supports both operational National Water Model (NMW)improvements for the [NOAA-NWS Office of Water Prediction (OWP)](https://water.noaa.gov/about/owp) as well as a diverse set of applications created by the hydrologic modeling community, particularly the [Cooperative Institute for Research to Operations in Hydrology (CIROH)](https://ciroh.ua.edu/).

Like the other OWP products detailed on this page the [NextGen codebase](https://github.com/NOAA-OWP/ngen) is freely available online via GitHub. For those just getting started with the framework, we recommend reading the [NextGen Wiki](https://github.com/NOAA-OWP/ngen/wiki).

## Formulations

A NextGen formulation is any single model or combination of models and/or modules running in a basin to simulate that location's hydrology. A formulation may include one more hydrologic models and modules, plus a streamflow routing module, a coastal model, and other supporting routines. The final configuration depends on a given basin's dominant hydrologic processes and the model representations thereof. 

It is important to highlight that NextGen is a framework and not a model. It can run a variety of hydrologic models and modules, plus other routines, but it does not perform any water or energy balance computations on its own. 

To that end, OWP has prepared a large selection models and modules to run in NextGen, which we detail in the following subsections. We note here that these are a starting point. We encourage the community to develop new models and adapt existing ones for use in the framework. Currently NextGen supports models written in the following languages:

- C
- C++
- Fortran
- Python

NextGen's core functionalities leverage [version 2.0 of the Basic Model Interface (BMI)](https://github.com/csdms/bmi/), developed by the [Community Surface Dynamics Modeling System (CSDMS)](https://csdms.colorado.edu/) at the University of Colorado Boulder. Therefore, each model must have its own complete implementation of BMI to run in the framework. 

For more details on BMI, please see the [CSDMS Wiki](https://csdms.colorado.edu/wiki/BMI) and [documentation](https://bmi.readthedocs.io/en/stable/). 

For more information on making a model NextGen compliant with BMI, please see the training materials [here](https://github.com/SnowHydrology/ciroh_workshop_2023/) and [here](https://github.com/LynkerIntel/bmi-tutorial).

### Hydrologic models and modules

The initial selection of hydrologic models and modules includes routines for many hydrologic processes and states, such as: interception of precipitation by vegetation, snow accumulation and melt, evapotranspiration, infiltration-runoff partitioning, overland flow, terrain routing, subsurface lateral flow, groundwater flux, and soil moisture. 

The table below indicates the key processes and primary programming language for each of the models and modules OWP has implemented to date. Click on a model's name to go to its source code hosted on GitHub.

| Model      | Key processes | Language     |
| :---        |    :----:   |          ---: |
| [Noah-OWP-Modular](https://github.com/NOAA-OWP/noah-owp-modular)      |  Interception, snow accumulation and melt, evapotranspiration      | Fortran   |
| [Snow-17](https://github.com/NOAA-OWP/snow17)   | Snow accumulation and melt        | Fortran      |
| [TopoFlow](https://github.com/NOAA-OWP/topoflow)   | Snow accumulation and melt, glacier melt        | Python      |
| [PET](https://github.com/NOAA-OWP/evapotranspiration)   | Potential evapotranspiration        | C      |
| [CFE](https://github.com/NOAA-OWP/cfe)   | Infiltration-runoff partitioning, soil moisture        | C      |
| [TOPMODEL](https://github.com/NOAA-OWP/topmodel)   | Infiltration-runoff partitioning, soil moisture        | C      |
| [LASAM](https://github.com/NOAA-OWP/lgar-c)   | Infiltration-runoff partitioning, soil moisture        | C      |
| [Sac-SMA](https://github.com/NOAA-OWP/sac-sma)   | Infiltration-runoff partitioning, soil moisture        | Fortran      |
| [SoilFreezeThaw](https://github.com/NOAA-OWP/soilfreezethaw)   | Soil temperature        | C++      |
| [SoilMoistureProfiles](https://github.com/NOAA-OWP/SoilMoistureProfiles)   | Soil moisture        | C++      |
| [LSTM](https://github.com/NOAA-OWP/lstm)   | Streamflow (machine learning)        | Python      |

### Streamflow routing

The T-Route routing module converts water flux data (e.g., overland flow, later flow, groundwater flow) from the hydrologic models listed above into streamflow and routes it through the river network. T-Route features different routing options, including Muskingum-Cunge, diffusive wave, and dynamic wave. For more information and the source code, please visit the [T-Route repo](https://github.com/NOAA-OWP/t-route/).

### Coastal models

To produce total water level forecasts, OWP is adapting two coastal models to work with NextGen: the [Semi-implicit Cross-scale Hydroscience Integrated System Model (SCHISM)](https://schism-dev.github.io/schism/master/index.html) and [D-Flow Flexible Mesh (FM)](https://www.deltares.nl/en/software-and-data/products/delft3d-fm-suite/modules/d-flow-flexible-mesh). The initial development stages for the coastal model BMIs have been completed and their respective GitHub repositories are stored in the following pathways: SCHISM - [https://github.com/LynkerIntel/SCHISM_BMI](https://github.com/LynkerIntel/SCHISM_BMI); DFlowFM - [https://github.com/LynkerIntel/DFlowFM_BMI](https://github.com/LynkerIntel/DFlowFM_BMI). Once the NextGen framework evaluation is complete with the SCHISM Total Water Level (TWL) capability, the SCHISM BMI repository will be merged with the SCHISM master repository (https://github.com/schism-dev/schism) in the near future. For the DFlowFM model, its BMI development is an entire separate branch of the BMI code that is incompatible with the Deltares DFlowFM master branch and that will remain the case until the DFlowFM EC-module is redeveloped entirely for BMI compatibility. For now, the NOAA DFlowFM BMI development is a frozen DFlowFM version for research and development initiatives under the NextGen project in the future with the TWL capability.

### Other modules

There are several other modules that complement the formulations running in NextGen. 

One is the Simple Logical Tautology Handler (SLoTH), which provides constant values and performs simple calculations outside of the hydrologic models. 

Another module in development, jinjaBMI, performs variable transformations that cannot be calculated by unit conversion alone.

Finally, aorc_bmi provides forcing data to several models (e.g., CFE and PET) when running in standalone mode outside of the NextGen framework.

| Module      | Key processes | Language     |
| :---        |    :----:   |          ---: |
| [SLoTH](https://github.com/NOAA-OWP/SLoTH)      |  Constant values, simple calculations      | C++   |
| [jinjaBMI](https://github.com/mattw-nws/jinjabmi)   | Variable transformations       | Python      |
| [aorc_bmi](https://github.com/NOAA-OWP/aorc_bmi)   | Forcing data for standalone models        | C      |

## Forcings Engine

The NextGen Forcings Engine is a set of Python utilities that builds NextGen-compatible forcing files from various meteorological reanalysis and forecast datasets that are utilized for NWM operational configurations (https://github.com/NCAR/WrfHydroForcing/tree/main/Config/WCOSS/v3.0). These include:

- AORC: Analysis of Record for Calibration
- GFS: Global Forecast System
- HRRR: High-Resolution Rapid Refresh
- RAP: Rapid Refresh
- CFS: Climate Forecast System
- NAM: North American Mesoscale Forecast System
- WRF-ARW: Weather Research and Forecast Model- Advanced Research
- MRMS: Multi-Radar/Multi-Sensor System
- Stage IV: Quantitative Precipitation Estimates
- NWM: NWM Retrospective Forcing Files
- NBM: National Blend of Models
- NDFD: National Digital Forecast Database
- ERA5: ERA5-Interim Reanalysis Dataset

The NextGen Forcings Engine BMI can produce a single NetCDF forcing file for a regular grid, catchment scale, or an unstructured mesh or act as a forcings provider BMI under the NextGen framework. The NextGen lumped forcings driver can also produce NextGen eligible forcing NetCDF/CSV forcing files for a small range of forcing datasets (AORC, GFS, HRRR, CFS) as supplementary tool for the community using a fast regridding method called ExactExtract if desired. More tools in the Forcing Engine repository include pre-processing domain configurations (hydrofabric geopackages, coastal meshes) into ESMF-compliant NetCDF files as well as a suite of Python scripts for downloading forcing datasets off the NOMADs operational server. For further information, please see the [Forcings Engine repo](https://github.com/NOAA-OWP/ngen-forcing).

## Geospatial data

### Hydrofabric

The hydrofabric is an essential component of every NextGen run. It serves several purposes, such as:

- Delineating the catchment(s) within the simulation domain
- Defining the topology of the catchments and nexuses to enable fast, accurate streamflow routing via T-Route
- Providing important basin attributes and parameter value estimates (e.g., land cover category, soil type, basin slope, etc.) that can populate model configuration files

Pre-generated hydrofabric data are publicly accessible online [here](https://www.lynker-spatial.com/data).

https://github.com/NOAA-OWP/hydrofabric

### HARBOR


## Calibration and regionalization

https://github.com/NOAA-OWP/ngen-cal
https://github.com/NOAA-OWP/NextGen_Regionalization
https://github.com/NOAA-OWP/formulation-selector

## Distributed Model on Demand

https://github.com/NOAA-OWP/DMOD

# Getting help

For more information or help with a specific component, follow the links.  General inqueries can also be logged in the Issues of this repository and may be re-directed as appropriate.

# Getting involved

Pull Requests with additional relevant NextGen information and/or documentation will be reviewed and considered!  
For contributions/involvement of individual components, please check the respective repository.

----

# Open source licensing info
1. [TERMS](TERMS.md)
2. [LICENSE](LICENSE)

----
