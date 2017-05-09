--EXPLICIT TUPLE, THINK OF THIS AS AN X,Y COORDINATE, WE ARE NOT BUILDING SETS, THIS IS IDENTIFYING COORDINATES WITHIN A CUBE

SELECT ([Date].[Calendar Year].&[2006],[Measures].[Reseller Sales Amount])ON 0


FROM [Adventure Works];

--CREATING MULTIPLE EXPLICIT TUPLES WITHIN A QUERY, WHICH IS A MULTIPLE EXPLICIT TUPLE SET

SELECT {([Date].[Calendar Year].&[2006],[Measures].[Reseller Sales Amount])
		, ([Date].[Calendar Year].&[2007],[Measures].[Reseller Sales Amount])}
		ON 0


FROM [Adventure Works];


--This IS REFERRED TO AN ASSYMETRIC SET, BECAUSE THERE ARE DIFFERENT MEASURES IN THIS EXPLICIT TUPLE SET
--SSRS DOES NOT SUPPORT ASSYMETRIC MULTIPLE TUPLE SETS 
SELECT {([Date].[Calendar Year].&[2006],[Measures].[Reseller Sales Amount])
		, ([Date].[Calendar Year].&[2007],[Measures].[Reseller Sales Amount])
		,([Date].[Calendar Year].&[2007],[Measures].[Internet Sales Amount])}
		ON 0

FROM [Adventure Works];

--another example, notice how the dimensionality of the hierarchy needs to be the same
SELECT {([Date].[Calendar Year].&[2006],[Measures].[Reseller Sales Amount])
		, ([Date].[Calendar Year].&[2007],[Measures].[Reseller Sales Amount])
		,([Date].[Calendar Year].&[2007],[Measures].[Internet Sales Amount])}
		ON 0,
{
([Promotion].[Promotion].&[1],[Product].[Category].&[1]),
([Promotion].[Promotion].&[2],[Product].[Category].&[3])

} ON 1
FROM [Adventure Works];



--NavIGATIONAL TYPE FUNCTIONS
