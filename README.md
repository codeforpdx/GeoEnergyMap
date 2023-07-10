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
As mentioned above, one key aspect is setting up an environment in which you have GeoPandas installed. There are a number of prerequisite packages that are necessary to have installed. [This is a very simple guide to using Anaconda to have those set up from a clean environment. ](https://krutarthpatel929.medium.com/complete-and-easy-installation-of-geopandas-in-python-aaad3b5f9660).
Next, open a Jupyter Notebook and import geoPandas, numpy, pandas, shapely, and matplotlib. From there, I downloaded the shapely file to obtain a map of the US and converted it to a JSON file format via a converter like [this one](https://products.aspose.app/gis/conversion/shapefile-to-json). Now, you can read in this map file along with the CSV file that was saved in the early process with the `pd.read.csv()` command. Join the two tables on state name (making sure you spelled out all state names in the csv file so that there are exact matches). Then add a column that allows you to categorize each state by which 20% it fits into. That is accomplished with the lines:
`Full_State = pnd.merge(states,St_energy,on=["NAME"])`

`Ranked = Full_State["Clean"].str.replace("%","",).astype(float).rank()`

`Ranked = pnd.Series(Ranked)`

`Full_State.insert(27,"Ranked",Ranked)`

`RR = []`

`for i in Full_State["Ranked"]:`

`    if 1 <= i <=10:`

`        RR.append(["0-20th Percentile"])`

`    elif 11<= i <=20:`

`        RR.append(["20-40th Percentile"])`

`    elif 21<= i <=30:`

`        RR.append(["40-60th Percentile"])`

`    elif 31<= i <=40:`

`        RR.append(["60-80th Percentile"])`

`    else:`

`        RR.append(["Top 20th Percentile"])`
        
`RR=pnd.to_numeric(RR,errors='coerce')`

`Full_State.insert(27,"RankRange",RR)`

This basicaly sets up the data. The only challenge left was that we had Alaska and Hawaii's space from the continental states dwarfing what distinctions we could see from the continental states. So it was worth setting up a sub dataframe for those two states. Then, after setting up a new color map with "lime", and "red", create 3 maps for continental, Alaska, and Hawaii. You can then just copy and paste them together into a new .png file with any graphics editor like Paint. 
![Here is the final result](https://github.com/AxisMeetsWorld/GeoEnergyMap/blob/main/Sustainable%20energy%20mix%20by%20State.png)

### Updating this File after 2 years; exploring the possibilities of exporting the rankings and maps to other applications (.csvs, and  .png files)

I added a branch to address the above needs. ⚠️I discovered after revisiting this code that there were some geopandas issues due to updates in some packages that didn't translate well.⚠️ In the lastest "Update" code, I handled the versioning for that, and noted that Alaska didn't color in appropriately. So this update also accounted for the need to extract the right color blend of red and lime. I haven't pushed this out yet, but you can see what was modified in the **EnergyDataUpdate** file.




