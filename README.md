# Serverless-Kinesis-Solution-for-Streamed-Data

Included Technologies: Cloudwatch, Elasticsearch, Kibana, Kinesis Firehose, Kinesis Analytics, S3

Languages: JSON

This project demonstrates the process for ingesting streamed Apache access log data through Kinesis Firehose, aggregating data for analysis with Kinesis Analytics, persisting the analyzed data to Elasticsearch, and visualizing data with Kibana.

Project walkthrough can be found here: http://www.mikiasemanuel.com/serverless-kinesis-solution-for-streamed-data/

## Prerequisites:

### Create an S3 bucket: “apachetestbucket,” copy “requirements.txt and “apache-.py” files from "Fake-Apache-Log-Generator" to S3 bucket, set bucket and object permissions to: “Public Read.”

### Configure an EC2 instance. Copy “EC2_Firehose_Bash” text and add to configuration details. *Please make sure to change wget command to navigate to S3 bucket location.*

### Provide EC2 instance a role with full EC2 and Kinesis Firehose permissions.

### SSH into instance and navigate to /etc/aws-test and edit the agent.json config file with the text from “KinesisAgentJSON:"
```
{
  "cloudwatch.emitMetrics": true,
  "cloudwatch.endpoint": "",
  "firehose.endpoint": "",

  "flows": [
    {
      "filePattern": "/usr/apachetest/access_log_????????-??????.log*",
      "deliveryStream": "KF-Log-Ingest",
	  "dataProcessingOptions":	[
	  {
		"initialPosition": "START_OF_FILE",
		"maxBufferAgeMillis": "2000",
		"optionName": "LOGTOJSON",
		"logFormat": "COMBINEDAPACHELOG"
		}]
    }
  ]
}
```

### Start the Kinesis Agent manually:
```
sudo service aws-kinesis-agent start
```

### When configuring the Kinesis Analytics Application, use “Kinesis_Analytis_SQL” file:
```
CREATE OR REPLACE STREAM "DESTINATION_SQL_STREAM" (datetime TIMESTAMP, status INTEGER, statusCount INTEGER);
	
CREATE OR REPLACE PUMP "STREAM_PUMP" AS INSERT INTO "DESTINATION_SQL_STREAM"
SELECT STREAM ROWTIME as datetime, "response" as status, COUNT(*) AS statusCount
FROM "SOURCE_SQL_STREAM_001"
GROUP BY "response", FLOOR(("SOURCE_SQL_STREAM_001".ROWTIME - TIMESTAMP '1970-01-01 00:00:00') minute / 1 TO MINUTE);
```

