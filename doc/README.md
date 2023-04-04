# Documentation - tSimpleJSON
# Talend components for easy writing and reading of JSON strings

These Talend components are designed for the easy manipulation of JSON strings. More specifically, tSimpleWriteJSON generates a JSON string from an input row and tSimpleReadJSON performs the reverse opration, that is it populates a row from an input JSON string.

These components map a Talend schema to the attributes at the root of a JSON string, or vice versa. Their main advantage is their ease of use. However, they present the following drawbacks:
- no handling of arrays
- unpredictable behavior for highly complex JSON strings

<br>
<hr>

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


### Configuration

<u>Basic settings</u>

The component has 2 modes of operation.
In the first one, all colums are included in the JSON.  This is the default mode.

![Screenshot of tSimpleWriteJSON Basic settings - Mode 1 - Include all columns](/doc/images/tsimplewritejson_basic_settings1.png)

1. Check **Include all columns in the JSON string** to use this mode.
2. Check **Convert non-null values to quoted strings** to generate a JSON string in which the value of every attribute is a string.
3. Check **Include null values** if needed.   
 *This option is set by default.*
4. Check **Convert null values to empty strings** if needed.  
 *Checking this option automatically implies that null values are included, irrespective of whether the box above is checked or not.**
5. Check **Remove \n, \r, \t from strings** to clean up strings.


In the second mode of operation, the user selects a subset of columns to include in the JSON string.

![Screenshot of tSimpleWriteJSON Basic settings - Mode 2 - Subset of columns](/doc/images/tsimplewritejson_basic_settings2.png)

<br>

### Global variable

NB_LINE : the number of rows read by the component.  This is an After variable and it returns an integer.

<br>

### Usage

The component is not startable and it requires an output component.

<br>
<hr>


## tSimpleReadJSON

*scans the attributes at the root of an input JSON string and assigns their values to the corresponding columns of the output schema, if any*


### Features
- exclude one or more columns of the output schema from the parsing of the input JSON string
- reject  input rows to a secondary flow if an error occurs, for instance because the input JSON string is invalid or because the value of a JSON attribute can not be converted to the type assigned to the output column      
- conversion of the date format during processing. For instance a date value with format MM-dd-yyyy in the JSON can populate a column with format yyyy-MM-dd HH:mm:ss
- choice of separator for decimal numbers found in the input JSON strings (period or comma)


### External libraries
- [jackson-core-2.13.5.jar](https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-core/2.13.5)
- [jackson-databind-2.13.5.jar](https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-core/2.13.5)
- [jackson-annotations-2.13.5.jar](https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-core/2.13.5)

The .jar are included in the component folder. The components have not been tested with other versions of the [Jackson library](https://github.com/FasterXML/jackson).   


### Configuration

Basic settings

![Screenshot of tSimpleReadJSON Basic settings](/doc/images/tsimplereadjson_basic_settings.png)

1. Add the columns which are to be filled with values from the input JSON string to the output **schema**. The names of these output columns must be identical to the names of the attributes in the JSON.  
 *Pay close attention to the data types of the columns that are to be extracted from the input JSON. For instance, if the chosen type is int|Integer, then the values in the JSON string must be either an integer or a string representation of an integer, otherwise an error will occur.*   
 *There is no need to add columns for all of the attributes of the input JSON, only for the ones needed.*


2. Select the **input JSON column** from the dropdown list.  
 *The data type of this column must be String, otherwise an exception is thrown upon execution of the job.*

3. Click the **Exclude input JSON column from parsing** option if you do not wish the component to change the value of this column.  
 *This option is set by default.*

4. If needed, select the **columns to exclude from parsing**. Use the + button to add a line and select the column from the dropdown list. Use the X button to remove a line.
 *The value of an output column that is excluded is copied from the corresponding input column if it exists, otherwise it is null.*

5. Select one of 3 ways to proceed if an error occurs during treatment of the input row:
   - **assign null values** to output columns that do not exist in the input row
   - redirect the input row to a **reject flow**
   - **die** (i.e. throw an exception). This will interrupt data flow.

 *The most likely causes of error are either an invalid input JSON string or a JSON value that can not be cast to the output column type.*

<br>

<u>Advanced settings</u>

![Screenshot of tSimpleReadJSON Advanced settings](/doc/images/tsimplereadjson_advanced_settings.png)


1. If the input JSON string contains date values which are to be extracted, specify the date format used in the JSON string. See the [Talend documentation](https://help.talend.com/r/en-US/8.0/data-preparation-user-guide/list-of-date-and-date-time-formats) for available date/time formats.   
 *Please note that the date format in the output column is specified in the Talend schema.  If this format is different than the one used in the input JSON string, then the component attempts to make the conversion.*

2. If needed, specify the decimal separator that is used in the input JSON string (period [default], comma, or both).   
 *This applies to columns of types Double, Float or Big Decimal.*

<br>

### Global variable

NB_LINE : the number of rows read by the component.  This is an After variable and it returns an integer.

NB_LINE_OK : the number of rows that had a valid JSON string that could be processed. This is an After variable and it returns an integer.

NB_LINE_REJECTED : the number of rows that had an invalid JSON string. This is an After variable and it returns an integer.

<br>

### Usage

The component is not startable and it requires an output component.

<br>

### Example 1 - Extracting first and last names from input JSON string



1. Drop a tFixedFlowInput with schema ```id``` (integer) and ```json``` (string).
2. Check ```Use Inline Table```.
3. Specify 2 input rows.  
    ```
    id = 1, json = "{\"id\": 11, \"firstName\":\"Michel\", \"lastName\":\"Tremblay\"}"
    id = 2, json = "{"
    ```
    *The second row intentionally contains an invalid JSON string.*

4. Drop a tSimpleReadJSON and connect to the tFixedFlowInput.  
5. Add a ```firstName``` (string) and a ```lastName``` (string) columns to the output schema.
6. Select ```json``` as the input JSON column.
7. Exclude the ```id``` column from parsing.
8. Check the ```Redirect to Reject flow``` box for invalid input JSON
9. Drop 2 tLogRow and connect one to the JSON flow of the tSimpleReadJSON and the other to the Reject flow of the tSimpleReadJSON.
10. Run the job.
