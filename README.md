# EMODnet Biology Web service documentation

## EMODnet Biology Data

The EMODnet Biology data are available as a [Web Feature Service (WFS)](https://docs.geoserver.org/latest/en/user/services/wfs/index.html) in accordance with the [Open Geospatial Consortium (OGC)](https://www.ogc.org/) specifications. This webservice supports requests for geographical feature data (with vector geometry and attributes). The base link for performing a WFS request to EMODnet Biology is:

[https://geo.vliz.be/geoserver/Dataportal/ows?](https://geo.vliz.be/geoserver/Dataportal/ows?)

Essentially, the [Download Toolbox](https://www.emodnet-biology.eu/toolbox](https://www.eurobis.org/toolbox/en/download/occurrence/explore) developed by EMODnet Biology acts as an interactive builder for WFS requests . The URL to the WFS request can be copied to the clipboard at the last step of the selection in the Download Toolbox.

![](images/toolbox_screenshot.png)

Technical details of the WFS can be retrieved by following GetCapabilities request:

https://geo.vliz.be/geoserver/Dataportal/ows?service=wfs&version=1.1.0&request=GetCapabilities

---


### Building a WFS query to get data:

For composing your own query, you need to combine different parts:

1.  the base url
1.  the data format
1.  filtering options (optional)
1.  the output format

#### 1. The base url

The base link for performing a WFS request to EMODnet Biology is:
```
https://geo.vliz.be/geoserver/Dataportal/ows?service=WFS&version=1.0.0&request=GetFeature&
```

#### 2. The data format: 

There are [three data formats](https://emodnet.ec.europa.eu/en/biology#biology-data-and-products-format) presented by EMODnet Biology.

* Basic Occurrence Data
* Full Occurrence Data
* Full Occurrence Data and Parameters

The syntax for specifying a data format is:

```
typeName=Dataportal:<data_format>
```
  - **Basic Occurrence Data**
  
    The **Basic Occurrence Data** provides you with all data required to perform temporal spatial analysis for the different taxa. It indicates which taxa was found (`scientificname` and `aphiaid`), when (`datecollected`) and where (`decimallongitude` and `decimallatitude` in WGS84 - EPGS:4326), along with a dataset identifier (`datasetid`).
    
    The syntax for requesting this data format is: `eurobis-obisenv_basic`

    For example, the following request will return the first 50 basic occurrence records: 

    [https://geo.vliz.be/geoserver/Dataportal/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=Dataportal:**eurobis-obisenv_basic**&**maxFeatures=50**&outputformat=csv](https://geo.vliz.be/geoserver/Dataportal/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=Dataportal:eurobis-obisenv_basic&maxFeatures=50&outputformat=csv)

  - **Full Occurrence Data**
  
    The **Full Occurrence Data** provides additional information which may help interpret the basic data. This format offers all the data from the Basic Occurrences plus additiontal information which may help interpret the basic data such as information on the institute collecting the data, the methodology, the exact time and location (and uncertainty),...
    
    The syntax for requesting this data format is: `eurobis-obisenv_full`
    
    For example, the following request will return the first 50 full occurrence records: 
    [https://geo.vliz.be/geoserver/Dataportal/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=Dataportal:**eurobis-obisenv_full**&**maxFeatures=50**&outputformat=csv](https://geo.vliz.be/geoserver/Dataportal/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=Dataportal:eurobis-obisenv_full&maxFeatures=50&outputformat=csv)

  - **Full Occurrence Data and Parameters**

    The **Full Occurrence Data and Parameters** provides you with all measurement or facts associated to the occurrence or the sample.
    
    The syntax for requesting this data format is: `eurobis-obisenv`
    
    For example, the following request will return the first 50 full occurrence data and parameter records: 

    [https://geo.vliz.be/geoserver/Dataportal/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=Dataportal:**eurobis-obisenv**&**maxFeatures=50**&outputformat=csv](https://geo.vliz.be/geoserver/Dataportal/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=Dataportal:eurobis-obisenv&maxFeatures=50&outputformat=csv)

You can find more information about the terms returned by each download type in the [documentation](https://emodnet.ec.europa.eu/en/biology#biology-data-and-products-format). 


#### 3. Filtering options: 

In this part you can filter the result by several parameters.
We highlight three options:

* Taxonomy
* Geography 
* Dataset

These filter options are linked to other services that you can find at the end of this page.

  - **Taxonomy: World Record of Marine Species (WoRMS)**

    Concerning taxonomy, EMODnet Biology data are connected to the services provided by the [World Record of Marine Species (WoRMS)](https://www.marinespecies.org/) using the [AphiaID](https://www.marinespecies.org/about.php#what_is_aphia). This is provided in the `aphiaid` and `aphiaidaccepted` columns. 

    To retrieve a Shapefile with the full occurrence data of the seabird Herring gull (*Larus argentatus*) using its AphiaID [137138](http://www.marinespecies.org/aphia.php?p=taxdetails&id=137138), you can run the following request (this might take a while to complete):
    [https://geo.vliz.be/geoserver/Dataportal/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=Dataportal:eurobis-obisenv_full&&viewParams=where:**aphiaidaccepted=137138**&outputformat=csv](https://geo.vliz.be/geoserver/Dataportal/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=Dataportal:eurobis-obisenv_full&&viewParams=where:aphiaidaccepted=137138&outputformat=csv)


  - **Geography: Marine Regions**

    EMODnet Biology allows to query information to standardized areas by using the [MRGID](https://marineregions.org/mrgid.php) provided by [MarineRegions.org](https://marineregions.org/). This is a unique and persistent identifier for geographic objects. You can select the area of interest using the [Download Toolbox](https://www.emodnet-biology.eu/toolbox](https://www.eurobis.org/toolbox/en/download/occurrence/explore) and copying the WFS request generated. 

    For example, the following request returns all occurrences in the Belgian Exclusive Economic Zone (MRGID [3293](https://marineregions.org/gazetteer.php?p=details&id=3293)): (this might take a while)

    [https://geo.vliz.be/geoserver/wfs/ows?service=WFS&version=1.1.0&request=GetFeature&typeName=Dataportal:eurobis-obisenv_basic&resultType=results&viewParams=where:((up.geoobjectsids+&&+ARRAY[**3293**]));context:0100;&outputFormat=csv](https://geo.vliz.be/geoserver/wfs/ows?service=WFS&version=1.1.0&request=GetFeature&typeName=Dataportal%3Aeurobis-obisenv_basic&resultType=results&viewParams=where%3A%28%28up.geoobjectsids+%26%26+ARRAY%5B3293%5D%29%29%3Bcontext%3A0100%3B&outputFormat=csv)

    Currenlty you can subset on EEZ, IHO Sea Area, Marine Ecoregion of the World (MEOW), Marine Region, Territorial Sea.


  - **Dataset (Integrated Marine Information System - IMIS)**

    Filtering on datasets is possible thanks to the connection to the [Integrated Marine Information System (IMIS)](https://www.vliz.be/en/imis?module=dataset) described via this [link](https://emodnet.ec.europa.eu/en/emodnet-web-service-documentation).

    This request will return the first 50 basic occurrences from the dataset *Monitoring of birds in the Voordelta* ([datasetid=4569](https://www.eurobis.org/imis?module=dataset&dasid=4659)) as a csv file:

    [https://geo.vliz.be/geoserver/Dataportal/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=Dataportal:eurobis-obisenv_basic&&viewParams=where:**datasetid=4659**&maxFeatures=50&outputformat=*csv](https://geo.vliz.be/geoserver/Dataportal/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=Dataportal:eurobis-obisenv_basic&&viewParams=where:datasetid=4659&maxFeatures=50&outputformat=csv)


  - **Combining filter parameters**
  
    Here are some examples of combining filters:
    
    Querying for two or more conditions is also possible as seen below when requesting the occurrences of the seabird Herring gull (*Larus argentatus* [137138](http://www.marinespecies.org/aphia.php?p=taxdetails&id=137138)) in the dataset titled *Monitoring of birds in the Voordelta* ([datasetid=4569](https://www.eurobis.org/imis?module=dataset&dasid=4659)) as JSON:

    [https://geo.vliz.be/geoserver/Dataportal/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=Dataportal:eurobis-obisenv_full&&viewParams=where:**aphiaidaccepted=137138 AND datasetid=4659**&outputformat=application/json](https://geo.vliz.be/geoserver/Dataportal/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=Dataportal:eurobis-obisenv_full&&viewParams=where:aphiaidaccepted=137138%20AND%20datasetid=4659&outputformat=application/json)
    
    The WFS request allows the use of `OR` statements. You can retrieve a CSV file with the full occurrence data and parameters of both the White furrow (*Alba alba* [141433](http://www.marinespecies.org/aphia.php?p=taxdetails&id=141433)) and the Common razor shells (*Ensis ensis* [140733](http://www.marinespecies.org/aphia.php?p=taxdetails&id=140733)):
  
    [https://geo.vliz.be/geoserver/Dataportal/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=Dataportal:**eurobis-obisenv**&viewParams=where:**aphiaidaccepted=141433 OR aphiaidaccepted=140733**&outputformat=csv](https://geo.vliz.be/geoserver/Dataportal/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=Dataportal:eurobis-obisenv&viewParams=where:aphiaidaccepted=141433 OR aphiaidaccepted=140733&outputformat=csv)


#### 4. The output format: 

EMODnet Biology data are available in a number of [output formats](https://docs.geoserver.org/latest/en/user/services/wfs/outputformats.html), which are indicated at the end of the WFS request as:

```
  outputFormat=<output_format>
```

  - **csv**

    [https://geo.vliz.be/geoserver/Dataportal/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=Dataportal:eurobis-obisenv_basic&maxFeatures=50&outputformat=**csv**](https://geo.vliz.be/geoserver/Dataportal/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=Dataportal:eurobis-obisenv_basic&maxFeatures=50&outputformat=csv)
    
  - **json**
  
    [https://geo.vliz.be/geoserver/Dataportal/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=Dataportal:eurobis-obisenv_basic&maxFeatures=50&outputformat=**application/json**](https://geo.vliz.be/geoserver/Dataportal/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=Dataportal:eurobis-obisenv_basic&maxFeatures=50&outputformat=application/json)
  
 - **shapefile**
  
    [https://geo.vliz.be/geoserver/Dataportal/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=Dataportal:eurobis-obisenv_basic&maxFeatures=50&outputformat=**SHAPE-ZIP**](https://geo.vliz.be/geoserver/Dataportal/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=Dataportal:eurobis-obisenv_basic&maxFeatures=50&outputformat=SHAPE-ZIP)

 - **kml**
  
    [https://geo.vliz.be/geoserver/Dataportal/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=Dataportal:eurobis-obisenv_basic&maxFeatures=50&outputformat=**application/vnd.google-earth.kml+xml**](https://geo.vliz.be/geoserver/Dataportal/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=Dataportal:eurobis-obisenv_basic&maxFeatures=50&outputformat=application/vnd.google-earth.kml+xml)

  
    For other output formats, see the GetCapabilities request which returns the complete list of output formats available for each type of WFS request: https://geo.vliz.be/geoserver/Dataportal/ows?service=wfs&version=1.1.0&request=GetCapabilities


---

## EMODnet Biology summary and data product services


In addition to the three data formats, EMODnet Biology also makes available through its webservices the possibility of returning the total **occurrences count** for a certain query, its occurrence data as **gridded abundances** in a GIS Layers, and the [**data products**](https://emodnet.ec.europa.eu/geonetwork/srv/eng/catalog.search#/search?facet.q=sourceCatalog%2F17310db5-a423-4a81-b4d6-946d5a53696e&resultType=details&sortBy=sortDate&fast=index&_content_type=json&from=1&to=20) developed by the EMODnet Biology community:

  - **Retrieve the count of occurrences**: You can retrieve the total number of records available in EMODnet Biology for a certain query using the sentence `Dataportal:eurobis-obisenv_count`. The example below will return the number of occurrences in the dataset *Monitoring of birds in the Voordelta* ([datasetid=4569](https://www.eurobis.org/imis?module=dataset&dasid=4659)):
  
    [https://geo.vliz.be/geoserver/Dataportal/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=Dataportal:**eurobis-obisenv_count**&&viewParams=where:**datasetid=4659**&outputformat=application/json](https://geo.vliz.be/geoserver/Dataportal/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=Dataportal:eurobis-obisenv_count&&viewParams=where:datasetid=4659&outputformat=application/json)

- **Retrieve a geospatial grid, summarising the number of eurobis occurrences:** EMODnet Biology also offers occurrences summarised as the number of occurrences in a geospatial grid. There are four grid size levels available, plus the possibility of retrieving each point directly. To query any of these five options, the correct table must be specified in the request:

  - One degree: `&typeName=Dataportal:eurobis_grid_1d-obisenv`

  - 30 minutes: `&typeName=Dataportal:eurobis_grid_30m-obisenv`

  - 15 minutes: `&typeName=Dataportal:eurobis_grid_15m-obisenv`

  - Six minutes:  `&typeName=Dataportal:eurobis_grid_6m-obisenv`

  - Points layer: `&typeName=Dataportal:eurobis_points-obisenv`

  For example, this WFS request will return the 30 minutes gridded abundance of the seabird Herring gull (*Larus argentatus* [137138](http://www.marinespecies.org/aphia.php?p=taxdetails&id=137138)) as KML:

  [https://geo.vliz.be/geoserver/wfs/ows?service=WFS&version=1.3.0&request=GetFeature&typeName=Dataportal:**eurobis_grid_30m-obisenv**&viewParams=**aphiaid:137138**&outputFormat=**application/vnd.google-earth.kml+xml**](https://geo.vliz.be/geoserver/wfs/ows?service=WFS&version=1.3.0&request=GetFeature&typeName=Dataportal:eurobis_grid_30m-obisenv&viewParams=aphiaid:137138&outputFormat=application/vnd.google-earth.kml+xml)

- **Retrieving data products**: EMODnet Biology develops data products that are accessible through WFS services. For example, the following requests return the [OOPS Copepod gridded abundances](https://www.eurobis.org/imis?module=dataset&dasid=5438) in 1 and 10 years bin respectively:

    - [OOPS Copepod gridded abundances 10-year bin](http://geo.vliz.be/geoserver/Emodnetbio/wms?service=WMS&version=1.1.0&request=GetMap&layers=Emodnetbio:OOPS_products&styles=&bbox=-4.95,48.05,12.25,60.75&width=512&height=378&srs=EPSG:4326&format=application/openlayers&viewparams=scientificName:Large copepods;season:1;AphiaID:1080;startYearCollection:1958;endYearCollection:1967)
    - [OOPS Copepod gridded abundances 1-year bin](http://geo.vliz.be/geoserver/Emodnetbio/wms?service=WMS&version=1.1.0&request=GetMap&layers=Emodnetbio:OOPS_products&styles=&bbox=-4.95,48.05,12.25,60.75&width=512&height=378&srs=EPSG:4326&format=application/openlayers&viewparams=scientificName:Large copepods;season:1;AphiaID:1080;startYearCollection:1958;endYearCollection:1958)


---

## Other marine biogeographical data systems

EMODnet Biology follows the [FAIR principles](https://www.go-fair.org/fair-principles/). The data formats available via EMODnet Biology allow for enhanced interoperability by providing links to controlled vocabularies, like the [World Record of Marine Species (WoRMS)](https://www.marinespecies.org/), [MarineRegions.org](https://marineregions.org/) and the [British Oceanographic Data Centre](https://www.bodc.ac.uk). It also has links to the [Integrated Marine Information System (IMIS)](https://www.vliz.be/en/imis?module=dataset), a system developed by the [Flanders Marine Institute (VLIZ)](https://www.vliz.be/) where information about people, organisations, publications and more is stored and managed. 

Please find below more information about the webservices offered by these projects.

* **Taxonomy: World Record of Marine Species (WoRMS)**

  Concerning taxonomy, EMODnet Biology data are connected to the services provided by the [World Record of Marine Species (WoRMS)](https://www.marinespecies.org/) using the [AphiaID](https://www.marinespecies.org/about.php#what_is_aphia). This is provided in the `aphiaid` and `aphiaidaccepted` columns. WoRMS can provide detailed descriptions of marine species. For example, this request returns the distribution of the Herring Gull (*Larus argentatus* [137138](http://www.marinespecies.org/aphia.php?p=taxdetails&id=137138)). Notice how the distribution areas are linked to Marine Regions by providing the MRGID of the geographical area where this species has been observed. 

  [https://www.marinespecies.org/rest/**AphiaDistributionsByAphiaID**/**137138**](https://www.marinespecies.org/rest/AphiaDistributionsByAphiaID/137138)

  A general description of all the webservices offered by WoRMS can be found in [this link](http://www.marinespecies.org/aphia.php?p=webservice):

* **Geographic: Marine Regions**

  EMODnet Biology allows to query information to standardized areas by using the [MRGID](https://marineregions.org/mrgid.php) provided by [MarineRegions.org](https://marineregions.org/). This is a unique and persistent identifier for geographic objects. You can select the area of interest using the [Download Toolbox](https://www.eurobis.org/toolbox/en/download/occurrence/explore) and copying the WFS request generated. 

  For example, the following request returns all Herring gull (*Larus argentatus* [137138](http://www.marinespecies.org/aphia.php?p=taxdetails&id=137138)) seabird occurrences in the Belgian Exclusive Economic Zone (MRGID [3293](https://marineregions.org/gazetteer.php?p=details&id=3293)):

  [https://geo.vliz.be/geoserver/wfs/ows?service=WFS&version=1.1.0&request=GetFeature&typeName=Dataportal%3Aeurobis-obisenv_basic&resultType=results&viewParams=where%3A%28%28up.geoobjectsids+%26%26+ARRAY%5B**3293**%5D%29%29%3Bcontext%3A0100%3Baphiaid%3A**137138**&propertyName=datasetid%2Cdatecollected%2Cdecimallatitude%2Cdecimallongitude%2Ccoordinateuncertaintyinmeters%2Cscientificname%2Caphiaid%2Cscientificnameaccepted&outputFormat=csv](https://geo.vliz.be/geoserver/wfs/ows?service=WFS&version=1.1.0&request=GetFeature&typeName=Dataportal%3Aeurobis-obisenv_basic&resultType=results&viewParams=where%3A%28%28up.geoobjectsids+%26%26+ARRAY%5B3293%5D%29%29%3Bcontext%3A0100%3Baphiaid%3A137138&propertyName=datasetid%2Cdatecollected%2Cdecimallatitude%2Cdecimallongitude%2Ccoordinateuncertaintyinmeters%2Cscientificname%2Caphiaid%2Cscientificnameaccepted&outputFormat=csv)

  If you want to obtain information on the Belgian Exclusive Economic Zone (MRGID [3293](https://marineregions.org/gazetteer.php?p=details&id=3293)) from the Marine Regions Gazetteer you can use the WFS request below:

  [https://marineregions.org/rest/**getGazetteerRecordByMRGID**.json/**3293**/](https://marineregions.org/rest/getGazetteerRecordByMRGID.json/3293/)

  If you wish to retrieve the geometry associated to this Gazetteer record, you can use the WFS offered by Marine Regions. The following request makes use of a [CQL filter](https://docs.geoserver.org/latest/en/user/tutorials/cql/cql_tutorial.html) to download a Zip file with the ESRI Shapefile of the Belgian Exclusive Economic Zone (MRGID [3293](https://marineregions.org/gazetteer.php?p=details&id=3293)):

  [https://geo.vliz.be/geoserver/**MarineRegions**/wfs?service=WFS&version=1.0.0&request=GetFeature&typeNames=eez&cql_filter=**mrgid=3293**&outputFormat=**SHAPE-ZIP**](https://geo.vliz.be/geoserver/MarineRegions/wfs?service=WFS&version=1.0.0&request=GetFeature&typeNames=eez&cql_filter=mrgid=3293&outputFormat=SHAPE-ZIP)

  Following the links you can find the complete description of the [Gazetteer ](https://marineregions.org/gazetteer.php?p=webservices) and [OGS services](https://marineregions.org/webservices.php) offered by Marine Regions

* **Metadata: Integrated Marine Information System (IMIS)**

  Retrieving metadata about a dataset is possible thanks to the connection to the [Integrated Marine Information System (IMIS)](https://www.vliz.be/en/imis?module=dataset) described via this [link](https://emodnet.ec.europa.eu/en/emodnet-web-service-documentation).

  The example below returns metadata about the dataset *Monitoring of birds in the Voordelta* ([4569](https://www.eurobis.org/imis?module=dataset&dasid=4659)) in the JSON file format:

  [https://www.emodnet-biology.eu/data-catalog?module=dataset&**dasid=4659**&**show=json**](https://www.eurobis.org/imis?module=dataset&dasid==4659&show=json)

---

## R client for EMODnet Biology webservices:

EMODnet Biology data are accessible via an R package `eurobis`, which provides webservice wrapper. You can find more information on the [GitHub repository](https://github.com/lifewatch/eurobis/):

Some data applications using EMODnet Biology data has been written in R using the shiny framework: These are available at:

- http://rshiny.emodnet-biology.eu/OOPS/
- http://rshiny.emodnet-biology.eu/SHARKshiny/
