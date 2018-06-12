
--order function, that includes all sales--

	SELECT [Measures].[Reseller Sales Amount] ON 0,
	Order(
		[Product].[Product].MEMBERS
		, [Measures].[Reseller Sales Amount]
		, DESC
		) ON 1
	FROM [Adventure Works];

--specifying the level to get rid of all sales amount--

	SELECT [Measures].[Reseller Sales Amount] ON 0,
	Order(
		[Product].[Product].[Product].MEMBERS
		, [Measures].[Reseller Sales Amount]
		, DESC
		) ON 1
	FROM [Adventure Works];

--or just use the children function to get rid of all amount--

	SELECT [Measures].[Reseller Sales Amount] ON 0,
	Order(
		[Product].[Product].CHILDREN
		, [Measures].[Reseller Sales Amount]
		, DESC
		) ON 1
	FROM [Adventure Works];

--ascending order--

	SELECT [Measures].[Reseller Sales Amount] ON 0,
	Order(
		[Product].[Product].CHILDREN
		, [Measures].[Reseller Sales Amount]
		, ASC
		) ON 1
	FROM [Adventure Works];

--example of correct ordering, but in the order of the hierarchy

	SELECT [Measures].[Reseller Sales Amount] ON 0,
	Order(
		[Product].[Product Categories].MEMBERS
		, [Measures].[Reseller Sales Amount]
		, ASC
		) ON 1
	FROM [Adventure Works];

--USING BDESC WILL BREAK THE STRUCTURE OF THE HIERARCHY

	SELECT [Measures].[Reseller Sales Amount] ON 0,
	Order(
		[Product].[Product Categories].MEMBERS
		, [Measures].[Reseller Sales Amount]
		, BDESC
		) ON 1
	FROM [Adventure Works];

--HEAD JUST LIKE THE R HEAD FUNCTION, RETURNS THE FIRST 3 OF DEFAULT ORDER IN THE CUBE

	SELECT [Measures].[Reseller Sales Amount] ON 0,
	HEAD(
		[Product].[Product Categories].MEMBERS,3) ON 1
	FROM [Adventure Works];


--TAIL DOES THE LAST VALUES

	SELECT [Measures].[Reseller Sales Amount] ON 0,
	HEAD(
		[Product].[Product Categories].MEMBERS,5) ON 1
	FROM [Adventure Works];

--TOP 5 PRODUCTS

	SELECT [Measures].[Reseller Sales Amount] ON 0,
	HEAD(
		ORDER(
		[Product].[Product].CHILDREN
		, [Measures].[Reseller Sales Amount]
		, DESC)
		, 5)
	 ON 1
	FROM [Adventure Works];

