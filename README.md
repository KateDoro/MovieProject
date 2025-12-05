# Movie Dashboard
## Table of Content
[Problem Statment](#problem-statment)
[Data Source](#data-source)
[Tools](#tools)
[Data Cleaning](#data-cleaning)
[Dashboard](#dashboard)
[M Code](#m-code)
[Recommendations](#recommendations)
### Problem Statment
Netflix wants to better understand which movie they should produce next, including the most suitable actors and directors. We have a dataset containing movie budgets, box office performance, actors, directors, and genres. Your task is to build an Excel dashboard that provides insights into this dataset. The dashboard should help identify:
- The best-performing actors
- The top movies based on box office metrics
- Director performance
- Genre trends
- Seasonal patterns in movie performance
- Any additional insights that can guide future production decisions
  
The final dashboard should be clear, interactive, and visually compelling, enabling Netflix to make data-driven decisions.
### Data Source
Movie Data : The primary dataset used for this analysis is the "Movie Data Homework.xlsx" file, containing detailed information about each movie's performance (box office and budget), actors, directors and genres. 
u can download original datasource here:[Movie Dataset Excel file](https://github.com/user-attachments/files/23946215/NMP.Dashboard.clean.xlsx)
)

### Tools
1. Power Query - I used Power Query for Data Cleaning
2. Excel - I used Excel for Data Analysis
3. Pivot Tables - for Creating the dashboard and Visualizations

### Data Cleaning

- Data loading and inspection.
- Handling errors, missing values.
- Data cleaning and formatting. The excel file after the data cleaning & preparation process can be downloaded here - [Movie Dashboard](https://github.com/user-attachments/files/23950268/MovieDashboard.Github.xlsx)

### Dashboard

<img width="679" height="486" alt="Screenshot 2025-12-04 at 9 17 41 PM" src="https://github.com/user-attachments/assets/e3e4b270-0e24-4467-b78e-b7c2f201ebb3" />

### M Code
```
let
  Source = Excel.Workbook(File.Contents("/Users/anna/Downloads/Movies_Data_Homework.xlsx"), null, true),
  #"Navigation 1" = Source{[Item = "Movie Data", Kind = "Sheet"]}[Data],
  #"Promoted headers" = Table.PromoteHeaders(#"Navigation 1", [PromoteAllScalars = true]),
  #"Merged queries" = Table.NestedJoin(#"Promoted headers", {"Genre_First_ID"}, Genres, {"ID"}, "Genres", JoinKind.LeftOuter),
  #"Expanded Genres" = Table.ExpandTableColumn(#"Merged queries", "Genres", {"Genre"}, {"Genre"}),
  #"Removed columns" = Table.RemoveColumns(#"Expanded Genres", {"Column14", "Column15", "Column16", "Column17", "Column18", "Column19", "Column20", "Column21"}),
  #"Reordered columns" = Table.ReorderColumns(#"Removed columns", {"Movie Title", "Release Date", "Wikipedia URL", "Genre_First_ID", "Genre", "Genre_Second_ID", "Director_First_ID", "Cast_First_ID", "Cast_Second_ID", "Cast_Third_ID", "Cast_Fourth_ID", "Cast_Fifth_ID", "Budget ($)", "Box Office Revenue ($)"}),
  #"Merged queries 1" = Table.NestedJoin(#"Reordered columns", {"Genre_Second_ID"}, Genres, {"ID"}, "Genres", JoinKind.LeftOuter),
  #"Expanded Genres 1" = Table.ExpandTableColumn(#"Merged queries 1", "Genres", {"Genre"}, {"Genre.1"}),
  #"Renamed columns" = Table.RenameColumns(#"Expanded Genres 1", {{"Genre.1", "Genre second"}}),
  #"Reordered columns 1" = Table.ReorderColumns(#"Renamed columns", {"Movie Title", "Release Date", "Wikipedia URL", "Genre_First_ID", "Genre", "Genre_Second_ID", "Genre second", "Director_First_ID", "Cast_First_ID", "Cast_Second_ID", "Cast_Third_ID", "Cast_Fourth_ID", "Cast_Fifth_ID", "Budget ($)", "Box Office Revenue ($)"}),
  #"Merged queries 2" = Table.NestedJoin(#"Reordered columns 1", {"Director_First_ID"}, Directors, {"ID"}, "Directors", JoinKind.LeftOuter),
  #"Expanded Directors" = Table.ExpandTableColumn(#"Merged queries 2", "Directors", {"Director"}, {"Director"}),
  #"Reordered columns 2" = Table.ReorderColumns(#"Expanded Directors", {"Movie Title", "Release Date", "Wikipedia URL", "Genre_First_ID", "Genre", "Genre_Second_ID", "Genre second", "Director_First_ID", "Director", "Cast_First_ID", "Cast_Second_ID", "Cast_Third_ID", "Cast_Fourth_ID", "Cast_Fifth_ID", "Budget ($)", "Box Office Revenue ($)"}),
  #"Merged queries 3" = Table.NestedJoin(#"Reordered columns 2", {"Cast_First_ID"}, Actors, {"ID"}, "Actors", JoinKind.LeftOuter),
  #"Expanded Actors" = Table.ExpandTableColumn(#"Merged queries 3", "Actors", {"Actor"}, {"Actor"}),
  #"Reordered columns 3" = Table.ReorderColumns(#"Expanded Actors", {"Movie Title", "Release Date", "Wikipedia URL", "Genre_First_ID", "Genre", "Genre_Second_ID", "Genre second", "Director_First_ID", "Director", "Cast_First_ID", "Actor", "Cast_Second_ID", "Cast_Third_ID", "Cast_Fourth_ID", "Cast_Fifth_ID", "Budget ($)", "Box Office Revenue ($)"}),
  #"Merged queries 4" = Table.NestedJoin(#"Reordered columns 3", {"Cast_Second_ID"}, Actors, {"ID"}, "Actors", JoinKind.LeftOuter),
  #"Expanded Actors 1" = Table.ExpandTableColumn(#"Merged queries 4", "Actors", {"Actor"}, {"Actor.1"}),
  #"Reordered columns 4" = Table.ReorderColumns(#"Expanded Actors 1", {"Movie Title", "Release Date", "Wikipedia URL", "Genre_First_ID", "Genre", "Genre_Second_ID", "Genre second", "Director_First_ID", "Director", "Cast_First_ID", "Actor", "Cast_Second_ID", "Actor.1", "Cast_Third_ID", "Cast_Fourth_ID", "Cast_Fifth_ID", "Budget ($)", "Box Office Revenue ($)"}),
  #"Merged queries 5" = Table.NestedJoin(#"Reordered columns 4", {"Cast_Third_ID"}, Actors, {"ID"}, "Actors", JoinKind.LeftOuter),
  #"Expanded Actors 2" = Table.ExpandTableColumn(#"Merged queries 5", "Actors", {"Actor"}, {"Actor.2"}),
  #"Reordered columns 5" = Table.ReorderColumns(#"Expanded Actors 2", {"Movie Title", "Release Date", "Wikipedia URL", "Genre_First_ID", "Genre", "Genre_Second_ID", "Genre second", "Director_First_ID", "Director", "Cast_First_ID", "Actor", "Cast_Second_ID", "Actor.1", "Cast_Third_ID", "Actor.2", "Cast_Fourth_ID", "Cast_Fifth_ID", "Budget ($)", "Box Office Revenue ($)"}),
  #"Merged queries 6" = Table.NestedJoin(#"Reordered columns 5", {"Cast_Fourth_ID"}, Actors, {"ID"}, "Actors", JoinKind.LeftOuter),
  #"Expanded Actors 3" = Table.ExpandTableColumn(#"Merged queries 6", "Actors", {"Actor"}, {"Actor.3"}),
  #"Reordered columns 6" = Table.ReorderColumns(#"Expanded Actors 3", {"Movie Title", "Release Date", "Wikipedia URL", "Genre_First_ID", "Genre", "Genre_Second_ID", "Genre second", "Director_First_ID", "Director", "Cast_First_ID", "Actor", "Cast_Second_ID", "Actor.1", "Cast_Third_ID", "Actor.2", "Cast_Fourth_ID", "Actor.3", "Cast_Fifth_ID", "Budget ($)", "Box Office Revenue ($)"}),
  #"Merged queries 7" = Table.NestedJoin(#"Reordered columns 6", {"Cast_Fifth_ID"}, Actors, {"ID"}, "Actors", JoinKind.LeftOuter),
  #"Expanded Actors 4" = Table.ExpandTableColumn(#"Merged queries 7", "Actors", {"Actor"}, {"Actor.4"}),
  #"Reordered columns 7" = Table.ReorderColumns(#"Expanded Actors 4", {"Movie Title", "Release Date", "Wikipedia URL", "Genre_First_ID", "Genre", "Genre_Second_ID", "Genre second", "Director_First_ID", "Director", "Cast_First_ID", "Actor", "Cast_Second_ID", "Actor.1", "Cast_Third_ID", "Actor.2", "Cast_Fourth_ID", "Actor.3", "Cast_Fifth_ID", "Actor.4", "Budget ($)", "Box Office Revenue ($)"}),
  #"Added custom" = Table.AddColumn(#"Reordered columns 7", "ROI", each ([#"Box Office Revenue ($)"]-[#"Budget ($)"])/[#"Budget ($)"]),
  #"Changed column type" = Table.TransformColumnTypes(#"Added custom", {{"ROI", Percentage.Type}})
in
  #"Changed column type"
```
### Recommendations

<img width="358" height="143" alt="Screenshot 2025-12-04 at 10 19 50 PM" src="https://github.com/user-attachments/assets/78e13e6a-49e9-46a5-9686-386366a452bb" />

<img width="400" height="144" alt="Screenshot 2025-12-04 at 10 35 36 PM" src="https://github.com/user-attachments/assets/55cd0e00-c520-4a7c-b090-cdf9ed58881d" />


Top 5 genres are Action, Comedy, Drama, Sci-Fi, Adventure. I would recommend to Netflix to produce one of these genres as they brought in more in box office revenue based on the data from 2012 to 2016. Horror is the most succesful genre in terms of ROI. Summer releases deliver the highest commercial returnes, while April shows the biggest drop in revenue.

Top 5 best actors

<img width="393" height="139" alt="Screenshot 2025-12-04 at 10 47 04 PM" src="https://github.com/user-attachments/assets/9c3e070c-3be5-40b9-a120-2cda032df9cb" />
