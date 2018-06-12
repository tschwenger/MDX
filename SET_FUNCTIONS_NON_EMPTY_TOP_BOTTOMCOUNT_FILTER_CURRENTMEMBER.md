--PART 2 SET FUNCTIONS

	SELECT [Measures].[Reseller Sales Amount] ON 0,

	[Product].[Product].[Product] ON 1

	FROM [Adventure Works];


--HIDING EMPTY CELLS NO EMPTY SPACE AND NON EMPTY FUNCTION

	SELECT [Measures].[Reseller Sales Amount] ON 0,

	NON EMPTY [Product].[Product].[Product] ON 1

	FROM [Adventure Works];


--RETURNS VALUES WITH NON EMPTY CELLS, NON EMPTY GETS APPLIED IN LAST STEP OF THE ORDER OF OPERATIONS

	SELECT [Measures].[Reseller Sales Amount] ON 0,

	NON EMPTY [Product].[Product].[Product] ON 1

	FROM [Adventure Works]

	WHERE [Date].[Calendar Year].&[2006];

--HEAD AND ORDER FUNCTION TO SORT THE SET FOR US

	SELECT [Measures].[Reseller Sales Amount] ON 0,

	HEAD(
		ORDER([Product].[Product].[Product]
			,[Measures].[Reseller Sales Amount]
			,DESC)
		, 10) ON 1

	FROM [Adventure Works]

	WHERE [Date].[Calendar Year].&[2006];


--TOPCOUNT IS A COMBO OF ORDER AND HEAD

	SELECT [Measures].[Reseller Sales Amount] ON 0,

	TOPCOUNT([Product].[Product].[Product].MEMBERS
			, 10,
			[Measures].[Reseller Sales Amount]) ON 1

	FROM [Adventure Works]

	WHERE [Date].[Calendar Year].&[2006];

--BOTTOMCOUNT IS A COMBO OF ORDER AND TAIL

	SELECT [Measures].[Reseller Sales Amount] ON 0,

	BOTTOMCOUNT([Product].[Product].[Product].MEMBERS
			, 10,
			[Measures].[Reseller Sales Amount]) ON 1

	FROM [Adventure Works]

	WHERE [Date].[Calendar Year].&[2006];


--FILTER FUNCTION

	SELECT [Measures].[Reseller Sales Amount] ON 0,

	FILTER([Product].[Product].[Product].MEMBERS
			, [Measures].[Reseller Sales Amount] >= 10000) ON 1

	FROM [Adventure Works]

	WHERE [Date].[Calendar Year].&[2006];


--FILTER FUNCTION CURRENT MEMBER FUNCTION, this will return all the products with a product that starts with a C (left function is needed in this case)

	SELECT [Measures].[Reseller Sales Amount] ON 0,

	FILTER([Product].[Product].[Product].MEMBERS
			, LEFT([Product].[Product].CURRENTMEMBER.NAME,1) = "c") ON 1

	FROM [Adventure Works]

	WHERE [Date].[Calendar Year].&[2006];



