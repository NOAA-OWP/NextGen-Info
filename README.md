# NextGen-Info

**Description**:  This repo serves as the central location for information on the Next Generation Water Resources Modeling Framework (NextGen) and its related tools, software, and datasets.

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

To produce total water level forecasts, OWP is adapting two coastal models to work with NextGen: the  [Semi-implicit Cross-scale Hydroscience Integrated System Model (SCHISM)](https://schism-dev.github.io/schism/master/index.html) and [D-Flow Flexible Mesh (FM)](https://www.deltares.nl/en/software-and-data/products/delft3d-fm-suite/modules/d-flow-flexible-mesh). Work on these models is ongoing and we will post repo links to their BMI implementations when they are completed.

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

The NextGen Forcings Engine is a set of Python utilities that builds NextGen-compatible forcing files from various meteorological reanalysis and forecast datasets. These include:

- AORC: Analysis of Record for Calibration
- GFS: Global Forecast System
- HRRR: High-Resolution Rapid Refresh
- CFS: Climate Forecast System 

The Forcings Engine can produce either CSV or NetCDF files on a regular grid or at the catchment scale. For further information, please see the [Forcings Engine repo](https://github.com/NOAA-OWP/ngen-forcing).

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




Put a meaningful, short, plain-language description of what
this project is trying to accomplish and why it matters.
Describe the problem(s) this project solves.
Describe how this software can improve the lives of its audience.

Other things to include:

  - **Technology stack**: Indicate the technological nature of the software, including primary programming language(s) and whether the software is intended as standalone or as a module in a framework or other ecosystem.
  - **Status**:  Alpha, Beta, 1.1, etc. It's OK to write a sentence, too. The goal is to let interested people know where this project is at. This is also a good place to link to the [CHANGELOG](CHANGELOG.md).
  - **Links to production or demo instances**
  - Describe what sets this apart from related-projects. Linking to another doc or page is OK if this can't be expressed in a sentence or two.


**Screenshot**: If the software has visual components, place a screenshot after the description; e.g.,

![](https://raw.githubusercontent.com/NOAA-OWP/owp-open-source-project-template/master/doc/Screenshot.png)


## Dependencies

Describe any dependencies that must be installed for this software to work.
This includes programming languages, databases or other storage mechanisms, build tools, frameworks, and so forth.
If specific versions of other software are required, or known not to work, call that out.

## Installation

Detailed instructions on how to install, configure, and get the project running.
This should be frequently tested to ensure reliability. Alternatively, link to
a separate [INSTALL](INSTALL.md) document.

## Configuration

If the software is configurable, describe it in detail, either here or in other documentation to which you link.

## Usage

Show users how to use the software.
Be specific.
Use appropriate formatting when showing code snippets.

## How to test the software

If the software includes automated tests, detail how to run those tests.

## Known issues

Document any known significant shortcomings with the software.

## Getting help

Instruct users how to get help with this software; this might include links to an issue tracker, wiki, mailing list, etc.

**Example**

If you have questions, concerns, bug reports, etc, please file an issue in this repository's Issue Tracker.

## Getting involved

This section should detail why people should get involved and describe key areas you are
currently focusing on; e.g., trying to get feedback on features, fixing certain bugs, building
important pieces, etc.

General instructions on _how_ to contribute should be stated with a link to [CONTRIBUTING](CONTRIBUTING.md).


----

## Open source licensing info
1. [TERMS](TERMS.md)
2. [LICENSE](LICENSE)


----

## Credits and references

1. Projects that inspired you
2. Related projects
3. Books, papers, talks, or other sources that have meaningful impact or influence on this project
