 PART 1 ADDING CALCULATIONS TO A QUERY 
--SET BASED CALCULATIONS


--FIRST EXAMPLE YTD CALCULATION


SELECT {} ON 0,

	YTD([Date].[Calendar].[Date].&[20051122]) ON 1

FROM [Adventure Works]


--QTD
SELECT {} ON 0,

	QTD([Date].[Calendar].[Date].&[20051122]) ON 1

FROM [Adventure Works];

--BUILDING SET BASED CALCULATIONS
WITH MEMBER [Measures].[YTD Sales] AS

	AGGREGATE(YTD([Date].[Calendar].CURRENTMEMBER),[Measures].[Reseller Sales Amount])

SELECT {[Measures].[YTD Sales]} ON 0,

	[Date].[Calendar].[Date] ON 1

FROM [Adventure Works];

--BUILDING SET BASED CALCULATIONS
WITH MEMBER [Measures].[YTD Sales] AS

	AGGREGATE(YTD([Date].[Calendar].CURRENTMEMBER),[Measures].[Reseller Sales Amount])

SELECT {[Measures].[YTD Sales]} ON 0,

	[Date].[Calendar].[Month] ON 1

FROM [Adventure Works];

--Another calculations that shows when the sale goes through
WITH MEMBER [Measures].[YTD Sales] AS

	AGGREGATE(YTD([Date].[Calendar].CURRENTMEMBER),[Measures].[Reseller Sales Amount])

SELECT {[Measures].[YTD Sales],[Measures].[Reseller Sales Amount]} ON 0,

	[Date].[Calendar].[Date] ON 1

FROM [Adventure Works];

--with internet sales
WITH MEMBER [Measures].[YTD Sales] AS

	AGGREGATE(YTD([Date].[Calendar].CURRENTMEMBER),[Measures].[Internet Sales Amount])

SELECT {[Measures].[YTD Sales],[Measures].[Internet Sales Amount]} ON 0,

	[Date].[Calendar].[Date] ON 1

FROM [Adventure Works];


--looking at year over year growth with prior year sales

WITH MEMBER [Measures].[YTD Sales] AS

	AGGREGATE(YTD([Date].[Calendar].CURRENTMEMBER),[Measures].[Internet Sales Amount])

		MEMBER [Measures].[Prior Year Sales] AS

		(PARALLELPERIOD(
		[Date].[Calendar].[Calendar Year],
		1,
		[Date].[Calendar].CURRENTMEMBER),
		[Measures].[Internet Sales Amount]
		), FORMAT_STRING="CURRENCY"

SELECT {[Measures].[YTD Sales]
	,[Measures].[Prior Year Sales]
	,[Measures].[Internet Sales Amount]} ON 0,

	[Date].[Calendar].[Month] ON 1

FROM [Adventure Works];

--WITH YEAR OVER YEAR GROWTH AMOUNT

WITH MEMBER [Measures].[YTD Sales] AS

	AGGREGATE(YTD([Date].[Calendar].CURRENTMEMBER),[Measures].[Internet Sales Amount])

		MEMBER [Measures].[Prior Year Sales] AS

		(PARALLELPERIOD(
		[Date].[Calendar].[Calendar Year],
		1,
		[Date].[Calendar].CURRENTMEMBER),
		[Measures].[Internet Sales Amount]
		), FORMAT_STRING="CURRENCY"

		MEMBER [Measures].[YEAR OVER YEAR GROWTH/LOSS] AS

		[Measures].[YTD Sales]-[Measures].[Prior Year Sales],
		FORMAT_STRING="CURRENCY"

		MEMBER [Measures].[YEAR OVER YEAR PERCENT INCREASE] AS
		[Measures].[YEAR OVER YEAR GROWTH/LOSS]/[Measures].[Internet Sales Amount],
		FORMAT_STRING="PERCENT"

SELECT {[Measures].[YTD Sales]
	,[Measures].[Prior Year Sales]
	,[Measures].[Internet Sales Amount]
	,[Measures].[YEAR OVER YEAR GROWTH/LOSS]
	,[Measures].[YEAR OVER YEAR PERCENT INCREASE]} ON 0,

	[Date].[Calendar].[Month] ON 1

FROM [Adventure Works];

--CHANGING ABOVE TO INCLUDE PREVIOUS MONTH DATA
WITH MEMBER [Measures].[YTD Sales] AS

	AGGREGATE(YTD([Date].[Calendar].CURRENTMEMBER),[Measures].[Internet Sales Amount])

		MEMBER [Measures].[Prior Year Sales] AS

		(PARALLELPERIOD(
		[Date].[Calendar].[Month],
		1,
		[Date].[Calendar].CURRENTMEMBER),
		[Measures].[Internet Sales Amount]
		), FORMAT_STRING="CURRENCY"

		MEMBER [Measures].[YEAR OVER YEAR GROWTH/LOSS] AS

		[Measures].[YTD Sales]-[Measures].[Prior Year Sales],
		FORMAT_STRING="CURRENCY"

		MEMBER [Measures].[YEAR OVER YEAR PERCENT INCREASE] AS
		[Measures].[YEAR OVER YEAR GROWTH/LOSS]/[Measures].[Internet Sales Amount],
		FORMAT_STRING="PERCENT"

SELECT {[Measures].[YTD Sales]
	,[Measures].[Prior Year Sales]
	,[Measures].[Internet Sales Amount]
	,[Measures].[YEAR OVER YEAR GROWTH/LOSS]
	,[Measures].[YEAR OVER YEAR PERCENT INCREASE]} ON 0,

	[Date].[Calendar].[Month] ON 1

FROM [Adventure Works];

--prior year to date sales

WITH MEMBER [Measures].[YTD Sales] AS

	AGGREGATE(YTD([Date].[Calendar].CURRENTMEMBER),[Measures].[Internet Sales Amount])

		MEMBER [Measures].[Prior Year Sales] AS

		(PARALLELPERIOD(
		[Date].[Calendar].[Month],
		1,
		[Date].[Calendar].CURRENTMEMBER),
		[Measures].[Internet Sales Amount]
		), FORMAT_STRING="CURRENCY"

		MEMBER [Measures].[YEAR OVER YEAR GROWTH/LOSS] AS

		[Measures].[YTD Sales]-[Measures].[Prior Year Sales],
		FORMAT_STRING="CURRENCY"

		MEMBER [Measures].[YEAR OVER YEAR PERCENT INCREASE] AS
		[Measures].[YEAR OVER YEAR GROWTH/LOSS]/[Measures].[Internet Sales Amount],
		FORMAT_STRING="PERCENT"


SELECT {[Measures].[YTD Sales]
	,[Measures].[Prior Year Sales]
	,[Measures].[Internet Sales Amount]
	,[Measures].[YEAR OVER YEAR GROWTH/LOSS]
	,[Measures].[YEAR OVER YEAR PERCENT INCREASE]} ON 0,

	[Date].[Calendar].[Month] ON 1

FROM [Adventure Works];



--creating a basic measure	

WITH MEMBER [Measures].[Stuff] AS

	[Date].[Calendar].CURRENTMEMBER.NAME

SELECT {[Measures].[Stuff]}ON 0,
NON EMPTY
[Date].[Calendar].[Month].MEMBERS ON 1

FROM [Adventure Works]

--OR

WITH MEMBER [Measures].[Stuff] AS

	[Date].[Calendar].CURRENTMEMBER.UNIQUENAME

SELECT {[Measures].[Stuff]}ON 0,
NON EMPTY
[Date].[Calendar].[Month].MEMBERS ON 1

FROM [Adventure Works]

--OR
WITH MEMBER [Measures].[Stuff] AS

	[Date].[Calendar].CURRENTMEMBER.LEVEL.NAME

SELECT {[Measures].[Stuff]}ON 0,
NON EMPTY
[Date].[Calendar].MEMBERS ON 1

FROM [Adventure Works]


--CALCULATE SALES FROM THE SAME PERIOD AS LAST YEAR: PARALLELPERIOD FUNCTION

-- ROLLING 12 MONTH QUERY
--NEED TO ADD THE RANGE

--USING AGGREGATE (TO AGGREGATE THE TOTALS) AND THE PARALLELPERIOD FUNCTION TO GET A ROLLING 12 MONTH TOTAL
--NOTE TO GO BACK 11 MONTH PERIODS
WITH MEMBER [Measures].[Rolling 12 Month Internet Sales] AS


AGGREGATE(
	{PARALLELPERIOD([Date].[Calendar].[Month]
	,11
	,[Date].[Calendar].CURRENTMEMBER)
	:[Date].[Calendar].CURRENTMEMBER},
	[Measures].[Internet Sales Amount]
	)

SELECT {[Measures].[Internet Sales Amount],[Measures].[Rolling 12 Month Internet Sales]} ON 0,

	[Date].[Calendar].[Month].MEMBERS ON 1

FROM [Adventure Works];


--new testing measure PREVMEMBER

WITH MEMBER [Measures].[Stuff] AS

	[Date].[Calendar].CURRENTMEMBER.PREVMEMBER.NAME

SELECT {[Measures].[Stuff]} ON 0,

[Date].[Calendar].[Calendar Quarter] ON 1

FROM [Adventure Works]

//.PREVMEMBER
//.NEXTMEMBER
//.LAG(1)

--NOW YOU CAN CALCULATE Q OVER Q RESULTS
WITH MEMBER [Measures].[Stuff] AS

	([Date].[Calendar].CURRENTMEMBER.PREVMEMBER,[Measures].[Internet Sales Amount])

SELECT {[Measures].[Stuff],[Measures].[Internet Sales Amount]} ON 0,

[Date].[Calendar].[Calendar Quarter] ON 1

FROM [Adventure Works]

--NOW YOU CAN CALCULATE Q OVER Q RESULTS
WITH MEMBER [Measures].[Stuff] AS

	([Date].[Calendar].CURRENTMEMBER.PREVMEMBER,[Measures].[Internet Sales Amount])

SELECT {[Measures].[Stuff],[Measures].[Internet Sales Amount]} ON 0,

[Date].[Calendar].MEMBERS ON 1

FROM [Adventure Works]

--NEXTMEMBER EXAMPLE SAME AS PREVMEMBER, BUT ONE PERIOD FORWARD
WITH MEMBER [Measures].[Stuff] AS

	([Date].[Calendar].CURRENTMEMBER.NEXTMEMBER,[Measures].[Internet Sales Amount])

SELECT {[Measures].[Stuff],[Measures].[Internet Sales Amount]} ON 0,

[Date].[Calendar].MEMBERS ON 1

FROM [Adventure Works]


--STACKING NEXTMEMBER FUNCTIONS APPLIES TO PREVMEMBER, NEXTMEMBER, PARENT, CHILD ETC

WITH MEMBER [Measures].[Stuff] AS

	([Date].[Calendar].CURRENTMEMBER.NEXTMEMBER.NEXTMEMBER.NEXTMEMBER,[Measures].[Internet Sales Amount])

SELECT {[Measures].[Stuff],[Measures].[Internet Sales Amount]} ON 0,

[Date].[Calendar].MEMBERS ON 1

FROM [Adventure Works];


--GETTING FIRST DAY OF THE YEAR RETURNED NO MATTER WHAT
--USEFUL FOR CALCULATING GROWTH SINCE THE BEGINNING OF THE YEAR

WITH MEMBER [Measures].[Stuff] AS

	ANCESTOR([Date].[Calendar].CURRENTMEMBER, [Date].[Calendar].[Calendar Year]).FIRSTCHILD.FIRSTCHILD.FIRSTCHILD.FIRSTCHILD.NAME

SELECT {[Measures].[Stuff],[Measures].[Internet Sales Amount]} ON 0,

[Date].[Calendar].MEMBERS ON 1

FROM [Adventure Works];


--USING A DESCENDANTS FUNCTION TO NAVIGATE DOWN IN A HIERARCHY BUT SAME AS ABOVE, BUT MORE DYNAMIC

WITH MEMBER [Measures].[Stuff] AS

DESCENDANTS(
	ANCESTOR([Date].[Calendar].CURRENTMEMBER, [Date].[Calendar].[Calendar Year])
	,[Date].[Calendar].[Date]
	).ITEM(0).NAME

SELECT {[Measures].[Stuff],[Measures].[Internet Sales Amount]} ON 0,
NON EMPTY
[Date].[Calendar].MEMBERS ON 1

FROM [Adventure Works];


--LAG EXAMPLE, NAVIGATE BACKWARDS AND FORWARDS

WITH MEMBER [Measures].[Stuff] AS

[Date].[Calendar].CURRENTMEMBER.LAG(-5).NAME

SELECT {[Measures].[Stuff],[Measures].[Internet Sales Amount]} ON 0,
NON EMPTY
[Date].[Calendar].MEMBERS ON 1

FROM [Adventure Works];
