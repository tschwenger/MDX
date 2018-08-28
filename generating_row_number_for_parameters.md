# Generate row number

      WITH

      MEMBER [Measures].[Count of Rows] AS
          Axis(1).Count

      MEMBER [Measures].[Count of Hierarchies on Rows] AS
          Axis(1).Item(0).Count

      MEMBER [Measures].[Row Number] AS 
          Rank( 
               StrToTuple( 
                          "( " +
                           Generate( Head(Axis(1),
                                          [Measures].[Count of Hierarchies on Rows] ) AS L,
                                    "Axis(1).Item(0).Item(" +
                                     CStr(L.CurrentOrdinal – 1) +
                                    ").Hierarchy.CurrentMember",
                                    " , " ) +
                          " )" 
                         ),
               Axis(1)
              )
          , FORMAT_STRING = "#,#"

      SELECT
          {
              –[Measures].[Count of Rows],
              –[Measures].[Count of Hierarchies on Rows],
              [Measures].[Row Number]
          } ON 0,
          {
              [Date].[Calendar].[Calendar Year].MEMBERS * 
              [Product].[Product Categories].[Category].MEMBERS
          } ON 1
      FROM
          [Adventure Works]
