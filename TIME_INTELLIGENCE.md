- ADDING CALCULATIONS TO A QUERY (TIME INTELLIGENCE)
//// WTD, MTD, QTD, YTD AND PARRALLELPERIOD
//// EACH FUNCTION RETURNS A SET OF MEMBERS
////DEFAULT DATE MEMBER IS THE CURRENTMEMBER UNLESS OTHERWISE SPECIFIED
////CREATES SET FROM FIRST PERIOD OF THE YEAR TO SPECIFIED MEMBER

		WITH 

		MEMBER [Measures].[YTD Sales] AS

		AGGREGATE(
		YTD([Date].[Calendar].CURRENTMEMBER)
		,[Measures].[Internet Sales Amount]
		)

		MEMBER [Measures].[MTD Sales] AS

		AGGREGATE(
		MTD([Date].[Calendar].CURRENTMEMBER)
		,[Measures].[Internet Sales Amount]
		)

		MEMBER [Measures].[PRIOR YEAR SALES] AS

		(PARALLELPERIOD([Date].[Calendar].[CALENDAR YEAR]
			,1
			,[Date].[Calendar].CURRENTMEMBER)
			,[Measures].[Internet Sales Amount])

		MEMBER [Measures].[Year over Year Growth] AS

		[Measures].[Internet Sales Amount]-[Measures].[PRIOR YEAR SALES]

		MEMBER [Measures].[PRIOR MONTH SALES] AS

		(PARALLELPERIOD([Date].[Calendar].[Month] ////ALL YOU HAVE TO DO IS CHANGE THE LEVEL
			,1
			,[Date].[Calendar].CURRENTMEMBER)
			,[Measures].[Internet Sales Amount])

		MEMBER [Measures].[MONTH over MONTH Growth] AS

		[Measures].[Internet Sales Amount]-[Measures].[PRIOR MONTH SALES]

		MEMBER [MEASURES].[PRIOR YTD SALES] AS

		(PARALLELPERIOD([Date].[Calendar].[Calendar Year]
			,1
			,[Date].[Calendar].CURRENTMEMBER)
		,[Measures].[YTD Sales] )

		SELECT {[Measures].[YTD Sales]
		,[Measures].[Internet Sales Amount]
		,[Measures].[MTD Sales]
		,[Measures].[PRIOR YEAR SALES]
		,[Measures].[Year over Year Growth]
		,[Measures].[PRIOR MONTH SALES]
		,[Measures].[MONTH over MONTH Growth]
		,[MEASURES].[PRIOR YTD SALES] } ON 0,

		[Date].[Calendar].[MONTH].MEMBERS ON 1

		FROM [Adventure Works];
