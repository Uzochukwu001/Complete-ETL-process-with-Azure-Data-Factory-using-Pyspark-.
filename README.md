## Complete-END-TO-END-ETL-process-with-Azure-Data-Factory-and-Databricks-using-Pyspark-.


This is a data engineering project that involves moving data in a format(JSON) from one Azure Blob Storage container to a second container in another format(CSV) and finally mounting on Databricks with relevant data transformations using pyspark. Below is the data architecture


![Data Architecture Photo](https://user-images.githubusercontent.com/112668327/210150282-49af0c41-3ffd-430b-ad61-deeedac3744f.png)



The steps implemented for the project was documented in the 'Uploaded' file which can be accessed from this repository.


Below are the codes run on the python notebook created on databricks:


- Mounting Blob Storage on Databricks

dbutils.fs.mount(
    source = "wasbs://target@storageaccounttesting0.blob.core.windows.net",
	mount_point = "/mnt/target1",
	extra_configs = {"fs.azure.account.key.storageaccounttesting0.blob.core.windows.net":"KeEozqigF94yHdr7rzwO1d6AK71ePd3gY+/4ggYVTD9keMfGhl0JAScyMNy5s39nqaIrwWrW1akE+AStblgpEA=="})


- Confirming Mounted File Path

%fs ls /mnt/target1


- Formatting the file as a csv dataframe with header.

df=spark.read.format("csv").option("header","True").option("inferSchema","True").load("dbfs:/mnt/target1/iris.txt")

Another way to read an imported file

df=spark.read.csv("filepath")

display(df)


- Common Dataframe operations

1. To list all columns

df.columns 

2. To display the first 150 rows

df.show(150) df.take(150) 

3. To display the last 10 rows

df.tail(10) 

4.  To display the two specified columns

df.select("column1","column2").show

5. To rename a column name to a new column name

df=df.withColumnRenamed("columnname","newcolumnname")

6. To delete the specified column

df=df.drop('columnname') 

7. To display the statistics of the dataframe

df.describe().show() 


- Group By Calculation.

df.groupby("species").count().show()


- Filter Operations.

df.filter(df.species=="setosa").show()


- To order by or sort a dataframe

df.sort(df.species.asc()).show()

df.orderBy(df.species.desc()).show()

- Create a view or temp table

df.createOrReplaceTempView("Table")


- Use sql to view the saved temp table.

%sql select * from Table


- To save the dataframe as a csv file.

Iris_csv=df.write.format("csv")


- The dataset was visualized and published on tableau public. The link can be seen below:

https://public.tableau.com/app/profile/uzochukwu.onwuegbu.vincent/viz/TheIrisDataset/Dashboard_?publish=yes


The dataset dashboard is also shown below:

![Tableau Visualization](https://user-images.githubusercontent.com/112668327/208252857-4771a19d-ffc5-4ab9-86fa-3ea3c94cde38.png)

