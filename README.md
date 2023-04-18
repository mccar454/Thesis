**MATERIALS AND METHODS USED FOR "Trends in Water Use, Energy Consumption, and Carbon Emissions from Irrigation: Role of Shifting Technologies and Energy Source"**
*A publication in Environmental Sceince and Technology*

Authors: *Benjamin McCarthy, Robert Anex, Yong Wang, Anthony D. Kendall, Annick Anctil, Erin M. K. Haacker, David W. Hyndman*


In this repository () there are four main folders: 01_Input, 02_Analysis, 03_EnergySources, 04_Plotter, 05_Results.  Here's what they do:

**01_Input:**
This folder contains the first script that needs to be run. Here, the first run will process our initial WIMAS data (found in the Data folder, along with a series of other datasets mentioned below).


 *FPDIV_WIMAS_INPUT.py*
-This script is what gathers the initial data from the WIMAS database.  It also extracts other     values from other databases, such as:

1. Hydraulic Conductivity (meters/day)
	
   *Cederstrand, Joel, and Mark Becker. Digital Map of Hydraulic Conductivity for High Plains Aquifer in Parts of Colorado, Kansas, Nebraska, New Mexico, Oklahoma, South Dakota, Texas, and Wyoming. 1998, doi:10.3133/ofr98548.Cederstrand, Joel, and Mark Becker. Digital Map of Hydraulic Conductivity for High Plains Aquifer in Parts of Colorado, Kansas, Nebraska, New Mexico, Oklahoma, South Dakota, Texas, and Wyoming. 1998, doi:10.3133/ofr98548.*

2. Specific Yield
	
    *McGuire, V. L., et al. Specific Yield, High Plains Aquifer. 2012, http://pubs.er.usgs.gov/publication/sir20125177.*

3. Depth to water (meters)

Depth to water is estimated in the script using two different sources:

Water Levels were obtained from:

   *Haacker, Erin M. K., et al. “Water Level Declines in the High Plains Aquifer: Predevelopment to Resource Senescence.” Groundwater, vol. 54, no. 2, 2016, pp. 231–42, doi:10.1111/gwat.12350.*

CONUS DEM from:
	
   *National Elevation Dataset - NAVD88 Meters - 1/3rd-Arc-Second (Approx. 10m). 2012, https://gisdata.nd.gov/Metadata/ISO/html/metadata_DEM_NED_10m.html#ID0EEBBGOA.

4. Saturated Thickness (meters)
Saturated thickness was obtained from:

   *Haacker, Erin M. K., et al. “Water Level Declines in the High Plains Aquifer: Predevelopment to Resource Senescence.” Groundwater, vol. 54, no. 2, 2016, pp. 231–42, doi:10.1111/gwat.12350.*

5. High Plains Aquifer Boundary

	*Qi, S. L. Digital Map of the Aquifer Boundary of the High Plains Aquifer in Parts of Colorado, Kansas, Nebraska, New Mexico, Oklahoma, South Dakota, Texas, and Wyoming: U.S. Geological Survey Data Series 543. 2010, https://pubs.usgs.gov/ds/543/.* 

- End product from this script is a pandas dataframe 'LiftDF' which is stored in a pandas hdfs:
	C:/WIMAS_Methods_Scripts/00_Data/Kansas_WIMAS.h5

**02_Analysis:** Here we have the scripts that do the analytical work. We have the two irrigation scenarios in this folder, along with th energy source scenarios. 

*FPDIV_WIMAS_ANALYSIS.py*

   * This script gathers the 'LiftDF' stored data and calculates total lift energy using simple energy equation
			ENERGY = MASS * GRAVITY * WATER_LIFT
   * This energy result DOES NOT take into account pump efficiencies

   * In this script:  Drawdown calculation, irrigation system pressurization, pump rate estimates.
	- End product from this script is a pandas dataframe 'LiftEnergyDF' which is stored in a pandas hdfs:
	S:\Users\mccar454\Project_Files\2017\WIMAS_CHP\Kansas_WIMAS.h5
	-The other output is a shapefile stored in a gdb file name "Kansas_FPDIV.gdb" That will go into the Energy Sources work described below

*FPDIV_WIMAS_ANALYSIS_NOLEPARUN.py*

   * This is a similar script to the one above, but it assumes that no LEPA occured, and converts all center pivot technology to conventional center pivot. 

*GWCO2_Contribution_Analysis.py*
	This script takes into account the CO2 released from directly depleting the aquifer.
 	
   Bicarbonate site data:
	Two versions were downloaded
	Site Data only
	Sample results (narrow)
https://www.waterqualitydata.us/portal/#countrycode=US&statecode=US%3A20&sampleMedia=Water&characteristicName=Bicarbonate&providers=NWIS&providers=STORET&mimeType=csv
	
   Saturated Thickness (meters)
	Saturated thickness was obtained from:
   *Haacker, Erin M. K., et al. “Water Level Declines in the High Plains Aquifer: Predevelopment to Resource Senescence.” Groundwater, vol. 54, no. 2, 2016, pp. 231–42, doi:10.1111/gwat.12350.

   Water Levels were obtained from:
   *Haacker, Erin M. K., et al. “Water Level Declines in the High Plains Aquifer: Predevelopment to Resource Senescence.” Groundwater, vol. 54, no. 2, 2016, pp. 231–42, doi:10.1111/gwat.12350.

**03_EnergySources:** This section was created by Yong Wang (wangyong0910@gmail.com)

Here, we estimate energy source shift in the region and assign an energy code value to each well. Along with that, the scripts create a new scenario, where energy sources did not shift from what they were in 1994. 
In the folder there is a readme file that delves into more detail regarding the process. 

**04_Plotter:**
Here we have the script that created the plots for our manuscript

*FPDIV_FInalFigurePlotter.py*
   *Inputs needed:
	Palmer Drought index
	https://www.ncdc.noaa.gov/cag/statewide/time-series/14/pdsi/all/1/1994-2016

   *Outputs from EnergySources

   *S:\Users\mccar454\Project_Files\2017\WIMAS_CHP\Kansas_WIMAS.h5

**05_Results:** This folder contains a simplified version of the energy results.

   *AT_AE_Converted.csv*
	This csv file contains the results from our actual observations of energy source and technology 
	
   *AT_VE_Converted.csv*
	This csv file contains the results from our actual technology observations and virtual ('static') energy source

   *VT_AE_Converted.csv*
	This csv file contains our results from our Virtual ('static') technology change and actual energy change

   *VT_VE_Converted.csv*
	This csv file contains our results from our Virtual ('static') energy and technology change

-In all of the mentioned csv files, columns are as follows:
	
**FPDIVKEY**: Unique well identifier
	energy_cd: 
		7	*Wind*
		1	*Electricity*
		2	*Gasoline*
		3	*Propane*
		4	*Oil*
		5	*Natural Gas*
		6	*Diesel*	
	
**FID**: Indexer
**WUA_YEAR**: year of use
**LONGITUDE**: 
**LATITUDE**:
**LIFT_ENERGY**: Direct energy needed at the farm (excluding pump and prime mover efficiency) in MJ/yr
**CEDTotal**: Energy Footprint in MJ/yr (including pump and prime mover efficiency)
**GHGTotal**: (kg CO2/yr)Total Greenhouse Gas emissionsbased on energy source from the energy footprint results. 


*FilterandCovnersion.py*
    This script merges the results of FPDIV_WIMAS_ANALYSIS.py and EnergySources to output the final results. 
    
**AUTHOR INFORMATION**
*Corresponding author:* Benjamin McCarthy, mccar454@msu.edu
288 Farm Lane, Room 207 Natural Science, Michigan State University, East Lansing, MI 48824-1115


*This work is supported by INFEWS grant 2018-67003-27406 (accession No. 1013707) from the USDA National Institute of Food and Agriculture, 
“Developing Pathways Toward Sustainable Irrigation across the United States Using Process-based Systems Models (SIRUS)”. Any opinions, findings, 
and conclusions or recommendations expressed in this publication are those of the authors and do not necessarily reflect the views of the USDA. 
Work by E.M.K. Haacker was supported by the National Institute of Food and Agriculture, U.S. Department of Agriculture, under award 2016-68007-25066, “
Sustaining agriculture through adaptive management to preserve the Ogallala Aquifer under a changing climate.”
