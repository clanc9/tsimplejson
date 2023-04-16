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

<br>

### Features

- choose which columns to include in the output JSON string
- enclose non-null values in quotes in the output JSON string
- decide whether to include null values in the output JSON string
- suppress \n, \r and \t characters from string values
- use comma as decimal separator for Doubles, Floats and BigDecimals

<br>

### Configuration - Basic settings

The component has 2 modes of operation. In both cases, the output schema must include a column of type String to contain the JSON string that is generated. For convenience, the component adds by default a column named simpleJson to the output schema.  This column can be edited.

In the first mode of operation, all colums are included in the JSON.  This is the default mode.

![Screenshot of tSimpleWriteJSON Basic settings - Mode 1 - Include all columns](/doc/images/tsimplewritejson_basic_settings1.png)

1. Select the **output JSON column** from the dropdown list.
 *The data type of the output JSON column must be String, otherwise an exception is thrown upon execution of the job.*
2. Check **Include all columns in the JSON string** to use this mode.
3. Check **Convert non-null values to quoted strings** to generate a JSON string in which the value of every attribute is a string.
4. Check **Include null values** if needed.   
 *This option is set by default.*
5. Check **Convert null values to empty strings** if needed.  
 *Checking this option automatically implies that null values are included, irrespective of whether the box above is checked or not.*
6. Check **Remove \n, \r, \t from strings** to clean up strings of new lines, carriage returns and tabs.

<br>

In the second mode of operation, the user selects a subset of columns to include in the JSON string.

![Screenshot of tSimpleWriteJSON Basic settings - Mode 2 - Subset of columns](/doc/images/tsimplewritejson_basic_settings2.png)

1. Use the **+** and **X** buttons to add or remove a column from the output JSON, respectively.
2. Select the **column to add to the output JSON string** from the dropdown list.
 *Colums can be added only once to the output JSON string, i.e. duplicate entries will be ignored.*

For each column to be added to the output JSON string, treatment options can be specified. See above for details of steps 3 to 6.

<br>

### Configuration - Advanced settings

![Screenshot of tSimpleWriteJSON Advanced Settings](/doc/images/tsimplewritejson_advanced_settings.png)

<br>

1. Specify the default date format to be used in the JSON string **in case it is missing for one or more columns of the input schema**. See the [Talend documentation](https://help.talend.com/r/en-US/8.0/data-preparation-user-guide/list-of-date-and-date-time-formats) for available date/time formats.   

2. Check this box to use a comma as decimal separator and enclose Double, Float and BigDecimal values in double quotes in the output JSON string.   

<br>

### Global variable

NB_LINE : the number of rows processed by the component.  This is an After variable and it returns an integer.

<br>

### Usage

The component is not startable.

<br>

### Example 1 - Generating the JSON payload of an HTTP request

![Screenshot of tSimpleWriteJSON Example](/doc/images/tsimplewritejson_example1.png)

<br>

1. Drop a tFixedFlowInput with schema ```username``` (string), ```dateOfBirth``` (date), ```numberInt``` (integer) and ```notIncluded``` (string).
2. Check ```Use Single Table```.
3. Specify the following input row:  
    ```
    username = "newborn"
    dateOfBirth = TalendDate.getCurrentDate()
    numberInt = 100
    notRelevant = "wrong"
    ```
4. Drop a tSimpleWriteJSON and connect to the tFixedFlowInput.  
5. Edit the output schema of the tSimpleWriteJSON to contain a ```body``` (document) and a ```string``` (string) columns.
6. Select ```string``` as the output JSON column.
7. Uncheck the ```Ìnclude all columns in the JSON string```.
8. Add the username, dateOfBirth and numberInt columns to the list of JSON attributes.
9. Check ```Include null``` and ```Null=empty``` for the username and dateOfBirth attributes.  Check ```Quote``` for the numberInt attribute.
10. Drop a tRESTClient and connect to the tSimpleWriteJSON.
11. Enter "https://postman-echo.com" as URL and "/post" as Relative Path.  Select POST as HTTP Method and JSON as Content-Type and Accept Type.
 *This site exposes mock REST endpoints to test HTTP requests.*
12. Drop a tLogRow and connect it to the Response flow of the tRESTClient.
13. Run the job.

<br>
<hr>


## tSimpleReadJSON

*scans the attributes at the root of an input JSON string and assigns their values to the corresponding columns of the output schema, if any*

<br>

### Features
- exclude one or more columns of the output schema from the parsing of the input JSON string
- reject  input rows to a secondary flow if an error occurs, for instance because the input JSON string is invalid or because the value of a JSON attribute can not be converted to the type assigned to the output column      
- convert date format during processing. For instance a date value with format MM-dd-yyyy in the JSON can populate a column with format yyyy-MM-dd HH:mm:ss
- interpret 0 (false) and 1 (true) as boolean values
- choose separator for decimal numbers found in the input JSON strings (period or comma)

<br>

### External libraries
- [jackson-core-2.13.5.jar](https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-core/2.13.5)
- [jackson-databind-2.13.5.jar](https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-core/2.13.5)
- [jackson-annotations-2.13.5.jar](https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-core/2.13.5)

The .jar are included in the component folder. The components have not been tested with other versions of the [Jackson library](https://github.com/FasterXML/jackson).   

<br>

### Configuration - Basic settings

![Screenshot of tSimpleReadJSON Basic settings](/doc/images/tsimplereadjson_basic_settings.png)

1. Add the columns which are to be filled with values from the input JSON string to the output **schema**. The names of these output columns must be identical to the names of the attributes in the JSON.  
 *Pay close attention to the data types of the columns that are to be extracted from the input JSON. For instance, if the chosen type is int|Integer, then the values in the JSON string must be either an integer or a string representation of an integer, otherwise an error will occur.*   
 *There is no need to add columns for all of the attributes of the input JSON, only for the ones needed.*


2. Select the **input JSON column** from the dropdown list.  
 *The data type of the input JSON column must be String, otherwise an exception is thrown upon execution of the job.*

3. Click the **Exclude input JSON column from parsing** option if you do not wish the component to change the value of this column.  
 *This option is set by default.*

4. If needed, select the **columns to exclude from parsing**. Use the + button to add a line and select the column from the dropdown list. Use the X button to remove a line.   
 *The value of an output column that is excluded is copied from the corresponding input column if it exists, otherwise it is null.*

5. Select one of 3 ways to proceed if an error occurs during treatment of the input row:
   - redirect the input row to a **reject flow** (default)
   - **assign null values** to output columns that do not exist in the input row
   - **die** (i.e. throw an exception). This will interrupt the data flow.   

 *The most likely causes of error are either an invalid input JSON string or a JSON value that can not be cast to the output column type.*

<br>

### Configuration - Advanced settings

![Screenshot of tSimpleReadJSON Advanced settings](/doc/images/tsimplereadjson_advanced_settings.png)


1. If the input JSON string contains date values which are to be extracted, specify the date format used in the JSON string (default is yyyy-MM-dd HH:mm:ss). See the [Talend documentation](https://help.talend.com/r/en-US/8.0/data-preparation-user-guide/list-of-date-and-date-time-formats) for available date/time formats.   
 *Please note that the date format in the output column is specified in the Talend schema.  If this format is different than the one used in the input JSON string, then the component attempts to make the conversion.*

2. If needed, specify the decimal separator that is used in the input JSON string (period [default], comma, or both).   
 *This applies to columns of types Double, Float or BigDecimal.*

<br>

### Global variables

NB_LINE: the number of rows read by the component.  This is an After variable and it returns an integer.

NB_LINE_OK: the number of rows that had a valid input JSON string and that could be processed. This is an After variable and it returns an integer.

NB_LINE_ERROR: the number of rows that had an invalid input JSON string or for which an error occurred during processing. This is an After variable and it returns an integer.  



<br>

### Usage

The component is not startable.

<br>

### Example 1 - Extracting first and last names from input JSON string

![Screenshot of tSimpleReadJSON job example](/doc/images/tsimplereadjson_example1.png)

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
