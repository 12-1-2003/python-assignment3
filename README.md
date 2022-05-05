Most of the BigData analytics will be using Pandas, NumPy for analyzing big data. All the mentioned packages support a wide variety of computations. But when the dataset doesn’t fit in the memory these packages will not scale. Here comes dask. When the dataset doesn’t “fit in memory” dask extends the dataset to “fit into disk”. Dask allows us to easily scale out to clusters or scale down to single machine based on the size of the dataset. 

Installation
To install this module type the below command in the terminal – 

python -m pip install "dask[complete]" If you are into the data world then the need of using pandas might occur very frequently. Well, we all are also aware of one thing that Pandas is amazing and is more like a boon to the data world. Most of us spend at least a few minutes or even hours using pandas for data manipulation and day to day analysis. Pandas does not need much introduction but here are a few things that you should be aware of:

pandas is well suited for many different kinds of data:

Tabular data with heterogeneously-typed columns, as in an SQL table or Excel spreadsheet
Ordered and unordered (not necessarily fixed-frequency) time-series data.
Arbitrary matrix data (homogeneously typed or heterogeneous) with row and column labels
Any other form of observational/statistical data sets. The data need not be labeled at all to be placed into a pandas data structure.
Using pandas with gigabytes or terabytes of data is more like a pain in the notebook :D. I frequently use files that are into GBs and the notebook goes like “wait a minute, let me freeze now!”. By the way, my system has some amazing configuration. Dealing with a humongous amount of data cannot be done using pandas, Hence that’s where we have to start using dask. In this article I’ll demonstrate how you can use pandas with dask and speed up your notebook. In dask, reading GBs of files takes only a few seconds. Before jumping onto the demo, let me give you a brief introduction about dask.

Dask:

Dask has 3 parallel collections namely Dataframes, Bags, and Arrays. Which enables it to store data that is larger than RAM. Each of these can use data partitioned between RAM and a hard disk as well distributed across multiple nodes in a cluster. A Dask DataFrame is partitioned row-wise, grouping rows by index value for efficiency. These Pandas objects may live on disk or other machines. Dask DataFrames coordinate many Pandas DataFrames or Series arranged along the index

Dask can enable efficient parallel computations on single machines by leveraging their multi-core CPUs and streaming data efficiently from disk. It can run on a distributed cluster. Dask also allows the user to replace clusters with a single-machine scheduler which would bring down the overhead. These schedulers require no setup and can run entirely within the same process as the user’s session. Few other things about dask:

The ability to work in parallel with NumPy array and Pandas DataFrame objects
integration with other projects.
Distributed computing
Faster operation because of its low overhead and minimum serialization
Runs resiliently on clusters with thousands of cores
Real-time feedback and diagnostics
Demonstration:

The entire code used for this demonstration can be found in my Github repo and you can find the link at the end of this article. Now, let’s jump on to the demo:

Importing the libraries:


Code to calculate elapsed time and file size:


The first snippet is to calculate the elapsed time for an operation that we perform in here, for example: reading a file. In the second snippet we are displaying the file size that we’ll be using for this demo. The file size is around 4GB.

First, we’ll see a few things Dask

is better at and then we’ll skip to the things that pandas does better. This will help you to combine these two libraries and perform your analysis.

Dask over Pandas:
Reading a file — Pandas & Dask:


Pandas took around 5 minutes to read a file of size 4gb. Wait, the size is not everything, the number of columns and rows present in a data set plays a major role in the time consumption. Let’s see how much time Dask takes for the same file.


Holy moly, It just took around 2 milliseconds to read the same file whereas pandas took around 5 minutes. Isn’t it just amazing? Let’s perform some more operations on both the pandas data frame and the dask data frame.

Appending two files — Pandas & Dask:

To perform this operation, we’ll be reading another file and then append it to the previous one.


It took around 9 minutes to perform the above-mentioned operations. Now let’s see how we can optimize it by using Dask.


Well, Well, you just saved around another 9 minutes there. Let us see now other frequently used this we do with pandas.

Grouping the data — Pandas & Dask:


Pandas took over 6 minutes to do a simple grouping. Let’s see now how much more time we can save with dask.


Woah, you just saved whooping 6 minutes again. There numerous other things where you can save much more time if you are using dask.

Merging the datasets — Pandas & Dask:



So this is what happened. It tried for like 30 minutes and still couldn’t merge those two files with pandas. Let’s see if we can do it by using dask.


Well, It barely took me a second to do it using dask. What just happened is, Unlike pandas.read_csv which reads in the entire file before inferring data types, dask.dataframe.read_csv only reads in a sample from the beginning of the file (or first file if using a glob). These inferred data types are then enforced when reading all partitions.

Pandas over Dask:
Sorting — Pandas & Dask:


I tried to sort the data frame based on the values in a column and it took me around a minute and a half, that’s pretty decent. Let’s see how dask can help us with this.


Unfortunately, dask cannot even start this task because dask has no functionality of sorting although it uses pandas API. All hail pandas!

Unique & notNA — Pandas & Dask:


It took around only 1 minute to get these tasks done. Pandas does most of the things pretty well but screws in quite a few.


Dask does not support these two things as well. There are numerous other things for which you’ll have to use pandas.

Saving a Dataframe to a file — Pandas & Dask:


Pandas does a good job of saving the file. It took around 3 minutes for me to save the filtered file.


