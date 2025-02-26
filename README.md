# 1808 Sommarioni: Data description

# Files:
* `sommarioni_geometries_<date>.geojson`: the vectorization of Venice's 1808 Napolean cadastral maps saved as GeoJSON polygons. Relate to the text data entries through the `geometry_id` field.
* `sommarioni_text_data_<date>.json`: the transcribed textual entries of the cadaster's manuscript (called "sommarioni") with the field `geometry_id` relating the group of geometry from the geometries' geojson file. Parcels function (type of ownership and qualities) have been normalized.

## sommarioni_text_data: metadata fields
- `geometry_id`: a non-null unique integer linking the entry to the group of geometries in the corresponding geojson. 
- `district`: a non-null standardized string representing the sestiere (district) of the entry. Possible values are  `San Marco`, `Castello`, `Cannaregio`, `San Polo`, `Santa Croce` and `Dorsoduro`.
- `parcel_number`: a nullable string linking the entry to the coressponding cadaster on the map. The name in the original document is `Numero della Mappa`.
- `sub_parcel_number`: a nullable string used to indicate sub-division of the entry. The name in the original document is `subalterno`.
- `place`: a non-null string representing the toponym (place) locating the entry. This attribute is written in the original document above the entries in the `Denominazione dei Pezzi di Terra` column.
- `house_number`: nullable string of the physical house number of the entry, can be multiple numbers in a chain separated by commas. The name in the original document is `Denominazione dei Pezzi di Terra`.
- `owner`: nullable string denoting the owner of the entry. The family names of this field are transcribed in uppercase, except for the `Cannaregio` sestiere. The name in the original document is `Possessori`.
- `owner_standardised`: nullable string which is a standardised version of the above string.
- `area`: a non-null float value describing the parcel's area in square meters. Likely computed from the vectorization available in the GeoJSON file. Useful for plotting statistics. 
- `quality`: a nullable string of the quality of the entry. It describes the type of good from the entry in a more or less systematic way. The name in the original document is `Qualità`.
- `ownership_types`: a nullable **list** of standardized strings representing the types of ownership. The possible values are `AFFITO`, `PUBBLICO`, `COMMUNE` and `PROPRIO`. This standardized information stems from the `quality` field.
- `qualities`: a nullable **list** of standardized strings representing the possible qualities of a parcel. They are 72 possible values. This standardized information stems from the `quality` field.

## sommarioni_geometries: metadata fields
Only the fields stored in the "properties" sub-dict per geoJSON object are described, as they are domain dependant metadata, whereas all other fields are standard geojson properties.
- `id`: non-null string, operational ID attributed to each single geometry, in the format `<type>/<id>`. Multiple geometries may hold the same `id`, it generally means a single parcel with many different kind of geometries (building with a courtyard for instance)
- `parcel_type`: non-null standardized string, represent the type of the geometry, mainly for display purpose. Possibles values are `building`, `courtyard`, `sottoportico`, `street`, `water`.
- `parcel_number`: nullable string, refers to the parcel number associated to the current geometry. Matches with the corresponding value from the "sommarioni_text_data" JSON file. 
- `geometry_id`: non-null integer, an unique identifier grouping all the different geometries to the sommarioni entries they correspond. Matches the values from the corresponding field in the "sommarioni_text_data" and serve as the key to link both files. The value "-1" is used for all the geometries without entries in the register.
