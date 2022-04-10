---
description: Create and Schedule Jobs to Be Run On the Moralis Dapp.
---

# Jobs

Sometimes you want to execute long-running functions, and you don’t want to wait for a response. <mark style="color:green;">**Cloud Jobs**</mark> are meant for just that.

### Define a Job

Define the job in your cloud functions.&#x20;

```javascript
    Moralis.Cloud.job("myJob", (request) =>  {
      // params: passed in the job call
      // headers: from the request that triggered the job
      // log: the Moralis Server logger passed in the request
      // message: a function to update the status message of the job object
      const { params, headers, log, message } = request;
      message("I just started");
      return doSomethingVeryLong(request);
    });
```

### Schedule a Job

You can schedule a job in the "Jobs" section in the dashboard by clicking "Schedule a job" in the top right corner.

![Click on Schedule a Job](../../.gitbook/assets/Moralis\_dashboard\_Schedule\_Job.png)

Pick a description, the job to run and provide parameters if needed

![](../../.gitbook/assets/Moralis\_dashboard\_schedule\_job\_options.png)

When you have configured your job, you confirm it by pressing "Schedule" in the bottom right corner.
