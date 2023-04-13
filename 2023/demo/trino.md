Hello, every one this is Ella speaking, this demo is about graphql trino's performance Summary

First of all, why do we use trino? Trino supports queries across different data sources, real-time calculations, local calculations, and fast calculations. After migrate graphql's calculations to trino, to improve request response time.

Then, let's look at a set of data. This set of data is the previous graphql and the trino version of graphql  calling the three requests summaryActivity/group by activities/filter activities, graphql db request response time comparison chart,

 The JIC data used in the environment, 

we can see that graphql uses trino, the request response time will be much faster than the previous graphql, such as groupby activity, the previous graphql took 108 seconds, but the trino version graphql only took 1.75 seconds, and the filter activity request, the previous graphql running timed out, but now the trino version graphql can return results in more than 40 seconds

Thirdly, after using trino, the performance of trino depending on the number of workers and cup settings. Currently, we set up 4 workers, each worker cup is set to 2000, and memory is set to 4G。 Now you can query the results of JIC data and 201 data, and The cost performance ratio of trino configuration is the best。

Let's look at this chart. For example, the number of our wokers is fixed at 4, and each worker's memory is set to 4G, 8G, 16G, and 32G. Let's take the example of total AUM, and Groupby position requests. We can see that as the worker's memory increases Larger, the request response time has increased, but it is not proportional to the increase in the speed of the cup. Let's look at the filter position request for example. When the memeory increases to 16G, the response speed of the request does not increase when the memory is increased.

We are looking at fixing the meomory and cup of each woker and increasing the number of workers. We can see that as the number of workers increases to 2, 4, and 8, the request response speed will increase, but it will not continue to increase to a certain number, so we can conclude that the database data connected by trino is different, and there are different optimal configurations. Currently we use 4 workers, each worker cup is 2000, and the memory is set to 4G. The cost performance is the highest, and the response speed is also what we can achieve. Accepted, can accept 50 threads, 60 seconds of porformance test, and test with 201 and 600 data at the same time

At the same time, if our total CUP is 4, the total memory is set to 8G, we set the number of different workers, for example, if the worker number is set to 2, then each worker cup is 2, and the memory is 4G, if we set the worker number to 4, Then each worker cup is 1, memory is 2G, let's compare the performance,

From the chart, we can see that when the woker cup is 0.5 and the memory is 1G, the worker cannot work normally. Let’s look at the totalaum and groupby position requests. When the total cup and momory remain unchanged, the increase in the worker number will reduce the request Response time, and the filter position request, when the worker increases to 4, the request takes more time than when the worker is 2, so we conclude that,

First There are minimum limits for worker cup and memoty, the cup cannot be lower than 1000, and the memory cannot be lower than 2G
	Second, if we add resources to trino, adding the number of workers will be better than adding cup and memoty to every workers

Third, when the total resources remain unchanged. For some requests, such as filter position, increasing the number of workers to a certain amount will not increase the response time of the request



Finally, let's look at a set of data， under 201 data,graphql trino performance will be slower than the previous graphql request response time, 

such as groupby activity trino version, filter activty DB average response time is 420 milliseconds, while the average response time of graphql before is 16 milliseconds, we You can see that compared with the previous graphql version, the trino version of graphql is a tug-of-war between the time taken by trino to execute sql and the time taken by graphql dateframe. When there is a lot of data, the trino version is fast, and when there is little data, the dataframe calculation is fast, so next we will Continue to optimize the sql executed by trino,

For example Compared with the join method that we use in SQL, we used to get the portfolio code through the portfolio group code first, and then use the portfolio code to filter the activity table to get data. Now we use the sql join method,  we join table actiivty and portfolio by group， on portfolio code and the tenant id, 

so the sql filters will does not have the portfolio code at this time. so , the portfolio code should be moved from the partition key to the cluster key in the activity table.

In this way, the execution speed of the current SQL will be faster. This has been verified by Hao and me. Regarding the partition, please see Hao’s sharing.

Regarding the performance optimization of trino, we will continue to research, and the configration of trino has been written on the confluence page

That’all, thanks, any question?

