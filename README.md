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

From within ..data/wa_state_bridge_and_tunnel. Struggled to open/access the Bridge & Tunnel folder's technical data within command line. Command utilized (failed):  

**ogrinfo -so AllBridgeAndTunnelInventory.htm AllBridgeAndTunnelInvetory**  

From within ..data/wa_state_bridge_and_tunnel. Tried another command to gather info from the overarching folder itself. Command utilized:
**ogrinfo AllBridgeAndTunnelInventory.gdb** 
This atleast told me the entire folder was *point*.

Aware that the data is given within the .htm file and opened it online to view data that was interested in. Learned about Bridge & Tunnel data:  
Contains 10380 point/vector objects.  
NAD 1983 HARN StatePlane Washington South FIPS 4602 Feet projection.
80+ Attributes per object
Interested in the following Attributes-BridgeNumber, BridgeName, CityName, CountyName, YearBuilt, YearRebuilt, , MinVertClrncUnderBridge, BridgeOverallConditionState, InspectionDate, InspectionDueDate, Latitude, Longitude. 

Settling on King County (Seattle's County) and pair with Seattle Urban Area. 

🔥 Curious as to the work around for accessing GDB files using ogr. Current workaround as only have worked with shapefiles was to open in QGIS (😱) and save the file (without changing any other attributes/projection/data) as a shapefile to continue working through the lab as had in the lesson.

**ogrinfo -so wa_bridge_tunnel.shp wa_bridge_tunnel** was successful now.

Edited wa_bridge_tunnel.shp from original projection (NAD1983) to WGS 84 and saved new shapefile to folder. Command utilized:
**ogr2ogr wa_bridge_tunnel_4326.shp -t_srs "EPSG:4326" wa_bridge_tunnel.shp** 

Confirmed projection change. Command utilized:
**ogrinfo -so wa_bridge_tunnel_4326.shp wa_bridge_tunnel_4326** 

Repeated same projection change procedure for census urban areas. Commands utilized:
**ogrinfo -so cb_2018_us_ua10_500k.shp cb_2018_us_ua10_500k** 
**ogr2ogr wa_bridge_tunnel_4326.shp -t_srs "EPSG:4326" wa_bridge_tunnel.shp**
**ogrinfo -so cb_2018_us_ua10_500k_4326.shp cb_2018_us_ua10_500k_4326**

Converted Washington Bridges/Tunnels and Urban Areas Shapefiles to GeoJSON. Commands utilized:
**ogr2ogr -f "GeoJSON" ../urban_areas.json cb_2018_us_ua10_500k_4326.shp**
**ogr2ogr -f "GeoJSON" ../../wa_bridge.json wa_bridge_tunnel_4326.shp**

Reduce file size of urban areas (currently 24.78 MB ) by only pulling to geoJSON Seattle Urban areas. Command utilized:
**ogr2ogr -f "GeoJSON" -where "NAME10='Seattle, WA'" seattle_urban.json urban_areas.json**
**dir**
dir command demonstrated current size of json now 85 KB

Reduce file size of wa_bridge.json (which actually still had bridges and tunnels currently 19.45 MB) by only pulling to geoJSON King County Bridges/tunnels. Command utilized: 
**ogr2ogr -f "GeoJSON" -where "CountyName='King County'" king_cnty_bridge_tunnels.json wa_bridge.json** 
**dir**
dir command demonstrated current size of json now 3.25 MB


