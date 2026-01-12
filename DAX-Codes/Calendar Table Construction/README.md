# Calendar Table Construction

Power Query (M) code to generate a complete calendar table for the semantic model, including a future date for proper SPLY calculations.

```
let
    // ============================================================
    // PARAMETERS
    // ============================================================
    StartDate =
        #date(2023, 1, 1),

    EndDate =
        DateTime.Date(
            DateTime.LocalNow() + #duration(1, 0, 0, 0)
        ),

    // ============================================================
    // DATE GENERATION
    // ============================================================
    DateList =
        List.Dates(
            StartDate,
            Duration.Days(EndDate - StartDate) + 1,
            #duration(1, 0, 0, 0)
        ),

    DateTable =
        Table.FromList(
            DateList,
            Splitter.SplitByNothing(),
            {"Date Column"},
            null,
            ExtraValues.Error
        ),

    ChangedType =
        Table.TransformColumnTypes(
            DateTable,
            {{"Date Column", type date}}
        ),

    // ============================================================
    // DATE ATTRIBUTES
    // ============================================================
    InsertedYear =
        Table.AddColumn(
            ChangedType,
            "Year",
            each Date.Year([Date Column]),
            Int64.Type
        ),

    InsertedQuarter =
        Table.AddColumn(
            InsertedYear,
            "Quarter Number",
            each Date.QuarterOfYear([Date Column]),
            type nullable number
        ),

    InsertedMonth =
        Table.AddColumn(
            InsertedQuarter,
            "Month Number",
            each Date.Month([Date Column]),
            type nullable number
        ),

    InsertedDay =
        Table.AddColumn(
            InsertedMonth,
            "Day Number",
            each Date.Day([Date Column]),
            type nullable number
        ),

    InsertedMonthName =
        Table.AddColumn(
            InsertedDay,
            "Month Name",
            each Date.MonthName([Date Column]),
            type nullable text
        ),

    AddedQuarterName =
        Table.TransformColumnTypes(
            Table.AddColumn(
                InsertedMonthName,
                "Quarter Name",
                each "Q" & Number.ToText([Quarter Number])
            ),
            {{"Quarter Name", type text}}
        ),

    // ============================================================
    // FLAGS
    // ============================================================
    Is_Future_Day =
        Table.AddColumn(
            AddedQuarterName,
            "Is Future Day",
            each if [Date Column] = EndDate then "Yes" else "No",
            type text
        ),

    // ============================================================
    // MONTH RANK LOGIC
    // Assigns sequential rank to each Year–Month combination
    // Useful for Top-N month filtering without relative date filters
    // ============================================================
    Year_Month_Group =
        Table.Group(
            Is_Future_Day,
            {"Year", "Month Number"},
            {
                {
                    "Table",
                    each _,
                    type nullable table[
                        #"Date Column" = nullable date,
                        Year = Int64.Type,
                        #"Quarter Number" = nullable number,
                        #"Month Number" = nullable number,
                        #"Day Number" = nullable number,
                        #"Month name" = nullable text,
                        #"Quarter Name" = nullable text,
                        #"Is Last Day" = nullable text
                    ]
                }
            }
        ),

    Add_Month_Rank =
        Table.AddIndexColumn(
            Year_Month_Group,
            "Month Rank",
            1,
            1,
            Int64.Type
        ),

    ChooseColumns =
        Table.SelectColumns(
            Add_Month_Rank,
            {"Table", "Month Rank"}
        ),

    ExpandedTable =
        Table.ExpandTableColumn(
            ChooseColumns,
            "Table",
            {
                "Date Column",
                "Year",
                "Quarter Number",
                "Month Number",
                "Day Number",
                "Month name",
                "Quarter Name",
                "Is Last Day"
            },
            {
                "Date Column",
                "Year",
                "Quarter Number",
                "Month Number",
                "Day Number",
                "Month name",
                "Quarter Name",
                "Is Last Day"
            }
        )
in
    ExpandedTable

```

After the creation of the table, the following steps need to be ensured to guarantee it works properly:

- Create a 1 to many relationship from the Date Column of the calendar table with the date columns of the other tables in the model.
  
- Sort Month Name and Quarter Name columns respectively by Month Number and Quarter Number columns
  
- (Optional) Create a hierarchy for faster usage - suggested: year, quarter name, month name
  
- In the Filters Pane > Filters on all pages locate the Is Future Day column and select ONLY "No" - This guarantees the SPLY function or the DATEADD function that brings back the time of 1 yr work well, returning for the last available and incomplete period only those values referring to the same period last year, not for the full same period last year. For instance if we are on the 12th of December of 2025 and we do not use this "future day technique" we would return as SPLY the full December 2024. Using the technique we return values until 12th of Dec 2024.

**💡 Pro Tip**
The Month Rank column enables stable Top-N month filtering independent of relative date logic.
Unlike relative date filters—which frequently produce partial periods—the Month Rank approach always returns fully populated months for the selected time window (e.g. last 24 months).

**Instructions**
- Add the Month Rank column to Visual-level filters
- Set the filter type to Top N
- Choose Top N by Max of Month Rank
- Enter the number of months to display (e.g. 24)
- The visual will dynamically show full data for the last 24 complete months

Month Rank avoids the partial-period problem of relative date filters by enforcing complete month selection.
