
Discovering run-id for a check in gh
https://github.com/cli/cli/issues/3558#issuecomment-1426779325
```
It's a bit tricky to do this now with the `gh` cli. While we wait for an easier solution to be implemented, I came up with a [temporary solution](https://gist.github.com/nitrocode/c95d915f7589f4c2ba4522834035aff0#get-output-of-a-github-actions-run).

We can use PR [#6990](https://github.com/cli/cli/pull/6990) as an example

```shell
$ org=cli
$ repo=cli
$ ref=$(gh pr view -R cli/cli 6990 --json 'commits' --jq '.commits | last | .oid')
$ echo $ref
b07410d48554bc39594850dc74e5384c5edca6cf
$ gh api \
  -H "Accept: application/vnd.github+json" \
  /repos/$org/$repo/commits/$ref/check-runs \
  --jq '[ .check_runs[] | {name: .name, run_id: .details_url | capture("(?<id>[0-9]+)") | .id } ]' | jq .
[
  ...
  {
    "name": "lint",
    "run_id": "4118227632"
  }
]
```

Then choose a run id and view it


### Using GH API to rerun failed PR checks
Note: this doesn't work for external checks currently (e.g. Azure)

```
$ gh run -R cli/cli view 4118227632
✓ remote-add-no-fetch Lint #6990 · 4118227632
Triggered via push about 3 days ago

JOBS
✓ lint in 2m26s (ID 11178577059)

For more information about the job, try: gh run view --job=11178577059
View this run on GitHub: https://github.com/cli/cli/actions/runs/4118227632
```

```
org=cat-digital-platform
repo=P-Notifications-API
ref=$(gh pr view -R cat-digital-platform/P-Notifications-API 2748 --json 'commits' --jq '.commits | last | .oid')

gh api \
  -H "Accept: application/vnd.github+json" \
  /repos/$org/$repo/commits/$ref/check-runs \
  --jq '[ .check_runs[] | {name: .name, run_id: .details_url | capture("(?<id>[0-9]+)") | .id } ]' | jq .

[
  ...
[
  {
    "name": "P-Datahub-Notifications_API_V2-CI",
    "run_id": "7"
  },
  {
    "name": "P-Datahub-Notifications_API_V2-CI (Build & Tests Notification Subscription API Functional Tests)",
    "run_id": "7"
  },]

https://learn.microsoft.com/en-us/rest/api/azure/devops/build/stages/update?view=azure-devops-rest-7.1

PATCH https://dev.azure.com/{organization}/{project}/_apis/build/builds/{buildId}/stages/{stageRefName}?api-version=7.1


{"state":1,"forceRetryAllJobs":false,"retryDependencies":true}


Request URL: https://dev.azure.com/cat-digital/7dc32e0c-84d4-46a4-aec6-1f0a22b60ef8/_apis/build/builds/6185860/stages/SonarCheckStatusStage
Request Method:
PATCH
Status Code:
204 No Content
Remote Address:
13.107.42.20:443
Referrer Policy:
strict-origin-when-cross-origin
access-control-allow-headers:
authorization
access-control-allow-methods:
OPTIONS,GET,POST,PATCH,PUT,DELETE
access-control-allow-origin:
*
access-control-expose-headers:
ActivityId,X-TFS-Session,X-MS-ContinuationToken,X-VSS-GlobalMessage,ETag
access-control-expose-headers:
Request-Context
access-control-max-age:
3600
activityid:
cebaeef9-c0c4-4d6b-906b-598233df48a5
cache-control:
no-cache
date:
Tue, 11 Feb 2025 15:48:02 GMT
expires:
-1
p3p:
CP="CAO DSP COR ADMa DEV CONo TELo CUR PSA PSD TAI IVDo OUR SAMi BUS DEM NAV STA UNI COM INT PHY ONL FIN PUR LOC CNT"
pragma:
no-cache
request-context:
appId=cid-v1:f88d1338-e7ba-4d7b-83e0-e573c40a2f62
strict-transport-security:
max-age=31536000; includeSubDomains
x-cache:
CONFIG_NOCACHE
x-content-type-options:
nosniff
x-msedge-ref:
Ref A: 1034B681181A4EDCBA08F39347CBBEF4 Ref B: BL2AA2030110019 Ref C: 2025-02-11T15:48:03Z
x-tfs-processid:
dbfae999-cd89-4efb-bc88-1e488ee4a32d
x-tfs-session:
57f5f45b-54a8-4dcb-859e-14241452f1a8
x-vss-e2eid:
cebaeef9-c0c4-4d6b-906b-598233df48a5
x-vss-senderdeploymentid:
371dc223-5e58-9481-da2c-e75ac4f20939
x-vss-userdata:
e1d380ab-721c-60a2-9cb0-446611ccb3c3:lance.gatlin@cat.com
:authority:
dev.azure.com
:method:
PATCH
:path:
/cat-digital/7dc32e0c-84d4-46a4-aec6-1f0a22b60ef8/_apis/build/builds/6185860/stages/SonarCheckStatusStage
:scheme:
https
accept:
application/json;api-version=6.0-preview.1;excludeUrls=true;enumsAsNumbers=true;msDateFormat=true;noArrayWrap=true
accept-encoding:
gzip, deflate, br, zstd
accept-language:
en-US,en;q=0.9
authorization:
Bearer ***
content-length:
62
content-type:
application/json
cookie: ***
origin:
https://dev.azure.com
priority:
u=1, i
referer:
https://dev.azure.com/cat-digital/Cat%20Digital/_build/results?buildId=6185860&view=results
sec-ch-ua:
"Not A(Brand";v="8", "Chromium";v="132", "Google Chrome";v="132"
sec-ch-ua-mobile:
?0
sec-ch-ua-platform:
"macOS"
sec-fetch-dest:
empty
sec-fetch-mode:
cors
sec-fetch-site:
same-origin
user-agent:
Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/132.0.0.0 Safari/537.36
x-tfs-session:
57f5f45b-54a8-4dcb-859e-14241452f1a8
x-vss-clientauthprovider:
MsalTokenProvider

```