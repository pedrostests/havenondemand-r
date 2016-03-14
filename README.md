# HPE Haven OnDemand R Client Library

----
## Overview
This library can be used to consume [HPE Haven OnDemand - https://dev.havenondemand.com/apis](https://dev.havenondemand.com/apis) in Salesforce.

----
## What is HAVEN ONDEMAND?
Haven OnDemand is a set of over 70 APIs for handling all sorts of unstructured data. Here are just some of our APIs' capabilities:

- Speech to text
- OCR
- Text extraction
- Indexing documents
- Smart search
- Language identification
- Concept extraction
- Sentiment analysis
- Web crawlers
- Machine learning

For a full list of all the APIs and to try them out, check out [https://www.havenondemand.com/developer/apis](https://dev.havenondemand.com/apis).

----
## Installation

Development version from github:

install.packages("devtools")
devtools::install_github("chok68/havenondemand-r")

## Usage

### Initialization

``` R
# include havenondemand library
library(havenondemand)

# with apikey and version
client <- HODClient(apikey = "your-api-key", version = "v1")

# with apikey (version will default to v1)
client <- HODClient(apikey = "your-api-key")
```
### Sample post sync call (INDEX_STATUS)

``` R
# include havenondemand library
library(havenondemand)

# initialize HOD Client
client <- HODClient(apikey = apikey)

# get response
# include havenondemand library
library(havenondemand)

# initialize HOD Client
client <- HODClient(apikey = "0ba06d23-d053-4b1f-802f-ef296cf25ac7")

# call that result in error ('ur' parameter is wrong, it should be 'url')

result <- tryCatch({
    client$post(params = list(index = "test"), hodApp = HODApp$INDEX_STATUS, mode = HODClientConstants$REQUEST_MODE$SYNC)
}, warning = function(w) {
    print('Warning block called.')
}, error = function(e) {
    print('Error block called.')
    print(e)
}, finally = {
    print('Finally block called.')
})

# print result
print(result)
```

### Sample post async call (list resources)

``` R
# include havenondemand library
library(havenondemand)

# initialize HOD Client
client <- HODClient(apikey = "0ba06d23-d053-4b1f-802f-ef296cf25ac7")

result <- tryCatch({

    # create client get job id
    r <- client$postRequest(params = 
           list(flavor = 'standard', flavor = 'explorer', type = 'content', type = 'connector')
           , hodApp = HODApp$LIST_RESOURCES, mode = HODClientConstants$REQUEST_MODE$ASYNC)

    # get job id
    jobID = httr::content(r, 'parsed')$jobID
    print (jobID)

    # check job status using the getJobStatus method call
    # If job status is finished then getJobStatus method itself will return job data as part of response
    jobStatus <- client$getJobStatus(jobID)
    print (jobStatus)

    # get job data using the getJobResult method
    jobResult <- client$getJobResult(jobID = jobID)
    print(jobResult)

}, warning = function(w) {
    print('Warning block called.')
}, error = function(e) {
    print('Error block called.')
    print(e)
}, finally = {
    print('Finally block called.')
})

# print result
print(result)
```

### Sample post async call with file attachment (PREDICT API)

``` Apex
try{
      HODClient client = new HODClient(apiKey, version);
      // list of Param has to be passed for request with file attachment
      List<Param> params = new List<Param>();
      params.add(new Param('test.csv',Blob.valueOf(csvString),'application/CSV'));
      params.add(new Param('service_name','test'));
      // call API
      Map<String,Object> data = client.postRequest(params, HODAPP.PREDICT, HODClientConstants.REQ_MODE.ASYNC);
      String jobId = data.get(HODClientConstants.JOB_ID);
}
catch (HODClientException ex)
{
     String message = ex.getMessage();
     System.debug(message);
}


```

### Sample post async call with multiple file attachments (Text Extraction API)

``` R
# include havenondemand library
library(havenondemand)

# initialize HOD Client
client <- HODClient(apikey = "0ba06d23-d053-4b1f-802f-ef296cf25ac7")

result <- tryCatch({

    # create client get job id

    # upload a file named 'broch.pdf' in drive c: (windows os), adjust path to your os / file name.
    r <- client$postRequest(params = list(
            file = httr::upload_file("c:/broch.pdf"),
            file = httr::upload_file("c:/HP_License_terms_may2012.doc"))
           , hodApp = HODApp$TEXT_EXTRACTION, mode = HODClientConstants$REQUEST_MODE$ASYNC)

    # get job id
    jobID = httr::content(r, 'parsed')$jobID
    print (jobID)

    # check job status using the getJobStatus method call
    # If job status is finished then getJobStatus method itself will return job data as part of response
    jobStatus <- client$getJobStatus(jobID)
    print (jobStatus)

    # get job data using the getJobResult method
    jobResult <- client$getJobResult(jobID = jobID)
    print(jobResult)

}, warning = function(w) {
    print('Warning block called.')
}, error = function(e) {
    print('Error block called.')
    print(e)
}, finally = {
    print('Finally block called.')
})

# print result
print(result)
``` 
### Error Handling

All the calls call stop(message) if an error is detected:
``` R
# include havenondemand library
library(havenondemand)

# initialize HOD Client
client <- HODClient(apikey = "0ba06d23-d053-4b1f-802f-ef296cf25ac7")

# call that result in error ('ur' parameter is wrong, it should be 'url')

result <- tryCatch({
    client$postRequest(params = list(ur = "https://www.havenondemand.com/sample-content/videos/hpnext.mp4"), hodApp = "recognizespeech", mode = "aSyNc")
}, warning = function(w) {
    print('Warning block called.')
}, error = function(e) {
    print('Error block called.')
    print(e)
}, finally = {
    print('Finally block called.')
})

```

### HODClient Instance Methods

``` R

    # calls POST Request
    #
    # @param params params to be passed
    # @param hodApp end point to be called
    # @param mode sync/async
    # @return json response
    postRequest = function(params, hodApp, mode)
```


``` R
    # Get status of the job submitted
    # @param jobId id of the job submitted
    getJobStatus = function(jobID)
```

``` R
    # Get result of the job submitted
    # @param jobId id of the job submitted
    getJobResult = function(jobID)
```

## Contributing
TODO: Pending from HPE Ondemand team
