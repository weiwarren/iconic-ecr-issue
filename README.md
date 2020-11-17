
### 1. automated build on master branch ###

| `2020-08-26T05:37:29Z` | Tags: 0d8a4c9, master-0d8a4c9 | Image 'sha256:40e664b403086c18aa2732597bb0bf64f9a83f978607b19781cbef8e02bee168' pushed 


| Tag | sha |
| --- | --- |
| 0d8a4c9 | 40e66...168 |
| master-0d8a4c9 | 40e66...168 |
### 2. automated build on staging branch resulted the tag 0d8a4c9  refers to the new image ###
| `2020-08-31T00:41:11Z` | Tags: 0d8a4c9, staging-0d8a4c9 | Image 'sha256:3fbc1092bbcf068d189e769e19fa736111f6e64c0fce31adcb915f11ce4e5ec2' pushed


| Tag | sha |
| --- | --- |
| 0d8a4c9 | 3fbc10...ec2 |
| staging-0d8a4c9 | 3fbc10...ec2 | 
| master-0d8a4c9 | 40e66...168 |



### 3. lifecycle policy 2 removed staging image as no master tag is associated now ###
| `2020-09-15T05:24:00Z` | Tags: 0d8a4c9, staging-0d8a4c9 | Image 'sha256:3fbc1092bbcf068d189e769e19fa736111f6e64c0fce31adcb915f11ce4e5ec2' got deleted

### 4. got alerted for failed image pulling on 0d8a4c9 at `2020-09-15T14:49:00Z`
### 
<br>

### 5. Developer pushed a new image to fix the failed pulling on 0d8a4c9  ###

| `2020-09-15T19:35:16Z` | Tags: 0d8a4c9, master-0d8a4c9 | Image 'sha256:f0662af3916cb6c5006daf86fcca546e431025f05e52e42335546841a03f6980' pushed 

| Tag | sha |
| --- | --- |
| 0d8a4c9 | 3fbc10...ec2 |
| staging-0d8a4c9 | 3fbc10...ec2 |
| master-0d8a4c9 | f0662a...6980 |
| Untagged | 40e66...168 |


### 6. Now the original image has no tags associated and got removed by the lifecycle policy

| `2020-09-15T19:53:01Z` | Tags: Untagged | Image 'sha256:40e664b403086c18aa2732597bb0bf64f9a83f978607b19781cbef8e02bee168' got deleted | lifecycle policy triggered


## Conclusion:
When a staging builds after master on the same commit hash, then the commit hash tag will reference the staging image, result policy 2 to kick in after 15 days and remove the image. 

In normal cases, the image builds will follow staging, then master which result the same image having xxxxxxx(commit hash), staging-xxxxxxx, master-xxxxxxx, which means policy 1 will always take precedence, image will not be removed. Or they could have complete different build hash. In this case, when the same build gets triggered in staging, it resulted undesired deletions, which consequently cause error on image pulling.

To avoid this from happening in the future, short term solution is  to ensure the uniqueness of each build hash (include timestamp, or incremental id), long term solution is to use different image tags between staging and production.
