# APL2XL 
APL2XL is a Dyalog APL library which exports APL arrays to Excel in the form of a .xlsx file. This library is a work in progress. 

APL2XL assumes `⎕IO ⎕ML←0 1` and is not tested for any other settings for `⎕IO` or `⎕ML`.

## Example Usage
Workbooks consist of Worksheets, and Worksheets consist of Ranges. Ranges, Worksheets and Workbooks are simple to define as Namespaces, and include the following properties
|Type|Required Variables|Optional Variables|
|---|---|---|
|Ranges|Value, Address|NumberFormat, Font, Fill, Border, Address|
|Worksheets|Name, Ranges||
|Workbook|Sheets, FileName||

Define range comprised of values starting at an address, and optional formatting. **Note: Currently, APL2XL assumes the shape of the namespace.Value as the shape of the data in Excel.**  This means that `range.Value←'simple depth 1 character vector'` will place each character into a single cell accross the row starting at `range.Adress`. If you would like to place a character vector into a single cell, enclose the character vector. Ex. `range.Value←⊂'this will occupy a single cell'`
```APL
⎕IO ⎕ML←0 1
r1             ←⎕NS''
r1.Value       ←⍪10000+⍳5
r1.NumberFormat←'#,##0'           ⍝ 10,000
r1.Address     ←2 3               ⍝ or 'B3'

r2             ←⎕NS''
r2.Value       ←⍪⍳10
r2.NumberFormat←'m/d/yyyy'
r2.Font        ←11 1 'Broadway' 5         ⍝ size color name family
r2.Fill        ←'solid' 'ff6699' '33cc33' ⍝ style foregroundColor backgoundColor
r2.Border      ←5⍴(⊂'thin' '0000cc')      ⍝ 5 elemtent vector of tuples containing ('thickness' 'hexcolor')       
r2.Address     ←'C1'
```

Place the Ranges into a Worksheet
```APL
s1←⎕NS''
s1.Name  ←'Sample1'
s1.Ranges←r1 r2
```

Add the sheets to a workbook with a fully qualified path. **Note: path cannot be of the form `c:\{filename}` due to writing privileges.** Select a path of the form `c:\myfolder\myfile.xlsx`

```APL
wb←⎕NS''
wb.Sheets  ←s1 s2
wb.FileName←'c:\{path}\myfile.xlsx'

Main.Export wb
```

## Styling and Formats
This table is an exhaustive list of currently implemented styling features, and known missing style features. Existing styling currently applies styles to cells that exist in the range. Styles more granular than the cell level are not currently implemented. 

|Style|Supported|Value|Usage|Note|
|---|---|---|---|---|
|Number Format|x|ExcelNumberFormat:'Character Vector'|range.NumberFormat←'m/d/yyyy'||
|Font|x|Size:Integer Color:Integer Name:'Character Vector' Font-Family:Integer|range.Font←11 1 'Broadway' 5||
|Fill|x|Pattern:'Character Vector' ForegroundColor:'HEXColor' BackgroundColor:'HEXColor'|range.Fill←'solid' 'ff6699' '33cc33'||
|Border|x|5⍴(⊂Thickness:'Character Vector' Color:'HEXColor')|range.Border←5⍴('thin' 1)('thick' '0000cc')||
|String Formatting| | | | Not Implemented|
|Table| | | | Not Implemented|


### Selecting Colors
Colors can be selected by copying the #FFFFFF color using a color picker [like this one.](https://www.w3schools.com/colors/colors_picker.asp)


Additional questions may be directed to nathan@dyalog.com