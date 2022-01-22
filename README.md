# GeoEnergyMap
A geographic breakdown of state energy percentage. There was a shapefile, and a data file gathered from the EIA to gather the outlines of the shapes, and associate energy generation data with each state to the mapping respectively.
The three most used packages were the GeoPandas, Numpy, and the MatPlotLib. This project required a new environment was created in Anaconda, just so we could work with the required GeoPandas package (primarilly to provide a cleaner environment here). Below is a little more of the process used here with a **primary focus on the Python/GeoPandas code**.
## Early Process
After downloading the data from the EIA, I broke the energy generation down as clean or dirty as seen in the following table:
|Clean|Non-Clean|
|-----|------|
|Geothermal|Coal|
|Pumped Storage|Other Gases|
|Hydroelectric Conventional|Other (due to uncertainty of the word)|
|Natural Gas (While this doesn’t classify as sustainable, it does classify as clean according to the Center for liquified Natural Gas)|Petroleum|
|Nuclear| |
|Solar Thermal and Photovoltaic| |
|Other Biomass| |
|Wind| |
|Wood and Wood Derived Fuels| |

Then, in Excel, I ran some array formulas to just sum up the energy generation as clean or dirty. Then, I just saved that as a CSV file.
## Working with GeoPandas



