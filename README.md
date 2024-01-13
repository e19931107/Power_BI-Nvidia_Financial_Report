# Power_BI-Nvidia_Financial_Report

## Result:
![image](https://github.com/e19931107/Power_BI-Nvidia_Financial_Report/assets/50692450/92e3c496-bdac-43e8-bcce-5947ab4a5207)


## Step
### Step 1: Clear data, remove subtotal and total raw
#### Before
![image](https://github.com/e19931107/Power_BI-Nvidia_Financial_Report/assets/50692450/934d1aab-35dc-4a79-9087-9bc4973ea390)
#### After
![image](https://github.com/e19931107/Power_BI-Nvidia_Financial_Report/assets/50692450/8bbd2c76-6678-48f1-b718-25e373995f44)

### Step 2: Import into Power BI
![image](https://github.com/e19931107/Power_BI-Nvidia_Financial_Report/assets/50692450/7c378a69-2756-4a0b-920a-8ba7d6c650eb)

### Step 3: Unpivot the table, make into long table.
![image](https://github.com/e19931107/Power_BI-Nvidia_Financial_Report/assets/50692450/10b17365-bb7d-487d-8eca-c7a58c39b7f9)

### Step 4: Replace Assets - Pure.csv into Liabilities and Equity - Pure.csv
let
    Source = Csv.Document(File.Contents("D:\GitHub\Power_BI-Nvidia_Financial_Report\CSV\ <code style="color : red">Assets - Pure.csv </code> "),[Delimiter=",", Columns=7, Encoding=1252, QuoteStyle=QuoteStyle.None]),
    #"Changed Type" = Table.TransformColumnTypes(Source,{{"Column1", type text}, {"Column2", type text}, {"Column3", type text}, {"Column4", type text}, {"Column5", type text}, {"Column6", type text}, {"Column7", type text}}),
    #"Removed Top Rows" = Table.Skip(#"Changed Type",4),
    #"Promoted Headers" = Table.PromoteHeaders(#"Removed Top Rows", [PromoteAllScalars=true]),
    #"Changed Type1" = Table.TransformColumnTypes(#"Promoted Headers",{{"", type text}, {"Jan 29, 2023", Int64.Type}, {"Jan 30, 2022", Int64.Type}, {"Jan 31, 2021", Int64.Type}, {"Jan 26, 2020", Int64.Type}, {"Jan 27, 2019", Int64.Type}, {"Jan 28, 2018", Int64.Type}}),
    #"Unpivoted Columns" = Table.UnpivotOtherColumns(#"Changed Type1", {""}, "Attribute", "Value"),
    #"Changed Type2" = Table.TransformColumnTypes(#"Unpivoted Columns",{{"Attribute", type date}, {"Value", Int64.Type}}),
    #"Inserted Year" = Table.AddColumn(#"Changed Type2", "Year", each Date.Year([Attribute]), Int64.Type)
in
    #"Inserted Year"


let
    Source = Csv.Document(File.Contents("D:\GitHub\Power_BI-Nvidia_Financial_Report\CSV\ <code style="color : red">Liabilities and Equity - Pure.csv </code> "),[Delimiter=",", Columns=7, Encoding=1252, QuoteStyle=QuoteStyle.None]),
    #"Changed Type" = Table.TransformColumnTypes(Source,{{"Column1", type text}, {"Column2", type text}, {"Column3", type text}, {"Column4", type text}, {"Column5", type text}, {"Column6", type text}, {"Column7", type text}}),
    #"Removed Top Rows" = Table.Skip(#"Changed Type",4),
    #"Promoted Headers" = Table.PromoteHeaders(#"Removed Top Rows", [PromoteAllScalars=true]),
    #"Changed Type1" = Table.TransformColumnTypes(#"Promoted Headers",{{"", type text}, {"Jan 29, 2023", Int64.Type}, {"Jan 30, 2022", Int64.Type}, {"Jan 31, 2021", Int64.Type}, {"Jan 26, 2020", Int64.Type}, {"Jan 27, 2019", Int64.Type}, {"Jan 28, 2018", Int64.Type}}),
    #"Unpivoted Columns" = Table.UnpivotOtherColumns(#"Changed Type1", {""}, "Attribute", "Value"),
    #"Changed Type2" = Table.TransformColumnTypes(#"Unpivoted Columns",{{"Attribute", type date}, {"Value", Int64.Type}}),
    #"Inserted Year" = Table.AddColumn(#"Changed Type2", "Year", each Date.Year([Attribute]), Int64.Type)
in
    #"Inserted Year"


