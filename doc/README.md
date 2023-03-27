
# Talend components for easy writing and reading of JSON strings

These Talend components are designed for the easy manipulation of JSON strings. More specifically, tSimpleWriteJSON generates a JSON string from an input row and tSimpleReadJSON performs the reverse opration, that is it populates a row from an input JSON string.

These components map a Talend schema to the attributes at the root of a JSON string, or vice versa. Their main advantage is their ease of use. However, they present the following drawbacks:
- no handling of arrays
- unpredictable behavior for highly complex JSON strings

<br>

## tSimpleWriteJSON

*translates an input row into a single level JSON string*


### Features
- choice of which columns to include in the output JSON string
- possibility to enclose non-null values in quotes in the output JSON string
- decide whether to include null values in the output JSON string
- suppress \n, \r and \t characters from string values
- conversion of date format during processing, for instance an input column with format MM-dd-yyyy can be written
out in the JSON string as yyyy-MM-dd HH:mm:ss
- use comma as decimal separator for Doubles and Floats


### Configuring the component - Basic settings

![tsimpleRead_basic_settings](tsimpleread_basic_settings.jpg)


<br>

## tSimpleReadJSON

*scans the attributes at the root of an input JSON string and assigns their values to the corresponding columns of the output schema, if any*


### Features
- exclude one or more columns of the output schema from the parsing of the input JSON string
- reject  input rows to a secondary flow if an error occurs, for instance because the input JSON string is invalid or because the value of an attribute can not be converted to the type assigned to the output column      
- conversion of the date format during processing. For instance a date value with format MM-dd-yyyy in the JSON can populate a column with format yyyy-MM-dd HH:mm:ss
- choice of separator for decimal numbers found in the input JSON strings (period or comma)


### External libraries
- [jackson-core-2.13.5.jar](https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-core/2.13.5)
- [jackson-databind-2.13.5.jar](https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-core/2.13.5)
- [jackson-annotations-2.13.5.jar](https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-core/2.13.5)

The .jar are included in the component folder. The components have not been tested with other versions of the [Jackson library](https://github.com/FasterXML/jackson).   


### Configuring the component - Basic settings

![tsimpleRead_basic_settings](tsimpleread_basic_settings.jpg)

1. Add to the output **schema** the columns which are to be filled with values from the input JSON string. The names of these columns must be identical to the names of the attributes an the JSON.  

> Pay close attention to the data types of the columns that are to be extracted from the input JSON. For instance, if the chosen type is int|Integer, then the values in the JSON string must be either an integer or a string representation of an integer, otherwise an error will occur.

> There is no need to add columns for all of the attributes of the input JSON, only for the ones needed.


2. Select the **input JSON column** from the dropdown list.  
> The data type of this column must be String, otherwise an exception is thrown upon execution of the job.

3. Click the **Exclude input JSON column from parsing** option if you do not wish the component to change the value of this column.  This option is set by default.

4. If needed, select **columns to exclude from parsing**.  The value of these output columns is copied from the input columns if they exist, otherwise it is null.

5. Select one of 3 ways to treat the rows if an error occurs during processing : a. **assign null values** to output columns that do not already exist in the input row; b. reject    


> The most likely causes of error are an invalid
