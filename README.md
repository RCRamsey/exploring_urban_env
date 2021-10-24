# exploring_urban_env

##Data Sources:  
https://www.wsdot.wa.gov/mapsdata/geodatacatalog/default.htm  

https://www.census.gov/geographies/mapping-files/time-series/geo/carto-boundary-file.html   


##Source Data Files:  

##Process:  
Explored (online) map of open source data at https://www.opendatasoft.com/blog/2015/11/02/how-we-put-together-a-list-of-1600-open-data-portals-around-the-world-to-help-open-data-community    
Decided to pick data from Washington State and discovered Washington State Department of Transportation.  

Chose the WSDOT GIS Data Download site to review "Bridge Data - All Bridge and Tunnel Inventory" with the idea of paring it down to an urban area.  

Used command line to download US Census Cartographic Boundary File for US urban areas shapefile. Command utilized:  

**curl -LOk https://www2.census.gov/geo/tiger/GENZ2018/shp/cb_2018_us_ua10_500k.zip**  

Attempted to use command line to download All Bridge and Tunnel Inventory but realized the name of the folder on the page did not match the name that downloads (manually downloaded using download link on page. Instead the command only downloaded information pertaining to the webpage itself.  Command utilized (failed):  

**curl -LOk https://www.wsdot.wa.gov/mapsdata/geodatacatalog/default.htm/AllBridgeandTunnelInventory.zip**  

Struggled to open/access the Bridge & Tunnel folder's technical data within command line. Aware that the data is given within the .htm file and opened it online to view data that was interested in. Command utilized (failed):  

**ogrinfo -so AllBridgeAndTunnelInventory.htm AllBridgeAndTunnelInvetory**  

Later tried another command to gather info from the overarching folder itself. Command utilized:
**ogrinfo AllBridgeAndTunnelInventory.gdb** 
This atleast told me the entire folder was *point*.

Learned about Bridge & Tunnel data:  
Contains 10380 point/vector objects.  
NAD 1983 HARN StatePlane Washington South FIPS 4602 Feet projection.
80+ Attributes per object
Interested in the following Attributes-BridgeNumber, BridgeName, CityName, CountyName, YearBuilt, YearRebuilt, , MinVertClrncUnderBridge, BridgeOverallConditionState, InspectionDate, InspectionDueDate, Latitude, Longitude. 

Interested in King County (Seattle's County) and pair with Seattle Urban Area. 
