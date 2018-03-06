# MDX for setting up parameters for usage in SSRS

//this enables the report developere to set up the parameters for the ssrs query

WITH
MEMBER    Measures.ParameterCaption AS [Date].[Calendar].CURRENTMEMBER.MEMBER_CAPTION
MEMBER    Measures.ParameterUniqueName AS [Date].[Calendar].CURRENTMEMBER.UNIQUENAME
MEMBER    Measures.ParameterLevel AS [Date].[Calendar].CURRENTMEMBER.LEVEL.ORDINAL

SELECT
{
[Measures].[ParameterCaption]
,    [Measures].[ParameterUniqueName]
,    [Measures].[ParameterLevel]
}
ON COLUMNS,
{
[Date].[Calendar].[Calendar Year].Members
}
ON ROWS 

FROM [Adventure Works]
