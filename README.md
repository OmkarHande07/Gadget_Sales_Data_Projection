# Gadget_Sales_Data_Projection
Developed Realtime + Near-Realtime CDC (Change Data Capture) pipeline with different AWS services to process &amp; analyse the sales data of tech gadgets in near real time.

#Tech Stack:
1. Python Mock Generator Script.
2. AWS DynamoDB : Used as NoSQL database.
3. AWS DynamoDB Stream : Used for CDC (Change Data Capture).
4. AWS Kinesis Stream : Used as a replayable system where it actually holds data.
5. AWS EventBridge Pipe: Used for streaming ingestion.
6. AWS Kinesis Firehose : Used for batch streaming data.
7. Lambda : Used for transformation.
8. AWS Athena : Used for analysis.
9. AWS S3 : Used for storing records into bucket.
