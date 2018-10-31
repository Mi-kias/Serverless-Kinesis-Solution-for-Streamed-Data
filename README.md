# Serverless-Kinesis-Solution-for-Streamed-Data

Included Technologies: Cloudwatch, Elasticsearch, Kibana, Kinesis Firehose, Kinesis Analytics, S3
Languages: JSON

This project demonstrates the process for ingesting streamed Apache access log data through Kinesis Firehose, aggregating data for analysis with Kinesis Analytics, persisting the analyzed data to Elasticsearch, and visualizing data with Kibana.

Project summary can be found here: www.mikiasemanuel.com/serverless-kinesis-solution-for-streamed-data


Prerequisites:

Step 1: Save “Fake Apache Log Generator.zip” to a local directory and extract files.

Step 2: Create an S3 bucket: “apachetestbucket,” copy extracted “requirements.txt and “apache-.py” files to S3 bucket, set bucket and object permissions to: “Public Read.”

Step 3: Create an EC2 instance. Copy “EC2_Firehose_Bash” text and add to configuration details. 

Step 4: Provide EC2 instance a role with full EC2 and Kinesis Firehose permissions.

Step 5: Create Kinesis Firehose delivery streams outlined in Project Summary.

Step 6: SSH into instance and navigate to /etc/aws-test and edit the agent.json config file with the text from “Kinesis_Agent_JSON.”

Step 7: Start the Kinesis Agent manually: 'sudo service aws-kinesis-agent start'

Step 8: When configuring the Kinesis Analytics Application, use “Kinesis_Analytis_SQL.”
