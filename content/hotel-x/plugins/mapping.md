+++
title = "Mapping"
pagetitle = "HotelX Mapping"
description = "Map different codes from different suppliers in order to get a de-duped response."
icon = "fa-sitemap"
weight = 2
alwaysopen = false
+++

The map plugins are used to change the supplier codes to client codes or vice versa. There are four types:

* Hotel map 
* Board Map 
* Room Map 
* Rate Map 

Our map formats share a common structure. In order to load your maps you just need to follow the instructions below:

## Example files
You can download example for the files structure [here](/content/sample_mapping.zip)


## Entity Maps

### File Format

The file should be in the following format:

* **Encoding**: UTF-8

* **File Name**: [Context Source]\_[Context Destination]\_[entity]\_map.csv

  * Context Source: it corresponds to the client code

  * Context Destination: it corresponds to the supplier code

    * 1 file for each supplier

* **Header Row**: Code Source, Code Destination

  * Context Source: it corresponds to the client codes

  * Context Destination: it corresponds to the supplier codes    

* **Delimiter**: Comma (",")

* **Directory**: /F[folder code]\_[unique code]/HotelX\_[unique code]/Maps/[entity]/

### File Names

All map files must have the same name structure as follows - you need create a file for *Context Destionation*

|Entity|File Name|
|---|----|
|Hotel|[Context Source]\_[Context Destination]\_hotel\_map.csv|
|Board|[Context Source]\_[Context Destination]\_board\_map.csv|
|Room|[Context Source]\_[Context Destination]\_room\_map.csv|
|Rate|[Context Source]\_[Context Destination]\_rate\_map.csv|

### Directories

|Entity|File Name|
|---|---|
|Hotel|/F[folder code]\_[unique code]/HotelX\_[unique code]/Maps/Hotel/|
|Board|/F[folder code]\_[unique code]/HotelX\_[unique code]/Maps/Board/|
|Room|/F[folder code]\_[unique code]/HotelX\_[unique code]/Maps/Room/|
|Rate|/F[folder code]\_[unique code]/HotelX\_[unique code]/Maps/Rate/|

### Sample Files

Let's suppose we have the following client code and supplier code, then we need to create one file for each supplier we have
* Client code: GUE

* Supplier Code: BVJ

**Name**: GUE\_BVJ\_hotel\_map.csv


```csv
Code Source, Code Destination
10,c11\#10
10000,7604
10000,1274249
```

## Plugin Name

|Entity Map|Plugin Name|
|---|---|
|Hotel|HotelMapX|
|Board|BoardMapX|
|Room|RoomMapX|
|Rate|RateMapX|

## Other Maps

### Other Maps in Booking API

#### Amenity mapping

This plugin allows to convert the amenity codes from supplier context to a desired context. 

Our map formats share a common structure. In order to load your maps you just need to follow the instructions below:

##### File Format

The file should be in the following format:

* **Encoding**: UTF-8

* **File Name**: [Context Source]\_[Context Destination]\_amenity\_map.csv

  * Context Source: it corresponds to the code in desired context
  
  * Context Destination: it corresponds to the supplier context 

    * Only one file for each supplier should be uploaded

* **Header Row**:  Code Destination, Code Source

  * Context Destination: it corresponds to the codes in desired context

  * Context Source: it corresponds to the supplier codes    

* **Delimiter**: Comma (",")

* **Directory**: /F[folder code]\_[unique code]/HotelX\_[unique code]/Maps/Amenity/

##### File Names

The map files must have the same name structure as follows - you need create one file for any supplier context.

[Context Source]\_[Context Destination]\_amenity\_map.csv|


##### Sample Files

Let's suppose we have the following client code and supplier code, then we need to create one file for each supplier we have
* Client code: EXG

* Supplier Code: GTRM2

**Name**: EXG\_GTRM2\_amenity\_map.csv


```csv
Code Destination,Code Source
FreeParking,free_parking
FreeInternet,free_internet
```

#### Map by provider hotel

This plugin allows to convert the room codes in supplier context but by hotel. Is the same plugin (room map) explained before but it offers the possibility to map by supplier and hotel. 

##### Format File

The file must be in the below format:

* **Encoding**: UTF-8 
* **File Name**: [Context Source]\_[Context Destination]\_room\_map.csv
* **Header Row**: Code Source,Code Destination
* **Directory**: /F[folder code]\_[unique code]/HotelX\_[unique code]/Maps/

If you are using a file of room map, is necessary that you modify this file adding a new column. Please, see the next example down:

##### Sample File

**Name**: xtg_provider_room_map.csv
**Data**:

``` csv
Code Source,Code Destination,Code Hotel
1,X,A
1,Y,A
1,Z,A
2,X1,B
2,X2,C
3,X3,D
4,X4,
5,X5,
```

How you can see, in the same file are combined maps with 3 values and maps with 2 values. The rows with two values are mapped by provider. The files with three values are mapped by provider and hotel. Is possible to use only the mapping by provider hotel, in this case, your file only has rows with three values.

### How applies

What happens if you use the combined plugin (room map and room map by provider hotel)? In this case, all the rooms with provider hotel map will be mapped to your context (the context put in the first value of file's name (client context)) and the room codes that don't have provider hotel map, will be mapped with provider map code (in case that exist). If no map codes are found, the option can be discarded (this rule is configurable). 

### Execution example

```json
{
    "plugins": {
        "step": "RESPONSE_OPTION",
        "pluginsType": [
            {
                "type": "ROOM_MAP",
                "name": "room_map",
                "parameters":[{"key":"hotel", "value":"true"}]
            }
        ]
    }
}
```

Besides, an alternative for room map is also shown below:

## Modifying data

**Once mapping files are loaded, we can perform the following operations on them:**

### Updating data 
We have two options:

1. Reprocessing the same data by renaming the file and just removing "_processed": **Example example_processed.csv --> example.csv**
2. Changing the data by deleting the processed file and uploading a new one with new information.
   
### Deleting data
Uploading a new file only with headers (no information).
