# Querying data in AWS Athena, an HDFS based Architecture

## Description

For this, we will be connecting to an external data source, loading it into a S3 bucket that you create and query it using [AWS Athena](https://aws.amazon.com/athena/?whats-new-cards.sort-by=item.additionalFields.postDateTime&amp;whats-new-cards.sort-order=desc). The data set we will use is the 2019 IRS submissions from the IRS 990 database – the same we used in Lab #4

Here are the steps to follow:

- Set-up the infrastructure and create the database
- Load the data in S3
- Create the connections and AWS ETL job using Glue
- Query the results

The background of this dataset can be found in the following documentation:

- [https://docs.opendata.aws/irs-990/readme.html](https://docs.opendata.aws/irs-990/readme.html)
- [https://aws.amazon.com/opendata/public-datasets/](https://aws.amazon.com/opendata/public-datasets/)

The goal is to generate a CSV file from public data source (IRS 990) data and load it into a data store that you created (S3 bucket) which we will connect to using s3 and Athena.

**What is Athena?**
 ![](./images/screenshot_1.png)
 ![](./images/screenshot_2.png) 


## Set up the Infrastructure and Create the Database

The following depicts the framework we will create for this solution:
 ![](./images/framework.png)

## Part I. Load the data in S3 bucket

Now that we have set up the database connection, we will create an S3 bucket called &#39;irs-990&#39;. This is where we will run our script to update the S3 bucket.

- **Step 1.** Log into AWS Console and create an S3 bucket called **&#39;irs-990/database/&#39;.**
- **Step 2**. Configure the bucket so that it is publicly accessible.
- **Step 3.** For more information on how to create an S3 bucket, refer to the materials in class or follow this documentation [here](https://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html).

See the s3 bucket below:
 ![](./images/screenshot_3.png)

## Part II. Create a Database using Athena and AWS Glue

Now that we have created your S3 bucket, we will need to load the data from IRS 990 into that database. The data file is provided for convenience. Of course, we can run an AWS Glue job to update this file directly. _Note: you can create an AWS crawler directly from the Athena Interface._

- **Step 1.** Load the CSV file into your S3 bucket that you specified above. This will serve as your data source
- **Step 2.** Navigate to Athena from the AWS Management Console
- **Step 3.** From the Athena console, navigate to the tab called &#39;Data Sources&#39;
- **Step 4**. Select &#39;Connect data source&#39;
- **Step 5.** From this screen, select Query data in S3 and select AWS Glue data catalog.
  - This allows you to use Glue and S3 as a data source. Here is where Athena and S3 act as a large clustered, file system much like HDFS.
- **Step 6.** Select &quot;Set up Crawler in Glue&quot;
- **Step 7.** Set up a crawler with the name _M12\_Assignment\_Crawler_

![](./images/screenshot_5.png)

- **Step 8.** Add a datastore and under the &quot;specified path for my account&quot;, put the path of the folder where your file is stored. Click next, and choose not to add any more data stores.

![](./images/screenshot_6.png)

- **Step 9.** Choose the IAM role corresponding with access to administering Glue functions and connecting to S3. _Note: you would have created this in Lab 4._
- **Step 10.** Create a schedule for your crawler and have it run daily. This will look for updated data in this folder daily.

![](./images/screenshot_7.png)

- **Step 11**. You will want to store the output of contents of the crawler. Therefore, you&#39;ll need to create a database. Select &quot;Add database&quot; and give it a name.

- **Step 12.** Once saved, run the crawler on demand.

![](./images/screenshot_8.png)


## Part III. Querying your data

Once we have established a pipeline for loading the IRS 990 data into a bucket, the next step is to query the data.

Follow the steps below to query the data stored in your s3 bucket.

- **Step 1.** From the Athena &#39;Query Editor&#39; tab, navigate to your database and write the query that corresponds with your database:

```**SELECT \* FROM &quot;irs\_database&quot;.&quot;irs\_990\_data\_table&quot; limit 10;**```

![](./images/screenshot_9.png)

## Part IV. Answer questions by querying your data:

1. **Question 1:** How many records are in the file? Take a screenshot of your results and save the query to a text file.
2. **Question 2:** How many records are available by form type? Take a screenshot of your results and save the query to a text file.
3. **Question 3:** How many records were submitted in the month of March? Take a screenshot of your results and save the query to a text file.

##

Submission:
 Please upload a zipped package of your screenshots and the queries as a response to the M12 Assignment.

