# si618w17-spark

This is a tutorial designed to help you install PySpark on your local machine in order to run and debug scripts locally for SI618 instead of having to run them on the Flux cluster. Flux runs Spark version 1.5, so we will focus on installing and using this version. See the [Flux hadoop documentation](http://arc-ts.umich.edu/hadoop-user-guide/) for more information on Flux's PySpark framework, and view the [Spark 1.5 documentation](http://spark.apache.org/docs/1.5.2/) for more information on using Spark. The [Apache Spark documentation](http://spark.apache.org/documentation.html) website also contains links to several videos, books, and tutorials designed to help new users learn Spark.


# Installing PySpark

## Step 1: Download Spark

In order to use Spark, we need to download and unpack it--Spark is really just a set of scripts and is relatively lightweight. Let’s start by downloading a recent precompiled released version of Spark. Visit http://spark.apache.org/downloads.html, select Spark version 1.5.2, the package type of “Pre-built for Hadoop 2.6,” and click “Direct Download.” This will download a compressed TAR file, or tarball, called spark-1.2.0-bin-hadoop2.4.tgz. Make sure to download the file to somewhere easy to access on your computer (i.e., in your home directory or another high-level directory).

Note to Windows users: Windows users may run into issues installing Spark into a direc‐ tory with a space in the name. Instead, install Spark in a directory with no space (e.g., C:\spark).

## Step 2: Untar your downloaded file

Open a terminal window, change to the directory where you just downloaded the compressed Spark file, and untar the file using the commands below. This will create a new directory with the same name but without the final .tgz suffix. 
```
$ cd~
$ tar -xf spark-1.5.2-bin-hadoop2.6.tgz 
$ cd spark-1.5.2-bin-hadoop2.6
$ ls
```

In the line containing the tar command, the x flag tells tar we are extracting files, and the f flag specifies the name of the tarball.

You will see several files, including:

* README.md: Contains short instructions for getting started with Spark.
* bin: Contains executable files that can be used to interact with Spark in various ways (e.g., the Spark shell, whichallows you to run your PySpark code).
* core, streaming, python, ... : Contains the source code of major components of the Spark project.
* examples : Contains some helpful Spark standalone jobs that you can look at and run to learn about the Spark API.

All of the work we will do will be with Spark running in local mode; that is, nondistributed mode, which uses only a single machine. Spark can run in a variety of different modes, or environments. Beyond local mode, Spark can also be run on Mesos, YARN (which Flux uses), or the Standalone Scheduler included in the Spark distribution. Note that local mode will be far less performant than using a cluster like Flux; the trade-off is that it's a bit simpler to edit and execute scripts on your own machine without dealing with the remote connection.

## Step 3: Open the PySpark shell

To open the interactive PySpark shell, navigate into the spark directory you just created (```spark-1.5.2-bin-hadoop2.6```) and type:

```
bin/pyspark
```

(in windows this will be ```bin\pyspark```).

```
...
17/02/02 20:07:26 INFO SparkEnv: Registering OutputCommitCoordinator
17/02/02 20:07:26 INFO Utils: Successfully started service 'SparkUI' on port 4040.
17/02/02 20:07:26 INFO SparkUI: Started SparkUI at http://192.168.0.102:4040
17/02/02 20:07:26 WARN MetricsSystem: Using default name DAGScheduler for source because spark.app.id is not set.
17/02/02 20:07:26 INFO Executor: Starting executor ID driver on host localhost
17/02/02 20:07:27 INFO Utils: Successfully started service 'org.apache.spark.network.netty.NettyBlockTransferService' on port 55743.
17/02/02 20:07:27 INFO NettyBlockTransferService: Server created on 55743
17/02/02 20:07:27 INFO BlockManagerMaster: Trying to register BlockManager
17/02/02 20:07:27 INFO BlockManagerMasterEndpoint: Registering block manager localhost:55743 with 530.0 MB RAM, BlockManagerId(driver, localhost, 55743)
17/02/02 20:07:27 INFO BlockManagerMaster: Registered BlockManager
Welcome to
      ____              __
     / __/__  ___ _____/ /__
    _\ \/ _ \/ _ `/ __/  '_/
   /__ / .__/\_,_/_/ /_/\_\   version 1.5.2
      /_/

Using Python version 2.7.9 (default, Dec 15 2014 10:37:34)
SparkContext available as sc, HiveContext available as sqlContext.
>>> 
```

You'll see several messages, and then the PySpark shell will open. Try some example commands:

```
>>> sc # this is your SparkContext; it is created for you automatically in the shell but must be initialized explicitly in your scripts
<pyspark.context.SparkContext object at 0x1038e2090>
>>> lines = sc.textFile("README.md") # Create an RDD called lines
>>> lines.count() # Count the number of items in this RDD
127
>>> lines.first() # First item in this RDD, i.e. first line of README.md u'# Apache Spark'
>>> lines.take(10)
...
[u'# Apache Spark', u'', u'Spark is a fast and general cluster computing system for Big Data. It provides', u'high-level APIs in Scala, Java, Python, and R, and an optimized engine that', u'supports general computation graphs for data analysis. It also supports a', u'rich set of higher-level tools including Spark SQL for SQL and DataFrames,', u'MLlib for machine learning, GraphX for graph processing,', u'and Spark Streaming for stream processing.', u'', u'<http://spark.apache.org/>']
>>> exit()
```

This is a great tool to use to run your Spark code line-by-line. Debugging Spark code is definitely not as nice as debugging standard Python, but the interactive shell makes is less painless, especially with small scripts like the ones we're using in this course.

## Step 4: Run a script
To run a script, just type:
```
$ bin/spark-submit my_script.py
```
(Note that you will have to use backslashes instead of forward slashes on Windows.)

Let's try running the example Spark word count script. (Note that this script isn't right in our working directory, so we need to give the path to that script; note also that we provide README.md as an additional input.)

```
$ bin/spark-submit examples/src/main/python/wordcount.py  README.md
```

You'll notice that you don't need to provide all of the extra parameters like ```--queue```, ```num_cores```, etc. here like you do in Flux. That's because we are running your script in local mode; you still need to specify these parameters if you choose to run your script on Flux.

Almost all of the example scripts in the python/ directory can be run similarly, without an input file:

```
$ bin/spark-submit examples/src/main/python/logistic_regression.py
```

## More Resources

If you're interested in learning more about Spark and PySpark, check out the resources here:

More Resources:
http://spark.apache.org/docs/1.5.2/
http://spark.apache.org/docs/1.5.2/api/python/index.html
http://spark.apache.org/docs/1.5.2/quick-start.html
http://spark.apache.org/docs/1.5.2/programming-guide.html

Also, this tutorial is based heavily on [Learning Spark: Lightning-Fast Data Analysis by Holden Karau et. al.](http://shop.oreilly.com/product/0636920028512.do). You can [access it through the UM Library here!](https://mirlyn.lib.umich.edu/Record/013603581) (Just click 'available online').



