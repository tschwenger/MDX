# Get dynamic 13 week date range

//Note: this works for the date hierarchy below of Y-Q-P-W adjust accordingly

select [Measures].[your measure] on 0,


non empty
//code below gives us the last 13 weeks of FTE
//Tail gets the last 13 weeks
// nonempty function gets us all non empty tuples with FTE values. The assumption is the last non empty tuple at the week level is the current week
//descendants is used to get the week level in the Y-Q-p-w hierarchy
// range is to get the current year and the previous year
//strtomember is a vb function used to make the year level dynamic
//format is used to convert the VB datetime function of now() (system date and time) converted to the current year
// the dateadd function is a vb date time function used in conjunction with the current year to get the prior year's date in conjunction with the format function
	
	tail(
		nonempty(
			descendants(StrToMember("[Date].[Fiscal Y-Q-P-W].[Fiscal Year].&[FY"+Format(DateAdd("yyyy",-1,now()),"yyyy")+"]")
						:StrToMember("[Date].[Fiscal Y-Q-P-W].[Fiscal Year].&[FY"+Format(now(),"yyyy")+"]"),3)
		,[Measures].[your measure] )
	,13)

	on 1

	from [your ssas data model]
