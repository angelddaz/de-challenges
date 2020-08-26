part 0:
1. Programmatically create a CSV dataset that is 100MB with four columns; unique integer, text, float, and boolean.
2. Create two scripts; one leveraging spark and another on pandas alone

part 1: 
1. In the pandas script, read_csv() 
2. time the performance of several data transformations:
* count of rows
* a sql or sql-like query, extracting rows based on unique col conditions

part 2:
1. In the pyspark script
* create a spark dataframe with correct structfields and datatypes specified.
* import the 100MB CSV into the dataframe
* partition across the unique integer column
2. Time the performance of several data transfomations:
* count of rows
* a sql or sql-like query, extracting rows based on unique col conditions
