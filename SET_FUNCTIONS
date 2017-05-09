--Filter function: filters the function


//ONLY RETURN PRODUCTS WITH SALES > 15K
SELECT {[Measures].[Reseller Sales Amount] } ON 0,
	
ORDER(
	FILTER(
	[Product].[Product].[Product].MEMBERS  //SET
	,[Measures].[Reseller Sales Amount] >15000  //SEARCH CONDITION
	)
	,[Measures].[Reseller Sales Amount]
	,ASC
	)ON 1

FROM [Adventure Works];


//ORDER(SET,MEASURE,ASC/DESC)

// ONLY RETURN PRODUCTS WHERE THE PRODUCT NAME STARTS WITH C
//SIMULATING THE SQL LIKE FUNCTION USING THE LEFT FUNCTION
SELECT {[Measures].[Reseller Sales Amount] } ON 0,
	

	FILTER(
	[Product].[Product].[Product].MEMBERS  //SET
	, LEFT([Product].[Product].CURRENTMEMBER.NAME,1) ="C" //SEARCH CONDITION
	)
ON 1

FROM [Adventure Works]

--Crossjoin: creates a set of tuples from two set. for more than two sets, use nested crossjoin functions or the * operator

SELECT {[Measures].[Reseller Sales Amount] } ON 0,
	
		[Product].[Category].[Category].MEMBERS*
		[Date].[Calendar Year].[Calendar Year].MEMBERS*
		[Sales Channel].[Sales Channel].[Sales Channel].MEMBERS ON 1

FROM [Adventure Works];







--Except Function: creates a set of members that exist only in the first set
--Except(set1, set2, [all])
//RETURN A SET THAT INCLUDES ALL CATEGORIES EXCEPT FOR BIKES

SELECT {[Measures].[Reseller Sales Amount] } ON 0,
	
    {[Product].[Category].&[4]
	,[Product].[Category].&[3]
	,[Product].[Category].&[2]}
	ON 1

FROM [Adventure Works];  //PROBLEM, BECAUSE IT'S HARDCODED SO IF MORE CATEGORIES ARE ADDED, THEN REPORTING WILL BE INACCURATE

//SO USE EXCEPT FUNCTION


SELECT {[Measures].[Reseller Sales Amount] } ON 0,
	
	EXCEPT([Product].[Category].[Category].MEMBERS
	,[Product].[Category].&[1])
	ON 1

FROM [Adventure Works]; 





--Generate Function: creates a new set by iteratively applying an expression to a specified set. For H loop. 
--Get a list of members in an array and iterate over the array one member at a time and applying the member to the set. And once it's done iterating, it then unions results back together to give one result set
--Generate(set 1, set 2, [all]) for string generate(set1, set2, [,delimiter])

// RETURN TOP 5 SELLING PRODUCTS PER YEAR
//PASS IN EACH YEAR AND THEN ITERATE FUNCTION OVER EACH YEAR AND THEN JOIN THE RESULTS TOGETHER

SELECT {[Measures].[Reseller Sales Amount] } ON 0,
	
	NON EMPTY
	GENERATE(
	[Date].[Calendar Year].[Calendar Year].MEMBERS,
		([Date].[Calendar Year].CURRENTMEMBER,
		TOPCOUNT([Product].[Product].[Product].MEMBERS
			,5
			,[Measures].[Reseller Sales Amount]
	)))
	ON 1

FROM [Adventure Works]; 



--Nonempty: eliminates members that result in empty tuple with the specified measure
--NonEmpty(set, measure)

//LOOK AT WITH BOTTOMCOUNT
//NEED TO MANIPULATE THE SET TO GET RID OF THE EMPTY VALUES

SELECT {[Measures].[Reseller Sales Amount] } ON 0,
	
	NON EMPTY
		BOTTOMCOUNT([Product].[Product].[Product].MEMBERS
			,10
			,[Measures].[Reseller Sales Amount]
	)
	ON 1

FROM [Adventure Works]; 

// NON EMPTY KEYWORD WILL NOT WORK IN THIS INSTANCE. WE WILL NEED TO USE NONEMPTY() FUNCTION


SELECT {[Measures].[Reseller Sales Amount] } ON 0,
	
	NON EMPTY
		BOTTOMCOUNT(
			NONEMPTY([Product].[Product].[Product].MEMBERS  //FIRST ARGUMENT IN THE BOTTOMCOUNT FUNCTION
					,[Measures].[Reseller Sales Amount])
			,10
			,[Measures].[Reseller Sales Amount]
	)
	ON 1

FROM [Adventure Works]; 
