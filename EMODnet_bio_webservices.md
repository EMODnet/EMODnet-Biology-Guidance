# Web services documentation

The EMODnet Biology data are available as a [Web Feature Service (WFS)](https://docs.geoserver.org/latest/en/user/services/wfs/index.html) in accordance with the [Open Geospatial Consortium (OGC)](https://www.ogc.org/) specifications. This webservice supports requests for geographical feature data (with vector geometry and attributes). The base link for performing a WFS request to EMODnet Biology is:

[https://geo.vliz.be/geoserver/Dataportal/ows?](https://geo.vliz.be/geoserver/Dataportal/ows?)

The [Download Toolbox](https://www.emodnet-biology.eu/toolbox) developed by EMODnet Biology acts as an interactive WFS requests builder. The URL to the WFS request can be copied to the clipboard at the last step of the selection in the Download Toolbox.

*insert image*





1. **Occurrence data (species observations) as Web Feature Services (WFS)**

   - **Output formats**: The EMODnet Biology data is available in a number of [output formats](https://docs.geoserver.org/latest/en/user/services/wfs/outputformats.html), which are indicated in the WFS request as:

     ```
     outputFormat=<output_format>
     ```

      The following GetCapabilities request returns the complete list of output formats available for each type of WFS request (For example, see the GetFeatures request)

     https://geo.vliz.be/geoserver/Dataportal/ows?service=wfs&version=1.1.0&request=GetCapabilities

   - **Data formats**: There are three data formats presented by EMODnet Biology. The syntax for specifying a data format is: 

     ```
     typeName=Dataportal:<data_format>
     ```

     ​	The **Basic Occurrence Data** provides you with all data necessary  to do temporal spatial analysis of the different taxa. The syntax for requesting this data format is: `eurobis-obisenv_basic`

     ​	The **Full Occurrence Data** provides additional information which may help interpret the basic data. The syntax for requesting this data format is: `eurobis-obisenv_full`

     ​	The **Full Occurrence Data and Parameters** provides you also with all quantitative data and facts associated to the occurrence or the sample. The syntax for requesting this data format is: `eurobis-obisenv`

     You can find more information about the terms retrieved by each download type in the [documentation](https://www.emodnet-biology.eu/emodnet-data-format). 

   * **Building a query:** 
   * 

     - **Basic Occurrence Data**

       This request will return the first 50 basic occurrences from the dataset *Monitoring of birds in the Voordelta* ([4569](http://www.emodnet-biology.eu/data-catalog?module=dataset&dasid=4659)) as csv:

       [https://geo.vliz.be/geoserver/Dataportal/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=Dataportal:**eurobis-obisenv_basic**&&viewParams=where:**datasetid=4659**&maxFeatures=50&outputformat=**csv**](https://geo.vliz.be/geoserver/Dataportal/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=Dataportal:eurobis-obisenv_basic&&viewParams=where:datasetid=4659&maxFeatures=50&outputformat=csv)

       The different attributes of these occurrences on which you perform a selection can be retrieved using a `DescribeFeatureType` request. In the case of the Basic Occurrence Data this is:

       [https://geo.vliz.be/geoserver/Dataportal/ows?service=wfs&version=2.0.0&request=**DescribeFeatureType**&typeName=Dataportal:**eurobis-obisenv_basic**&outputFormat=application/json](http://geo.vliz.be/geoserver/Dataportal/ows?service=wfs&version=2.0.0&request=DescribeFeatureType&typeName=Dataportal:eurobis-obisenv_basic&outputFormat=application/json)

       In addition, it is possible to spatially query EMODnet Biology data through its interoperability with the [MarineRegions Gazetteer](https://marineregions.org/gazetteer.php?p=search). For example, the following request will return all specimens of the seabird Herring gull (*Larus argentatus*) in the Belgian Exclusive Economic Zone. It combines the AphiaID of the species ([137138](http://www.marinespecies.org/aphia.php?p=taxdetails&id=137138)) and the MRGID of the region ([3293](https://marineregions.org/gazetteer.php?p=details&id=3293)): 

       [https://geo.vliz.be/geoserver/wfs/ows?service=WFS&version=1.1.0&request=GetFeature&typeName=Dataportal%3Aeurobis-obisenv_basic&resultType=results&viewParams=where%3A%28%28up.geoobjectsids+%26%26+ARRAY%5B**3293**%5D%29%29%3Bcontext%3A0100%3Baphiaid%3A**137138**&propertyName=datasetid%2Cdatecollected%2Cdecimallatitude%2Cdecimallongitude%2Ccoordinateuncertaintyinmeters%2Cscientificname%2Caphiaid%2Cscientificnameaccepted&outputFormat=csv](https://geo.vliz.be/geoserver/wfs/ows?service=WFS&version=1.1.0&request=GetFeature&typeName=Dataportal%3Aeurobis-obisenv_basic&resultType=results&viewParams=where%3A%28%28up.geoobjectsids+%26%26+ARRAY%5B3293%5D%29%29%3Bcontext%3A0100%3Baphiaid%3A137138&propertyName=datasetid%2Cdatecollected%2Cdecimallatitude%2Cdecimallongitude%2Ccoordinateuncertaintyinmeters%2Cscientificname%2Caphiaid%2Cscientificnameaccepted&outputFormat=csv)

     - **Full Occurrence Data**

       Adding the following sentence in your GET request will return the full occurrence data: `&typeName=Dataportal:eurobis-obisenv_full`

       For example, you can retrieve a Shapefile with the full occurrence data of the seabird Herring gull (*Larus argentatus*) using its AphiaID [137138](http://www.marinespecies.org/aphia.php?p=taxdetails&id=137138) (this request might take a while to complete):

       [https://geo.vliz.be/geoserver/Dataportal/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=Dataportal:**eurobis-obisenv_full**&&viewParams=where:**aphiaidaccepted=137138**&outputformat=**SHAPE-ZIP**](https://geo.vliz.be/geoserver/Dataportal/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=Dataportal:eurobis-obisenv_full&&viewParams=where:aphiaidaccepted=137138&outputformat=SHAPE-ZIP)

       Querying for two or more conditions is also possible. See in the following example how to request the occurrences of the seabird Herring gull (*Larus argentatus* [137138](http://www.marinespecies.org/aphia.php?p=taxdetails&id=137138)) in the dataset *Monitoring of birds in the Voordelta* ([4569](http://www.emodnet-biology.eu/data-catalog?module=dataset&dasid=4659)) as JSON:

       [https://geo.vliz.be/geoserver/Dataportal/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=Dataportal:eurobis-obisenv_full&&viewParams=where:**aphiaidaccepted=137138 AND datasetid=4659**&outputformat=**application/json**](https://geo.vliz.be/geoserver/Dataportal/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=Dataportal:eurobis-obisenv_full&&viewParams=where:aphiaidaccepted=137138%20AND%20datasetid=4659&outputformat=application/json)

     - **Full Occurrence Data and Parameters**

       Adding the following sentence in your GET request will return the full occurrence data and parameters: `&typeName=Dataportal:eurobis-obisenv`
       
       For example, you can retrieve a CSV with the full occurrence data and parameters of both the White furrow (*Alba alba* [141433](http://www.marinespecies.org/aphia.php?p=taxdetails&id=141433)) and the Common razor shells (*Ensis ensis* [140733](http://www.marinespecies.org/aphia.php?p=taxdetails&id=140733)):
       
       [https://geo.vliz.be/geoserver/Dataportal/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=Dataportal:**eurobis-obisenv**&viewParams=where:**aphiaidaccepted=141433 OR aphiaidaccepted=140733**&outputformat=**csv**](https://geo.vliz.be/geoserver/Dataportal/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=Dataportal:eurobis-obisenv&viewParams=where:aphiaidaccepted=141433 OR aphiaidaccepted=140733&outputformat=csv)
       
       This request will return one row per parameter available in EMODnet Biology, instead of one row per occurrence as the basic and full occurrence data do. 

   - **Get Count**: In addition, you can retrieve the total number of records available in EMODnet Biology for a certain query using the sentence `Dataportal:eurobis-obisenv_count`. The example below will return the number of occurrences in the dataset *Monitoring of birds in the Voordelta* ([4569](http://www.emodnet-biology.eu/data-catalog?module=dataset&dasid=4659)):

     [https://geo.vliz.be/geoserver/Dataportal/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=Dataportal:**eurobis-obisenv_count**&&viewParams=where:**datasetid=4659**&outputformat=application/json](https://geo.vliz.be/geoserver/Dataportal/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=Dataportal:eurobis-obisenv_count&&viewParams=where:datasetid=4659&outputformat=application/json)

2. **Points and gridded abundance of occurrence data**

   - EMODnet Biology also offers occurrences as geospatial data. There are four grid size levels available, plus the possibility of retrieving each point directly. To query any of these five options, the correct table must be specified in the request:
     - One degree: `&typeName=Dataportal:eurobis_grid_1d-obisenv`
     
     - 30 minutes: `&typeName=Dataportal:eurobis_grid_30m-obisenv`
     
     - 15 minutes: `&typeName=Dataportal:eurobis_grid_15m-obisenv`
     
     - Six minutes:  `&typeName=Dataportal:eurobis_grid_6m-obisenv`
     
     - Points layer: `&typeName=Dataportal:eurobis_points-obisenv`
     

   For example, this WFS request will return the 30 minutes gridded abundance of the seabird Herring gull (*Larus argentatus* [137138](http://www.marinespecies.org/aphia.php?p=taxdetails&id=137138)) as KML

   [https://geo.vliz.be/geoserver/wfs/ows?service=WFS&version=1.3.0&request=GetFeature&typeName=Dataportal:**eurobis_grid_30m-obisenv**&viewParams=**aphiaid:137138**&outputFormat=**application/vnd.google-earth.kml+xml**](https://geo.vliz.be/geoserver/wfs/ows?service=WFS&version=1.3.0&request=GetFeature&typeName=Dataportal:eurobis_grid_30m-obisenv&viewParams=aphiaid:137138&outputFormat=application/vnd.google-earth.kml+xml)

3. **Using the AphiaID to query by (biological) taxonomy**

- Description how to retrieve the AphiaID i.e. Larus argentatus
   [http://www.marinespecies.org/rest/AphiaIDByName/Larus%20argentatus?marine_only=true](http://www.marinespecies.org/rest/AphiaIDByName/Larus argentatus?marine_only=true)
- General WoRMS webservice description
   http://www.marinespecies.org/aphia.php?p=webservice

4. **Using the MRGID to query by marine region**
- Retrieve the MRGID record of the Belgian Exclusive Economic Zone:
  https://marineregions.org/rest/getGazetteerRecordsByName.json/belgian%20exclusive%20economic%20zone/true/true/


- General Marine Regions webservice description

  https://marineregions.org/gazetteer.php?p=webservices
5. **Using the IMISDasID to query by metadata dataset**

- How to retrieve the datasetid (IMIS DasID) i.e. searching on "birds" & "Voordelta"
   http://www.emodnet-biology.eu/data-catalog?module=dataset&show=jsonportal&searchField=birds+Voordelta

- General IMIS (metadatacatalogue) webservice description
   http://www.emodnet-biology.eu/data-catalog?page=webservices

6. **Retrieve data of the gridded abundance data products as WFS/WMS**

- [OOPS Copepod gridded abundances 10-year bin](http://geo.vliz.be/geoserver/Emodnetbio/wms?service=WMS&version=1.1.0&request=GetMap&layers=Emodnetbio:OOPS_products&styles=&bbox=-4.95,48.05,12.25,60.75&width=512&height=378&srs=EPSG:4326&format=application/openlayers&viewparams=scientificName:Large copepods;season:1;AphiaID:1080;startYearCollection:1958;endYearCollection:1967)
- [OOPS Copepod gridded abundances 1-year bin](http://geo.vliz.be/geoserver/Emodnetbio/wms?service=WMS&version=1.1.0&request=GetMap&layers=Emodnetbio:OOPS_products&styles=&bbox=-4.95,48.05,12.25,60.75&width=512&height=378&srs=EPSG:4326&format=application/openlayers&viewparams=scientificName:Large copepods;season:1;AphiaID:1080;startYearCollection:1958;endYearCollection:1958)

7. **R client for EMODnet Biology webservices:**
- https://github.com/lifewatch/eurobis/
8. **Examples of EMODnet Biology Data appplications written in R are available at:**

- http://rshiny.emodnet-biology.eu/OOPS/
- http://rshiny.emodnet-biology.eu/SHARKshiny/
