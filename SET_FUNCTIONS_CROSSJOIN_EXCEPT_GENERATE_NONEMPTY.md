--EXAMPLE OF A CROSS JOIN

	SELECT [Measures].[Reseller Sales Amount] ON 0,

	CROSSJOIN(
		[Product].[Category].[Category].MEMBERS,
		[Date].[Calendar Year].[Calendar Year].MEMBERS
	) 
	ON 1
	FROM [Adventure Works]

--EXAMPLE OF A NESTED CROSS JOIN

	SELECT [Measures].[Reseller Sales Amount] ON 0,

	CROSSJOIN(
	CROSSJOIN(
		[Product].[Category].[Category].MEMBERS,
		[Date].[Calendar Year].[Calendar Year].MEMBERS
	)
		,[Sales Channel].[Sales Channel].[Sales Channel].MEMBERS
	) ON 1

	FROM [Adventure Works];


--EXAMPLE OF EFFICIENT CROSS JOIN WITH 

	SELECT [Measures].[Reseller Sales Amount] ON 0,

	([Product].[Category].[Category].MEMBERS
		,[Date].[Calendar Year].[Calendar Year].MEMBERS
		,[Sales Channel].[Sales Channel].[Sales Channel].MEMBERS)
	ON 1
	FROM [Adventure Works];


--THIRD WAY OF CROSSJOINING WITH *

	SELECT [Measures].[Reseller Sales Amount] ON 0,

	[Product].[Category].[Category].MEMBERS*
	[Date].[Calendar Year].[Calendar Year].MEMBERS*
	[Sales Channel].[Sales Channel].[Sales Channel].MEMBERS ON 1
	FROM [Adventure Works];

--ADD A FOURTH SET

	SELECT [Measures].[Reseller Sales Amount] ON 0,

	[Product].[Category].[Category].MEMBERS*
	[Date].[Calendar Year].[Calendar Year].MEMBERS*
	[Sales Channel].[Sales Channel].[Sales Channel].MEMBERS* 
	[Reseller].[Business Type].MEMBERS ON 1
	FROM [Adventure Works];


--BUILDING AN EXAMPLE FOR THE EXCEPT FUNCTION

	SELECT [Measures].[Reseller Sales Amount] ON 0,

	([Product].[Product].[Product].MEMBERS,[Date].[Calendar Year].&[2007])
	ON 1
	FROM [Adventure Works];


--TOP 10 BEST SELLERS IN A YEAR
	
	SELECT [Measures].[Reseller Sales Amount] ON 0,

	TOPCOUNT(
	([Product].[Product].[Product].MEMBERS,[Date].[Calendar Year].&[2007])
	, 10
	,[Measures].[Reseller Sales Amount]
	)
	ON 1
	FROM [Adventure Works];

	SELECT [Measures].[Reseller Sales Amount] ON 0,

	TOPCOUNT(
	([Product].[Product].[Product].MEMBERS,[Date].[Calendar Year].&[2006])
	, 10
	,[Measures].[Reseller Sales Amount]
	)
	ON 1
	FROM [Adventure Works];

--TOP 10 BEST SELLERS IN THIS YEAR THAT WERE NOT IN THE PREVIOUS YEAR

	SELECT [Measures].[Reseller Sales Amount] ON 0,

	EXCEPT(
	TOPCOUNT(
	([Product].[Product].[Product].MEMBERS,[Date].[Calendar Year].&[2007])
	, 10
	,[Measures].[Reseller Sales Amount]
	),
	TOPCOUNT(
	([Product].[Product].[Product].MEMBERS,[Date].[Calendar Year].&[2006])
	, 10
	,[Measures].[Reseller Sales Amount])
	)
	ON 1
	FROM [Adventure Works];

--bACK TO LOOKING AT TOP 5 BEST PRODUCTS WITH A WRONG ANSWER, BECAUSE THE ISSUE IS THE CROSSJOIN THAT TAKES THE YEARS AND TOP COUNTS FOR ALL YEARS INSTEAD OF 
--OF THE FOR EACH YEAR. SO THIS QUERY IS SHOWING THE TOP 5 BEST SELLING OF ALL TIME AND WILL SHOW THE SETS INCORRECTLY. WE WANT TO DEFINE TOP 5 RESULTS OF EACH YEAR AND THEN UNION THEM TOGETHER. THIS CAN BE DONE WITH THE GENERATE FUNCTION

	SELECT [Measures].[Reseller Sales Amount] ON 0,

	(
	[Date].[Calendar Year].[Calendar Year].MEMBERS,
	TOPCOUNT(
	([Product].[Product].[Product].MEMBERS)
	, 5
	,[Measures].[Reseller Sales Amount]
	)
	)
	ON 1
	FROM [Adventure Works];


--GENERATE FUNCTION AND GETTING THE TOP 5 SELLERS FOR EACH YEAR
-- 2 ARGUMENTS, 1 ARGUMENT: MEMBERS WE WANT TO ITERATE THROUGH (IN THIS CASE MEMBERS OF THE CALENDAR YEAR HIERARCHY), 2 ARGUMENT: THE SET YOU WANT TO APPLY THE ITERATE OVER TO EACH MEMBER IN THE FIRST SET
-- SO THE CURRENTMEMBER IS ADDED TO THE CROSSJOIN, BECAUSE WE WANT TO LOOP OVER THE FIRST SET IN 2005, 2006 ETC, SO YOU NEED THE CURRENTMEMBER
--SO IN THIS CASE WE WANT TO APPLY THE TOP 5 PRODUCTS TO EACH CURRENT MEMBER (CALENDAR YEAR NUMBER) WHEN WE LOOP OVER 

	SELECT [Measures].[Reseller Sales Amount] ON 0,

	GENERATE(
		[Date].[Calendar Year].[Calendar Year].MEMBERS
		,[Date].[Calendar Year].CURRENTMEMBER*
		TOPCOUNT(
			([Product].[Product].[Product].MEMBERS)
			, 5
			,[Measures].[Reseller Sales Amount]
			)
		) ON 1
	FROM [Adventure Works];

--what if you want to get the worst selling products that were actually sold (not null products)
--only consider products that ACTUALLY HAD A SALE AMOUNT
--USE NONEMPTY FUNCTION

	SELECT [Measures].[Reseller Sales Amount] ON 0,

		BOTTOMCOUNT(
			NONEMPTY([Product].[Product].[Product].MEMBERS,[Measures].[Reseller Sales Amount])
			, 5
			,[Measures].[Reseller Sales Amount]
			) ON 1
	FROM [Adventure Works];

--BOTTOM COUNT OVER THE YEARS

	SELECT [Measures].[Reseller Sales Amount] ON 0,

	GENERATE(
		[Date].[Calendar Year].[Calendar Year].MEMBERS
		,[Date].[Calendar Year].CURRENTMEMBER*
			BOTTOMCOUNT(
					NONEMPTY([Product].[Product].[Product].MEMBERS,[Measures].[Reseller Sales Amount])
				, 5
				,[Measures].[Reseller Sales Amount]
				)
		) ON 1
	FROM [Adventure Works];

