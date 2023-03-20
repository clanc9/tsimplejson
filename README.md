These Talend components are designed for the easy manipulation of JSON strings.
More specifically, tSimpleReadJSON allows to populate a Talend schema from an input JSON string and
tSimpleWriteJSON performs the reverse operation, that is it generates a JSON string from a Talend schema.

These components map a Talend schema to the attributes at the root of a JSON string, or vice versa. As such, they present the following drawbacks:
- no handling of arrays
- unpredictable behavior for highly complex JSON strings


Installation



tSimpleReadJSON

Operation

scans the attributes at the root of a JSON string and assign its value to


Features
- possibility to exclude one or more columns of the schema from being 


from the reading process
- conversion of date format during processing, for instance an input column with format MM-dd-yyyy can be written
out in the JSON string as yyyy-MM-dd HH:mm:ss
- in the case of the tSimpleReadJSON, possibility to reject invalid input JSON strings to a secondary flow
- since the tSimpleWrite
does not depend on any external library to generate a JSON string, less chance of class conflicts that may arise
between different JSON parsers being used in the same job or during job migrations.
