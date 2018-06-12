# Cascading parameters SSRS

## Link to url resource

https://harshasrisampath.wordpress.com/2014/07/09/cascading-parameters-in-ssrsmdx/ 

## overview

Cascading parameters in Reporting Services (SSRS) in combination with MDX coding.

Cascading parameters is about defining multiple parameters in such a way 
that a list of values of  a parameter depends on the values of another parameter.

## sample scenario

Region Parameters value depend on the Company parameters,so the cascading parameter should show Region(KL,Pengen etc)  
when the first parameter is Company M&S Malaysia.  Again,  
when Company M&S Singapore is chosen the other parameter should be limited to Region (Changi,Harborfront).

## example code

    WITH 
    MEMBER [Measures].[ParameterCaption] AS iif(IsEmpty([Company].[Company Name].[Company Name]), null, 
    [City].[City Name].CURRENTMEMBER.MEMBER_CAPTION)

    MEMBER [Measures].[ParameterValue] AS iif(IsEmpty([Company].[Company Name].[Company Name]), null, 
    [City].[City Name].CURRENTMEMBER.UNIQUENAME)

    MEMBER [Measures].[ParameterLevel] AS iif(IsEmpty([Company].[Company Name].[Company Name]), null, 
    [City].[City Name].CURRENTMEMBER.LEVEL.ORDINAL)

    SELECT { 
    [Measures].[ParameterCaption], 
    [Measures].[ParameterValue], 
    [Measures].[ParameterLevel]} ON COLUMNS ,

    NONEMPTY([City].[City Name].ALLMEMBERS,[Company].[Company Name])  ON ROWS

    FROM ( SELECT ( STRTOSET(@CompanyCompanyName, CONSTRAINED))  ON COLUMNS

    FROM [Retail])
