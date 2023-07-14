
https://medium.com/@schogini/aws-kinesis-data-streams-a-tiny-cli-demo-9a20813bd082

```
awslocal kinesis get-shard-iterator --shard-id shardId-000000000000 --shard-iterator-type TRIM_HORIZON --stream-name cat-onboarding-demo

awslocal kinesis get-records --shard-iterator AAAAAAAAAAFjOlj2mOq3QpJypaular/3TRwNHRXVjZdwHxrd/wDuOLrWmjMOvl3rimFaAvTcPkQ2HEVtNQ59TmPkzuidbSdlXmPszMEilAgV1sHYnV1dtc05rkmWLD/PBL1Z72sH99OM7aBwGnCglsxxtn+zdJc8knPZkxtwTsh/EVz/9mSiZ+2dMI441IlXsPWi69pTlTfR/OmkZv1u5QEbwKwz65Zs

awslocal kinesis put-record --stream-name cat-onboarding-demo --partition-key 1 --data '{"value":"123"}'
```