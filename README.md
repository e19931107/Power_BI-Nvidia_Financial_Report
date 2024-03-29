# Power_BI-Nvidia_Financial_Report

## Result:
### First Page
![image](https://github.com/e19931107/Power_BI-Nvidia_Financial_Report/assets/50692450/4d409aa4-966d-4424-a0f4-e7f2931e4fa0)

### Second Page
![image](https://github.com/e19931107/Power_BI-Nvidia_Financial_Report/assets/50692450/da39eabe-4497-4e04-aaa6-f509dc7c4a41)

## Explain
First page: Click the current ratio button, quick ratio button, or cash flow ratio button, below bar chart will also change.

Second page: Bar chart only connected with Year button. so the current ratio, quick ratio and cash flow ratio didn't connect with the bar chart.
This means the bar chart shows the status of assets, liabilities and equity with selected year.

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

### Step 5: Build key and index order for Assets sheets, Liabilities and Equity sheets

![image](https://github.com/e19931107/Power_BI-Nvidia_Financial_Report/assets/50692450/710260c6-6392-44fc-8d07-86b4eee7c051)

![image](https://github.com/e19931107/Power_BI-Nvidia_Financial_Report/assets/50692450/fdb21126-2e4f-4748-b239-17ce46eda226)

## DAX

Title = SELECTEDVALUE('Year'[Year])&" Nvidia Assests, Liabilities and Equity sheets"

Ratio = DIVIDE('Assets - Pure'[Assets],'Liabilities and Equity - Pure'[Liabilities and Equity])
Quick Ratio = DIVIDE(CALCULATE('Year'[Assets - Cash and cash equivalents]+
                                'Year'[Assets - Marketable securities]+
                                'Year'[Assets - Accounts receivable, net]),
                        CALCULATE('Year'[Equity - Accounts payable]+
                                'Year'[Equity - Customer program accruals]+
                                'Year'[Equity - Excess inventory purchase obligations]+
                                'Year'[Equity - Accrued payroll and related expenses]+
                                'Year'[Equity - Taxes payable]+
                                'Year'[Equity - Deferred revenue]+
                                'Year'[Equity - Short-term operating lease liabilities]+
                                'Year'[Equity - Miscellaneous Payables]))

Cash Flow Ratio = DIVIDE(CALCULATE('Year'[Assets - Cash and cash equivalents]),
                        CALCULATE('Year'[Equity - Accounts payable]+
                                'Year'[Equity - Customer program accruals]+
                                'Year'[Equity - Excess inventory purchase obligations]+
                                'Year'[Equity - Accrued payroll and related expenses]+
                                'Year'[Equity - Taxes payable]+
                                'Year'[Equity - Deferred revenue]+
                                'Year'[Equity - Short-term operating lease liabilities]+
                                'Year'[Equity - Miscellaneous Payables]))

Current Ratio = DIVIDE(CALCULATE('Year'[Assets - Cash and cash equivalents]+
                                'Year'[Assets - Marketable securities]+
                                'Year'[Assets - Accounts receivable, net]+
                                'Year'[Assets - Inventories]),
                        CALCULATE('Year'[Equity - Accounts payable]+
                                'Year'[Equity - Customer program accruals]+
                                'Year'[Equity - Excess inventory purchase obligations]+
                                'Year'[Equity - Accrued payroll and related expenses]+
                                'Year'[Equity - Taxes payable]+
                                'Year'[Equity - Deferred revenue]+
                                'Year'[Equity - Short-term operating lease liabilities]+
                                'Year'[Equity - Miscellaneous Payables]))

Assets - Accounts receivable, net = CALCULATE(SUM('Assets - Pure'[Value]), 'Assets - Pure'[Sub-Type]="Accounts receivable, net")
Equity - Accounts payable = CALCULATE(SUM('Liabilities and Equity - Pure'[Value]), 'Liabilities and Equity - Pure'[Sub-Type] = "Accounts payable")
