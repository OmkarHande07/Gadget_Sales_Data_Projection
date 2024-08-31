# Gadget_Sales_Data_Projection
Developed Realtime + Near-Realtime CDC (Change Data Capture) pipeline with different AWS services to process &amp; analyse the sales data of tech gadgets in near real time.

# Tech Stack:
1. Python Mock Generator Script.
2. AWS DynamoDB : Used as NoSQL database.
3. AWS DynamoDB Stream : Used for CDC (Change Data Capture).
4. AWS Kinesis Stream : Used as a replayable system where it actually holds data.
5. AWS EventBridge Pipe: Used for streaming ingestion.
6. AWS Kinesis Firehose : Used for batch streaming data.
7. Lambda : Used for transformation.
8. AWS Athena : Used for analysis.
9. AWS S3 : Used for storing records into bucket.

# Architecture:

![Project 2 - Architecture ](https://github.com/user-attachments/assets/0839788e-2df5-439e-8695-ffec03a70fb1)

# Steps to design data pipeline:

1. Start with AWS DynamoDB by creating a table with a partition key.
2. Run the script for generating mock data (mock_data_generator_for_dynamodb.py) to run it theiry must be user register under the IAM and also configure the AWS CLI using access key and secret key.
3. After running the script data will be inserted into the table.
   
   ![image](https://github.com/user-attachments/assets/195032d0-468f-4084-83e9-25204aec9ebf)
   
4. **For capturing any updates using DynamoDB Stream:**
DynamoDB Tables -> click on table -> Export and Streams -> scroll down to DynamoDB stream details -> select New Image and Turn on stream.
   
   ![image](https://github.com/user-attachments/assets/ceff5c91-53a8-4b5d-bc75-2d446197f20f)
5. Now go to AWS Kinesis and create Data stream.

![image](https://github.com/user-attachments/assets/4fff84eb-d55c-4716-9697-1fee4d03ea3b)

6. **For capturing CDC from DynamoDB to Kinesis:**
go to AWS EventBridge -> EventBridge Pipes->create pipe with name -> will need only source and target so remove the remaining options -> attach source as DynamoDB ->DynamoDB stream as table_name -> Starting position as latest 
In Target -> select service as Kinesis stream -> stream as kinesis-sales-order -> partion key as eventID -> create pipe.

![image](https://github.com/user-attachments/assets/16aa8551-e7a3-40c0-86ca-2a7f6900de9a)
7. Once pipe gets created go to Permissions and click on Execution role -> Add Permission -> Attach Policy -> add Kinesis Full Access and DynamoDB Full Access.

8. **For checking the record in real time:**
again run the generator script -> open Kinesis Data Stream -> click on Data Viewer -> Choose any Shard -> Starting Position as Timestamp -> Start data as Date -> Time as 00:00:00 -> Get Record the records will be listed in real-time.

![image](https://github.com/user-attachments/assets/12942f91-01f4-4be2-9086-e896384c6983)
as it is a CDC pipeline any updates to the record can also be listed.
9. Now setup AWS Kinesis Firehose -> create Firehose Stream -> source as Kinesis Data Stream -> Destination as S3 -> select already created Kinesis Data Stream -> update stream name -> click on Transform source records with AWS Lambda ( here we will use AWS Lambda for transformation as it will get batch of some records from Firehose then lambda will apply transformation on top of it and create mini batches of that transformed records then finally records will be dumped into S3 bucket)
use (transformation_layer_with_lambda.py) -> in Destination Settings -> select the S3 bucket for storing the records -> create stream.

Here after running the script,data is getting generated - which will be captured by EventBridge pipe  - data will get into Kinesis Stream - batches will be generated from that data by Kinesis Firehose - Lambda will apply transformation on that - records will be dumped into S3 bucket.

10. **To identify and parse Json Data:**
    AWS Glue -> create Crawler -> Source as S3 bucket -> Create Customer Classifier -> select JSON from types -> In json path write ($.orderid,$.product_name,$.quantity,$.price) ( for getting tabular view in AWS Athena we have to add this path) -> select the DataBase -> save and run crawler
     glue crawler 
  ![image](https://github.com/user-attachments/assets/e1544c87-f14f-4498-b800-5ca28affa8fa)

11. After running the crawler meta data table will be created under Tables.
    ![image](https://github.com/user-attachments/assets/98acfc56-42de-4fe2-a5c5-487cbdc292be)
12. Open AWS Athena -> Query Editor -> select DataBase and Table -> in tables click on (+) -> in (:) select Preview Table -> now we are getting Realtime data sync using the pipeline    
    ![image](https://github.com/user-attachments/assets/3048e4aa-6bca-464a-b64f-1212a65a126b)

    ![image](https://github.com/user-attachments/assets/b75bc575-3002-49f8-b3d5-d43c0f34bdc0)





   
 

   

