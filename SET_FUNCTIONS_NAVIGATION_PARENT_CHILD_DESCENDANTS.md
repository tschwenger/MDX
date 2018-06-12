--NAVIGATION FUNCTIONS CHILDREN TAKES YOU DOWN A LEVEL
--CHILDREN RETURNS A SET OF MEMBERS BELOW A HIERARCHY

SELECT [Measures].[Reseller Sales Amount] ON 0,

[Date].[Calendar].[Month].&[2006]&[3].CHILDREN ON 1


FROM [Adventure Works];


--PARENT TAKES YOU UP A LEVEL IN THE HIERARCHY

SELECT [Measures].[Reseller Sales Amount] ON 0,

[Date].[Calendar].[Month].&[2006]&[3].PARENT ON 1


FROM [Adventure Works];

--ANCESTOR TAKES YOU UP A LEVEL IN THE HIERARCHY

SELECT [Measures].[Reseller Sales Amount] ON 0,

ANCESTOR(
[Date].[Calendar].[Month].&[2006]&[3],2) 
ON 1

FROM [Adventure Works];


--DECENDANTS GOES DOWN IN ORDER

SELECT [Measures].[Reseller Sales Amount] ON 0,

DESCENDANTS(
[Date].[Calendar].[Calendar Year].&[2007],3)
ON 1

FROM [Adventure Works];

--DECENDANTS GOES DOWN IN ORDER WITH AN OPTION

SELECT [Measures].[Reseller Sales Amount] ON 0,

DESCENDANTS(
	[Date].[Calendar].[Calendar Year].&[2007]
	,3
	,SELF_AND_BEFORE)
ON 1

FROM [Adventure Works];


--ANOTHER EXAMPLE USING DESCENDANTS AND THE LEAVES FUNCTION
SELECT [Measures].[Reseller Sales Amount] ON 0,
DESCENDANTS(
	[Employee].[Employees].[Employee Level 02]
	, 
	, LEAVES)
ON 1
FROM [Adventure Works];


--SIBLINGS FUNCTION

SELECT [Measures].[Reseller Sales Amount] ON 0,

[Date].[Calendar].[Month].&[2006]&[3].SIBLINGS
ON 1

FROM [Adventure Works];
first
--FIRSTSIBLINGS FUNCTION

SELECT [Measures].[Reseller Sales Amount] ON 0,

[Date].[Calendar].[Month].&[2006]&[3].FIRSTSIBLING
ON 1

FROM [Adventure Works];

--LASTSIBLINGS FUNCTION

SELECT [Measures].[Reseller Sales Amount] ON 0,

[Date].[Calendar].[Month].&[2006]&[3].LASTSIBLING
ON 1

FROM [Adventure Works];

--LASTCHILD FUNCTION
SELECT [Measures].[Reseller Sales Amount] ON 0,

[Date].[Calendar].[Month].&[2006]&[3].LASTCHILD
ON 1

FROM [Adventure Works];

--FIRSTCHILD FUNCTION
SELECT [Measures].[Reseller Sales Amount] ON 0,

[Date].[Calendar].[Month].&[2006]&[3].FIRSTCHILD
ON 1

FROM [Adventure Works];
