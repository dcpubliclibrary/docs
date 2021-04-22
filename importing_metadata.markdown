---
title: Importing metadata in Islandora
layout: default
nav_exclude: true
---

## Importing metadata in Islandora

- [Part I: Opening and Preparing Metadata in OpenRefine](#part-i-opening-and-preparing-metadata-in-openrefine)

- [Part II: Opening and Preparing the XML Template](#part-ii-opening-and-preparing-the-xml-template)

- [Part III: Templating and Exporting in OpenRefine](#part-iii-templating-and-exporting-in-openrefine)

- [Part IV: Splitting XML Files into MODS Records](#part-iv-splitting-xml-files-into-mods-records)

- [Part V: Ingesting MODS Records into Islandora](#part-v-ingesting-mods-records-into-islandora)

- [Atom Tips](#atom-tips)

## Part I: Opening and Preparing Metadata in OpenRefine

1. Open the metadata file to be ingested into Islandora. The file will be a XLS, CSV, or ODS format. 

2. Open OpenRefine -- click Create Project>This Computer>Choose Files and select your desired file>Next

3. OpenRefine will create a preview of the project, this may take awhile to generate.

4. Take a look at the preview. If it fields appear to have loaded correctly click Create Project.

5. Check for columns with multiple entries separated by semicolons (;). These entries need to be separated into individual lines to display properly in Islandora. Example: Portraits; Cyclists; Bicycles and tricycles; Cartes de visite

6. Click the blue triangle to the left of the column name>Edit column>Split into several columns...

7. Under How to Split Column select by separator column and change Separator from a comma (,) to a semi-colon (;)

8. Under After Splitting uncheck Guess cell type and Remove this column

9. Click OK

10. The newly split columns should appear to the right of the original column. Scroll to the right to confirm the splitting was successful. There should now be many columns with the original column name plus a number. Example: Subject 1, Subject 2...

11. Remove the original column by clicking the blue triangle to the left of the column name> Edit column>Remove this column

12. Repeat until all columns contain only one entry per row. 

13. Check for and delete empty columns. Any empty fields in Islandora will display as Null. Because metadata templates are used for multiple collections not all fields will be applicable. NOTE: Many of the previous columns that were split will contain null values. These values will be addressed with the XML code. Removing null values only applies to entire empty columns.

14. To confirm if a column is empty click the blue triangle to the left of the column name>Facet>Text facet. In the left margin of the screen the Facet/Filter tab will appear with the column name. If there are values present in the column they will be listed. If there are no values only (blank) will appear. If the column contains no values remove the column by clicking the blue triangle to the left of the column name> Edit column>Remove this column. Repeat until all null columns have been removed. 

15. If a column titled File Name 2 is present delete it by clicking the blue triangle to the left of the column name> Edit column>Remove this column.

16. Rename/add columns Rights1, Rights2, and Rights3. These columns may or may not already be present and may or may not be named Rights Identifier, Rights Statement, and Rights Holder. All 3 must be present. 
Rename a column by clicking the blue triangle to the left of the column name>Edit column>Rename this column. Add content to an entire column by hovering your mouse in the first row and clicking the blue Edit box>enter the text according to guide below>Apply to All Identical Cells. If all 3 rights columns are not present add a new column by clicking the blue triangle to the left of the column name>Edit column>Add column based on the column...>New column name>OK. Edit the column values are needed.

    - Rights1:
        - Column name: Rights Identifier
        - Value: http://rightsstatements.org/vocab/InC/1.0/
        
    - Rights2:
        - Column name: Rights Statement
        - Value: See collection guide

    - Rights3:
        - Column name: Rights holder
        - Value: See collection guide

17. Change Column Format Medium from text/pdf to image/tiff. Hover your mouse over the first cell in the column and click the blue Edit box>image/tiff>Apply to All Identical Cells.

Congratulations! You've finished Part I! Leave OpenRefine and the spreadsheet open for Part II.

## Part II: Opening and Preparing the XML Template

The next step is to prepare an XML file fo OpenRefine templating. You will create a working file from the dc_to_mods.xml template. 

1. Open the text editor of your choice and open the dc_to_mods.xml template. Template can be retrieved from [Github](https://github.com/dcpubliclibrary/digital_projects/blob/master/Islandora/Open%20Refine/dc_to_mods.xml).

2. Save a working copy. Click File>Save As...>YYYYMMDD_IngestFileName_dc_to_mods_templating.xml>Save. Example: 20190301_Blade1991_dc_to_mods_templating.xml

3. Check that each column from OpenRefine is present on the XML working copy. On the XML working copy enter Ctrl F>enter the column name in Find in current buffer>Find. The column name should appear as an attribute between parenthesis. Example: "File Name". Note: Ingest into Islandora is case sensitive, column names must be entered on the XML working copy exactly as they appear in OpenRefine. 

4. Check all attributes present on the XML working copy to verify they are present in OpenRefine. If the attribute does not appear as a column name in OpenRefine remove it from the XML working copy. 

5. Add attributes for entries split into columns in OpenRefine during Part I. Each value that was separated by a semicolon and split into a new column in OpenRefine needs an individual attribute in the XML working copy. Create a new line for each new column, enter an opening and closing brace on that line, and copy and paste the following between them (like so - ```{paste here}``` ): 

    ```
    {if(isBlank(cells["ATTRIBUTE"].value),"","<topic>"+jsonize(cells["ATTRIBUTE "].value).replace('"', '')+"</topic>")}
    ``` 


    Edit the ATTRIBUTE names to reflect the column names. Example: 
    

    ```
    {if(isBlank(cells["Subject 1"].value),"","<topic>"+jsonize(cells["Subject 1 "].value).replace('"', '')+"</topic>")}
    ```

    Note that the lines you create should begin with a double brace and end with a double closing brace.

Congratulations! You've finished Part II! Leave OpenRefine, the spreadsheet, and the XML working copy open for Part III.

## Part III: Templating and Exporting in OpenRefine

1.  In OpenRefine click Export>Templating...

2.  On the XML working copy notice sections divided by light gray notes. These notes correspond to the sections in the Templating Export screen in OpenRefine. Enter the corresponding XML from the XML working copy into Prefix, Row Template, and Suffix.

3.  Under Row Separator delete the comma (,).

4.  As you edit the sections the output on the right should change. When all sections have been entered review the output for any null values ("null") and edit as needed in Row Template and the XML working copy.

5.  Click Export. 

6.  Open the exported text file. Copy the entire contents of the text file Ctrl A>Ctrl C

7.  Open a new file in your text editor and paste in the text file Ctrl V

8.  Save the new file in the text editor as an XML file with the naming conventions YYYYMMDD_IngestFileName_dc_to_mods.xml Example: 20190301_Blade1991_dc_to_mods.xml

9.  Replace ampersands (&) with the word and. Ctrl F>enter & in the Find field>enter and in the Replace field>Replace All

10. Search and remove null attributes. Ctrl F>enter null in the Find field. Take care to closely review the null attributes. It may be necessary to make changes to XML working copy and regenerate the template in OpenRefine.

Congratulations! You've finished Part III! You may close OpenRefine, the spreadsheet, and the XML working copy.

## Part IV: Splitting XML Files into MODS Records

OpenRefine generated bulk XML files of MODS records. For example, each issue of a newspaper is one individual MODS record, but an entire year of issues is included in the XML file generated by OpenRefine.  These records must now be split into individual MODS records for ingest into Islandora. 

1. The splitting process utilizes Terminal and Python scripts which can be found on [GitHub](https://github.com/dcpubliclibrary/digital_projects/blob/master/Islandora/split_mods.py). An example Terminal for splitting appears below.

2. Open Terminal and navigate to where the scripts are stored on your computer.

3. Declare the version of Python used and import the script split_mods.py

4. Establish the XML file to be used with split_mods.py and the destination folder for the newly split MODS records. 

Mac Example:

```
cd desktop
python3
from split_mods import*
split_mods('filepath/whatever_your_mods_file_is_named.xml' , 'filepath/where/you/want/the/split/files/to/go')
```
Windows Example:

```
cd desktop
py
from split_mods import*
split_mods(r'filepath/whatever_your_mods_file_is_named.xml' , r'filepath/where/you/want/the/split/files/to/go')
```
Take care to add an r before the file path so python will read the file. 

1. The destination folder should now contain indiviual MODS XML records split from the OpenRefine XML document. 

2. Open an Excel Spreadsheet or Google Sheet. Copy and paste all the newly created XML file names by selecting all of the files>Ctrl C>Ctrl V into a new workbook.

3. Strip the .xml file extensions from the file names using Find and replace. 

4. Prepare the file names for the create_directories.py script. Use the concatenate formula to add inverted commas ("") before and after each file name and a comma (,) after each file name.  ```=CONCATENATE("""",A1,"""",",")```

6. Copy the concatenated list from column B and paste it into a new column to remove the formula. Ctrl Shift down arrow>Ctrl C>Ctrl V>Paste Values Only

7. Open the Python script [create_directories.py](https://github.com/dcpubliclibrary/digital_projects/blob/master/Islandora/create_directories.py) in the text editor of your choice. Follow the prompts in the script to enter the folder location and files names. Save to a convenient location.

8. Go back to terminal and exit out of the Python terminal. 

9. Run the create_directories.py script. 

Example:
```
exit ()
python create_directories.py
```

10. In the destination folder there should now be a directory created for each XML file with the name of the XML file. Drag XML file into its corresponding directory. 

11. Rename each XML file MODS.xm. Open the rename.mods.py script in the text editor of your choice and edit the file path. Save to a convenient location. 

12. Go back to Terminal and run the [rename_mods.py](https://github.com/dcpubliclibrary/digital_projects/blob/master/Islandora/rename_mods.py) script. 

Example:

```
python rename_mods.py
```

Example of Entire Terminal Prompt:

```
cd desktop
python3
from splitmods import*
split_mods('filepath/whatever_your_mods_file_is_named.xml' , 'filepath/where/you/want/the/split/files/to/go')
exit ()
python create_directories.py
python rename_mods.py
```

13.  Zip the folder containing all the individual MODS directories for ingest into Islandora. 

Congratulations! You've finished Part IV! Only one part left to go.

## Part V: Ingesting MODS Records into Islandora 

1. Open Islandora stage: <https://dcplislandora-stage.wrlc.org/>

2. Click Browse in the upper right corner and navigate to the collection for metadata ingest. 

3. Click the Manage tab.

4. For newspapers click +Newspaper Batch. Other media types will have different options. 

5. Drag the zip file into the the Zip file box and click Start Upload

6. When the zip has been uploaded unclick Generate OCR and Generate HOCR

7. Click Ingest

Congratulations! You've finished Part V! If all went according to plan metadata files for a collection should now be ingested into Islandora. Image files will be added separately.

## Atom Tips

1. Find and replace across multiple files in a directory: Ctrl Shift F