# Calendar Table for the semantic model

Below the full m-code to create a complete calendar table to use in the semantic model.

```
let
    StartDate = #date(2023, 1, 1),
    EndDate = DateTime.Date(DateTime.LocalNow() + #duration(1, 0, 0, 0)),
    DateList = List.Dates(StartDate, Duration.Days(EndDate - StartDate) + 1, #duration(1, 0, 0, 0)),
    DateTable = Table.FromList(DateList, Splitter.SplitByNothing(), {"Date Column"}, null, ExtraValues.Error),
    ChangedType = Table.TransformColumnTypes(DateTable,{{"Date Column", type date}}),
    InsertedYear = Table.AddColumn(ChangedType, "Year", each Date.Year([Date Column]), Int64.Type),
    InsertedQuarter = Table.AddColumn(InsertedYear, "Quarter Number", each Date.QuarterOfYear([Date Column]), type nullable number),
    InsertedMonth = Table.AddColumn(InsertedQuarter, "Month Number", each Date.Month([Date Column]), type nullable number),
    InsertedDay = Table.AddColumn(InsertedMonth, "Day Number", each Date.Day([Date Column]), type nullable number),
    InsertedMonthName = Table.AddColumn(InsertedDay, "Month Name", each Date.MonthName([Date Column]), type nullable text),
    AddedQuarterName = Table.TransformColumnTypes(Table.AddColumn(InsertedMonthName, "Quarter Name", each "Q" & Number.ToText([Quarter Number])), {{"Quarter Name", type text}}),
    Is_Future_Day = Table.AddColumn(AddedQuarterName, "Is Future Day", each if [Date Column] = DateTime.Date(DateTime.LocalNow() + #duration(1, 0, 0, 0)) then "Yes" else "No", type text)
in
    Is_Future_Day
```

After the creation of the table, the following steps need to be ensured to guarantee it works properly:

- Create a 1 to many relationship from the Date Column of the calendar table with the date columns of the other tables in the model.
- Sort Month Name and Quarter Name columns respectively by Month Number and Quarter Number columns
- (Optional) Create a hierarchy for faster usage - suggested: year, quarter name, month name
- In the Filters Pane > Filters on all pages locate the Is Future Day column and select ONLY "No" - This guarantees the SPLY function or the DATEADD function that brings back the time of 1 yr work well, returning for the last available and incomplete period only those values referring to the same period last year, not for the full same period last year. For instance if we are on the 12th of December of 2025 and we do not use this "future day technique" we would return as SPLY the full December 2024. Using the technique we return values until 12th of Dec 2024.
