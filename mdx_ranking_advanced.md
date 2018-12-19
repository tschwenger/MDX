

# This is an overview to rank across a crossproduct

In this example, ranking the entire result set.

Problem: Ranking across a cross product is tricky. Using a normal rank function will result in ranking the level of the hierarchy within 
the crossproduct

Example of what we want:

Ranking rev across an Org, City and Brand code, we want a constant ranking.

However normal rank function will only rank the org within the city and brand code.

  Example of bad ranking:

          With
            MEMBER [Measures].[revenue rank] AS 

                RANK(
                [Organization].[Field Org Hierarchy].CURRENTMEMBER,
                OrgSelection,
                [Measures].[revenue]
              )
           select 
           {[Measures].[revenue rank]
           ,[Measures].[revenue]
           }
           on 0,

           non empty
          { (OrgSelection,[Organization].[City State].[City State],[Organization].[Brand Code].[Brand Code]) }  on 1

          from cube
      
 This will return bad ranking
 
 
 correct ranking over the entire set
 
 
           With
            MEMBER [Measures].[revenue rank] AS 

                  RANK(


          ([Organization].[Field Org Hierarchy].CURRENTMEMBER,[Organization].[City State].currentmember,[Organization].[Brand Code].currentmember),
          //{ (OrgSelection,[Organization].[City State].[City State],[Organization].[Brand Code].[Brand Code]) } ),
          order(
            crossjoin(orgselection,[Organization].[City State].currentmember.level.members,[Organization].[Brand Code].currentmember.level.members
                )
            ,[Measures].[revenue],asc
          ),


          [Measures].[revenue]
        )
           select 
           {[Measures].[revenue rank]
           ,[Measures].[revenue]
           }
           on 0,

           non empty
          { (OrgSelection,[Organization].[City State].[City State],[Organization].[Brand Code].[Brand Code]) }  on 1

          from cube
      


