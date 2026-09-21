# Geographical Flexibility of Hyperscale Data-Centre Electricity Demand

This project is part of the master's thesis “Can AI data centres solve the energy flexibility problem?” and it provides the computational workflow supporting the analysis.

--- 
## Overview

The workflow builds a geographically referenced inventory of Amazon, Google and Microsoft data centres, estimates their capacity and combines these estimates with hourly renewable-availability and load profiles. It then models how flexible workloads could be shifted between locations operated by the same provider and explores future demand scenarios.

The 16 Jupyter notebooks provide a transparent basis for assessing the technical potential of geographical workload shifting to align data-centre demand with renewable availability. Comparisons across workload assumptions, allocation methods and future demand scenarios help identify the conditions and constraints that influence this potential. Capacity estimates and renewable availability are modelling proxies, rather than measured facility demand or generation.

--- 
## Setup

The notebooks require Python 3.12.13 and the Python packages at the versions specified in [requirements.txt](requirements.txt). These dependencies must be satisfied in the Python environment used to execute the notebooks.

--- 
## Notebook overview

- [01 Acquire OSM source data](notebooks/01_acquire_osm_source_data.ipynb): Loads or acquires OpenStreetMap data-centre records and nearby building footprints, preserving source identifiers and geometry.
- [02 Prepare hyperscaler candidates](notebooks/02_prepare_hyperscaler_candidates.ipynb): Cleans source records and identifies Amazon, Google and Microsoft candidates using the supplied operator rules.
- [03 Enrich geography and group campuses](notebooks/03_enrich_geography_and_group_campuses.ipynb): Adds countries, regions and time zones, removes exact source duplicates and groups nearby facilities belonging to the same operator into campuses.
- [04 Estimate facility capacity](notebooks/04_estimate_facility_capacity.ipynb): Combines building footprints and floor evidence to estimate facility IT load and total power, including alternative power-density assumptions.
- [05 Validate and prepare analysis datasets](notebooks/05_validate_and_prepare_analysis_datasets.ipynb): Validates the facility estimates and exports facility and campus datasets with reconciled capacity totals.
- [06 Prepare future-location scenario](notebooks/06_prepare_future_location_scenario.ipynb): Combines manually compiled future locations and estimated scenario capacities with the current facility baseline.
- [07 Prepare renewable query locations](notebooks/07_create_renewables_ninja_query_locations.ipynb): Creates representative locations for renewable-profile queries and maps the campuses to these locations.
- [08 Acquire renewable profiles](notebooks/08_acquire_renewables_ninja_profiles.ipynb): Reuses cached Renewables.ninja responses or downloads missing solar and wind profiles, then combines them into a 50/50 renewable-availability proxy.
- [09 Rank renewable availability](notebooks/09_rank_renewable_availability.ipynb): Ranks each location's hourly renewable availability within each UTC day to identify candidate sending and receiving hours for workload shifting.
- [10 Prepare UKPN load profiles](notebooks/10_prepare_ukpn_load_profiles.ipynb): Processes the supplied UK Power Networks data to derive normalised hourly load shapes for the workload model.
- [11 Build hourly model input](notebooks/11_build_hourly_workload_model_input.ipynb): Combines campus capacities, renewable profiles and local-clock load shapes into hourly inputs for the workload scenarios.
- [12 Model geographic workload shifting](notebooks/12_model_geographic_workload_shifting.ipynb): Evaluates same-operator shifting using Greedy and optimised linear programming across utilisation, flexibility and six-/nine-hour candidate settings.
- [13 Project future demand and flexibility](notebooks/13_project_future_demand_and_flexibility.ipynb): Applies IEA-based 2030 and 2035 demand scenarios to scale the validated workload results while retaining the represented spatial network.
- [14 Visualise facilities and campuses](notebooks/14_visualize_facilities_and_campuses.ipynb): Maps the geographical distribution of facilities and campuses by provider.
- [15 Visualise workload flexibility](notebooks/15_visualize_workload_flexibility_results.ipynb): Compares shifting potential, allocation methods, workload assumptions, geographical flows and historical weather variation.
- [16 Visualise projections](notebooks/16_visualize_projection_results.ipynb): Presents projected shifting potential, workload sensitivity and regional flows across the future demand scenarios.

--- 
## Data sources and references

**Facility inventory and building evidence:**
- OpenStreetMap contributors. *OpenStreetMap* [Data set]. Retrieved from [https://www.openstreetmap.org/](https://www.openstreetmap.org/)
- Overture Maps Foundation. (2026). *Overture Maps buildings* (Release 2026-04-15.0) [Data set]. [https://docs.overturemaps.org/blog/2026/04/15/release-notes/](https://docs.overturemaps.org/blog/2026/04/15/release-notes/)

**Operator location information:**
- Amazon Web Services. *AWS Global infrastructure*. Retrieved from [https://aws.amazon.com/about-aws/global-infrastructure/](https://aws.amazon.com/about-aws/global-infrastructure/)
- Google. *Google Data Centers*. Retrieved from [https://datacenters.google/locations/#data-center-list](https://datacenters.google/locations/#data-center-list)
- Microsoft. *Microsoft Datacenters*. Retrieved from [https://datacenters.microsoft.com/globe/explore/](https://datacenters.microsoft.com/globe/explore/)

**Renewable availability:**
- Renewables.ninja. *Renewables.ninja - solar PV and wind profile service* [Data set]. Retrieved from [https://www.renewables.ninja/](https://www.renewables.ninja/)
- Pfenninger, S., & Staffell, I. (2016). Long-term patterns of European PV output using 30 years of validated hourly reanalysis and satellite data. *Energy, 114*, 1251–1265. [https://doi.org/10.1016/j.energy.2016.08.060](https://doi.org/10.1016/j.energy.2016.08.060)
- Staffell, I., & Pfenninger, S. (2016). Using bias-corrected reanalysis to simulate current and future wind power output. *Energy, 114*, 1224–1239. [https://doi.org/10.1016/j.energy.2016.08.068](https://doi.org/10.1016/j.energy.2016.08.068)

**Electricity demand profiles:**
- UK Power Networks. *Data centre demand profiles* [Data set]. Retrieved from [https://ukpowernetworks.opendatasoft.com/explore/dataset/ukpn-data-centre-demand-profiles/information/](https://ukpowernetworks.opendatasoft.com/explore/dataset/ukpn-data-centre-demand-profiles/information/)

**Future demand scenarios:**
- International Energy Agency. (2025). *Energy and AI*. [https://www.iea.org/reports/energy-and-ai](https://www.iea.org/reports/energy-and-ai)

**Geographical boundaries and maps:**
- Natural Earth. (2022). *Admin 0 – Countries* (1:110 million; layer version 5.1.1, supplied in release 5.1.2) [Data set]. [https://www.naturalearthdata.com/downloads/110m-cultural-vectors/110m-admin-0-countries/](https://www.naturalearthdata.com/downloads/110m-cultural-vectors/110m-admin-0-countries/)

---
## Source attribution

- © OpenStreetMap contributors; OSM data are available under the [Open Database Licence (ODbL)](https://opendatacommons.org/licenses/odbl/1-0/). 
- Building evidence also uses Overture Maps Foundation data and the contributors listed in its [attribution and licensing documentation](https://docs.overturemaps.org/attribution/#buildings).  
- Maps are made with Natural Earth, whose map data are [public domain](https://www.naturalearthdata.com/about/terms-of-use/).  
- Renewables.ninja profiles are provided under [CC BY-NC 4.0](https://www.renewables.ninja/about). 
- IEA-based projections are derived from *Energy and AI*, licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Stefan Vasic is responsible for this derived analysis; it is not endorsed by the IEA or its member countries. See the [IEA attribution terms](https://www.iea.org/terms/creative-commons-cc-licenses).

--- 
## Educational use and attribution

The notebook code and documentation may be used, copied and adapted for non-commercial teaching, study and academic research, provided that Stefan Vasic is credited. Other uses require the author's permission. This is a custom educational-use notice. Third-party datasets and software remain subject to their respective licences and source terms.

--- 
## Author

Stefan Vasic
