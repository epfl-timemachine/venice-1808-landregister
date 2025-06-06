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
- `owner_transcription`: nullable string denoting the owner of the entry. The name in the original document is `Possessori`.
- `owner_standardised`: nullable string which is a standardised version of the `owner_transcription`.
- `owner_standardized_class`: nullable string representing the class of institution present in the owner in the case the current entity is an institution.
- `owner_wd`: nullable string, holds the entity code of the owner whenever a matching record was found on wikidata.org.
- `owner_right_of_use`: nullable string, represents the right of use attributed to the owner of the current parcel.
- `owner_type`: nullable string, represents the type of owner according to 3 possible values: `RELIGIOUS`, `SECULAR`, `ISTITUZIONE PUBBLICA`. An english-translated version of this field is also available under the name `owner_type_en`. 
- `old_entity`: nullable string, whenever in the `owner` field there was some forme of transfer of ownership, this field holds information regarding the current parcel previous owner.
- `old_entity_standardised`:  nullable string which is a standardised version of the `old_entity` field.
- `old_entity_standardized_class`: nullable string, similar to `owner_standardized_class` but in the case of the previous owner.
- `old_entity_wd`: nullable string, similar to `owner_wd` but in the case of the previous owner.
- `old_religious_entity_type`: nullable string, very often the previous owner of parcels happens to be a religious type, this field represent the different type of such religious institutions. An english-translated version of this field is also available under the name `old_religious_entity_type_en`. 
- `owner_supplementary`: nullable string, holds supplementary information regarding ownership of the current parcel.
- `old_owner_right_of_use`: nullable string, similar to `owner_right_of_use` but in the case of the previous owner. An english-translated version of this field is also available under the name `old_owner_right_of_use_en`. 
- `old_owner_type`: nullable string, similar to `owner_type` but in the case of the previous owner. 
- `quality`: a nullable string of the quality of the entry. It describes the type of good from the entry in a more or less systematic way. The name in the original document is `Qualità`. 
- `page`: a non-null string containing the reference to the archival document page from which the transcription of the current entry has been made.
- `ownership_types`: a nullable **list** of standardized strings representing the types of ownership. The possible values are `AFFITTO`, `PUBBLICO`, `COMUNE` and `PROPRIO`. This standardized information stems from the `quality` field. An english-translated version of this field is also available under the name `ownership_types_en`
- `qualities`: a nullable **list** of standardized strings representing the possible qualities of a parcel. This standardized information stems from the `quality` field. An english-translated version of this field is also available under the name `qualities_en`

## venice_1808_landregister_geometries:
Only the fields stored in the "properties" sub-dict per GeoJSON object are described, as they are domain dependant metadata, whereas all other fields are standard GeoJSON properties.
- `id`: non-null string, operational ID attributed to each single geometry, in the format `<type>/<id>`. Multiple geometries may hold the same `id`, it generally means a single parcel with many different kind of geometries (building with a courtyard for instance)
- `parcel_type`: non-null standardized string, represent the type of the geometry, mainly for display purpose. Possibles values are `building`, `courtyard`, `sottoportico`, `street`, `water`.
- `parcel_number`: nullable string, refers to the parcel number associated to the current geometry. Matches with the corresponding value from the "sommarioni_text_data" JSON file. 
- `area`: a non-null float value describing the parcel's area in square meters.
- `parish_standardised`: nullable string, refers to the adminstrative delimitation of the parish in which the geometries likely falls within. This is a derived data from an interpretation of the parish boundaries as they were thought to be organized in 1740. The appartenance of parcel was computed by checking under which parish boundaries most of a parcel's geometry fell under.
- `geometry_id`: nullable integer, an unique identifier grouping all the different geometries to the sommarioni entries they correspond. Matches the values from the corresponding field in the "sommarioni_text_data" JSON file and serve as the key to link both files. Entries without a value for this fields are urban objects drawn on the map that were vectorized without a corresponding description in the registry (for instance, streets and waterways).


## venice_1808_landregiste_aggregated_data
Each of the entries listed in the file holds two fields:
- `geometries`: list all geometries that fall within the relation between the (potentially multiple) entries of the registries and the different vectorized parcel delimitations. The fields and contained data from the object in this listing are the same as the ones from `venice_1808_landregister_geometries` with the omission of `geometries_id` (used to do the join) and `parcel_number` (already present in the text entries).
- `text`: list all the entries from the landregister that are related to the geometries listed in the aforementioned file. As there exist vectorized geometries without textual entries (for instance street and body of water delimitation, or confidential properties, such as the Arsenal) this field can be empty. The fields and contained data from the object in this listing are the same as the ones from `venice_1808_landregister_textual_entries` with the omission of `geometries_id`.

## scripts
A folder that holds a set of script that has been used to assist the cleaning, standardisation and preparation of the final version of the data. As the process was heavily intertwined with manual operations and intermediate working files, the notebooks within the folder are merely included in this repository to showcase the kind of data processes that was done, and as such, are not designed to be run sequencially in order to generate the current version of the data.
