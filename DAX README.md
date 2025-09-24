DAX formulas
Total Distance = SUM('Trip Details'[Distance])
Total Fare = SUM('Trip Details'[Fare])
Total Trips = COUNT('Trip Details'[Trip ID])
Total_Duration = SUM('Trip Details'[Duration])

Shift = 
VAR _hr = HOUR('Trip Details'[Pickup Time])
return
SWITCH(
TRUE(),
_hr>=6 && _hr<=21, "Day",
_hr>=21 && _hr<=23, "Night",
_hr>=0 && _hr<=6, "Night",
BLANK())

Night Shift % = 
VAR Nightcount = CALCULATE([Total Trips],'Trip Details'[Shift]="Night")
RETURN
DIVIDE(Nightcount,[Total Trips],0)
Filters = {
    ("Total Trips", NAMEOF('Trip Details'[Total Trips]), 0),
    ("Total Fare", NAMEOF('Trip Details'[Total Fare]), 1),
    ("Total_Duration", NAMEOF('Trip Details'[Total_Duration]), 2),
    ("Total Distance", NAMEOF('Trip Details'[Total Distance]), 3)
}


Supporting Table
Date Table = ADDCOLUMNS (
    CALENDAR(DATE(2024,1,1), DATE(2024,12,31)),
    //CALENDARAUTO(),
    "Year", YEAR([Date]),
    "Quarter", "Q" & FORMAT(CEILING(MONTH([Date])/3, 1), "#"),
    "Quarter No", CEILING(MONTH([Date])/3, 1),
    "Month No", MONTH([Date]),
    "Month Name", FORMAT([Date], "MMMM"),
    "Month Short Name", FORMAT([Date], "MMM"),
    "Month Short Name Plus Year", FORMAT([Date], "MMM,yy"),
    "DateSort", FORMAT([Date], "yyyyMMdd"),
    "Day Name", FORMAT([Date], "dddd"),
    "Details", FORMAT([Date], "dd-MMM-yyyy"),
    "Week Sort", FORMAT([DATE],"W"),
    "Day Number", DAY([Date])
)
