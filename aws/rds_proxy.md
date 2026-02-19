localstack rds setup
https://docs.localstack.cloud/aws/services/rds/

localstack rds-proxy demo
https://github.com/localstack-samples/sample-serverless-rds-proxy-demo


https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/rds-proxy-creating.html


measuring lambda cold start


filter @type = "REPORT"
| stats count(*) as invocations,
        percentile(@initDuration, 50) as p50_init,
        percentile(@initDuration, 90) as p90_init,
        percentile(@initDuration, 99) as p99_init 
        by bin(5m)
| sort by @timestamp desc


/aws/lambda/pfm-not-lambda-all-assets-batch-load-img-master-prod
4 (p50) - 5 (p90) second cold start time

