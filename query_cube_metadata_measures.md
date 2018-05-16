# Query the cube for all the measures in the meta data to bring back all the measures

    //--Query the cubes Metadata for Measures (Must be put in as an expression query without Query Builder)
    //SELECT Measure_Name, Measure_Unique_Name, Default_Format_String
    //FROM $System.mdschema_measures
    //WHERE Cube_Name = 'Adventure Works'
    //ORDER BY Measure_Name
