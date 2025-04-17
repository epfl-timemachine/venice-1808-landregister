# 1808 Sommarioni: Data description

# Files:
* `venice_1808_landregister_geometries.geojson`: the vectorization of Venice's 1808 Napolean cadastral maps saved as GeoJSON polygons. Relate to the textual data entries through the `geometry_id` field.
* `venice_1808_landregister_textual_entries.json`: the transcribed textual entries of the cadaster's manuscript (called "Sommarioni") with the field `geometry_id` relating the group of geometry from the geometries' GeoJSON file. Some fields have been standardized such as the parcels function and the identification of their owner.
* `venice_1808_landregister_aggregated_data.jpon`: an aggregated version of both aforementioned file in a single JSON file.

## venice_1808_landregister_textual_entries:
- `geometry_id`: a non-null unique integer linking the entry to the group of geometries in the corresponding GeoJSON. 
- `district`: a non-null standardized string representing the "sestiere" (district) in which the entry belong. Possible values are  `San Marco`, `Castello`, `Cannaregio`, `San Polo`, `Santa Croce` and `Dorsoduro`.
- `parcel_number`: a nullable string linking the entry to the coressponding cadaster on the map. The name in the original document is `Numero della Mappa`.
- `sub_parcel_number`: a nullable string used to indicate sub-division of the entry. The name in the original document is `subalterno`.
- `place`: a non-null string representing the toponym (place) locating the entry. This attribute is written in the original document above the entries in the `Denominazione dei Pezzi di Terra` column.
- `house_number`: nullable string of the physical door number of the entry, can be multiple numbers in a chain separated by commas. The name in the original document is `Denominazione dei Pezzi di Terra`.
- `owner`: nullable string denoting the owner of the entry. The family names of this field are transcribed in uppercase, except for the `Cannaregio` sestiere. The name in the original document is `Possessori`.
- `owner_standardised`: nullable string which is a standardised version of the above string.
- `area`: a non-null float value describing the parcel's area in square meters. It is a value computed from the vectorization available in the GeoJSON file.
- `quality`: a nullable string of the quality of the entry. It describes the type of good from the entry in a more or less systematic way. The name in the original document is `Qualità`.
- `ownership_types`: a nullable **list** of standardized strings representing the types of ownership. The possible values are `AFFITO`, `PUBBLICO`, `COMMUNE` and `PROPRIO`. This standardized information stems from the `quality` field.
- `qualities`: a nullable **list** of standardized strings representing the possible qualities of a parcel. They are 72 possible values. This standardized information stems from the `quality` field.

## venice_1808_landregister_geometries:
Only the fields stored in the "properties" sub-dict per GeoJSON object are described, as they are domain dependant metadata, whereas all other fields are standard GeoJSON properties.
- `id`: non-null string, operational ID attributed to each single geometry, in the format `<type>/<id>`. Multiple geometries may hold the same `id`, it generally means a single parcel with many different kind of geometries (building with a courtyard for instance)
- `parcel_type`: non-null standardized string, represent the type of the geometry, mainly for display purpose. Possibles values are `building`, `courtyard`, `sottoportico`, `street`, `water`.
- `parcel_number`: nullable string, refers to the parcel number associated to the current geometry. Matches with the corresponding value from the "sommarioni_text_data" JSON file. 
- `geometry_id`: nullable integer, an unique identifier grouping all the different geometries to the sommarioni entries they correspond. Matches the values from the corresponding field in the "sommarioni_text_data" JSON file and serve as the key to link both files. Entries without a value for this fields are urban objects drawn on the map that were vectorized without a corresponding description in the registry (for instance, streets and waterways).


## venice_1808_landregiste_aggregated_data
Each of the entries listed in the file holds two fields:
- `geometries`: list all geometries that fall within the relation between the (potentially multiple) entries of the registries and the different vectorized parcel delimitations. The fields and contained data from the object in this listing are the same as the ones from `venice_1808_landregister_geometries` with the omission of `geometries_id` (used to do the join) and `parcel_number` (already present in the text entries).
- `text`: list all the entries from the landregister that are related to the geometries listed in the aforementioned file. As there exist vectorized geometries without textual entries (for instance street and body of water delimitation, or confidential properties, such as the Arsenal) this field can be empty. The fields and contained data from the object in this listing are the same as the ones from `venice_1808_landregister_textual_entries` with the omission of `geometries_id`.